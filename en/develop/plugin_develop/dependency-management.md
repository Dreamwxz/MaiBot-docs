# 📦 Plugin Dependency Management System

Current Python dependency package management still has issues, please keep your `python_dependencies` attribute, waiting for subsequent refactoring.

## 📚 Detailed Tutorial

### PythonDependency Class Details

`PythonDependency` is the core class for dependency declaration:

```python
PythonDependency(
    package_name="PIL",          # Package name when importing
    version=">=11.2.0",          # Version requirement
    optional=False,              # Whether optional dependency
    description="Image processing library",     # Dependency description
    install_name="pillow"        # Package name when pip installing (optional)
)
```

#### Parameter Description

| Parameter | Type | Required | Description |
|------|------|------|------|
| `package_name` | str | ✅ | Package name used when Python importing (e.g., `requests`) |
| `version` | str | ❌ | Version requirement, using pip format (e.g., `>=1.0.0`, `==2.1.3`) |
| `optional` | bool | ❌ | Whether optional dependency, default `False` |
| `description` | str | ❌ | Dependency usage description |
| `install_name` | str | ❌ | Package name when pip installing, default same as `package_name`, used to handle situations where installation name and import name are inconsistent |

#### Version Format Examples

```python
# Common version formats
PythonDependency("requests", ">=2.25.0")           # Minimum version
PythonDependency("numpy", ">=1.20.0,<2.0.0")       # Version range
PythonDependency("pillow", "==8.3.2")              # Exact version
PythonDependency("scipy", ">=1.7.0,!=1.8.0")       # Exclude specific version
```