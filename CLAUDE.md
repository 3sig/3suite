# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

3suite is a collection of modular creative technology tools designed to work together. Each component is a self-contained Node.js application using the 3lib-config library for consistent configuration management.

## Architecture

The repository contains multiple independent modules:

- **3lib-config**: Configuration management library with file watching and live reload
- **3lib-orchestrator**: Library for downloading and orchestrating 3suite utilities at runtime/buildtime
- **3suite-orchestrator**: Process manager that spawns and monitors other 3suite components
- **3suite-browser**: Electron-based browser application for creative displays
- **3suite-http-server**: Static file server component with optional text replacement at runtime
- **3suite-socketio**: Socket.IO server for real-time communication
- **3suite-mock-server**: Development mock server
- **3suite-network-multiplexer**: Network routing and multiplexing tool
- **3suite-asset-cache**: Asset caching and management system
- **3suite-template**: Template for creating new 3suite components
- **3suite-orchestrator-project-template**: Template for using 3lib-orchestrator

## Configuration System

All components use 3lib-config for configuration:
- Configuration files are typically `config.json5` (JSON5 format)
- Supports nested configuration with `/` path syntax (e.g., `config.get("parent/child")`)
- File watching with automatic reloading
- Command line config override: `executable config_encoded_as_uri`
- Config file flag: `executable -f config_file.json5`

## Build and Release

Each component uses GitHub Actions for automated building:
- Workflow: `.github/workflows/build-on-push_release-if-tagged.yml`
- Builds on every push, creates releases only for tags
- Supports macOS ARM64 and Windows x64 platforms
- For Electron apps (3suite-browser): `npm run make` builds executables
- Release process: Create annotated git tag, push tag to trigger release

## Common Development Tasks

### Running/developing Components
Most components can be run with: `bun main.js`. Most components use bun for development.
Bun generally is the same as node in terms of its commands.

Components are usually forked from 3suite-template. This fork is managed by GitHub. When changes to 3suite-template are made, they need to be merged into the forked repository.

We do this by creating a pull request from the forked repository to the original repository, and then creating a merge commit.

### 3suite-browser Development
For 3suite-browser, we use npm for development, as bun and pnpm are not compatible with Electron apps.
- Development: `npm run start -- -- -- -f config2.json5`
- Production build: `npm run make`
- macOS execution: `open -n 3suite-browser.app --args -f config2.json5`

### Configuration Management
- Each component looks for `EXECUTABLE_NAME.json5` or `config.json5` in the same directory
- Live configuration reloading is supported through file watching
- Use 3lib-config API: `config.get("path/to/value", defaultValue)`

### Orchestrator Usage
3suite-orchestrator manages multiple processes:
- Configured via `processes` array in config.json5
- Can load configurations for child processes via `loadConfig` property
- Supports process-specific working directories via `cwd` property

## Dependencies

Primary dependencies across components:
- **3lib-config**: Configuration management (github:3sig/3lib-config)
- **golden-fleece**: JSON5 parsing
- **chokidar**: File watching
- **execa**: Process execution
- **electron**: For browser component
- **socket.io**: For real-time communication

## File Structure

Each component follows a consistent structure:
- `main.js`: Entry point
- `config.json5`: Default configuration
- `package.json`: Dependencies and metadata
- `README.md`: Component-specific documentation
