# 📱 Android Deployment Guide

### 1. Install ZeroTermux Environment

Go to [ZeroTermux-Github](https://github.com/hanxinhao000/ZeroTermux/releases/tag/release) to download and install the ZeroTermux installation package.

**Note: Installing other versions or choosing Termux may cause parts of this tutorial to be inapplicable. Not recommended.**

Enter the ZeroTermux software interface. When prompted to read the agreement completely, remember to scroll the text content to the bottom.

Double-tap the left edge of the screen (some ZT versions use volume up/down keys), swipe down and click "Switch Source", choose any, recommended to choose `Tsinghua Source`, wait for the script to complete.

> Unless otherwise specified, when `(Y/I/N/O/D/Z)[default=?]` or `[Y/N]` appears, simply press Enter to choose the default option. For users with some Linux foundation, use ZeroTermux to build a QQ robot based on MaiBot.

Before starting, **strongly recommend reading [MaiBot Documentation Center - Installation Guide](../installation.md) and [How to Ask Questions Efficiently](/en/manual/other/smart-question-guide)**, which is helpful for understanding the general process and asking questions.

::: info
This tutorial recommends using [uv](https://docs.astral.sh/uv/) as the Python package manager, which provides faster package installation speed and better dependency management experience. Of course, the traditional pip method is still available.
:::

::: warning
**This tutorial is for deployment to the QQ platform, deployment methods for other platforms may differ**
:::

## 📋 Environment Requirements
- 📱 Android 7.0+ system
- 💾 At least 2GB available storage space
- 🐍 Python >= 3.10
- 📦 uv >= 0.1.0 (recommended to use latest version)

## 1. Install ZeroTermux Environment

Go to [ZeroTermux-Github](https://github.com/hanxinhao000/ZeroTermux/releases/tag/release) to download and install the ZeroTermux installation package.

**Note: Installing other versions or choosing Termux may cause parts of this tutorial to be inapplicable. Document author does not recommend this.**

Enter the ZeroTermux software interface. When prompted to read the agreement completely, remember to scroll the text content to the bottom.

Double-tap the left edge of the screen (some ZT versions use volume up/down keys), swipe down and click "Switch Source", choose any, author recommends choosing `Tsinghua Source`, wait for the script to complete.

> Unless otherwise specified, when `(Y/I/N/O/D/Z)[default=?]` or `[Y/N]` appears, simply press Enter to choose the default option.

## 2. Install proot and Ubuntu

Run (paste line by line and press Enter) the following commands to install `proot`, `Ubuntu`:
```bash
pkg install proot-distro      # Install proot
proot-distro install ubuntu   # Install Ubuntu
proot-distro login ubuntu     # Login to Ubuntu
```

Then run the following commands to install necessary software:

```bash
apt update
apt install sudo vim git python3-dev python3.12-venv build-essential screen curl python3-pip
```

### User Account Configuration (Optional but Recommended)

::: details Create Non-root User (Recommended)
Directly using root user to operate all commands may have huge security risks (**especially for beginners!**), recommended to create a regular user account.

Replace `<username>` with your username, enter password twice then you can press Enter to leave all fields blank. **Password not displaying after input is normal.**
```bash
adduser <username>           # Create account
usermod -aG sudo <username>  # Add sudo permission
```

Then login to the newly created account:
```bash
su <username>                # Login to account
```
:::

## 3. Get Necessary Files

Create folder and pull source code from Github:

```bash
cd ~              # Enter home directory (~)
mkdir maimai      # Create maimai folder
cd maimai         # Enter maimai folder
git clone https://github.com/MaiM-with-u/MaiBot.git
git clone https://github.com/MaiM-with-u/MaiBot-Napcat-Adapter.git
```

## 4. Environment Configuration

### Confirm Python Version

Ensure Python version is 3.10 or higher:

```bash
python3 --version
```

### Install uv (Recommended)

Install uv package manager:
```bash
# Use pip to install uv
pip3 install uv --break-system-packages -i https://mirrors.huaweicloud.com/repository/pypi/simple/
grep -qF 'export PATH="$HOME/.local/bin:$PATH"' ~/.bashrc || echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

::: tip
When using uv, first run `uv venv` to create virtual environment, then use `uv pip install` to install dependencies, or directly use `uv run` command to automatically manage virtual environment.
:::

### Traditional Method Environment Configuration

If not using uv, can use traditional venv method:

```bash
python3 -m venv MaiBot/venv      # Create virtual environment    
source MaiBot/venv/bin/activate  # Activate environment
```

::: tip
When using traditional method, each time if `(venv)` is not displayed, need to reactivate environment
:::

## 5. Dependency Installation

### Use uv to Install Dependencies (Recommended)

1. Enter MaiBot folder, create virtual environment and install dependencies:
```bash
cd MaiBot
uv venv
uv pip install -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple --upgrade
```

::: tip
If you find `quick_algo` installation failure here, please refer to the `quick-algo` installation section in [LPMM Knowledge Base](/en/manual/configuration/lpmm/lpmm)
:::

2. Return to parent folder, enter MaiBot-Napcat-Adapter folder, install dependencies:
```bash
cd ..
cd MaiBot-Napcat-Adapter
uv venv
uv pip install -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple --upgrade
# Copy and rename configuration file
cp template/template_config.toml config.toml
```

### Use Traditional Method to Install Dependencies

```bash
cd MaiBot
pip3 install -r requirements.txt
```

::: tip
If you find `quick_algo` installation failure here, please refer to the `quick-algo` installation section in [LPMM Knowledge Base](/en/manual/configuration/lpmm/lpmm)
:::

Return to parent folder, then enter MaiBot-Napcat-Adapter folder, install dependencies:
```bash
cd ..
cd MaiBot-Napcat-Adapter
pip3 install -r requirements.txt
# Copy and rename configuration file
cp template/template_config.toml config.toml
```

Final folder structure should be similar to this:
```
maimai
├── MaiBot
│   ├── changelogs
│   ├── config
│   ├── data
│   ├── depends-data
│   ├── src
│   │   └── ...
│   └── template
└── MaiBot-Napcat-Adapter
    ├── config.toml
    └── template
```

## 6. NapCat Deployment

### Install NapCat

```bash
# Install NapCat
curl -o napcat.sh https://nclatest.znin.net/NapNeko/NapCat-Installer/main/script/install.sh
sudo bash napcat.sh --docker n --cli y

# Open NapCat
sudo napcat
```

Then use arrow keys and Enter to sequentially select
```
Configure Napcat
Configure Service
Enter QQ Number
Save
Configure Service
4  WebSocket Client
```
Name can be anything, Url change "8082" to "8095", keep others default

Then continue selecting
```
OK
enable (use space to select)
OK
Exit
Start Napcat
Start Account: xxxxxxxxx
```
Then screenshot QR code, send/cast to another device **（directly importing QR code from album tested cannot login）**, use mobile QQ logged into that QQ account to scan code. Then exit.

## 7. Configure MaiBot and Adapter

```bash
cd ../MaiBot
# Create folder
mkdir config
# Copy and rename configuration files
cp template/bot_config_template.toml config/bot_config.toml
cp template/model_config_template.toml config/model_config.toml
cp template/template.env .env
vim .env
# Modify .env, change port at beginning to 8000
```

**Then go to [MaiBot Documentation Center Configuration Guide](/en/manual/configuration/index.md) to complete configuration**
**Most problems occur at this step, if you don't understand something in the above document, welcome to communicate with community members**

Then configure MaiBot-Napcat-Adapter

Use vim to open config.toml in MaiBot-Napcat-Adapter folder, configure `Napcat_Server`, `MaiBot_Server` fields
```toml
[Napcat_Server] # Napcat connection ws service settings
host = "localhost" # Napcat set host address
port = 8095        # Napcat set port
heartbeat = 30     # Same heartbeat as Napcat settings (in seconds)

[MaiBot_Server] # Connection to MaiBot ws service settings
host = "localhost"   # MaiBot host address set in .env file, i.e., HOST field
port = 8000          # MaiBot port set in .env file, i.e., PORT field
```
`Napcat_Server` field `port`, should be same as Napcat set reverse proxy url (here is 8095)

`Napcat_Server` field `heartbeat`, should be same as Napcat set reverse proxy heartbeat interval (note, interval in Napcat is milliseconds, convert to seconds when filling, here is 30)

`MaiBot_Server` field `port`, should be same as MaiBot .env `PORT` (here is 8000)

Other fields refer to [MaiBot Documentation Center - Adapter Configuration](/en/manual/adapters/napcat.html)

**Remember to configure group or private chat whitelist here! Otherwise cannot reply to messages**

## 8. Start MaiBot

*Congratulations on completing most steps! Victory is in sight!*

### Use uv to Run (Recommended)

1. First start NapCat (if not already started)

2. Start MaiBot core:
```bash
screen -R mmc
# Window will clear, don't panic
cd ~/maimai/MaiBot
uv run python3 bot.py
# Press Ctrl+A then D to exit screen
```

3. Start MaiBot-Napcat-Adapter:
```bash
screen -R mmc-adapter
cd ~/maimai/MaiBot-Napcat-Adapter
uv run python3 main.py
# Press Ctrl+A then D to exit screen
```

### Use Traditional Method to Run

1. First start NapCat (if not already started)

2. Start MaiBot core:
```bash
screen -R mmc
# Window will clear, don't panic
cd ~/maimai/MaiBot
source venv/bin/activate  # Activate virtual environment
python3 bot.py
# Press Ctrl+A then D to exit screen
```

3. Start MaiBot-Napcat-Adapter:
```bash
screen -R mmc-adapter
cd ~/maimai/MaiBot-Napcat-Adapter
source ../MaiBot/venv/bin/activate  # Activate virtual environment
python3 main.py
# Press Ctrl+A then D to exit screen
```

### Use Community Management Script to Start
Author: kanfandelong

Script placement reference

```
../
├── MaiBot/
├── MaiBot-Napcat-Adapter/
├── maibot.sh
└── ...
```
#### Get Script and Run
```bash [apt]
# Install unbuffer and wget
sudo apt install expect wget
# Download management script
wget https://github.com/kanfandelong/maimai_install/raw/main/maibot.sh
# Run management script
bash maibot.sh
```

## 9. Command Quick Reference Table

### Basic Commands

| Command | Purpose |
|---|---|
| `cd xxx` | Enter xxx directory (if xxx is .. then return to parent directory) |
| `vim xxx` | Use vim editor to edit file |
| `ESC` + `:wq` | Exit vim and save | 

### uv Related Commands (Recommended)

| Command | Purpose |
|---|---|
| `uv venv` | Create Python virtual environment |
| `uv pip install -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple --upgrade` | Install dependency packages |
| `uv run python3 bot.py` | Run MaiBot core |
| `uv run python3 main.py` | Run Napcat adapter |

### Screen Session Management

| Command | Purpose |
|---|---|
| `screen -r xxx` / `screen -R xxx` | Enter xxx session (latter creates one when no such session exists) |
| `Ctrl` + `A` + `D` | Exit session |
| `screen -ls` | List all sessions |

### Traditional Method Commands

| Command | Purpose |
|---|---|
| `source ~/maimai/MaiBot/venv/bin/activate` | Activate Python virtual environment |
| `python3 bot.py` | Start MaiBot core |
| `python3 main.py` | Start Napcat adapter |

## 10. Regular Startup

### Use uv Method

```bash
# Enter Ubuntu environment
proot-distro login ubuntu
su <username>  # If using root user completed entire process skip this step

# Start adapter
screen -r mmc-adapter
cd ~/maimai/MaiBot-Napcat-Adapter
uv run python3 main.py
# Press Ctrl+A then D to exit

# Start MaiBot core
screen -r mmc
cd ~/maimai/MaiBot
uv run python3 bot.py
# Press Ctrl+A then D to exit
```

### Use Traditional Method

```bash
# Enter Ubuntu environment
proot-distro login ubuntu
su <username>  # If using root user completed entire process skip this step

# Start adapter
screen -r mmc-adapter
cd ~/maimai/MaiBot-Napcat-Adapter
source ../MaiBot/venv/bin/activate
python3 main.py
# Press Ctrl+A then D to exit

# Start MaiBot core
screen -r mmc
cd ~/maimai/MaiBot
source venv/bin/activate
python3 bot.py
# Press Ctrl+A then D to exit
```

---

**🎉 Congratulations on completing MaiBot deployment on Android!**