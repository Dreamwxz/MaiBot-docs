# 📄 MaiBot Backup and Recovery

> Don't let your MaiMai become an "amnesia patient" - backup timely, let it always remember you!

MaiBot folder structure:
```
MaiBot/
├── data/
├── config/
│   ├── bot_config.toml
│   └── model_config.toml
├── plugins/
│   └── ...
├── .env
└── ...
```
Therefore, if you want to backup MaiMai, need to backup following content:

### `data` folder
- Used to store emoji packs, knowledge base, memories, etc.
### `config` folder
- Used to store configuration files.
- Used to store called large model information, AI cloud service provider settings, etc.
### `plugins` folder
- Used to store called large model information, AI cloud service provider settings, etc.
### `.env` file
- Stores `maim_message` and `webui` ports and addresses.

::: details If using MaiBot version lower than 0.7.0, then click here
### MongoDB Database Backup and Recovery

MongoDB officially provides two main backup tools: **mongodump/mongorestore** and **file system snapshots**

---

Official backup tools can be downloaded here: [database-tools](https://www.mongodb.com/try/download/database-tools), extract after downloading.

Put extracted files somewhere you like, then copy bin folder complete path.

Then add to system path variable.

### 1. Using `mongodump` to backup data
`mongodump` is MongoDB's officially provided command line backup tool, can export data as BSON/JSON format.
```bash
# No password
mongodump --uri "mongodb://localhost:27017" --out ./backup
# With password
mongodump --uri "mongodb://user:passwd@localhost:27017/" --out ./backup
```
Here `mongodb://localhost:27017` refers to your database address, can copy from compass.

After execution, will create backup folder where you opened console, your data here.

### 2. Using `mongorestore` to restore data
After backup can use `mongorestore` to restore data:
You need to open console in same directory as previous step's backup file and input this command.
```bash
mongorestore --uri "mongodb://localhost:27017/" --dir ./backup
```

:::