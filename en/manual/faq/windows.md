# Windows FAQ

This page summarizes common issues and solutions you may encounter when deploying MaiBot on Windows systems. If you encounter other problems, feel free to provide feedback via Issues or group chats.

---

## 1. Dependency Installation Issues

**Q: What to do if `quick_algo` installation fails when installing dependencies?**

If you encounter compilation/installation failure of `quick_algo` when using `uv pip install` or `pip install` to install dependencies, please refer to the following steps:

1. **Confirm compilation environment**: Ensure you have installed C/C++ compilation environments like Visual C++ Build Tools or MinGW.
2. **Refer to LPMM documentation**: `quick_algo` is a core dependency of the LPMM (Language Model Memory Management) module. For installation methods, see the "Installing quick-algo" or "Manual compilation" sections in the [LPMM Knowledge Base](/en/manual/configuration/lpmm/lpmm).
3. **Temporarily skip**: If only for testing, you can try commenting out the `quick_algo` related lines in `requirements.txt` (but not recommended for long-term use).

**Note**: This issue may occur with both `uv` and traditional `pip` installations, and the solution is the same.

---

## 2. Virtual Environment Issues

**Q: What are the considerations when using uv vs traditional venv?**

- Virtual environments created with `uv venv` have different mechanisms from those created with `python -m venv` - **do not mix them**.
- When using `uv`, it's recommended to use the `uv run` command throughout to run scripts, as it automatically manages the virtual environment.
- When using traditional venv, you need to manually activate it before each run: `.\venv\Scripts\activate` (PowerShell) or `venv\Scripts\activate.bat` (CMD).

---

## 3. Other Common Issues

**Q: What to do if Napcat won't connect?**

1. Confirm that Napcat has started normally and that you've created a **WebSocket client** (reverse proxy) in Napcat.
2. Check if the `host` and `port` in `[Napcat_Server]` in `MaiBot-Napcat-Adapter/config.toml` match the Napcat settings.
3. Ensure the `PORT` in MaiBot's `.env` matches the `port` in `[MaiBot_Server]`.
4. The firewall may be blocking local port communication; you can temporarily disable the firewall for testing.

**Q: Getting "port already in use" error on startup?**

Modify the `PORT` value in the `.env` file to another unused port (such as 8001, 8080, etc.), and synchronously update the `[MaiBot_Server].port` in the Adapter configuration.

---

If the above content doesn't solve your problem, please refer to the [General FAQ](../faq/index.md) or visit the [MaiBot Community](/en/manual/other/group) for help.

(End of file - total 46 lines)