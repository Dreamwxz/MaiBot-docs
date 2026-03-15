# 📄 MaiMai Knowledge Base (New LPMM) Usage Instructions

## Notes

::: danger Reminder
Please read following notes carefully to avoid unnecessary trouble and expenses
:::
:::info
1. **Before knowledge extraction, ensure your text well segmented, no strange characters, otherwise extraction results may be very unsatisfactory or even cause extraction failure**

2. **During knowledge extraction, need to input large amounts of text to large models, longer text consumes more, higher costs generated**

    Painful experience from a group friend: 6 million words of full storyline, extraction using deepseek v3 0324, consumed about 40 yuan, hope everyone takes this as a warning.  
    If requirements are not high, it's recommended to use 32B~72B smaller large models for extraction, but not recommended to use models below 32B, otherwise extraction results will be very poor and extremely likely to fail.
    (If still want to choose deepseek v3 for extraction, author here recommends [DeepSeek](https://www.deepseek.com/), [context hard disk cache](https://api-docs.deepseek.com/zh-cn/guides/kv_cache) will help save money)

3. **During knowledge import, will send a large number of requests, may hit request speed limits, please note chosen model (can use local embedding model)**

    (Same example above: under local model, within 70 minutes we sent about 80,000 requests, under network permission, speed will be faster)

4. **During knowledge import, will consume a large amount of system resources, recommended to run on better configured computer**

    (Same example above: during import 10700K almost fully loaded, peak memory usage about 3G)

5. **This knowledge base not compatible with old version**, if need to migrate content from old version database to new version, please re-import
:::

## Install quick-algo
For Windows_x86_64 platform users, **please use pip for direct installation**. (Already included in MaiBot's requirements.txt, no need manual)  

For windows_x86_64 platform one-click package users, lpmm knowledge base required dependencies have already been全部 completed by **![Update all modules.bat**, no need to install dependencies yourself.

For Linux platform users, need to download gcc/g++ compiler, jump link: [Linux Environment Usage Method](#linux)  

For MacOS platform users please directly refer to [this link](/en/manual/configuration/lpmm/lpmm_knowledge_template)'s MacOS manual compilation section

For Docker users after configuration is complete can directly run script (LPMM pre-compiled in image), jump link: [Docker's LPMM Usage Method](#docker的lpmm食用方式)
::: tip
If after multiple attempts at installation, **you find there really isn't a version for your platform**, please refer to [this link](/en/manual/configuration/lpmm/lpmm_knowledge_template) content for manual compilation.
:::

### Windows
Directly run following command:
```bash
pip install quick_algo
```

### Linux
1. First install gcc/g++ compiler

Debian-based systems (like Ubuntu, Linux Mint, etc.)

```bash
# Update software package index
sudo apt update
# Run following command to install `gcc` and `g++`:
sudo apt install build-essential
```

Red Hat-based systems (like Fedora, CentOS, RHEL, etc.)

```bash
# Update software package index
sudo dnf check-update
# Install `gcc` and `g++`
sudo dnf install gcc gcc-c++
# If using `yum`, replace `dnf` with `yum`
```

2. Verify installation

After installation is complete, can verify if `gcc` and `g++` are installed successfully through following commands. If installation is successful, will display `gcc` and `g++` version information.

```bash
gcc --version
g++ --version
```

3. Install LPMM essential library `quick_algo`

Activate your used virtual environment, run:

```bash
source ./venv/bin/activate #Activate MaiBot virtual environment
pip install quick-algo
```

:::tip
If through above steps still cannot install `quick-algo`, please go to [LPMM Manual Compilation Instructions](/en/manual/configuration/lpmm/lpmm_knowledge_template) for manual compilation `quick-algo`
:::

## Configure LPMM

Currently lpmm model configuration has been merged into `config/model_config.toml`

```toml
#Embedding models
[model_task_config.embedding]
model_list = ["bge-m3"]

#------------LPMM Knowledge Base Models------------

[model_task_config.lpmm_entity_extract] # Entity extraction model
model_list = ["siliconflow-deepseek-v3"]
temperature = 0.2
max_tokens = 800

[model_task_config.lpmm_rdf_build] # RDF construction model
model_list = ["siliconflow-deepseek-v3"]
temperature = 0.2
max_tokens = 800

[model_task_config.lpmm_qa] # Q&A model
model_list = ["qwen3-30b"]
temperature = 0.7
max_tokens = 800
```

Default models are usually sufficient. If the API is slow or laggy, please change the model yourself. Configuration method please go to [Model Configuration Settings Tutorial](/en/manual/configuration/configuration_model_standard)

## MaiMai Learning Knowledge

::: tip
1. Knowledge base used text must be stored in txt format

2. Entity extraction, RDF extraction models not recommended to use models below 32B, otherwise extraction效果 very poor and extremely likely to fail
:::

### Segmentation

First, ensure your text well segmented.

Segmentation method: According to the same theme, set one large paragraph, with an empty line between each paragraph. Adjacent natural paragraphs will be considered one large paragraph. For example below:

```
精神状态良好：形容自己精神状态良好的反讽，实际精神状态非常不稳定。

躺平：是一个网络热梗。指无论对方做出什么反应，内心都毫无波澜，对此不会有任何反应或者反抗，表示顺从心理，也表示不再热血沸腾的状态。

内卷：指一类文化模式达到了某种最终的形态以后，既没有办法稳定下来，也没有办法转变为新的形态，而只能不断地在内部变得更加复杂的现象称为"内卷化"。
现在流行成了一个字，"卷"，很多高校学生用其来指非理性的内部竞争或"被自愿"竞争。
现应用在各行各业以及各个领域中的竞争，指同行间竞相付出更多努力以争夺有限资源，从而导致个体"收益努力比"下降的现象。

凡尔赛：源自于微博上的晒富行为，形容那些故意炫耀自己、表现自己富有的人或行为，讽刺意味十足。

社死：形容某个人在公共场合出现时，因某种原因感到极其尴尬和不好意思，表现出了人们在社会交往中的种种尴尬情景。
```

::: warning Warning
**Input data segmentation quality will directly affect knowledge graph construction效果, please ensure dialogue paragraph semantics complete!**
:::

### Import

New version LPMM has been improved:

First, put original files into `data/lpmm_raw_data` (if not exist please create it yourself)
Activate virtual environment (**already activated please skip**):

```bash
#Windows下:
.\MaiBot\venv\Scripts\activate
#Linux environment:
source ./venv/bin/activate
```

Run `info_extraction.py` to perform text segmentation & entity extraction.

```bash
python ./scripts/info_extraction.py
```

After extraction is complete, you'll see `月-日-时-分-openie.json` file in `data/openie` directory, which contains the extracted OpenIE data.

Similarly, you can also put others shared openie.json files into `data/openie` directory, directly use.

This file naming no longer needs to be strictly standardized like before, you can use your preferred naming style. But need to ensure file suffix is `.json`.

Finally, run `import_openie.py` to perform final knowledge graph import.

```bash
python ./scripts/import_openie.py
```

After import完成, if can see `rag` and `embedding` two folders under data folder,说明导入成功.

## MaiMai LPMM Acceleration

:::tip
GPU acceleration仅适用于系统为Linux, NVIDIA RTX20系及以上显卡及专业卡
:::

Installation method:

```bash
# Uninstall cpu version
pip uninstall faiss-cpu
# Install cuda11 version
pip install faiss-gpu-cu11
# Install cuda12 version
pip install faiss-gpu-cu12
```

When using Conda, GPU acceleration仅适用于Linux:
详见[faiss official documentation](https://github.com/facebookresearch/faiss/blob/main/INSTALL.md)
Usage method:

```bash
# CPU-only version
$ conda install -c pytorch faiss-cpu=1.11.0

# GPU(+CPU) version
$ conda install -c pytorch -c nvidia faiss-gpu=1.11.0

# GPU(+CPU) version with NVIDIA cuVS
$ conda install -c pytorch -c nvidia -c rapidsai -c conda-forge libnvjitlink faiss-gpu-cuvs=1.11.0

# GPU(+CPU) version using AMD ROCm not yet available
```

## Docker's LPMM Usage Method

Currently lpmm model configuration has been merged into `config/model_config.toml`

```toml
#Embedding models
[model_task_config.embedding]
model_list = ["bge-m3"]

#------------LPMM Knowledge Base Models------------

[model_task_config.lpmm_entity_extract] # Entity extraction model
model_list = ["siliconflow-deepseek-v3"]
temperature = 0.2
max_tokens = 800

[model_task_config.lpmm_rdf_build] # RDF construction model
model_list = ["siliconflow-deepseek-v3"]
temperature = 0.2
max_tokens = 800

[model_task_config.lpmm_qa] # Q&A model
model_list = ["qwen3-30b"]
temperature = 0.7
max_tokens = 800
```

Default models are usually sufficient. If the API is slow or laggy, please change the model yourself. Configuration method please go to [Model Configuration Settings Tutorial](/en/manual/configuration/configuration_model_standard)

:::tip
Entity extraction, RDF extraction models not recommended to use models below 32B, otherwise extraction效果 very poor and extremely likely to fail
:::

Then create folder

```bash
mkdir -p data/MaiMBot/lpmm_raw_data
```

Put your original knowledge into lpmm_raw_data
> Format requirements see [MaiMai Learning Knowledge](#麦麦学习知识)

Then run command:

```bash
docker run -it -v ./data/MaiMBot:/MaiMBot/data -v ./docker-config/mmc:/MaiMBot/config -v ./docker-config/mmc/.env:/MaiMBot/.env --network maim-bot_maim_bot  --entrypoint bash sengokucola/maibot:latest "scripts/run_lpmm.sh"
```

:::tip
Note此处 `network` configuration, should be your core's docker network, can use `docker network ls`查找你的网络名称, name后面有 `maim_bot`字样的网络即为你的core所在网络  
Example:

```bash
$ docker network ls
NETWORK ID     NAME              DRIVER    SCOPE
8f5c55aec8a1   bridge            bridge    local
9e3966fa583e   host              host      local
ec92e2f103a4   maibot_maim_bot   bridge    local  # This is MaiMai's docker network
cb43a6a60ed3   none              null      local
```
:::

After running is complete and `✅ All scripts completed successfully` appears, then your knowledge base import is successful

## About "MaiMai Notebook"

*MaiMai Notebook* is a MaiMai related content sharing site built by **non-MaiMai official**. Plans to support sharing knowledge bases (lpmm) and persona cards (configuration files).

Due to original site owner STMfan's personal reasons, MaiMai Notebook project plans to transfer. Interested parties welcome to join group chat 1049746444 for detailed discussion.

::: warning Warning
**Due to "MaiMai Notebook"'s non-official nature, please don't report any "MaiMai Notebook" Bugs and problems to official channels!**