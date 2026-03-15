# MaiBot Supported Command Parameter Table

## Mute Someone
```python
Seg.data: Dict[str, Any] = {
    "name": "GROUP_BAN"
    "args": {
        "qq_id": "User QQ number",
        "duration": "Mute duration (seconds)"
    },
}
```

The group chat ID will be automatically obtained through Group_Info.group_id.