---
title: "Comfy Guide To Neovim"
date: "2025-09-08"
tags:
    - guide
    - nvim
---

# Have you ever used Vim before?

If you haven't use it, you should check neovim distros rather than making your own.
A lot of my friend want to use vim because the "hype", And its not a bad thing.
But most of them gets bored after using them after 1 week. I know they just want
to flex their setup, and the bad thing is when they just focused on how to make
their setup better. And so I want to change that by this guide to basic usecase
of neovim without bloat(and with fancy UI stuff).

# Prerequisite
- Neovim
- Basic knowledge of lua

# The Steps
1. Just use NVChad for fancy UI stuff, most of the basic stuff is already builtin.
```bash
git clone https://github.com/NvChad/starter ~/.config/nvim && nvim
```
2. Open your nvim for necessary setup.
3. Then add these autocmds.lua

## Remove trailing whitespaces when save
```lua
autocmd("BufWritePre", {
  pattern = "*",
  callback = function()
    vim.cmd [[%s/\s\+$//e]]
  end
})
```

## Hightlight when copy/yank
```lua
autocmd("TextYankPost", {
  pattern = "*",
  callback = function ()
    vim.hl.on_yank()
  end,
})
```

## Auto make the directory when saving a file in non-existing dir
```lua
autocmd("BufWritePre", {
  callback = function (event)
    if event.match:match("^%w%w+:[\\/][\\/]") then
      return
    end
    local file = vim.uv.fs_realpath(event.match) or event.match
    vim.fn.mkdir(vim.fn.fnamemodify(file, ":p:h"), "p")
  end
})
```

## 'q' as the binding for quit for specific filename
```lua
autocmd("FileType", {
  pattern = {
    "PlenaryTestPopup",
    "checkhealth",
    "dbout",
    "gitsigns-blame",
    "help",
    "lspinfo",
    "notify",
    "qf",
    "spectre_panel",
    "startuptime",
    "tsplayground",
  },
  callback = function (event)
    vim.bo[event.buf].buflisted = false
    vim.schedule(function ()
      vim.keymap.set("n", "q", function ()
        vim.cmd("close")
        pcall(vim.api.nvim_buf_delete, event.buf, { force = true })
      end, {
          buffer = event.buf,
          silent = true,
          desc = "Quit buffer",
        })
    end)
  end
})
```

And so for our last config is to add some useful keybindings.

```lua
map("n", ";", ":", { desc = "CMD enter command mode" })

-- Switch windows
map("n", "<left>", "<c-w>h")
map("n", "<Right>", "<C-W>l")
map("n", "<Up>", "<C-W>k")
map("n", "<Down>", "<C-W>j")

-- Move current line up and down
map("n", "<A-j>", ":m .+1<CR>==", { desc = "Move line down" })
map("n", "<A-k>", ":m .-2<CR>==", { desc = "Move line up" })

-- Move current visual-line selection up and down
map("v", "<A-j>", ":m '>+1<CR>gv=gv", { desc = "Move selection down" })
map("v", "<A-k>", ":m '<-2<CR>gv=gv", { desc = "Move selection up" })
```

That's it. These are the basic of my config, make a changes to suit your taste.
