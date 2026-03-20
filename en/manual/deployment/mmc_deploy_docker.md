# 🐳 Docker Deployment

## 📋 Environment Requirements

- ✅ Docker environment installed
- ⚙️ Minimum system configuration: 2-core CPU / 2GB RAM / 5GB disk space
- 🐧 Tutorial test environment: Ubuntu Server 24.04 LTS

---

<!-- ::: warning
**When upgrading to 0.6.2, due to adapter changes, you need to re-pull docker-compose.yml and adapter-related configurations (adapter configuration method see [Modify Related Configuration](#⚙%EF%B8%8F-二、麦麦环境配置)),  
The new version adapter requires changing napcat to client ([Configuration Example](#_5-2-⚙%EF%B8%8F-napcat配置入口))**
::: -->

## 🛠️ 1. Prepare MaiBot Deployment Environment

### 1.1 📂 Create Project Directory

```bash
mkdir -p maim-bot/docker-config/{mmc,adapters} && cd maim-bot
```

### 1.2 📥 Get Docker Compose File

```bash
wget https://raw.githubusercontent.com/Mai-with-u/MaiBot/main/docker-compose.yml
```

> 🔄 **Alternative Download Method**  
> If GitHub direct connection is unstable, use a mirror source:
>
> ```bash
> wget https://fastly.jsdelivr.net/gh/Mai-with-u/MaiBot@main/docker-compose.yml
> ```

> For local build images, jump to [Local Build Process](#本地构建流程)

---

## ⚙️ 2. MaiBot Environment Configuration

### 2.1 📝 Prepare Configuration File Templates

```bash
# Get core component configuration template
wget https://raw.githubusercontent.com/MaiM-with-u/MaiBot/main/template/template.env \
     -O docker-config/mmc/.env
# If GitHub direct connection is unstable, use mirror source: https://fastly.jsdelivr.net/gh/Mai-with-u/MaiBot@main/template/template.env
```

Get `adapter`'s `config.toml`

```bash
wget https://github.com/MaiM-with-u/MaiBot-Napcat-Adapter/raw/refs/heads/main/template/template_config.toml \
     -O docker-config/adapters/config.toml
# If GitHub direct connection is unstable, use mirror source: https://fastly.jsdelivr.net/gh/Mai-with-u/MaiBot-Napcat-Adapter@main/template/template_config.toml
```

> If service names in configuration files are unavailable, replace with container names
>
> - `MaiBot_Server` configuration can be replaced with `maim-bot-core`
> - `napcat` ws client can be replaced with `ws://maim-bot-adapters:8095`

[//]: # (> - `ONEBOT_WS_URLS` configuration can be replaced with `ws://maim-bot-napcat:8095`)
[//]: # (> - `nonebot-qq` configuration can be replaced with `http://maim-bot-adapters:18002/api/message`)

### 2.2 **Reserved File**

- This file is MaiBot runtime statistics report.

`MacOS/Linux`

```bash
mkdir -p data/MaiMBot && touch ./data/MaiMBot/maibot_statistics.html
```

`Windows`

```cmd
mkdir data\MaiMBot && type nul > .\data\MaiMBot\maibot_statistics.html
```

### 2.3 ✏️ Modify Related Configuration

- Modify MaiBot main program environment variable file

```bash
vim docker-config/mmc/.env
```

Need to modify the following key parameters:

```ini
# Network listening configuration
HOST=0.0.0.0
```

- Modify adapter configuration file

```bash
vim docker-config/adapters/config.toml
```

Modify `Napcat_Server` host to `0.0.0.0`  
Modify `MaiBot_Server` host to `core`  

```toml
[napcat_server] # Napcat connection ws service settings
host = "0.0.0.0"      # Napcat set host address
port = 8095             # Napcat set port 
heartbeat_interval = 30 # Same heartbeat as Napcat settings (in seconds)

[maibot_server] # Connection to MaiBot ws service settings
host = "core" # MaiBot host address set in .env file, i.e., HOST field
port = 8000        # MaiBot port set in .env file, i.e., PORT field
```

### 2.3 📜 Uncomment docker-compose.yml eula

```bash
vim docker-compose.yml
# Uncomment the following two lines (lines 30-31)
- EULA_AGREE=bda99dca873f5d8044e9987eac417e01  # Agree to EULA
- PRIVACY_AGREE=42dddb3cbe2b784b45a2781407b298a1 # Agree to EULA
```

### 2.4 Database Management Tool

- **If not using sqlite-web, uncomment chat2db and comment out sqlite-web section (or delete)**

```bash
  #sqlite-web:
  #  # Note: coleifer/sqlite-web image does not support arm64
  #  image: coleifer/sqlite-web
  #  container_name: sqlite-web
  #  restart: always
  #  ports:
  #    - "8120:8080"
  #  volumes:
  #    - ./data/MaiMBot:/data/MaiMBot
  #  environment:
  #    - SQLITE_DATABASE=MaiMBot/MaiBot.db  # Your database file
  #  networks:
  #    - maim_bot

  # chat2db占用相对较高但是功能强大
  # 内存占用约600m，内存充足推荐选此
   chat2db:
     image: chat2db/chat2db:latest
     container_name: maim-bot-chat2db
     restart: always
     ports:
       - "10824:10824"
     volumes:
       - ./data/MaiMBot:/data/MaiMBot
     networks:
       - maim_bot
```

### 2.5 Directory Structure

- After current configuration, directory structure should be as follows

```text
.
├── docker-compose.yml
├── data
    ├── MaiMbot
        └── maibot_statitics.html
└── docker-config
    ├── adapters
    │   └── config.toml
    └── mmc
        └── .env
```

---

## 🚀 3. Initialize Container Environment

### 3.1 ⚡ First Start Containers to Generate Remaining Configuration Files

- Will automatically generate MaiBot main program basic configuration and model configuration files

```bash
docker compose up -d && sleep 15 && docker compose down
```

### 3.2 🔧 Adjust MaiBot Configuration

> Please modify according to your needs
> Configuration file related instructions see [Configuration Guide](/en/manual/configuration/index.md)

- `bot_config.toml`: Basic configuration file, contains MaiBot name, QQ number, persona, etc.

```bash
vim docker-config/mmc/bot_config.toml
```

- `model_config.toml`: Model configuration file, contains service provider, which model to use, etc.

```bash
vim docker-config/mmc/model_config.toml
```

---

## 🎉 4. Start MaiBot

::: tip

Note: The following operations are all executed in the `maim-bot` directory.

:::

### 4.1 🏁 Start All Components

```bash
docker compose up -d
```

### 4.2 🔍 Verify Service Status

```bash
docker compose ps
```

- Normally should show 4 containers (maim-bot-core, maim-bot-adapters, maim-bot-napcat, sqlite-web[or chat2db]) with status `running`

```bash
NAME                IMAGE                           COMMAND                  SERVICE     CREATED          STATUS          PORTS
maim-bot-adapters   unclas/maimbot-adapter:latest   "python main.py"         adapters    34 minutes ago   Up 34 minutes   8095/tcp
maim-bot-core       sengokucola/maimbot:main        "python bot.py"          core        34 minutes ago   Up 34 minutes   8000/tcp
maim-bot-napcat     mlikiowa/napcat-docker:latest   "bash entrypoint.sh"     napcat      34 minutes ago   Up 34 minutes   0.0.0.0:6099->6099/tcp, [::]:6099->6099/tcp
sqlite-web          coleifer/sqlite-web             "/bin/ash -c 'sqlite…"   sqlite-web  34 minutes ago   Up 34 minutes   0.0.0.0:8120->8080/tcp   
```

### 4.3 📜 Real-time Log Monitoring

- Real-time view of all container logs

```bash
docker compose logs -f
```

- Real-time view of specific container logs

```bash
# For example, only want to see main program logs
docker compose logs -f core
```

---

## 🔧 5. Subsequent Management Operations

### 5.1 🎛️ Service Start/Stop Commands

| Operation | Command |
|------|------|
| ▶️ Start Service | `docker compose up -d` |
| ⏹️ Stop Service | `docker compose down` |
| 🔄 Force Rebuild | `docker compose up -d --force-recreate` |

### 5.2 ⚙️ Napcat Configuration Entry

Access `http://<server IP>:6099` to complete Napcat configuration
> Network configuration uses `websocket client`, `url` is `ws://adapters:8095`
> Example:
>
> ![](/images/napcat_network_webcoket.png)
>
> ![Napcat Configuration](/images/mmc-napcat-client.png)

### 5.3 chat2db Parameter Settings (if enabled)

Access `http://<server IP>:10824` to complete chat2db configuration
> Database select `SQLite`, `file` is `/data/MaiMBot/MaiBot.db`
> Example:
> ![chat2db Configuration](/images/mmc-chat2db.png)

---

## 6. ❓ Common Issue Troubleshooting

1. ❌ **Container Startup Failure**:
   - 🔍 Check port conflicts (18002/8000/8095/6099/8120/10824)
      > If not mapped, ignore
   - 🔑 Verify API key validity in `.env` file

2. 🔄 **Configuration File Updates**:
   After modifying configuration, execute:

   ```bash
   docker compose down
   docker compose up -d
   ```

   or:

   ```bash
   docker compose restart
   ```

3. 📊 **Resource Monitoring**:

   ```bash
   docker stats
   ```

> 💡 Tip: When encountering issues, check logs for more information:
>
> ```bash
> docker compose logs -f
> ```

---

## 7. Local Build Process

### Prepare Necessary Files

1. Clone [MaiBot](https://github.com/MaiM-with-u/MaiBot) locally via git clone

2. Clone [MaiMBot-LPMM Package](https://github.com/MaiM-with-u/MaiMBot-LPMM) locally via git clone

3. Clone [MaiBot-Napcat-Adapter](https://github.com/MaiM-with-u/MaiBot-Napcat-Adapter) locally via git clone

```shell
git clone https://github.com/MaiM-with-u/MaiBot.git
git clone https://github.com/MaiM-with-u/MaiMBot-LPMM.git
git clone https://github.com/MaiM-with-u/MaiBot-Napcat-Adapter.git
```

> To switch branches, add `-b <branch name>` after the link  

Copy `MaiMBot-LPMM` to `MaiBot` directory

```bash
cp -r MaiMBot-LPMM MaiBot/MaiMBot-LPMM 
```

Pull required images

```bash
sudo docker pull python:3.13.5-slim-bookworm
sudo docker pull python:3.13.5-slim
sudo docker pull ghcr.io/astral-sh/uv:latest
```

Run build

```bash
cd MaiBot
sudo docker build -t mmc:local .
cd ../MaiBot-Napcat-Adapter
sudo docker build -t adapters:local .
```

To use local build, replace `docker-compose.yml` `image` accordingly

---