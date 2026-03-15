# Linux FAQ

This page summarizes common issues and solutions you may encounter when deploying MaiBot on Linux systems. If you encounter other problems, feel free to provide feedback via Issues or group chats.

---

## 1. Preparation Before Deployment

**Q: I'm not familiar with Linux systems and find the deployment steps too difficult. What should I do?**

If you're not very familiar with Linux commands and system administration, we recommend prioritizing the following simpler deployment methods:

- **Docker deployment**: Provides a containerized one-click running solution. See the [Docker Deployment Guide](../deployment/mmc_deploy_docker.md) for details.
- **Windows deployment**: Operate through graphical interface on Windows systems. See the [Installation Guide](../deployment/installation.md) for details.

---

## 2. Virtual Environment Issues

**Q: Can traditional venv and uv virtual environments be mixed?**

**No**. Traditional `venv` (created via `python -m venv`) and modern `uv` (created via `uv venv`) are two different virtual environment mechanisms - **they absolutely cannot be mixed**.

- If you choose `uv`, use `uv` commands throughout (`uv pip install`, `uv run`, etc.).
- If you choose traditional `venv`, activate the environment using the `venv`'s `activate` script before performing operations.
- Mixing them will cause dependency installation location confusion, leading to exceptions like `ModuleNotFoundError`.

**Q: When using traditional venv, do I need to manually activate it every time?**

Yes. When using traditional `venv`, **you need to manually activate the virtual environment every time you open a new terminal window or tab**. Otherwise, it will use the system's global Python environment, causing dependencies not to be found.

Activation command (in the MaiBot project root directory):
```bash
source venv/bin/activate
```

After activation, the terminal prompt will display the `(venv)` identifier.

**Q: Getting "command not found" or "module does not exist" error on startup?**

Please check:

1. **Whether the virtual environment is activated**: Confirm that the terminal prompt has `(venv)` or that you're using `uv run`.
2. **Whether the startup method matches the installation method**: If you installed dependencies with traditional `venv`, you should also use `source venv/bin/activate && python bot.py` when starting; if you installed with `uv`, you should use `uv run python bot.py` when starting.
3. **Whether the current path is correct**: Ensure you're executing the startup command in the MaiBot project root directory.

---

## 3. Dependency Installation Issues

**Q: What to do if `quick_algo` compilation fails when installing dependencies?**

`quick_algo` is a core dependency of the LPMM (Language Model Memory Management) module and requires compiling C/C++ extensions. If installation fails:

1. **Install compilation toolchain**: Ensure the system has compilation tools like `gcc`, `g++`, `make` installed.
   ```bash
   # Debian/Ubuntu
   sudo apt update && sudo apt install build-essential
   # CentOS/RHEL
   sudo yum groupinstall "Development Tools"
   ```
2. **Refer to LPMM documentation**: For detailed installation and manual compilation steps, refer to the "Manual Compilation" section in the [LPMM Usage Instructions](/en/manual/configuration/lpmm/lpmm).
3. **Temporarily skip**: If only for testing, you can try commenting out the `quick_algo` related lines in `requirements.txt` (but not recommended for long-term use).

---

## 4. Running and Connection Issues

**Q: What to do if Napcat won't connect?**

1. Confirm that Napcat has started normally and that you've created a **WebSocket client** (reverse proxy) in Napcat.
2. Check if the `host` and `port` in `[Napcat_Server]` in `MaiBot-Napcat-Adapter/config.toml` match the Napcat settings.
3. Ensure the `PORT` in MaiBot's `.env` matches the `port` in `[MaiBot_Server]`.
4. Check if the firewall is blocking local loopback (`127.0.0.1`) port communication:
   ```bash
   sudo ufw status  # Check firewall status
   ```

**Q: Getting "port already in use" error on startup?**

Modify the `PORT` value in the `.env` file to another unused port (such as 8001, 8080, etc.), and synchronously update the `[MaiBot_Server].port` in the Adapter configuration.

---

If the above content doesn't solve your problem, please refer to the [General FAQ](../faq/index.md) or visit the [MaiBot Community](/en/manual/other/group) for help.

(End of file - total 85 lines)