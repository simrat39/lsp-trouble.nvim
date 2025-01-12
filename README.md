
# 🚦 Trouble

A pretty list for showing diagnostics, references, telescope results, quickfix and location lists to help you solve all the trouble your code is causing.

![LSP Trouble Screenshot](./media/shot.png)

## ✨ Features

* pretty list of:
  - LSP Diagnostics
  - LSP references
  - quickfix list
  - location list
  - [Telescope](https://github.com/nvim-telescope/telescope.nvim) search results
* automatically updates on new diagnostics
* toggle **diagnostics** mode between **workspace** or **document**
* **interactive preview** in your last accessed window
* *cancel* preview or *jump* to the location
* configurable actions, signs, highlights,...

## ⚡️ Requirements

* Neovim >= 0.5.0
* Properly configured Neovim LSP client
* [nvim-web-devicons](https://github.com/kyazdani42/nvim-web-devicons) is optional to enable file icons
* a theme with properly configured highlight groups for Neovim LSP Diagnostics
* or install 🌈  [lsp-colors](https://github.com/folke/lsp-colors.nvim) to automatically create the missing highlight groups
* a [patched font](https://www.nerdfonts.com/) for the default severity and fold icons

## 📦 Installation

Install the plugin with your preferred package manager:

### [vim-plug](https://github.com/junegunn/vim-plug)

```vim
" Vim Script
Plug 'kyazdani42/nvim-web-devicons'
Plug 'folke/lsp-trouble.nvim'

lua << EOF
  require("trouble").setup {
    -- your configuration comes here
    -- or leave it empty to use the default settings
    -- refer to the configuration section below
  }
EOF
```

### [packer](https://github.com/wbthomason/packer.nvim)

```lua
-- Lua
use {
  "folke/lsp-trouble.nvim",
  requires = "kyazdani42/nvim-web-devicons",
  config = function()
    require("trouble").setup {
      -- your configuration comes here
      -- or leave it empty to use the default settings
      -- refer to the configuration section below
    }
  end
}
```

## ⚙️ Configuration

### Setup

Trouble comes with the following defaults:

```lua
{
    height = 10, -- height of the trouble list
    icons = true, -- use devicons for filenames
    mode = "lsp_workspace_diagnostics", -- "lsp_workspace_diagnostics", "lsp_document_diagnostics", "quickfix", "lsp_references", "loclist"
    fold_open = "", -- icon used for open folds
    fold_closed = "", -- icon used for closed folds
    action_keys = { -- key mappings for actions in the trouble list
        close = "q", -- close the list
        cancel = "<esc>", -- cancel the preview and get back to your last window / buffer / cursor
        refresh = "r", -- manually refresh
        jump = {"<cr>", "<tab>"}, -- jump to the diagnostic or open / close folds
        jump_close = {"o"}, -- jump to the diagnostic and close the list
        toggle_mode = "m", -- toggle between "workspace" and "document" diagnostics mode
        toggle_preview = "P", -- toggle auto_preview
        hover = "K", -- opens a small poup with the full multiline message
        preview = "p", -- preview the diagnostic location
        close_folds = {"zM", "zm"}, -- close all folds
        open_folds = {"zR", "zr"}, -- open all folds
        toggle_fold = {"zA", "za"}, -- toggle fold of current file
        previous = "k", -- preview item
        next = "j" -- next item
    },
    indent_lines = true, -- add an indent guide below the fold icons
    auto_open = false, -- automatically open the list when you have diagnostics
    auto_close = false, -- automatically close the list when you have no diagnostics
    auto_preview = true, -- automatyically preview the location of the diagnostic. <esc> to close preview and go back to last window
    auto_fold = false, -- automatically fold a file trouble list at creation
    signs = {
        -- icons / text used for a diagnostic
        error = "",
        warning = "",
        hint = "",
        information = "",
        other = "﫠"
    },
    use_lsp_diagnostic_signs = false -- enabling this will use the signs defined in your lsp client
}
```

> 💡 if you don't want to use icons or a patched font, you can use the settings below

```lua
-- settings without a patched font or icons
{
    fold_open = "v", -- icon used for open folds
    fold_closed = ">", -- icon used for closed folds
    indent_lines = false, -- add an indent guide below the fold icons
    signs = {
        -- icons / text used for a diagnostic
        error = "error",
        warning = "warn",
        hint = "hint",
        information = "info"
    },
    use_lsp_diagnostic_signs = false -- enabling this will use the signs defined in your lsp client
}
```

## 🚀 Usage

### Commands

Trouble comes with the following commands:

* `LspTrouble [provider]`: open the list
* `LspTroubleClose [provider]`: close the list
* `LspTroubleToggle [provider]`: toggle the list
* `LspTroubleRefresh`: manually refresh the active list

Example keybindings:

```vim
-- Vim Script
nnoremap <leader>xx <cmd>LspTroubleToggle<cr>
nnoremap <leader>xw <cmd>LspTroubleToggle lsp_workspace_diagnostics<cr>
nnoremap <leader>xd <cmd>LspTroubleToggle lsp_document_diagnostics<cr>
nnoremap <leader>xq <cmd>LspTroubleToggle quickfix<cr>
nnoremap <leader>xl <cmd>LspTroubleToggle loclist<cr>
nnoremap gR <cmd>LspTroubleToggle lsp_references<cr>
```

```lua
-- Lua
vim.api.nvim_set_keymap("n", "<leader>xx", "<cmd>LspTroubleToggle<cr>",
  {silent = true, noremap = true}
)
vim.api.nvim_set_keymap("n", "<leader>xw", "<cmd>LspTroubleToggle lsp_workspace_diagnostics<cr>",
  {silent = true, noremap = true}
)
vim.api.nvim_set_keymap("n", "<leader>xd", "<cmd>LspTroubleToggle lsp_document_diagnostics<cr>",
  {silent = true, noremap = true}
)
vim.api.nvim_set_keymap("n", "<leader>xl", "<cmd>LspTroubleToggle loclist<cr>",
  {silent = true, noremap = true}
)
vim.api.nvim_set_keymap("n", "<leader>xq", "<cmd>LspTroubleToggle quickfix<cr>",
  {silent = true, noremap = true}
)
vim.api.nvim_set_keymap("n", "gR", "<cmd>LspTrouble lsp_references<cr>",
  {silent = true, noremap = true}
)
```

### Telescope

You can easily open any search results in **Trouble**, by defining a custom action:

```lua
local actions = require("telescope.actions")
local trouble = require("trouble.providers.telescope")

local telescope = require("telescope")

telescope.setup {
  defaults = {
    mappings = {
      i = { ["<c-t>"] = trouble.open_with_trouble },
      n = { ["<c-t>"] = trouble.open_with_trouble },
    },
  },
}
```

When you open telescope, you can now hit `<c-t>` to open the results in **Trouble**

## 🎨 Colors

The table below shows all the highlight groups defined for LSP Trouble with their default link.

| Highlight Group             | Defaults to                      |
| --------------------------- | -------------------------------- |
| *LspTroubleCount*           | TabLineSel                       |
| *LspTroubleError*           | LspDiagnosticsDefaultError       |
| *LspTroubleNormal*          | Normal                           |
| *LspTroubleTextInformation* | LspTroubleText                   |
| *LspTroubleSignWarning*     | LspDiagnosticsSignWarning        |
| *LspTroubleLocation*        | LineNr                           |
| *LspTroubleWarning*         | LspDiagnosticsDefaultWarning     |
| *LspTroublePreview*         | Search                           |
| *LspTroubleTextError*       | LspTroubleText                   |
| *LspTroubleSignInformation* | LspDiagnosticsSignInformation    |
| *LspTroubleIndent*          | LineNr                           |
| *LspTroubleSource*          | Comment                          |
| *LspTroubleSignHint*        | LspDiagnosticsSignHint           |
| *LspTroubleSignOther*       | LspTroubleSignInformation        |
| *LspTroubleFoldIcon*        | CursorLineNr                     |
| *LspTroubleTextWarning*     | LspTroubleText                   |
| *LspTroubleCode*            | Comment                          |
| *LspTroubleInformation*     | LspDiagnosticsDefaultInformation |
| *LspTroubleSignError*       | LspDiagnosticsSignError          |
| *LspTroubleFile*            | Directory                        |
| *LspTroubleHint*            | LspDiagnosticsDefaultHint        |
| *LspTroubleTextHint*        | LspTroubleText                   |
| *LspTroubleText*            | Normal                           |
