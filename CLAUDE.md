# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a LazyVim-based Neovim configuration. LazyVim provides comprehensive defaults, and this configuration only adds specific customizations. The codebase follows LazyVim's minimal override pattern.

## Architecture

### Configuration Loading Order
1. `init.lua` - Entry point that bootstraps lazy.nvim
2. `lua/config/lazy.lua` - Initializes plugin manager and imports LazyVim base
3. `lua/config/options.lua`, `lua/config/keymaps.lua`, `lua/config/autocmds.lua` - Core Neovim settings
4. `lua/plugins/*.lua` - Auto-loaded plugin specifications (all .lua files in this directory)

### Plugin System
- **Manager**: lazy.nvim (auto-bootstraps on first run)
- **Pattern**: Plugin files return a Lua table array of plugin specs
- **Merging**: Plugin specs with same name are automatically merged with LazyVim defaults
- **Lockfile**: `lazy-lock.json` pins exact plugin versions

### Key Directories
- `lua/config/` - Core Neovim configuration (options, keymaps, autocmds)
- `lua/plugins/` - Plugin specifications (any .lua file here is auto-loaded)
- `.neoconf.json` - LSP workspace configuration

## Common Commands

### Plugin Management
```vim
:Lazy                  " Open lazy.nvim UI
:Lazy update           " Update all plugins
:Lazy sync             " Sync with lazy-lock.json
:Lazy clean            " Remove unused plugins
```

### LSP & Tools
```vim
:Mason                 " Manage LSP servers, formatters, linters
:LspInfo              " Show LSP client status
:ConformInfo          " Show formatter status (if conform.nvim is active)
```

### LazyVim Extras
```vim
:LazyExtras           " Browse and enable pre-configured language/tool bundles
```

## Adding Configurations

### Adding an LSP Server
Edit or create a file in `lua/plugins/`:
```lua
return {
  {
    "neovim/nvim-lspconfig",
    opts = {
      servers = {
        -- Add server name with optional config
        tsserver = {},
        rust_analyzer = {
          settings = {
            ["rust-analyzer"] = {
              cargo = { allFeatures = true },
            },
          },
        },
      },
    },
  },
}
```

### Adding Treesitter Parsers
```lua
return {
  {
    "nvim-treesitter/nvim-treesitter",
    opts = function(_, opts)
      vim.list_extend(opts.ensure_installed, {
        "rust",
        "go",
        "python",
      })
    end,
  },
}
```

### Adding Mason Tools
```lua
return {
  {
    "williamboman/mason.nvim",
    opts = {
      ensure_installed = {
        "prettier",
        "eslint_d",
        "black",
      },
    },
  },
}
```

### Adding/Configuring Plugins
```lua
return {
  -- Override existing LazyVim plugin
  {
    "folke/trouble.nvim",
    opts = { use_diagnostic_signs = true },
  },

  -- Add completely new plugin
  {
    "github/copilot.vim",
    event = "VeryLazy",
    config = function()
      -- Plugin setup
    end,
  },
}
```

## Current Configuration

### Configured Languages
- **OCaml**: LSP (ocamllsp)
- **TypeScript/TSX**: Treesitter parsers

### Installed Tools (via Mason)
- **stylua** - Lua formatter (config in `stylua.toml`)
- **shellcheck** - Shell script linter
- **shfmt** - Shell script formatter
- **flake8** - Python linter

### Lua Code Style
See `stylua.toml`:
- Indent: 2 spaces
- Column width: 120

## File Naming Conventions

- Plugin files: `lua/plugins/*.lua` (any name, all auto-loaded)
- Common pattern: Name files by category (e.g., `lsp.lua`, `editor.lua`, `ui.lua`)
- The existing `example.lua` can be renamed or split into multiple files

## Reference Documentation

- LazyVim docs: https://lazyvim.github.io
- Default options: https://github.com/LazyVim/LazyVim/blob/main/lua/lazyvim/config/options.lua
- Default keymaps: https://github.com/LazyVim/LazyVim/blob/main/lua/lazyvim/config/keymaps.lua
- Default autocmds: https://github.com/LazyVim/LazyVim/blob/main/lua/lazyvim/config/autocmds.lua
