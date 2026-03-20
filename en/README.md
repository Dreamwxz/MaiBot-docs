# MaiBot Documentation

This repository is the official documentation for MaiBot. MaiBot is an intelligent chatbot designed for QQ groups, featuring LLM-based conversation capabilities, memory systems, and emotional expression functions.

## About the Documentation

This documentation site is built using [VitePress](https://vitepress.dev/), covering all content needed for installing, deploying, configuring, and developing (unfinished) MaiBot.

## Documentation Sections

### Documentation Directory

- **Installation Guide**
    Provides installation steps for standard and beginner-friendly versions, helping users get started quickly.

- **API Reference (Unfinished)**
    Detailed introduction to the API interfaces provided by MaiBot and their usage methods.

- **Deployment Methods**
    Covers various deployment methods including Docker, Linux, Windows, and Synology NAS.

- **Frequently Asked Questions and Troubleshooting**
    Collects common problems and their solutions, helping users troubleshoot and resolve issues.

- **File Structure and Configuration**
    Explains the project's file structure and configuration methods, facilitating user customization and extension.

## Local Development

### Local Deployment Requirements
Use Node.js to install corresponding dependencies

```bash
# Install dependencies
pnpm install

# Start development server
pnpm docs:dev

# Build production version
pnpm docs:build

# Preview production version
pnpm docs:preview
```

## Contribution

### Complete Method
If you want to modify the documentation, first fork the repository, make your changes, and then submit a PR.

If you want to add files, after forking, add your written documentation to the corresponding directory (or create a new directory), modify the index.md files at each level to include your documentation.

Then find the `config.mts` file in the `.vitepress` directory and modify the navigation to correctly point to your files.

After that, submit a PR.

### Lazy Method
If you want to modify the documentation, first fork the repository, make your changes, and then submit a PR.

If you want to add files, after forking, add your written documentation to the root directory, then submit a PR, specifying the directory location where you want it placed.

If the PR is approved, we will manually configure the various directories for you.