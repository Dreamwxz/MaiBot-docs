# MaiBot Linux One-Click Script Deployment

Author: Astriora

## Use Script to Deploy MaiBot

## Install wget

::: code-group

```bash [apt]
sudo apt install wget
```

```bash [yum]
sudo yum install wget
```

```bash [dnf]
sudo dnf install wget
```

```bash [pacman]
sudo pacman -S wget
```

```bash [zypper]
sudo zypper install wget
```

:::

## Download Script & Deploy

```bash


wget -O maibot-install.sh https://raw.githubusercontent.com/Astriora/Antlia/refs/heads/main/Script/MaiBot/MaiBot-install.sh &&
bash maibot-install.sh


```

## Start
```bash
source ~/.bashrc # First time need to update shell
```
```bash
maibot
```

## Error Solutions

>If appears `[WARN] uv sync failed, retry 2/3
  × Failed to build astrbot @ file:///root/
  ├─▶ Failed to install requirements from build-system.requires
  ├─▶ Failed to install build dependencies
  ├─▶ Failed to install: trove_classifiers-2025.9.11.17-py3-none-any.whl
  │   (trove-classifiers==2025.9.11.17)
  ╰─▶ failed to hardlink file from
      /root/.cache/uv/archive-v0/10gPuxc61Audvy1Eg6SFz/trove_classifiers/.l2s.__init__.py0001
      to
      /root/.cache/uv/builds-v0/.tmp2lFVJx/lib/python3.10/site-packages/trove_classifiers/.l2s.__init__.py0001:
      Operation not permitted (os error 1)

Can first run following commands, then restart

>```bash
>echo 'export UV_LINK_MODE=copy' >> ~/.bashrc 
>```
>```bash
>source ~/.bashrc
>```