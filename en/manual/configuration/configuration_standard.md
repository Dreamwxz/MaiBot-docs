# Configuration Guide

## Introduction

This configuration file primarily involves all behavioral aspects of MaiMai.

(If you need to configure which groups can chat, you need to configure this in the adapter settings.)

If you want to understand model configuration content, including which models to choose, please refer to [Model Configuration Tutorial](./configuration_model_standard).

## Configuration File Structure

MaiBot now uses an independent `bot_config.toml` file to configure bot behavior. The configuration file contains the following main sections:

- `[bot]` - Basic bot information
- `[personality]` - Persona, expression style
- `[expression]` - Expression learning configuration (includes jargon configuration)
- `[chat]` - Chat rhythm and context
- `[memory]` - Memory module configuration
- `[dream]` - Dream function configuration
- `[message_receive]` - Message filtering rules
- `[tool]` - Tool switches
- `[emoji]` - Emoji pack functionality
- `[voice]` - Voice recognition
- `[lpmm_knowledge]` - LPMM knowledge base
- `[keyword_reaction]` - Keyword/regex triggered responses
- `[response_post_process]` - Response post-processing
- `[log]` - Log configuration
- `[debug]` - Debug switches
- `[maim_message]` - maim_message service
- `[webui]` - WebUI configuration
- `[telemetry]` - Telemetry information
- `[experimental]` - Experimental features

## Configuration File Details
<hr class="custom_hr"/>

```toml
[bot]
platform = "qq"
qq_account = "1145141919810" # MaiMai's QQ account

platforms = ["wx:114514","xx:1919810"] # MaiMai's other platform accounts

nickname = "麦麦" # MaiMai's nickname
alias_names = ["麦叠", "牢麦"] # MaiMai's aliases
```
This configures MaiBot's main platform account information and cross-platform account list.

Calling MaiMai by nickname or alias will get MaiMai's attention; if omitted, nickname/at recognition will not work properly.

<hr class="custom_hr"/>

```toml
[personality]
# Recommended within 100 characters, describing personality traits and identity characteristics
personality = "A sophomore female college student currently online chatting with group friends, sometimes a bit aggressive, sometimes gentler"  
#アイデンティティがない 生まれないらららら
# Describe MaiMai's speaking expression style, expression habits. If modifying, you can add content as appropriate, recommended 1-2 lines
reply_style = "Please don't deliberately highlight your academic background. You can refer to the reply styles of Tieba, Zhihu, and Weibo."

# Multiple reply styles, randomly select expression style based on probability
multiple_reply_style = [
    # "Your style is plain but not without sarcasm, very brief, very colloquial. You can refer to the reply styles of Tieba and Weibo.",
    # "Reply with 1-2 characters",
    # "Reply with 1-2 symbols",
    # "Words are concise and elegant, interspersed with Analects quotes but not obscure, based on classical Chinese short sentences, supplemented by plain meanings, maintaining an elder's gentle demeanor, all expressed in traditional characters, with the conversational charm of pre-Qin Confucian scholars.",
    # "With a bit of translation accent, but not too long",
]

# Probability of using multiple reply styles
multiple_probability = 0.3

# MaiMai's speaking rules and behavior rules:
plan_style = """
1. Consider **all** available actions and **each action** to see if it meets current conditions, use it if the action usage conditions match the chat content
2. If the same content has already been executed, please don't repeat it
3. You're interested in technology-related topics, game and anime-related topics, also interested in daily topics, don't like overly heavy or serious topics
4. Please control your speaking frequency, don't speak too frequently
5. If someone gets annoyed with you, please reduce replies
6. If someone is questioning you, or the topic isn't finished, please continue replying"""

# MaiMai's private chat speaking rules, behavior style:
private_plan_style = """
1. Consider **all** available actions and **each action** to see if it meets current conditions, use it if the action usage conditions match the chat content
2. If the same content has already been executed, please don't repeat it
3. If a sentence has already been replied to, don't repeat the reply"""

# Multiple personalities, randomly select personality based on probability
states = [
    "A female college student who likes online chatting, browses Xiaohongshu." ,
    "A sophomore psychology student, browses Tieba and CNKI." ,
    "A cyber netizen, recently wants to complain about people." 
]

# Probability of using multiple personalities
state_probability = 0.3

# MaiMai image recognition rules, not recommended to modify
visual_style = "Please describe the content of this image in Chinese. If there's text, please summarize the text description, pay attention to its theme, intuitive feelings, output as plain text, maximum 30 characters, please don't use bullet points, just output one paragraph of text"
```

This part is MaiMai's core persona section. Responsible for describing MaiMai's core personality traits and identity characteristics.

- `personality`: Personality traits and identity description, recommended within 100 characters
- `reply_style`: Speaking expression style and habits, recommended 1-2 lines
- `multiple_reply_style`: Multiple reply style list, randomly selected based on probability
- `multiple_probability`: Probability of using multiple reply styles (between 0-1)
- `plan_style`: Speaking rules and behavior style, includes interest preferences and speaking control
- `visual_style`: Image recognition rules
- `private_plan_style`: Private chat specific speaking rules
- `states` and `state_probability`: Configure personality diversity, replace basic `personality` based on probability

<hr class="custom_hr"/>

```toml
[expression]
# Expression learning configuration
learning_list = [ # Expression learning configuration list, supports configuration by chat stream
    ["", "enable", "enable", "enable"],  # Global configuration: use expressions, enable learning, enable jargon learning
    ["qq:1919810:group", "enable", "enable", "enable"],  # Specific group chat configuration: use expressions, enable learning, enable jargon learning
    ["qq:114514:private", "enable", "disable", "disable"],  # Specific private chat configuration: use expressions, disable learning, disable jargon learning
    # Format explanation:
    # First position: chat_stream_id, empty string means global configuration
    # Second position: whether to use learned expressions ("enable"/"disable")
    # Third position: whether to learn expressions ("enable"/"disable") 
    # Fourth position: whether to enable jargon learning ("enable"/"disable")
]

expression_groups = [
    # ["*"], # Global sharing group: all chat_ids share learned expressions (uncomment to enable global sharing)
    ["qq:1919810:private","qq:114514:private","qq:1111111:group"], # Specific sharing groups, chat_ids in the same group will share learned expressions
    # Format explanation:
    # ["*"] - Enable global sharing, all chat streams share expressions
    # ["qq:123456:private","qq:654321:group"] - Specific sharing groups, chat_ids within the group share expressions
    # Note: If it's a group chat, set to group; if it's a private chat, set to private
]

expression_checked_only = true # MaiMai will only use checked expressions

expression_self_reflect = true # Whether to enable automatic expression optimization
expression_auto_check_interval = 600 # Expression automatic check interval time (unit: seconds), default: 600 seconds (10 minutes)
expression_auto_check_count = 20 # Number of expressions randomly selected during each automatic check, default: 20 items
expression_auto_check_custom_criteria = [] # Additional custom evaluation criteria for expression automatic check, format: ["Criterion 1", "Criterion 2", "Criterion 3", ...], these criteria will be added to the evaluation prompt as additional evaluation requirements

expression_manual_reflect = false # Whether to enable manual expression optimization
manual_reflect_operator_id = "" # Manual expression optimization operator ID, format: platform:id:type (e.g., "qq:123456:private" or "qq:654321:group")
allow_reflect = [] # List of chat stream IDs allowed for expression reflection, format: ["qq:123456:private", "qq:654321:group", ...], only chat streams in this list will ask questions and track. If the list is empty, all chat streams can perform expression reflection (provided reflect = true)

all_global_jargon = true # Whether to enable global jargon mode, note: after turning off this function, already recorded global jargon won't change, need to manually delete
enable_jargon_explanation = true # Whether to try to explain jargon in context before replying (turning off can reduce one LLM call, only affects jargon matching and explanation before reply, doesn't affect jargon learning)
jargon_mode = "planner" # Jargon explanation source mode, options: "context" (use context automatic jargon matching) or "planner" (only use unknown_words list given by Planner in reply action)
```

- `learning_list` supports configuring expression learning by chat stream, can set different learning strategies for different group chats or private chats, fourth position field changed to jargon learning switch
- `expression_groups` can set sharing groups, allowing MaiMai to share learned expressions across different chats
- `expression_checked_only`: Controls whether to only use checked expressions
- `expression_self_reflect`: Enables automatic expression optimization function
- `expression_auto_check_interval`: Automatic check interval time for expressions
- `expression_auto_check_count`: Number of expressions selected during each automatic check
- `expression_auto_check_custom_criteria`: Custom evaluation criteria list
- `expression_manual_reflect`: Enables manual expression optimization function
- `manual_reflect_operator_id`: Manual expression optimization operator ID
- `allow_reflect`: List of chat stream IDs allowed for expression reflection
- `all_global_jargon`: Whether to enable global jargon mode (original jargon configuration moved here)
- `enable_jargon_explanation`: Whether to explain jargon before replying
- `jargon_mode`: Jargon explanation source mode, options "context" or "planner"

<hr class="custom_hr"/>

```toml
[chat] # MaiMai's chat settings
talk_value = 1 # Chat frequency, smaller means more silent, range 0-1
mentioned_bot_reply = true # Whether to enable mention-triggered replies
max_context_size = 30 # Context length
planner_smooth = 3 # Planner smoothing, increasing value reduces planner load, slightly reduces response speed, recommended 1-5, 0 means off, must be >= 0
think_mode = "dynamic" # Thinking mode, options: classic (default shallow thinking and reply), deep (longer, deeper replies), dynamic (dynamically selects between two modes)

enable_talk_value_rules = true # Whether to enable dynamic speaking frequency rules

# Dynamic speaking frequency rules: adjust talk_value by time period/by chat_id (prioritizes specific chat matching, then global matching)
# Recommended format (object array): { target="platform:id:type" or "", time="HH:MM-HH:MM", value=0.5 }
# Explanation:
# - target empty string means global; type is group/private, e.g., "qq:1919810:group" or "qq:114514:private";
# - Supports cross-night intervals, e.g., "23:00-02:00"; value range recommended 0-1, if value set to 0 automatically converts to 0.0001 to avoid division by zero error.
talk_value_rules = [
    { target = "", time = "00:00-08:59", value = 0.8 },
    { target = "", time = "09:00-22:59", value = 1.0 },
    { target = "qq:1919810:group", time = "20:00-23:59", value = 0.6 },
    { target = "qq:114514:private", time = "00:00-23:59", value = 0.3 },
]
```

This part controls MaiMai's chat rhythm and context:
- `talk_value`: Global chat activity level
- `mentioned_bot_reply`: Whether to enable mention-triggered replies
- `max_context_size`: Context window length
- `planner_smooth`: Planner smoothing, increasing value reduces planner load, slightly reduces response speed, recommended 1-5, 0 means off, must be >= 0
- `think_mode`: Thinking mode, options "classic" (default shallow thinking and reply), "deep" (longer, deeper replies), "dynamic" (dynamically selects between two modes)
- `enable_talk_value_rules` + `talk_value_rules`: Adjust `talk_value` based on time period or chat_id, supports cross-night intervals

<hr class="custom_hr"/>

```toml
[memory]
max_agent_iterations = 5 # Memory thinking depth (minimum 1)
agent_timeout_seconds = 180.0 # Maximum recall time (seconds), default 180 seconds
global_memory = false # Whether to allow memory retrieval to perform global queries
global_memory_blacklist = [
    
] # Global memory blacklist, when global memory is enabled, exclude specific chat streams from retrieval. Format: ["platform:id:type", ...], e.g., ["qq:1919810:private", "qq:114514:group"]
planner_question = true # Whether to use Planner-provided question as memory retrieval question. When enabled, when Planner provides a question in reply action, directly use that question for memory retrieval, skipping LLM question generation step; when disabled, uses old mode, uses LLM to generate question
```

- `max_agent_iterations`: Controls maximum iteration count for memory retrieval and summarization, larger values take more time, default 5 times
- `agent_timeout_seconds`: Maximum recall time (seconds), exceeding this time stops recall process, default 180 seconds
- `global_memory`: Whether to allow memory retrieval to perform global queries, when enabled can retrieve memories across chat streams
- `global_memory_blacklist`: Global memory blacklist, when global memory is enabled, exclude specific chat streams from retrieval
- `planner_question`: Whether to use Planner-provided question as memory retrieval question, when enabled skips LLM question generation step

<hr class="custom_hr"/>

```toml
[dream]
interval_minutes = 60 # Dream time interval (minutes), default 60 minutes
max_iterations = 20 # Maximum dream rounds, default 20 rounds
first_delay_seconds = 1800 # Delay time before first dream after program startup (seconds), default 1800 seconds (30 minutes)

# Dream result push target, format "platform:user_id"
# Example: "qq:123456" means after dream ends, additionally send dream text to this QQ private chat user.
# Empty string means no push.
dream_send = ""

# Dream time period configuration, format: ["HH:MM-HH:MM", ...]
# If list is empty, means dreams allowed all day.
# If time periods configured, dream process only actually executed within these time periods.
# Outside time periods, scheduler still checks at intervals but won't enter dream process.
# Supports cross-night intervals, e.g., "23:00-02:00" means from 23:00 to next day 02:00.
# Example:
dream_time_ranges = [
    # "09:00-22:00",      # Daytime dreams allowed
    "23:00-10:00",      # Cross-night time period (23:00 to next day 10:00)
]
# dream_time_ranges = []
```

- `interval_minutes`: Dream time interval (minutes), default 60 minutes
- `max_iterations`: Maximum dream rounds, default 20 rounds
- `first_delay_seconds`: Delay time before first dream after program startup (seconds), default 1800 seconds (30 minutes)
- `dream_send`: Dream result push target, format "platform:user_id", empty string means no push
- `dream_time_ranges`: Dream time period configuration, supports cross-night intervals, empty list means dreams allowed all day

<hr class="custom_hr"/>

```toml
[message_receive]
# Below are message filters, can filter specific messages based on rules, these messages won't be read
ban_words = [
    # "403","张三"
    ]

ban_msgs_regex = [
    # Regular expressions matching messages (original messages) to filter, matched messages will be filtered, if not familiar with regex please don't modify
    #"https?://[^\\s]+", # Match https links
    #"\\d{4}-\\d{2}-\\d{2}", # Match dates
]
```
- `ban_words` is a keyword blacklist, messages containing these words will be filtered.
- `ban_msgs_regex` is a regex blacklist, matched messages will be filtered.

<hr class="custom_hr"/>

```toml
[tool]
enable_tool = true # Whether to enable tools
```

- `enable_tool` controls whether to enable tool functionality in regular chats

<hr class="custom_hr"/>

```toml
[emoji]
emoji_chance = 0.4 # Probability of MaiMai activating emoji pack actions
max_reg_num = 100 # Maximum number of registered emoji packs
do_replace = true # When enabled, deletes (replaces) emoji packs when maximum number reached; when disabled, won't continue collecting emoji packs when maximum number reached
check_interval = 10 # Check emoji packs (registration, damage, deletion) time interval (minutes)
steal_emoji = true # Whether to steal emoji packs, allowing MaiMai to claim some emoji packs as her own
content_filtration = false  # Whether to enable emoji pack filtering, only emoji packs meeting requirements will be saved
filtration_prompt = "符合公序良俗" # Emoji pack filtering requirements, only emoji packs meeting these requirements will be saved
```

This part configures emoji pack related functionality:
- `emoji_chance`: Probability of actively sending emoji pack actions
- `max_reg_num` + `do_replace`: Controls emoji pack capacity and淘汰 strategy
- `check_interval`: Scan registration/damage/deletion cycle (minutes)
- `steal_emoji`: Whether to allow "emoji stealing"
- `content_filtration`/`filtration_prompt`: Filtering conditions

<hr class="custom_hr"/>

```toml
[voice]
enable_asr = false # Whether to enable voice recognition, when enabled MaiMai can recognize voice messages, enabling this function requires configuring voice recognition model [model_task_config.voice]
```

- `enable_asr` controls whether to enable voice recognition functionality

<hr class="custom_hr"/>

```toml
[lpmm_knowledge] # lpmm knowledge base configuration
enable = false # Whether to enable lpmm knowledge base
lpmm_mode = "agent"
# Can choose classic traditional mode/agent mode, used together with new memory
rag_synonym_search_top_k = 10 # Synonym retrieval TopK
rag_synonym_threshold = 0.8 # Synonym threshold, relations with similarity above this value will be treated as synonyms
info_extraction_workers = 3 # Entity extraction concurrent thread count, non-Pro models shouldn't set above 5
qa_relation_search_top_k = 10 # Relation retrieval TopK
qa_relation_threshold = 0.5 # Relation threshold, relations with similarity above this value will be considered related
qa_paragraph_search_top_k = 1000 # Paragraph retrieval TopK (can't be too small, may affect search results)
qa_paragraph_node_weight = 0.05 # Paragraph node weight (weight in graph search & PPR calculation, when search only uses DPR, this parameter doesn't work)
qa_ent_filter_top_k = 10 # Entity filtering TopK
qa_ppr_damping = 0.8 # PPR damping coefficient
qa_res_top_k = 3 # Final provided paragraph TopK
embedding_dimension = 1024 # Embedding vector dimension, output dimension consistent
# Performance and degradation parameters (low-spec machines can lower)
# Low-spec machine reference: single/dual core or memory ≤4GB (e.g., lightweight cloud hosts/cloud functions/development boards), recommended to first turn off PPR and reduce concurrency
max_embedding_workers = 3 # Embedding/extraction concurrent thread count
embedding_chunk_size = 4 # Number of items per embedding batch
max_synonym_entities = 2000 # Maximum number of entities participating in synonym edges, exceeding skips
enable_ppr = true # Whether to enable PPR, low-spec machines can turn off
```

This part configures LPMM knowledge base:
- `lpmm_mode`: Currently supports `classic` (traditional mode) and `agent` (mode, used together with new memory)
- `rag_synonym_search_top_k`: Synonym retrieval TopK
- `rag_synonym_threshold`: Synonym threshold, relations with similarity above this value will be treated as synonyms
- `info_extraction_workers`: Entity extraction concurrent thread count, non-Pro models shouldn't set above 5
- `qa_relation_search_top_k`: Relation retrieval TopK
- `qa_relation_threshold`: Relation threshold, relations with similarity above this value will be considered related
- `qa_paragraph_search_top_k`: Paragraph retrieval TopK (can't be too small, may affect search results)
- `qa_paragraph_node_weight`: Paragraph node weight (weight in graph search & PPR calculation, when search only uses DPR, this parameter doesn't work)
- `qa_ent_filter_top_k`: Entity filtering TopK
- `qa_ppr_damping`: PPR damping coefficient
- `qa_res_top_k`: Final provided paragraph TopK
- `embedding_dimension`: Embedding vector dimension, output dimension consistent
- `max_embedding_workers`: Embedding/extraction concurrent thread count (performance and degradation parameter, low-spec machines can lower)
- `embedding_chunk_size`: Number of items per embedding batch (performance and degradation parameter)
- `max_synonym_entities`: Maximum number of entities participating in synonym edges, exceeding skips (performance and degradation parameter)
- `enable_ppr`: Whether to enable PPR, low-spec machines can turn off (performance and degradation parameter)

<hr class="custom_hr"/>

```toml
[keyword_reaction]
keyword_rules = [
    { keywords = ["人机", "bot", "机器", "入机", "robot", "机器人", "ai", "AI"], reaction = "If someone says you're a bot or robot, deny this fact, attack or question them back. Note, only reply if they think you're a robot, otherwise don't deny" },
    { keywords = ["测试关键词回复", "test"], reaction = "Answer test successful" },
    #{ keywords = ["你好", "hello"], reaction = "Hello, how can I help you?" }    
    # Add more rules here, same format as above
]

regex_rules = [
    { regex = ["^(?P<n>\\S{1,20})是这样的$"], reaction = "Please make sentences according to the following template: [n] is like this, xx only needs xx, but [n] has many things to consider, like when to xx, when to xx, when to xx. (Please freely replace xx parts, just maintain sentence structure, while expressing an ironic sense of overemphasizing [n])" },
    { regex = ["(?:[^，。.\\s]+，\\s*)?我(?:也)?[没沒]要求你\\s*(?P<action>.+?)[.。,，]?$"],reaction = "Perform a 'Yamata no Orochi定型文' completion. Temporarily ignore reply style, enter 'fan吐槽' mode. Please strictly follow the following 'fill-in template', create around '[action]', making it full of ridiculous exaggeration and imaginative associations: I didn't ask you to [action], I'm not (a tolerant character related to action). But, what does (specific negative behavior related to action) mean? What's wrong with your (some concept related to action)? You're (some state or time point related to action), right? If this continues, you (a mid-term, geometric progression ridiculous deduction), (a late-term, geometric progression ridiculous deduction), finally become (an exaggerated final form related to the theme). As (a nemesis or authority identity related to final form), I might have to (execute a heavenly punishment-like action on you). Really." }
]
```

- `keyword_rules` used to set keyword-triggered additional reply knowledge.
- `regex_rules` used to set regex-triggered additional reply knowledge.

<hr class="custom_hr"/>

```toml
[response_post_process]
enable_response_post_process = true # Whether to enable response post-processing, including typo generator, response splitter

[chinese_typo]
enable = true # Whether to enable Chinese typo generator
error_rate=0.01 # Single character replacement probability
min_freq=9 # Minimum character frequency threshold
tone_error_rate=0.1 # Tone error probability
word_replace_rate=0.006 # Whole word replacement probability

[response_splitter]
enable = true # Whether to enable response splitter
max_length = 512 # Maximum allowed response length
max_sentence_num = 8 # Maximum allowed sentence count in response
enable_kaomoji_protection = false # Whether to enable kaomoji protection
enable_overflow_return_all = false # Whether to return all content at once when sentence count exceeds maximum allowed sentence count in response
```

This part can perform secondary processing on model responses.

<hr class="custom_hr"/>

```toml
[log]
date_style = "m-d H:i:s" # Date format
log_level_style = "lite" # Log level style, options FULL, compact, lite
color_text = "full" # Log text color, options none, title, full
log_level = "INFO" # Global log level (backward compatible, priority lower than separate settings below)
console_log_level = "INFO" # Console log level, options: DEBUG, INFO, WARNING, ERROR, CRITICAL
file_log_level = "DEBUG" # File log level, options: DEBUG, INFO, WARNING, ERROR, CRITICAL

# Third-party library log control
suppress_libraries = ["faiss","httpx", "urllib3", "asyncio", "websockets", "httpcore", "requests", "peewee", "openai","uvicorn","jieba"] # Completely suppressed libraries
library_log_levels = { aiohttp = "WARNING"} # Set specific library log levels
```

This part configures the logging system.

<hr class="custom_hr"/>

```toml
[debug]
show_prompt = false # Whether to show prompt
show_replyer_prompt = false # Whether to show replyer prompt
show_replyer_reasoning = false # Whether to show replyer reasoning
show_jargon_prompt = false # Whether to show jargon-related prompts
show_memory_prompt = false # Whether to show memory retrieval related prompts
show_planner_prompt = false # Whether to show planner's prompt and original return results
show_lpmm_paragraph = false # Whether to show lpmm found related paragraph logs
```

- These switches can output debug information by module: prompt, reasoning, jargon, memory, planner, LPMM, etc.

<hr class="custom_hr"/>

```toml
[maim_message]
auth_token = [] # Authentication tokens, for old API verification, empty means verification not enabled

# New API Server configuration (additional listening port)
enable_api_server = false # Whether to enable additional new API Server
api_server_host = "0.0.0.0" # New API Server host address
api_server_port = 8090 # New API Server port number
api_server_use_wss = false # Whether new API Server enables WSS
api_server_cert_file = "" # New API Server SSL certificate file path
api_server_key_file = "" # New API Server SSL key file path
api_server_allowed_api_keys = [] # New API Server allowed API Key list, empty means all connections allowed
```

Advanced settings, usually no need to modify.

<hr class="custom_hr"/>

```toml
[webui] # WebUI independent server configuration
# Note: WebUI listening address (host) and port (port) have been moved to WEBUI_HOST and WEBUI_PORT in .env file
enabled = true # Whether to enable WebUI
mode = "production" # Mode: development (development) or production (production)

# Anti-crawler configuration
anti_crawler_mode = "loose" # Anti-crawler mode: false (disabled) / strict (strict) / loose (loose) / basic (basic - only records doesn't block)
allowed_ips = "127.0.0.1" # IP whitelist (comma separated, supports exact IP, CIDR format and wildcards)
                          # Example: 127.0.0.1,192.168.1.0/24,172.17.0.0/16
trusted_proxies = "" # Trusted proxy IP list (comma separated), only X-Forwarded-For from these IPs trusted
                     # Example: 127.0.0.1,192.168.1.1,172.17.0.1
trust_xff = false # Whether to enable X-Forwarded-For proxy parsing (default false)
                  # When enabled, still requires direct connection IP to be in trusted_proxies to trust XFF header
secure_cookie = false # Whether to enable secure cookies (only transmitted via HTTPS, default false)
```

This part configures WebUI independent server:
- `enabled`: Whether to enable WebUI
- `mode`: Mode, options "development" (development) or "production" (production)
- `anti_crawler_mode`: Anti-crawler mode, options false (disabled), "strict" (strict), "loose" (loose), "basic" (basic - only records doesn't block)
- `allowed_ips`: IP whitelist (comma separated, supports exact IP, CIDR format and wildcards)
- `trusted_proxies`: Trusted proxy IP list (comma separated)
- `trust_xff`: Whether to enable X-Forwarded-For proxy parsing
- `secure_cookie`: Whether to enable secure cookies (only transmitted via HTTPS)

**Note**: WebUI listening address (host) and port (port) have been moved to WEBUI_HOST and WEBUI_PORT in .env file

<hr class="custom_hr"/>

```toml
[telemetry] # Send telemetry information, mainly to see how many MaiMais globally
enable = true

[experimental] # Experimental features
# Add additional prompt configuration for specified chats
# Format: ["platform:id:type:prompt content", ...]
# Example:
# chat_prompts = [
#     "qq:114514:group:This is a photography group, you're proficient in photography knowledge",
#     "qq:19198:group:This is an anime交流 group",
#     "qq:114514:private:This is your private chat with good friend"
# ]
chat_prompts = []
```

- `telemetry` controls whether to send telemetry information
- `experimental.chat_prompts` can add additional prompts for specific `platform:id:type`, format `["platform:id:type:prompt content", ...]`

<hr class="custom_hr"/>

## Notes

1. **API Key Security**:
    - Properly store API keys
    - Don't upload configuration files containing keys to public repositories

2. **Configuration Modification**:
    - Need to restart service after modifying configuration
    - Model configuration now in independent `model_config.toml` file
    - QQ numbers and group numbers use numeric format (except bot QQ number)
    - **Configuration file version number needs to increment**

3. **Other Notes**:
    - Project is in testing phase, may have unknown issues
    - Recommended to keep default configuration for first use
    - Configuration files now divided into two: `bot_config.toml` and `model_config.toml`

4. **Error Troubleshooting**:
    - Configuration errors: Check if configuration file syntax correct
    - Function abnormalities: Confirm if related function switches enabled
    - Model issues: Check model configuration in `model_config.toml`