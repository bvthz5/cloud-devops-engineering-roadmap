# 02 - Host Groups, Nested Groups & Built-in Groups

## Built-in Default Groups
- `all`: Contains every host defined in inventory.
- `ungrouped`: Contains hosts not belonging to any explicitly defined group.

## Nested Groups (Group of Groups)
```ini
[us_east_web]
web01.east
web02.east

[us_west_web]
web01.west

[webservers:children]
us_east_web
us_west_web
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Static Inventories](./01-Static-Inventories-INI-vs-YAML-Formats.md) | [README](./README.md) | [03 - group_vars & host_vars](./03-Inventory-Variables-host_vars-and-group_vars.md) |
