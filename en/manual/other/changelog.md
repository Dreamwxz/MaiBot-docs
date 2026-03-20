# Changelog
## [0.12.2] - 2025-1-11
### Feature Changes
- Optimized private chat wait logic
- Force quote reply on timeout
- Fixed disconnection issues with some adapters
- Fixed expression reflection configuration not taking effect
- Optimized memory retrieval logic
- Updated readme

## [0.12.1] - 2025-12-31
### 🌟 Major Updates
- Added annual summary! Can be viewed in webui
- Option to let LLM determine quote replies
- Expression optimization! Now supports automatic and manual evaluation for better accuracy
- Reply and planning records! WebUI can view details of each reply and plan

### Detailed Feature Changes
- Optimized display of messages with long intervals
- enable_jargon_detection
- global_memory_blacklist. Specify certain group chats to not participate in global memory
- Removed utils_small model, removed deprecated lpmm model

## [0.12.0] - 2025-12-21
### 🌟 Major Updates
- Added thinking intensity mechanism, dynamically controls reply time and length
- Planner and replyer now work together, better reply logic
- New private chat system, incorporating excellent mechanisms from pfc
- Added MaiMai dreaming function
- mcp plugin added as built-in plugin, not enabled by default
- Added global memory configuration option, now can choose to make memories global

### 🌟 WebUI Major Updates
- **Model preset market function officially completed and released**: Now can fully share model configurations, share button located in top right corner of model configuration interface
- **Comprehensive security hardening**: Added authentication protection for all WebUI API and WebSocket endpoints, Cookie added Secure and SameSite attributes, supports environment-aware dynamic configuration
- **Frontend authentication refactoring**: Migrated from localStorage to HttpOnly Cookie, added WebSocket temporary token authentication mechanism, solves cross-domain development environment Cookie carrying issues
- **Enhanced plugin configuration management**: Supports loading and saving original TOML configurations, frontend supports viewing and editing plugin configuration source files

### Detailed Feature Changes
- Removed frequency auto-adjustment
- Removed emotion function
- Optimized memory difference timeout settings
- Fixed bug where some configurations were 0
- Plugin installation can actively select clone branch
- Feedback questionnaire function on homepage, can submit feedback and suggestions
- Jargon and expressions no longer extract content containing names
- Model interface supports editing extra params additional fields
- Task allocation sub-interface in model interface supports editing slow request detection threshold
- Model interface supports specifying temperature parameter and max tokens parameter for individual models
- All data cards on homepage support automatic unit selection + detailed information display
- WebUI chat room emoji, image, rich text message support
- MaiMai adapter configuration interface work mode supports folding
- WebUI plugin configuration parsing supports dynamic list forms
- Dynamic lists in WebUI plugin configuration support switch, slider, and dropdown types
- Added restart button in plugin market, plugin configuration details interface
- Enhanced security and privacy protection: Added login interface rate limiting to prevent brute force attacks, tightened CORS configuration (restricts allowed HTTP methods and request headers), improved path validation (validate_safe_path prevents directory traversal attacks), fetchWithAuth supports FormData file upload, added robots.txt route and X-Robots-Tag response header to prevent search engine indexing, frontend added meta noindex/nofollow tags to block crawler indexing
- Fixed and optimized chat room, model configuration, log viewer, jargon management, WebUI port occupancy, configuration wizard, homepage charts, chat room message duplication, mobile log invisibility, model provider deletion, main program configuration line breaks, HTTP warning banner, restart interface, LPMM configuration, character information, plugin endpoint security authentication, WebSocket token and other issues, improving overall stability and experience.
- Completed main program configuration and model configuration interface refactoring, model provider and MaiMai adapter configuration refactoring (introduced TOML validation), WebSocket authentication logic extracted as shared module unifying WS endpoints, upgraded React to 19.2.1 and updated dependencies, WebUI configuration and visualization all migrated to main configuration and model configuration, optimized configuration update prompts, plugin details pages and path security validation, and enhanced visualization and automatic validation of model and dream configurations.

## [0.11.6] - 2025-12-2
### 🌟 Major Updates
- Significantly improved memory retrieval capability, slightly increased token consumption
- Refactored historical message summarizer, better topic memory
- Revolutionary optimization of log viewer performance
- Supports visual viewing of MaiMai LPMM knowledge graph
- Supports automatic model acquisition based on different model providers/templates/URLs, no need to manually input models
- Added Baka guidance system, using React-JoyTour to implement excellent user guidance system (so even Baka can understand!)
- Local chat room function!! You can directly chat with MaiMai in WebUI webpage!!
- Replaced original LocalStorage Token storage with cookie mode, may need to manually input Token again
- WebUI local chat room supports user simulation and platform simulation functions!
- WebUI added jargon management & editing interface

### Detailed Feature Changes
- Option to enable jargon in memory recognition
- Decoupled emoji recognition and image recognition
- Fixed parsing issues with some broken JSON
- Higher extraction efficiency for jargon, increased extraction accuracy
- Upgraded jargon, faster and more precise
- Added Lpmm visualization

### WebUI Detailed Updates
- Fixed sidebar collapse, UI and table horizontal scrolling issues, optimized Toast animation
- Fixed adapter configuration, plugin cloning, emoji registration related BUGs
- Added adapter/model preset modes and templates, automatically filling URL and type
- Supports model task list drag-and-drop sorting
- Updated restart popup and first-time guidance content
- Multiple interface naming and title optimizations, such as model configuration related menu renaming and description updates
- Fixed chat configuration "mention reply" related switch naming errors
- Debug configuration added "show memory/Planner/LPMM Prompt" option
- Added card size, sorting, font size, line spacing and other personalization functions
- Chat ID and group chat selection optimization, showing readable names
- Chat editing interface streamlined fields, added backend chat list API support
- Default line spacing reduced, display more compact
- Fixed page scrolling, emoji sorting, speech frequency 0 and other issues
- Added React exception Traceback interface and model list search
- Updated WebUI Icon, fixed adapter docker path and other issues
- Plugin configuration visual editing, form control/metadata/layout type extensions
- Added plugin API and development documentation
- Added bot status cards and quick operation buttons
- Adjusted pie chart display, color algorithms, fixed some statistics and parsing errors
- Added cache, WebSocket configuration
- Emoji supports upload and thumbnails
- Fixed homepage extreme loading, CtrlC failure after restart, main program configuration mobile adaptation and other issues
- Added expression reflection settings and WebUI chat room "thinking" placeholder component
- Details such as removing some fields or UI controls, optimizing button/popup/edit logic, etc.

## [0.11.5] - 2025-11-21
### 🌟 Major Updates
- WebUI now supports manual restart of MaiMai, workaround version of "hot reload"
- Added MaiMai QQ adapter visual editing UI (independent process, requires manual upload/download and overwrite adapter files)
- MaiMai main program configuration supports visual mode and source code mode dual-mode editing, backend performs TOML validation
- Optimized planner and replyer collaboration mechanism, more detailed debug logs

### New Additions
- Emoji management, character information management, expression method management interface mobile adaptation
- Configuration page "Restart MaiMai" prompt
- Detailed debug prompt display configuration
- MaiMai interface operation theme color button
- Frontend integrated CodeMirror (Python/JSON/TOML syntax highlighting) and provides automatic error correction hints for JSON configuration

### Fixes
- Emoji thumbnails too small
- Added models not immediately displayed
- Plugin market category errors
- Light mode log viewer background abnormal
- Emoji details window cannot close
- Emotion tags cannot be read normally (issues/1373)
- Model task configuration interface model temperature cannot be changed (issues/1369)
- Model task configuration interface some input boxes cannot delete default values
- Plugin store default checks "Only show compatible plugins"
- Plugin store tab count display error
- Log viewer log line break abnormal
- Theme color not correctly applied
- Sidebar scrollbar issues
- Removed frontend TOML parsing library compatibility issues

### Optimizations
- Homepage loading user experience
- Homepage loading speed (9s → <1s)
- Initial screen loading speed of entire interface

### Updates
- Adapter configuration supports upload mode and specified path mode; specified path mode avoids repeated upload/download of configuration files
- Adapter configuration interface tab responsive optimization, small screens only show short labels
- MaiMai resource management all interfaces batch deletion capability
- Resource management all interfaces pagination, items per page selection and page jump
- Plugin market added likes, dislikes, ratings and download statistics (based on Cloudflare, ensuring domestic accessibility)
- MaiMai emoji viewing interface description part supports Markdown rendering

## [0.11.4] - 2025-11-19
### 🌟 Main Update Content
- **First official Web management interface launched**: Before this version, MaiBot had no WebUI, all configurations required manual TOML file editing
- **Authentication system**: Token secure login (supports system-generated 64-bit random tokens / custom Token), first-time configuration wizard
- **Configuration management (visual editing, no need to manually edit TOML)**:
  - MaiMai main program configuration: Basic settings, personality, expressions, jargon, emotions, etc.
  - Model provider configuration: OpenAI, Anthropic, DeepSeek, Qwen, Ollama, etc.
  - Model configuration: Dialogue/visual/embedding model allocation
- **Resource management**:
  - Emoji management: View, search, register, ban
  - Expression method management: View MaiMai's expression records
  - Character information management: View contact list
- **Plugin system**:
  - Plugin market browsing
  - One-click install/uninstall/update
  - Version compatibility checking
  - Real-time installation progress push
- **Log viewer**:
  - WebSocket real-time log stream
  - Log level filtering (DEBUG/INFO/WARNING/ERROR/CRITICAL)
  - Search function
- **Theme customization**:
  - Light/dark/follow system
  - 12 theme colors (6 solid colors + 6 gradients)
  - Custom color picker
- **Global search**: Cmd/Ctrl + K shortcut, quick jump to any page

### Details
- **Technology stack**:
  - Frontend: React 19 + TypeScript + Vite + TanStack Router + shadcn/ui
  - Backend: FastAPI + Uvicorn + WebSocket
  - Features: SPA single-page application, frontend-backend same port, static file hosting
- **Usage**: Refer to template.env file to update .env file, add two fields:
  - `WEBUI_ENABLED=true`
  - `WEBUI_MODE=production`
- **WebUI open source license**: GPLv3
- **WebUI address**: https://github.com/Mai-with-u/MaiBot-Dashboard

Goodbye to manual configuration file editing, enjoy modern graphical interface!

## [0.11.3] - 2025-11-18
### Feature Changes and Fixes
- Optimized memory extraction strategy
- Optimized jargon extraction
- Optimized expression method learning
- Modified readme
- Added beta webui

Tip: Clean old memory data and expression methods for better performance
Method: Delete all content of expression jargon and thinking_back in database

## [0.11.2] - 2025-11-16
### 🌟 Main Feature Changes
- "Hippocampus Agent" memory system launched, latest and best memory system, default already connected to lpmm
- Added jargon learning system
- Added group special Prompt system
- Optimized direct mention reply speed

### Detailed Feature Changes
- Added WebUI module and related API routes and Token management functions
- Can record and query group nicknames through Hippocampus Agent
- Added chat record summary module
- Added many new statistical metrics

### Feature Changes and Fixes
- Removed expression method learning upper limit restriction
- Removed some unused code
- Removed issue tracking and old version memory
- Removed Exp+model expression method, removed useless code
- Removed issue tracking and memory organization
- Removed active speech function
- Optimized self-recognition and emotions
- Optimized memory extraction capability
- Optimized planner, less consumption when mentioned, reduced sensitivity during continuous no_reply
- Compressed 1/3 of planner consumption
- Optimized memory retrieval occupancy
- Optimized memory extraction and chat compression
- Optimized typo generation and segmentation
- Optimized log and added changelog
- Beautified statistics interface
- Corrected memory extraction LLM statistics
- Fixed docker issues
- Fixed some potential issues
- Fixed bool and boolean issues
- Fixed timeout giving all information Bug
- Fixed reply too long can now return original text
- Fixed private chat memory
- Fixed prompt issues
- Fixed (bot): Restored poke normal response
- Provided more detailed debug configuration

## [0.11.1] - 2025-11-4
### Feature Changes and Fixes
- Memories can now be forgotten, and have better merging
- Fixed some llm request issues
- Optimized memory extraction
- Provided detailed debug configuration for replyer

## [0.11.0] - 2025-10-27
### 🌟 Main Feature Changes
- Refactored memory system, new memory system more reliable, dual-channel query, can query text memories and past chat records
- Active speech function, MaiMai will autonomously ask questions (can finely control frequency)
- Supports multiple personality settings, can randomly switch to different states
- Added new expression method learning mode, less occupancy
- Added emoji management plugin
- Now better supports multiple platforms
- Added deepthink plugin (default off), allowing MaiMai to deeply think about some problems
- Now has built-in BetterFrequency plugin

### Detailed Feature Changes
- Fixed configuration file escape issues
- Emotion system can now be controlled by configuration file switch
- Fixed parallel action control failure issue
- Added planner debounce, preventing rapid token consumption in short time
- Optimized planner historical state recording
- Fixed character swallowing issue
- Fixed unexpected line break issue
- Removed VLM token limit
- Added chat_id field to tool tools
- Updated dependency table
- Fixed load balancing
- Now statistics model name instead of model identifier
- Modified default recommended model to ds v3.2
- Optimized support for gemini and different models, optimized support for gemini search