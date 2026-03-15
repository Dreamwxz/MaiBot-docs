# ❓ Frequently Asked Questions (FAQ)

This page summarizes the most common issues encountered during deployment, configuration, and daily use, along with suggested troubleshooting steps. For more detailed explanations, refer to the corresponding feature pages for in-depth documentation.

---

## 1. MaiBot Not Responding / Abnormal Response Frequency

**Q: Why isn't MaiBot replying to messages?** Please check:

1. Whether `[chat].talk_value` in `config/bot_config.toml` is set too low.
2. Whether `talk_value_rules` is triggered for the current time period, reducing the value to 0.
3. Whether `plan_style` requires "speak less" - you can moderately relax this.
4. Whether "reply only when mentioned" is enabled: when `mentioned_bot_reply = true`, MaiBot will only reply when someone @mentions it.

**Q: Mention-triggered replies aren't working?**
Ensure the group's `@MaiBot` format is correct, and confirm that `mentioned_bot_reply = true` and `at_bot_inevitable_reply` is greater than 0. If it still doesn't work, search for `planner` output in the logs to confirm if mention events are being detected.

---

## 2. Data and Privacy

**Q: Where are chat records and memory data stored?**
By default, they are stored in `data/maibot.db` (Peewee SQLite), including slang, memories, emojis, plugin data, etc. For migration or cleanup, you can back up/replace this file after stopping the service.

**Q: How to ensure data compliance?**
Please read the [Terms of Use](../other/EULA.md) before deployment. If your environment has requirements for log/memory storage, you can disable unnecessary modules (such as `[memory]`, `[emoji].steal_emoji`), or regularly execute cleanup scripts at the database level.

---

## 3. API Quotas and Rate Limits

**Q: Frequently encountering 429 errors or insufficient balance?**

- Check if `model_task_config` has selected a model with higher rate limits; you can choose `utils_small` for high-frequency tasks (like expression learning, slang inference) to reduce token usage.
- Adjust `[chat].planner_size`, `[expression].learning_list`'s `learning_intensity` to reduce high-frequency calls.
- Configure multiple API Keys in `.env` (if supported by the service provider) and enable rotation.

---

## 4. Personality and Expression

**Q: What happens if the personality configuration is too long?**
`personality` is recommended to be kept within 100~150 characters. If it's too long, it will occupy context space and make responses sluggish. You can split differentiated settings into fields like `states`, `plan_style`, `reply_style`, etc., and switch them in specific scenarios.

**Q: Old expressions aren't updating in expression learning?**
Check the `expression` data under `data/`, you can manually delete outdated expressions, or reduce `learning_intensity` to lower the learning frequency. If necessary, you can reset the `Expression` table to start accumulating again.

---

## 5. Common Customization Needs

**Q: Want to make MaiBot more "talkative"/more "socially anxious"?** Adjust these three areas:

- `[chat].talk_value` and `talk_value_rules` control overall frequency.
- `[personality].plan_style` determines trigger rules; you can add instructions like "jump in when you see lively conversation."
- Expression learning's `use_expression`/`enable_learning` controls language style diversity.

**Q: Need multilingual output?**
Specify language requirements in `reply_style`, or configure `experimental.chat_prompts` per group (e.g., "This is an English group, please reply in English"). Also, pay attention to emoji/character set compatibility across different platforms.

---

## 6. Platform-Specific Issues

If you encounter issues related to the operating system, please refer to the corresponding platform FAQ:

- **[Windows FAQ](./windows.md)** – For deployment, permissions, dependency issues specific to Windows systems.
- **[Linux FAQ](./linux.md)** – For virtual environment, compilation tools, firewall issues specific to Linux systems.
- **[macOS FAQ](./macos.md)** – For security prompts, port usage, Xcode tool issues specific to macOS.

---

If the above content doesn't cover your issue, feel free to provide feedback via Issues, group chats, or the plugin marketplace. We'll continue to improve the FAQ. You're also welcome to share your troubleshooting experiences to help future users get started faster.***

(End of file - total 74 lines)