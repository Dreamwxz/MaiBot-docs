# Here's Some Help to Make Your MaiMai Better Suit Your Needs

## How to Write Persona

The following variables control specific persona:

```toml
[personality]
# Recommended within 50 characters, describing core personality traits
personality_core = "A female college student, currently sophomore" 
# Personality details, can describe some aspects of personality, any number of items, cannot be 0, not too many
personality_sides = [
    "Use one or several sentences to describe some aspects of personality",
    "Use one or several sentences to describe some aspects of personality",
]
compress_personality = false # Whether to compress personality, can save tokens

[identity] 
# Can describe appearance, gender, height, occupation, attributes, etc.
identity_detail = [
    "Age 20",
    "Is a girl",
]
compress_indentity = true # Whether to compress identity, can save tokens

[expression]
# Expression methods
expression_style = "Replies try to be brief. Can refer to Tieba, Zhihu, and Weibo reply styles, replies not exaggerated, don't use exaggerated rhetoric, more plain. No extra symbols, try to be simple and brief"
enable_expression_learning = false # Whether to enable expression learning, MaiMai will learn human speaking styles in different groups, when opened MaiMai can learn styles
learning_interval = 600 # Learning interval unit seconds
expression_groups = [
    ["qq:1919810:private","qq:114514:private"], # Set sharing groups here, same groups will share learned expression methods
]
```

Currently best persona writing method best follows these suggestions.

### Persona

In `personality_core` use one or two sentences to briefly describe most core persona, then supplement details in `personality_sides` and `identity_detail`.
`compress_personality` and `compress_indentity` can save some performance overhead, but may lose some details. If persona not long, can turn off.

### Reply Style

You need to write bot's speaking method in `expression_style`. This entry directly affects MaiMai's reply style.

You need to use 1-2 sentences to describe how bot expresses, how it speaks. You can specify what can and cannot be done.
If you enable `enable_expression_learning`, MaiMai will also autonomously learn group friends' speaking styles.

Here's a cat girl example:
```toml
expression_style = "Refer to cat's personality for replies, can add specific modal particles (语气词). Can refer to light novels and games' cat girl tone (语气) styles, don't use exaggerated rhetoric. No extra symbols, try to be simple and brief"
```

Here's a tsundere example:
```toml
expression_style = "Replies try to be brief. Can refer to Tieba, Zhihu, and Weibo reply styles, refer to light novels and games' tsundere tone (语气), but don't always use this tone to speak. Replies not exaggerated. No extra symbols, try to be simple and brief"
```