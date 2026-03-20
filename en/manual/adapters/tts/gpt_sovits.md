# Voice Module - GPT_Sovits Configuration Guide ✨

::: tip Bailing's Important Notice
The API interfaced in this section is GPT_Sovits with the entry point as api_v2.py API interface.

Now GPT_Sovits has been updated with an inference specialization package, abbreviated as GSVI, with the entry point as GSVI.py.

**Please note that the GSVI interface cannot be used for this module, as its API interface is incompatible with this module's implementation**
:::

Meow ha~! Welcome to this GPT_Sovits voice module guide! I'm Mo Bailing, a cat mage who loves researching magic.

Below is the configuration instructions for this module: This module is used to connect to GPT_Sovits TTS. The sample configuration file is in the `template` folder~

Before starting, everyone needs to copy the template `template_configs/gpt_sovits.toml` to `configs/gpt_sovits.toml`, then adjust the configuration file according to your preferences~

```toml
[pipeline]
# pipeline preset configuration
default_preset = "default"  # Default preset name to use

# pipeline platform preset mapping
[pipeline.platform_presets]
qq = "default"       # QQ platform uses default preset

[tts]
# GPT-SoVITS API configuration
host = "127.0.0.1"
port = 9880

# Voice synthesis basic configuration
media_type = "wav"        # Audio format: wav

# GPT-SoVITS model parameters
top_k = 12                # top k sampling
top_p = 1.0              # top p sampling
temperature = 1.0        # Temperature coefficient
batch_size = 1           # Batch size
batch_threshold = 0.75   # Batch threshold
text_split_method = "cut5" # Text segmentation method
repetition_penalty = 1.35 # Repetition penalty coefficient
sample_steps = 32        # VITS sampling steps
super_sampling = false    # Whether to enable super sampling

[tts.models]
# Custom character presets
[tts.models.presets]
# Preset character configuration example
[tts.models.presets.default]
name = "Default Character"
gpt_model = ""
sovits_model = ""
ref_audio_path = "path to reference audio" # Reference audio path
aux_ref_audio_paths = []                   # Auxiliary reference audio path list
prompt_text = "this is a ref audio"        # Prompt text
text_language = "auto"                     # Text language
prompt_language = "zh"                     # Prompt language
speed_factor = 1.0                         # Speech speed control

[tts.models.presets.custom1]
name = "kanami"
gpt_model = "GPT_SoVITS/pretrained_models/XiangNaimei-e5.ckpt"
sovits_model = "GPT_SoVITS/pretrained_models/XiangNaimei_e4_s248.pth"
ref_audio_path = "GPT_SoVITS/pretrained_models/voices/平淡.WAV"
aux_ref_audio_paths = []                                              # Auxiliary reference audio path list
prompt_text = "嗨！我每天都会像这样做发声训练的。"                       # Prompt text
text_language = "zh"                                                  # Text language
prompt_language = "zh"                                                # Prompt language
speed_factor = 1.0                                                    # Speech speed control
```

### pipeline Configuration

This part specifies the presets used by platforms.

- `default_preset`: Default preset name to use.
#### pipeline.platform_presets Presets for Different Platforms
- `qq`: Preset name used by QQ platform~

::: tip Bailing's Tip!
The key values here need to match the names after `.` in `[tts.models.presets]`.

For example, if you want to use the `custom1` preset on the QQ platform, you need to configure it as `custom1` here, and also have the `[tts.models.presets.custom1]` configuration in `[tts.models.presets]`~
:::

::: tip Key Name Explanation~
The key names here are platform names, which are also adapter identifier names.

For example, MaiBot Napcat Adapter uses `qq` as the default identifier, so here it should be set as `qq=your preset`
:::

### tts Configuration 🎙️
Next, this part is used to configure the GPT-SoVITS API interface.

- `host`: GPT-SoVITS API host address, usually `127.0.0.1`, i.e., local address.
- `port`: GPT-SoVITS API port, default is `9880`.

- `media_type`: Audio format, only supports `wav` format. Bailing recommends **not changing this**.
::: details Advanced Configuration
If you're not very familiar with GPT_Sovits yet, you can leave these unchanged for now.

- `top_k`: top k sampling, default is `12`.
- `top_p`: top p sampling, default is `1.0`.
- `temperature`: Temperature coefficient, default is `1.0`.
- `batch_size`: Batch size, default is `1`.
- `batch_threshold`: Batch threshold, default is `0.75`.
- `text_split_method`: Text segmentation method, default is `cut5`.
- `repetition_penalty`: Repetition penalty coefficient, default is `1.35`.
- `sample_steps`: VITS sampling steps, default is `32`.
- `super_sampling`: Whether to enable super sampling, default is `false`.
:::
### tts.models Configuration 📂

This part is where you configure your model files!

#### tts.models.presets Exclusive Character Configuration
This part is where you configure preset characters.

As Mo Bailing reminded everyone [earlier](#pipeline-configuration), the preset names must exactly match the names after `.` in `[pipeline.platform_presets]`!

Let's take `default` as an example:

You need to add a configuration like `[tts.models.presets.default]` **(if it already exists, no need to add it again)**. Then configure the items like this:

- `name`: Character's name, fill it however you like~
- `gpt_model`: Exclusive GPT model path for this character. Please replace it with your own model before use and check the path.
- `sovits_model`: Exclusive SoVITS model path for this character. Similarly, replace it with your own model and check the path.
- `ref_audio_path`: Reference audio path. Please be sure to replace it with your own reference audio and ensure it exists with the correct path.
- `prompt_text`: Prompt text. The text needs to be the same as the content in the reference audio, and the language type needs to correspond to the `prompt_language` below.
- `text_language`: The language of the text you want to synthesize, default is automatic detection (`auto`).
- `prompt_language`: Language of the prompt text, default is Chinese (`zh`).
- `speed_factor`: Speech speed control, default is `1.0`.
::: tip
For `text_language` and `prompt_language`, you can choose from these:
- `zh`: Chinese-English mixed
- `ja`: Japanese-English mixed
- `yue`: Cantonese-English mixed
- `ko`: Korean-English mixed
- `auto`: Multilingual mixed
- `auto_yue`: Multilingual mixed (including Cantonese)
- `all_zh`: Pure Chinese
- `en`: Pure English
- `all_ja`: Pure Japanese
- `all_yue`: Pure Cantonese
- `all_ko`: Pure Korean

This content is copied from the [GPT_Sovits code section](https://github.com/RVC-Boss/GPT-SoVITS/blob/main/GPT_SoVITS/inference_webui.py#L153). If there are issues, please refer to the GPT_Sovits code.
:::