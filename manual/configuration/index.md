# ⚙️ 配置快速入门

<div class="manual-hero">
  <div class="hero-content">
    <h2 class="hero-title">MaiBot 配置指南</h2>
    <p class="hero-subtitle">了解 MaiBot 的三个核心配置文件，快速完成个性化设置</p>
    <div class="hero-decoration"></div>
  </div>
</div>

## 配置文件总览

MaiBot 现在使用以下三个主要配置文件：

<div class="manual-nav-grid">

<div class="nav-card">

<div class="card-header">
  <span class="card-icon">🤖</span>
  <div class="card-title-group">
    <h3 class="card-title">bot_config.toml</h3>
    <p class="card-desc">MaiBot 行为配置</p>
  </div>
</div>

<div class="card-links">
  <a href="./configuration_standard" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>查看配置教程</span>
  </a>
</div>

<div class="file-info">
  <p>📁 位于 <code>config</code> 文件夹</p>
  <p>包含机器人的名称、性格设定及功能开关</p>
</div>

</div>

<div class="nav-card">

<div class="card-header">
  <span class="card-icon">🧠</span>
  <div class="card-title-group">
    <h3 class="card-title">model_config.toml</h3>
    <p class="card-desc">模型和 API 配置</p>
  </div>
</div>

<div class="card-links">
  <a href="./configuration_model_standard" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>查看模型配置教程</span>
  </a>
</div>

<div class="file-info">
  <p>📁 位于 <code>config</code> 文件夹</p>
  <p>包含 AI 模型配置、API 服务商设置等</p>
</div>

</div>

<div class="nav-card">

<div class="card-header">
  <span class="card-icon">🔌</span>
  <div class="card-title-group">
    <h3 class="card-title">.env</h3>
    <p class="card-desc">端口和 WebUI 配置</p>
  </div>
</div>

<div class="card-links">
  <a href="./configuration_standard.md#环境配置" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>查看环境配置说明</span>
  </a>
</div>

<div class="file-info">
  <p>📁 位于项目根目录</p>
  <p>配置监听地址/端口及 WebUI 启用和模式</p>
</div>

</div>

</div>

<div class="adapter-notice">
  <p>💡 <strong>提示：</strong>为了让 MaiBot 连接 QQ 等平台，你还需要编辑对应适配器的配置文件。查看 <a href="/manual/adapters/">适配器列表</a> 了解更多。</p>
</div>

## 知识库配置

<div class="nav-card knowledge-card">

<div class="card-header">
  <span class="card-icon">📚</span>
  <div class="card-title-group">
    <h3 class="card-title">LPMM 知识库导入</h3>
    <p class="card-desc">通过 OpenIE 技术导入自定义知识</p>
  </div>
</div>

<div class="card-links">
  <a href="./lpmm/lpmm_knowledge_template" class="card-link card-link-main">
    <span class="link-arrow">→</span>
    <span>查看知识库导入要求</span>
  </a>
  <a href="./lpmm/" class="card-link">
    <span class="link-arrow">→</span>
    <span>LPMM 知识库概览</span>
  </a>
  <a href="./lpmm/lpmm_compile_and_install" class="card-link">
    <span class="link-arrow">→</span>
    <span>编译安装指南</span>
  </a>
</div>

<div class="file-info">
  <p>📋 文件命名需以 <code>-openie.json</code> 结尾</p>
  <p>🤖 让 MaiBot 拥有专属的领域知识</p>
</div>

</div>

## 配置步骤

<div class="quick-start-container">

<div class="step-card">
  <div class="step-number">1</div>
  <div class="step-content">
    <h4>配置 .env</h4>
    <p>设置监听地址、端口及 <a href="./configuration_standard">WebUI 参数</a></p>
  </div>
</div>

<div class="step-card">
  <div class="step-number">2</div>
  <div class="step-content">
    <h4>配置 model_config.toml</h4>
    <p>设置 <a href="./configuration_model_standard">AI 模型和 API</a></p>
  </div>
</div>

<div class="step-card">
  <div class="step-number">3</div>
  <div class="step-content">
    <h4>配置 bot_config.toml</h4>
    <p>调整 <a href="./configuration_standard">机器人行为和性格</a></p>
  </div>
</div>

<div class="step-card">
  <div class="step-number">4</div>
  <div class="step-content">
    <h4>配置适配器</h4>
    <p>设置 <a href="/manual/adapters/napcat">Napcat</a> 或其他适配器</p>
  </div>
</div>

</div>

## 常见问题

<div class="faq-container">

<div class="faq-item">
  <div class="faq-question">Q: 为什么现在有两个配置文件？</div>
  <div class="faq-answer">
    <p>A: 为了更好的配置管理和维护，将模型配置和机器人行为配置分离，使配置更加清晰和模块化。</p>
  </div>
</div>

<div class="faq-item">
  <div class="faq-question">Q: 如何从旧版本升级？</div>
  <div class="faq-answer">
    <p>A: 请参考模板文件创建新的配置文件，并确保版本号正确设置。</p>
  </div>
</div>

<div class="faq-item">
  <div class="faq-question">Q: 两个配置文件都需要配置吗？</div>
  <div class="faq-answer">
    <p>A: 是的，两个配置文件都是必需的。<code>bot_config.toml</code> 控制机器人行为，<code>model_config.toml</code> 控制 AI 模型和 API。</p>
  </div>
</div>

</div>

## 获取帮助

<div class="help-container">

<div class="help-card">
  <div class="help-icon">❓</div>
  <h4>常见问题</h4>
  <p>查阅 <a href="/manual/faq/">FAQ</a> 获取常见问题的解决方案</p>
</div>

<div class="help-card">
  <div class="help-icon">💬</div>
  <h4>加入社群</h4>
  <p>加入<a href="/manual/other/group">麦麦社群</a>获取实时帮助</p>
</div>

<div class="help-card">
  <div class="help-icon">📖</div>
  <h4>完整配置</h4>
  <p>查看 <a href="./settings">settings 配置说明</a> 了解所有选项</p>
</div>

</div>

<style scoped>
/* ===== Hero Section ===== */
.manual-hero {
  position: relative;
  padding: 2.5rem 2rem;
  margin-bottom: 2.5rem;
  border-radius: 16px;
  background: linear-gradient(135deg, rgba(255, 140, 0, 0.08) 0%, rgba(50, 205, 50, 0.08) 100%);
  border: 1px solid rgba(255, 140, 0, 0.15);
  overflow: hidden;
}

.hero-content {
  position: relative;
  z-index: 1;
}

.hero-title {
  margin: 0 0 0.75rem 0;
  font-size: 1.75rem;
  font-weight: 700;
  background: linear-gradient(120deg, #ff8c00 30%, #32cd32);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero-subtitle {
  margin: 0;
  font-size: 1rem;
  color: var(--vp-c-text-2);
  max-width: 600px;
  line-height: 1.6;
}

.hero-decoration {
  position: absolute;
  top: -50%;
  right: -10%;
  width: 300px;
  height: 300px;
  background: radial-gradient(circle, rgba(255, 140, 0, 0.1) 0%, transparent 70%);
  pointer-events: none;
}

/* ===== Navigation Grid ===== */
.manual-nav-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.25rem;
  margin-bottom: 2rem;
}

.nav-card {
  background: var(--vp-c-bg-soft);
  border: 1px solid var(--vp-c-divider);
  border-radius: 12px;
  padding: 1.5rem;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.nav-card:hover {
  border-color: rgba(255, 140, 0, 0.3);
  box-shadow: 0 8px 30px rgba(255, 140, 0, 0.12);
  transform: translateY(-4px);
}

.knowledge-card {
  max-width: 600px;
}

.card-header {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  margin-bottom: 1.25rem;
}

.card-icon {
  font-size: 2rem;
  line-height: 1;
  flex-shrink: 0;
}

.card-title-group {
  flex: 1;
}

.card-title {
  margin: 0 0 0.25rem 0;
  font-size: 1.125rem;
  font-weight: 600;
  color: var(--vp-c-text-1);
}

.card-desc {
  margin: 0;
  font-size: 0.875rem;
  color: var(--vp-c-text-3);
}

.card-links {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.card-link {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.625rem 0.875rem;
  border-radius: 8px;
  color: var(--vp-c-text-2);
  font-size: 0.9rem;
  text-decoration: none;
  transition: all 0.2s ease;
  background: rgba(255, 140, 0, 0.03);
}

.card-link:hover {
  background: rgba(255, 140, 0, 0.1);
  color: #ff8c00;
  padding-left: 1.125rem;
}

.link-arrow {
  color: #ff8c00;
  font-size: 0.8rem;
  transition: transform 0.2s ease;
}

.card-link:hover .link-arrow {
  transform: translateX(3px);
}

.card-link-main {
  background: linear-gradient(135deg, rgba(255, 140, 0, 0.08) 0%, rgba(50, 205, 50, 0.05) 100%);
  font-weight: 500;
  border-left: 3px solid #ff8c00;
}

.card-link-main:hover {
  background: linear-gradient(135deg, rgba(255, 140, 0, 0.15) 0%, rgba(50, 205, 50, 0.1) 100%);
}

.file-info {
  padding-top: 0.75rem;
  border-top: 1px solid var(--vp-c-divider);
}

.file-info p {
  margin: 0.25rem 0;
  font-size: 0.8rem;
  color: var(--vp-c-text-3);
}

.file-info code {
  background: rgba(255, 140, 0, 0.1);
  color: #ff8c00;
  padding: 0.125rem 0.375rem;
  border-radius: 4px;
  font-size: 0.75rem;
}

/* ===== Adapter Notice ===== */
.adapter-notice {
  background: linear-gradient(135deg, rgba(50, 205, 50, 0.08) 0%, rgba(255, 140, 0, 0.05) 100%);
  border: 1px solid rgba(50, 205, 50, 0.2);
  border-radius: 8px;
  padding: 1rem 1.25rem;
  margin: 1.5rem 0 2.5rem 0;
}

.adapter-notice p {
  margin: 0;
  font-size: 0.9rem;
  color: var(--vp-c-text-2);
}

.adapter-notice a {
  color: #32cd32;
  text-decoration: none;
  font-weight: 500;
}

.adapter-notice a:hover {
  text-decoration: underline;
}

/* ===== Quick Start Section ===== */
.quick-start-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 1rem;
  margin-bottom: 2.5rem;
}

.step-card {
  position: relative;
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  padding: 1.25rem;
  background: var(--vp-c-bg-soft);
  border: 1px solid var(--vp-c-divider);
  border-radius: 12px;
  transition: all 0.3s ease;
}

.step-card:hover {
  border-color: rgba(50, 205, 50, 0.3);
  box-shadow: 0 4px 20px rgba(50, 205, 50, 0.1);
}

.step-number {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background: linear-gradient(135deg, #ff8c00, #32cd32);
  color: white;
  font-weight: 700;
  font-size: 1rem;
  flex-shrink: 0;
}

.step-content h4 {
  margin: 0 0 0.375rem 0;
  font-size: 0.975rem;
  font-weight: 600;
  color: var(--vp-c-text-1);
}

.step-content p {
  margin: 0;
  font-size: 0.85rem;
  color: var(--vp-c-text-3);
  line-height: 1.5;
}

.step-content a {
  color: #ff8c00;
  text-decoration: none;
  font-weight: 500;
}

.step-content a:hover {
  text-decoration: underline;
}

/* ===== FAQ Section ===== */
.faq-container {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-bottom: 2.5rem;
}

.faq-item {
  background: var(--vp-c-bg-soft);
  border: 1px solid var(--vp-c-divider);
  border-radius: 12px;
  padding: 1.25rem 1.5rem;
  transition: all 0.3s ease;
}

.faq-item:hover {
  border-color: rgba(255, 140, 0, 0.3);
  box-shadow: 0 4px 20px rgba(255, 140, 0, 0.08);
}

.faq-question {
  font-weight: 600;
  color: var(--vp-c-text-1);
  margin-bottom: 0.5rem;
  font-size: 0.95rem;
}

.faq-answer p {
  margin: 0;
  font-size: 0.875rem;
  color: var(--vp-c-text-2);
  line-height: 1.6;
}

.faq-answer code {
  background: rgba(255, 140, 0, 0.1);
  color: #ff8c00;
  padding: 0.125rem 0.375rem;
  border-radius: 4px;
  font-size: 0.8rem;
}

/* ===== Help Section ===== */
.help-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1.25rem;
}

.help-card {
  text-align: center;
  padding: 2rem 1.5rem;
  background: var(--vp-c-bg-soft);
  border: 1px solid var(--vp-c-divider);
  border-radius: 12px;
  transition: all 0.3s ease;
}

.help-card:hover {
  border-color: rgba(255, 140, 0, 0.3);
  transform: translateY(-3px);
  box-shadow: 0 6px 25px rgba(255, 140, 0, 0.1);
}

.help-icon {
  font-size: 2.5rem;
  margin-bottom: 1rem;
}

.help-card h4 {
  margin: 0 0 0.5rem 0;
  font-size: 1rem;
  font-weight: 600;
  color: var(--vp-c-text-1);
}

.help-card p {
  margin: 0;
  font-size: 0.875rem;
  color: var(--vp-c-text-3);
  line-height: 1.5;
}

.help-card a {
  color: #ff8c00;
  text-decoration: none;
  font-weight: 500;
}

.help-card a:hover {
  text-decoration: underline;
}

/* ===== Responsive Design ===== */
@media (max-width: 640px) {
  .manual-hero {
    padding: 1.75rem 1.25rem;
  }
  
  .hero-title {
    font-size: 1.4rem;
  }
  
  .hero-subtitle {
    font-size: 0.9rem;
  }
  
  .manual-nav-grid {
    grid-template-columns: 1fr;
  }
  
  .quick-start-container {
    grid-template-columns: 1fr;
  }
  
  .help-container {
    grid-template-columns: 1fr;
  }
  
  .faq-item {
    padding: 1rem;
  }
}
</style>
