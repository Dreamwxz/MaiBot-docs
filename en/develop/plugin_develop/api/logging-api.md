# Logging API

The Logging API module provides functionality for obtaining the main logger, allowing plugins to record log information.

## Import Method

```python
from src.plugin_system.apis import get_logger
# or
from src.plugin_system import get_logger
```

## Main Functions
### 1. Get Main Logger
```python
def get_logger(name: str) -> structlog.stdlib.BoundLogger:
```
Get main logger instance.

**Args:**
- `name` (str): Logger name.

**Returns:**
- A logger instance with the following methods:
    - `debug`
    - `info`
    - `warning`
    - `error`
    - `critical`