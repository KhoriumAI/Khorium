# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with
code in this repository.

## Development Commands

### Installation & Setup

```bash
# Full installation (includes Vue components build)
./scripts/install.sh

# Manual installation
cd vue-components
npm i
npm run build
cd -
pip install uv
uv sync
```

### Running the Application

```bash
# Start the application
./scripts/start.sh
# Or manually:
source .venv/bin/activate
khorium --port 10000 --host 0.0.0.0

# Direct Python execution
python -m khorium.app
```

### Development Tools

```bash
# Linting (uses ruff)
nox -s lint
# Or directly: ruff check

# Testing
nox -s tests
# Or directly: pytest

# Build package
nox -s build

# Documentation build
nox -s docs
```

### Vue Components Development

```bash
cd vue-components
npm run dev      # Development server
npm run build    # Build components
npm run lint     # Lint Vue/JS files
```

## Architecture Overview

**Khorium** is a scientific visualization application built with the Trame
framework, combining Python backend with Vue.js frontend for interactive
VTK-based data visualization.

### Core Components

- **Main Entry Point**: `src/khorium/app/main.py` - Application bootstrapper
- **Trame App**: `src/khorium/app/app.py` - Main Trame application class with UI
  layout
- **VTK Pipeline**: `src/khorium/app/core/vtk_pipeline.py` - VTK rendering
  pipeline for mesh/contour visualization
- **Custom Widgets**: `src/khorium/widgets/khorium.py` - Custom Vue component
  integrations
- **Vue Components**: `vue-components/` - Frontend Vue.js components

### Key Architecture Patterns

1. **Trame Framework**: Uses `@TrameApp()` decorator pattern for the main
   application class
2. **VTK Integration**: Renders scientific data using VTK with remote/local view
   modes
3. **State Management**: Trame's reactive state system connects Python backend
   to Vue frontend
4. **Hot Reload**: File watching system for development UI updates
5. **Modular Components**: Separate Vue components build system that integrates
   with Python widgets

### Data Flow

- VTK pipeline loads `.vtu` files from `src/khorium/app/core/`
- Python controllers handle user interactions and state changes
- Vue.js frontend provides interactive UI components
- Trame bridges Python/JavaScript communication

### Development Notes

- The application currently has many commented-out UI components (drawer,
  controls) suggesting active development
- Uses `uv` for Python dependency management
- Vue components are built separately and integrated via the widgets system
- VTK data files (like `disk_out_ref.vtu`) are stored in the core module
  directory

## Project Structure

```
src/khorium/
   app/                    # Main Trame application
      main.py            # Entry point
      app.py             # Trame app with UI layout
      core/              # VTK pipeline and constants
      ui/components/     # UI component definitions
   widgets/               # Custom widget integrations
   module/                # Built Vue components

vue-components/            # Vue.js frontend components
   src/components/        # Vue component source
   dist/                  # Built components (generated)

scripts/                   # Installation and deployment scripts
bundles/                   # Desktop and Docker deployment configs
```
