# MaiBot Plugin Marketplace

## Plugin Marketplace

<div class="plugin-website-card">
  <div class="card-header">
    <h3>🌐 Plugin Showcase Website</h3>
  </div>
  <div class="card-content">
    <p>Our existing plugin list can already be viewed on the plugin showcase website.</p>
    <div class="website-links">
      <a href="https://plugins.maibot.chat" class="backup-link" target="_blank">
        <span class="link-text">plugins.maibot.chat</span>
      </a>
    </div>
  </div>
</div>

<style>
.plugin-website-card {
  border: 1px solid #e1e4e8;
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
  background: #f8f9fa;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.card-header h3 {
  margin: 0 0 15px 0;
  color: #24292e;
  font-size: 18px;
}

.card-content p {
  margin: 0 0 15px 0;
  color: #586069;
  line-height: 1.5;
}

.website-links {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.primary-link, .backup-link {
  padding: 12px 16px;
  background: white;
  border: 1px solid #d1d5da;
  border-radius: 6px;
  transition: all 0.2s ease;
  text-decoration: none;
  display: block;
  cursor: pointer;
}

.primary-link:hover, .backup-link:hover {
  border-color: #0366d6;
  box-shadow: 0 0 0 3px rgba(3, 102, 214, 0.1);
}

.link-text {
  color: #24292e;
  font-family: 'SFMono-Regular', Consolas, 'Liberation Mono', Menlo, monospace;
  font-size: 14px;
  text-decoration: none;
}

.primary-link .link-text {
  color: #0366d6;
}

.backup-link .link-text {
  color: #586069;
}
</style>

## Plugin Usage

For downloaded plugins, you just need to place them in the `plugins` directory under the root directory.

Then, please follow the plugin author's instructions to configure the configuration files yourself to use the plugin!

## Plugin Development

Please refer to the [Plugin Development Guide](/en/develop/plugin_develop/).

## Contributing Plugins

If you want your plugin to appear on the showcase website mentioned above, you can submit a PR to the [plugin repository](https://github.com/Maim-with-u/plugin-repo), following the [CONTRIBUTING.md](https://github.com/MaiM-with-u/plugin-repo/blob/main/CONTRIBUTING.md) to submit your plugin information.

Once reviewed and merged, your plugin will appear on the showcase website.