# 02 - Core Configuration Files

Linux manages users and groups through four primary system configuration files located under `/etc/`:
1. `/etc/passwd` - User account details and metadata (World readable)
2. `/etc/shadow` - Encrypted user passwords and password aging details (Root readable only)
3. `/etc/group` - Group definitions and membership lists (World readable)
4. `/etc/gshadow` - Encrypted group passwords and group administrators (Root readable only)

---

## 📄 1. `/etc/passwd`

The `/etc/passwd` file contains essential account information for every user on the system. It is a plain-text file where each line represents one user account formatted with 7 colon-separated (`:`) fields.

### Permissions
* **File Permissions:** `-rw-r--r--` (`644`)
* **Owner:** `root:root`
* Must be world-readable so applications and utilities (`ls -l`, `whoami`, `id`) can map UIDs to human-readable account usernames.

### Syntax Breakdown

```text
username:password_flag:UID:GID:GECOS_comment:home_directory:default_shell
```

```text
alice:x:1001:1001:Alice Smith,Rm 402,555-1234,555-5678:/home/alice:/bin/bash
  │   │  │    │   └──────────────────────────────────┘      │         │
  │   │  │    │                    │                        │         └─ 7. Login Shell
  │   │  │    │                    │                        └─────────── 6. Home Directory
  │   │  │    │                    └──────────────────────────────────── 5. GECOS (User Comment)
  │   │  │    └───────────────────────────────────────────────────────── 4. Primary GID
  │   │  └────────────────────────────────────────────────────────────── 3. UID
  │   └───────────────────────────────────────────────────────────────── 2. Password Placeholder ('x')
  └───────────────────────────────────────────────────────────────────── 1. Username
```

| Field # | Field Name | Description |
| :---: | :--- | :--- |
| **1** | `Username` | Unique text name for login (e.g., `alice`). Max 32 characters. |
| **2** | `Password` | Historical password field. Today contains `x`, indicating the encrypted hash is stored securely in `/etc/shadow`. |
| **3** | `UID` | User Identification Number (e.g., `1001`). |
| **4** | `Primary GID` | Primary Group Identification Number (e.g., `1001`). Matches group in `/etc/group`. |
| **5** | `GECOS / Comment` | General Electric Comprehensive Operating System field. Holds optional metadata (Full Name, Room Number, Office Phone, Home Phone, Other). |
| **6** | `Home Directory` | Absolute path to user's home directory (e.g., `/home/alice`). |
| **7** | `Default Shell` | Absolute path to the user's initial login shell (e.g., `/bin/bash`, `/sbin/nologin`). |

---

## 🔒 2. `/etc/shadow`

The `/etc/shadow` file stores encrypted password hashes and password aging enforcement parameters.

### Permissions
* **File Permissions:** `-rw-r-----` (`640` on Debian/Ubuntu) or `----------` (`0000` / `0600` on RHEL/CentOS).
* **Owner:** `root:shadow` or `root:root`.
* Access restricted strictly to privileged root processes to prevent offline password hash cracking attacks (using tools like John the Ripper or Hashcat).

### Syntax Breakdown

Each line contains 9 colon-separated fields:

```text
username:encrypted_password:last_change:min_age:max_age:warn_days:inact_days:expire_date:reserved
```

Example line:
```text
alice:$6$qx8Z9K1v$P2W...:19700:0:90:7:14:20089:
```

| Field # | Field Name | Explanation & Units |
| :---: | :--- | :--- |
| **1** | `Username` | Account name matching `/etc/passwd`. |
| **2** | `Encrypted Password` | Hash string prefixed with algorithm identifier (`$1$` = MD5, `$5$` = SHA-256, `$6$` = SHA-512, `$y$` = yescrypt). If set to `!` or `*`, account password is locked/disabled. |
| **3** | `Last Password Change` | Days since **Jan 1, 1970 (Epoch)** when password was last changed. (e.g., `19700`). |
| **4** | `Minimum Password Age` | Minimum days required before user is allowed to change password again. (`0` = change anytime). |
| **5** | `Maximum Password Age` | Maximum days password remains valid before mandatory change required. (`90` = valid 90 days). |
| **6** | `Warning Period` | Days prior to password expiration that user receives expiration warning messages on login (`7` days). |
| **7** | `Inactivity Period` | Days after password expiration before account is automatically locked out (`14` days). |
| **8** | `Expiration Date` | Absolute date account expires, measured in days since Jan 1, 1970. Empty = never expires. |
| **9** | `Reserved` | Reserved field for future use. |

---

## 👥 3. `/etc/group`

Defines groups on the system and lists their members.

### Permissions
* **File Permissions:** `-rw-r--r--` (`644`)
* **Owner:** `root:root`

### Syntax Breakdown

Contains 4 colon-separated fields:

```text
group_name:password_placeholder:GID:user_list
```

Example:
```text
devs:x:1005:alice,bob,charlie
```

| Field # | Field Name | Description |
| :---: | :--- | :--- |
| **1** | `Group Name` | Unique identifier string for group (e.g., `devs`). |
| **2** | `Password` | Group password placeholder (`x`). Used if group password stored in `/etc/gshadow`. |
| **3** | `GID` | Group Identification Number (e.g., `1005`). |
| **4** | `User List` | Comma-separated list of **supplementary** group members (`alice,bob,charlie`). Note: Primary members are defined in `/etc/passwd` and omitted here. |

---

## 🔐 4. `/etc/gshadow`

Stores secure encrypted group passwords and designated group administrators.

### Permissions
* **File Permissions:** `-rw-r-----` (`640` or `0600`)
* **Owner:** `root:shadow` or `root:root`

### Syntax Breakdown

4 colon-separated fields:
```text
group_name:encrypted_password:group_administrators:group_members
```

Example:
```text
devs:$6$kL8m...:alice:bob,charlie
```
* **Group Administrators:** Users who can add/remove members using `gpasswd` without root privileges.
