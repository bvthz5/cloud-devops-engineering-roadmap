# 05 - Reusing Content: `import_*` vs `include_*` (Static vs Dynamic)

One of the most fundamental architectural distinctions in Ansible is the difference between **Static Imports** (`import_tasks`, `import_role`, `import_playbook`) and **Dynamic Includes** (`include_tasks`, `include_role`).

While both directives allow modularizing playbooks and roles across multiple files, they execute through entirely different compilation pipelines in the Ansible engine. Choosing between them directly impacts how loops, conditionals (`when`), tags, handlers, and inventory variables are processed.

---

## 🏗️ 1. The Core Architectural Distinction: Parse Time vs. Run Time

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        STATIC IMPORTS vs. DYNAMIC INCLUDES                             │
│                                                                                        │
│   STATIC IMPORTS (import_tasks / import_role)                                          │
│   Parsed at PLAYBOOK COMPILE TIME (Pre-processing stage)                               │
│                                                                                        │
│   Playbook.yml             Inlined at Start                                            │
│   ┌────────────────────┐    ┌───────────────────────────────────────────────┐          │
│   │ - import_tasks:    │ ─► │ TASK 1: [imported file - step 1]              │          │
│   │     setup.yml      │    │ TASK 2: [imported file - step 2]              │          │
│   └────────────────────┘    │ TASK 3: [next regular task]                   │          │
│                             └───────────────────────────────────────────────┘          │
│                             • Behave like a C preprocessor #include                    │
│                             • Full visibility in 'ansible-playbook --list-tasks'       │
│                                                                                        │
│   ──────────────────────────────────────────────────────────────────────────────────   │
│                                                                                        │
│   DYNAMIC INCLUDES (include_tasks / include_role)                                      │
│   Parsed at RUNTIME (Execution engine encounters the include task)                     │
│                                                                                        │
│   Playbook.yml             Evaluated Host-by-Host                                      │
│   ┌────────────────────┐    ┌───────────────────────────────────────────────┐          │
│   │ - include_tasks:   │ ─► │ TASK: include_tasks (evaluated dynamically)   │          │
│   │     "{{ os }}.yml" │    │   └── Target 1 executes RedHat.yml            │          │
│   └────────────────────┘    │   └── Target 2 executes Debian.yml            │          │
│                             └───────────────────────────────────────────────┘          │
│                             • Treated as a single task until reached in execution      │
│                             • Supports loops and dynamic variable filenames            │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## ⚖️ 2. Comprehensive Comparison Matrix

| Technical Capability | Static Imports (`import_*`) | Dynamic Includes (`include_*`) |
| :--- | :--- | :--- |
| **Parsing Phase** | **Pre-processing (Compile Time)** before any tasks run. | **Execution Time (Runtime)** when task is reached. |
| **Dynamic Filenames (`{{ variable }}`)** | ❌ **No** (variables cannot be resolved at parse time). | ✅ **Yes** (`include_tasks: "{{ ansible_facts['os_family'] }}.yml"`). |
| **Loops (`loop:` / `with_items:`)** | ❌ **Not Allowed** (Ansible will error during syntax check). | ✅ **Fully Supported** (iterates and executes the file per item). |
| **Conditional (`when:`) Behavior** | Condition is **copied and applied to every individual child task**. | Condition is **evaluated once on the include statement itself**. |
| **Tags (`--tags`) Compatibility** | ✅ Children inherit tags; fully visible in `--list-tags`. | ⚠️ Children are NOT visible until runtime; `--list-tags` misses them. |
| **`ansible-playbook --list-tasks`** | ✅ Displays every single nested task. | ❌ Only displays the top-level `include_tasks` step. |
| **Handler Notification** | ✅ Handlers defined in imported roles can be notified anytime. | ⚠️ Handlers are not registered until the include task executes. |
| **Performance Overhead** | ⚡ **Fastest** (loaded once into memory during startup). | ⏱️ Slight overhead per host (dynamically evaluated per node). |

---

## 🔬 3. Deep Dive: Behavioral Differences

### 1. Dynamic Task Routing Based on Host Facts

Because `import_*` evaluates before any tasks (including fact gathering) can resolve host-specific variables, you **cannot** import a file whose name depends on a discovered fact:

```yaml
# ❌ THIS WILL FAIL WITH A PARSE ERROR:
- name: Import OS tasks
  ansible.builtin.import_tasks: "{{ ansible_facts['os_family'] }}.yml"

# ✅ THIS SUCCEEDS AT RUNTIME:
- name: Include OS tasks dynamically
  ansible.builtin.include_tasks: "{{ ansible_facts['os_family'] }}.yml"
```

### 2. Conditionals (`when:`) on Imports vs. Includes

Consider the following conditional applied to an import:

```yaml
# Static Import with when:
- name: Import maintenance tasks
  ansible.builtin.import_tasks: maintenance.yml
  when: run_maintenance | default(false) | bool
```

Under `import_tasks`, Ansible rewrites every single task inside `maintenance.yml` to include `when: run_maintenance | bool`. If `maintenance.yml` contains 20 tasks, Ansible will output 20 `skipping: [host]` lines if the condition is false.

Under `include_tasks`:

```yaml
# Dynamic Include with when:
- name: Include maintenance tasks
  ansible.builtin.include_tasks: maintenance.yml
  when: run_maintenance | default(false) | bool
```

Ansible evaluates the condition once. If false, it outputs a single `skipping: [host]` line and completely skips reading `maintenance.yml`.

### 3. Looping Over Roles or Tasks

Static imports cannot accept loops because Ansible cannot determine how many tasks will exist before execution starts:

```yaml
# ❌ SYNTAX ERROR:
- name: Import role in loop
  ansible.builtin.import_role:
    name: database_user
  loop: "{{ database_users }}"

# ✅ FULLY SUPPORTED:
- name: Include role dynamically in loop
  ansible.builtin.include_role:
    name: database_user
  loop:
    - { name: 'app_user', db: 'production' }
    - { name: 'reporting_user', db: 'analytics' }
  loop_control:
    loop_var: current_user
```

---

## 🎯 4. Production Decision Matrix: When to Use Which

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        DECISION FLOWCHART                              │
│                                                                        │
│   Do you need to loop over the tasks or role?                          │
│   ├── YES ──────────────────────────────────────► Use include_*        │
│   └── NO                                                               │
│        │                                                               │
│   Does the file or role name use a variable (e.g. {{ os_family }})?    │
│   ├── YES ──────────────────────────────────────► Use include_*        │
│   └── NO                                                               │
│        │                                                               │
│   Do you need tags (--tags) or --list-tasks to work accurately?        │
│   ├── YES ──────────────────────────────────────► Use import_*         │
│   └── NO ───────────────────────────────────────► Use import_* (Faster)│
└────────────────────────────────────────────────────────────────────────┘
```

### Summary of Best Practices
1. **Default to `import_tasks` and `import_role`**: Use static imports for normal codebase organization. They provide compile-time syntax validation, complete `--list-tasks` visibility, and seamless `--tags` behavior.
2. **Reserve `include_tasks` and `include_role` for Dynamic Needs**: Use dynamic includes strictly when you must iterate over a list of items (`loop:`) or branch based on runtime host facts (`ansible_facts['os_family']`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Role Variable Scoping defaults vs vars](./04-Role-Variable-Scoping-defaults-vs-vars.md) | [Index](../../../README.md) | [06 - Structuring Production Ansible Repositories →](./06-Structuring-Production-Ansible-Repositories.md) |
