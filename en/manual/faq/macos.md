# macOS FAQ

This page summarizes common issues and solutions you may encounter when deploying MaiBot on macOS systems. If you encounter other problems, feel free to provide feedback via Issues or group chats.

---

## 1. Compilation and Dependency Issues

**Q: What to do if `quick_algo` compilation fails?**

`quick_algo` is a core dependency of the LPMM (Language Model Memory Management) module and requires local compilation of C/C++ extensions. If compilation fails, check the following conditions:

1. **Xcode Command Line Tools**: Ensure Xcode Command Line Tools are installed.
   ```bash
   xcode-select --install
   ```
   After installation, verify:
   ```bash
   xcode-select -p
   ```

2. **Python version**: Confirm Python version ≥ 3.10.
   ```bash
   python3 --version
   ```

3. **Virtual environment**: Ensure you're operating within a virtual environment to avoid conflicts with the system Python environment.

4. **Refer to LPMM documentation**: If it still fails, refer to the manual compilation guide in the [LPMM Usage Instructions](/en/manual/configuration/lpmm/lpmm).

---

## 2. System Security and Permissions

**Q: What to do if getting "cannot be opened" or "from an unidentified developer" prompts at runtime?**

macOS displays security warnings for unsigned applications. If you encounter such prompts when running MaiBot or related scripts:

1. In "System Preferences" → "Security & Privacy" → "General", check if there's an "Allow from..." button and click to allow.
2. If no button appears, try adding execution permissions to the script in the terminal:
   ```bash
   chmod +x script_name.sh
   ```
3. If still blocked, you can temporarily disable Gatekeeper (only for testing environments, not recommended for long-term use):
   ```bash
   sudo spctl --master-disable
   ```
   **Note**: After completion, please re-enable it:
   ```bash
   sudo spctl --master-enable
   ```

---

## 3. Ports and Networking

**Q: Getting "port already in use" error on startup?**

The default port 8000 may be occupied by other applications. Solutions:

1. **Change port**: Edit the `.env` file, change `PORT` to another value (such as 8001, 8080, etc.), and synchronously update the `[MaiBot_Server].port` in the Adapter configuration.
2. **Check occupying process**:
   ```bash
   lsof -i :8000
   ```
   Find the PID occupying the port, then terminate it:
   ```bash
   kill -9 <PID>
   ```
3. **Check firewall**: Ensure the macOS firewall isn't blocking local port communication (System Preferences → Security & Privacy → Firewall).

---

## 4. Virtual Environment Issues

**Q: Virtual environment activation fails or commands not found?**

The virtual environment activation commands on macOS are different from Windows - please distinguish:

- **Correct activation command** (in the project root directory):
  ```bash
  source .venv/bin/activate
  ```
  After activation, the terminal prompt will display `(.venv)` or a similar identifier.

- **Don't use Windows commands**: Commands like `.\venv\Scripts\activate` are invalid on macOS.

- **When using uv**: If using `uv` to manage the virtual environment, you can directly use `uv run python bot.py` without manual activation.

---

## 5. Other Common Issues

**Q: What to do if Napcat won't connect?**

1. Confirm that Napcat has started normally and that you've created a **WebSocket client** (reverse proxy) in Napcat.
2. Check if the `host` and `port` in `[Napcat_Server]` in `MaiBot-Napcat-Adapter/config.toml` match the Napcat settings.
3. Ensure the `PORT` in MaiBot's `.env` matches the `port` in `[MaiBot_Server]`.
4. Try temporarily disabling the macOS firewall for testing.

---

If the above content doesn't solve your problem, please refer to the [General FAQ](../faq/index.md) or visit the [MaiBot Community](/en/manual/other/group) for help.

(End of file - total 103 lines)