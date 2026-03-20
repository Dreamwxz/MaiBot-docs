# MaiBot Configuration Quick Start

## Configuration Files

MaiBot now uses the following three main configuration files:

1. **`bot_config.toml`** - Located in the `config` folder, this is the MaiBot behavior configuration file containing the bot's name, personality settings, and feature switches.
2. **`model_config.toml`** - Located in the `config` folder, this is the model and API configuration file containing AI model configurations, API provider settings, etc.
3. **`.env`** - Located in the root directory, responsible for configuring the address and port MaiBot listens on; as well as enabling webui, its listening address and port, and mode.

To connect MaiBot to QQ and other platforms, you also need to edit the corresponding adapter configuration files.

## Configuration File Details

[bot_config Configuration Tutorial](./configuration_standard.md)

[model_config Model Configuration Tutorial](./configuration_model_standard.md)

## Knowledge Base Import Requirements

MaiBot supports importing knowledge bases through OpenIE technology. File names must end with `-openie.json`. For specific requirements, please refer to [LPMM Knowledge Base Import Requirements](./lpmm/lpmm_knowledge_template).

## Frequently Asked Questions

If you encounter issues during configuration, please refer to [Frequently Asked Questions](/en/manual/faq/) for solutions.

### Configuration File Related

**Q: Why are there two configuration files now?**
A: To improve configuration management and maintenance, model configuration and bot behavior configuration have been separated, making the configuration clearer and more modular.

**Q: How do I upgrade from an older version?**
A: Please refer to the template files to create new configuration files and ensure the version numbers are set correctly.

**Q: Do I need to configure both configuration files?**
A: Yes, both configuration files are required. `bot_config.toml` controls bot behavior, and `model_config.toml` controls AI models and APIs.