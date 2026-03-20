# Voice Module - Doubao TTS Configuration Guide ✨

This module interfaces with the TTS voice synthesis service provided by Doubao TTS!

First, we need to copy a configuration file template from `template_configs/Doubao_tts_template.toml` to `configs/Doubao_tts.toml`.

```toml
# Note: All items marked with [*] should be configured and interpreted by referring to the official documentation https://www.volcengine.com/docs/6561/1257584
[app] # Doubao TTS app configuration
base_url = "https://openspeech.bytedance.com/api/v1/tts" # API address
# The following parts are from official documentation, please refer to Q1 section of https://www.volcengine.com/docs/6561/196768
appid = ""
token = ""
cluster = ""
[audio] # Audio configuration
voice_type = "" # [*]
emotion = "" # Currently ineffective, please keep empty [*]
enable_emotion = false # Currently ineffective, please keep as false [*]
emotion_scale = 0 # Currently ineffective, please keep as 0 [*]
speed_ratio = 1.0 # Speech speed, range 0.8-2.0, default 1.0
explicit_language = "" # Explicit language [*]
context_language = "" # Reference language [*]
loudness_ratio = 1.0 # Volume, range 0.5-2.0, default 1.0 [*]
[request]
silence_duration = 0 # End-of-sentence silence duration, unit ms, default 0, maximum 300000 [*]
```

## Configuration Item Details

All configuration content here is written based on the [official documentation](https://www.volcengine.com/docs/6561/1257584), so if you have any questions, remember to check the official documentation~

### `app`
For how to obtain this content, please refer to the Q1 section of [another official document](https://www.volcengine.com/docs/6561/196768).
- `base_url`: This is the API interface address for Doubao TTS service. In most cases, keep it as default.
- `appid`: Application ID (AppID) applied for.
- `token`: Access token.
- `cluster`: Cluster information

### `audio` Audio Parameters

- `voice_type`: Voice setting! Doubao TTS service provides many different voices. You can check the [voice list provided by Doubao official](https://www.volcengine.com/docs/6561/1257544), find the code for your favorite voice from there, and fill in the code here.
- `emotion`: Currently not used, must be left empty!
- `enable_emotion`: Currently not used, must be left empty!
- `emotion_scale`: Currently not used, must be left empty!
- `speed_ratio`: Speech speed setting, range is `0.8` to `2.0`, default value is `1.0`.
- `explicit_language`: Explicit language. This part tells the large model what language to read.
::: tip Bailing's Tip (2025/05/22 version)
According to the [official documentation](https://www.volcengine.com/docs/6561/1257584), the fillable methods are as follows:
- Leave empty: Normal Chinese-English mixed
- `crosslingual`: Multilingual mixed (including `zh`/`en`/`ja`/`es-ms`/`id`/`pt-br`)
- `zh`: Mainly Chinese, supports Chinese-English mixed
- `en`: English only
- `ja`: Japanese only
- `es-mx`: Mexican Spanish only
- `id`: Indonesian only
- `pt-br`: Brazilian Portuguese only
:::
- `context_language`: Reference language provided to the model.
::: tip Bailing's Tip (2025/05/22 version)
According to the [official documentation](https://www.volcengine.com/docs/6561/1257584), the fillable methods are now as follows:
- Leave empty: Western European languages use English.
- `id`: Western European languages use Indonesian.
- `es`: Western European languages use Mexican Spanish.
- `pt`: Western European languages use Brazilian Portuguese.
:::
- `loudness_ratio`: Volume adjustment. The volume here can be selected as a decimal between 0.5 and 2.0, default is 1.0.

### `request`
This content is categorized under `request` because that's how the official parameter list writes it~

- `silence_duration`: Sets how much additional silence duration to add after each sentence is spoken. Unit is milliseconds. Default is `0`, meaning no additional silence is added. Maximum can be set to `300000` milliseconds.