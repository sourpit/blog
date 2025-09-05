---
title: "How to load json snippets on NVChad"
date: "2025-09-05"
tags: guide
---

In this guide I will be using meson as example. You can replace meson
with any language you want. First make these json files in your nvim
config folder.

`~/.config/nvim/lua/snippets/package.json`
```json
{
  "name": "custom_snippets",
  "engines": {
    "vscode": "^1.11.0"
  },
  "contributes": {
    "snippets": [
      {
        "language": "meson",
        "path": "./meson.json"
      }
    ]
  }
}
```

`~/.config/nvim/lua/snippets/meson.json`
```json
{
  "new project": {
    "prefix": ["nu"],
    "body": [
      "project('${1}', '${2}'",
      "        default_options: [])"
    ],
    "description": "Create a new project"
  }
}
```

Then add this line to options.lua
```lua
vim.g.vscode_snippets_path = vim.fn.stdpath "config" .. "/lua/snippets"
```

finally, check if the snippets loaded with `:LuaSnipListAvailable` and
you are done. From here you can try to make big list of snippets with your
own brain.

---
References:
- https://www.reddit.com/r/neovim/comments/18wgdyu/nvchad_luasnips_how_can_i_get_my_custom_latexjson/
