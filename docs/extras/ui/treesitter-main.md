# `Treesitter-main`

<!-- plugins:start -->

:::info
You can enable the extra with the `:LazyExtras` command.
Plugins marked as optional will only be configured if they are installed.
:::

Below you can find a list of included plugins and their default settings.

:::caution
You don't need to copy the default settings to your config.
They are only shown here for reference.
:::

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

## [nvim-treesitter-textobjects](https://github.com/nvim-treesitter/nvim-treesitter-textobjects)

<Tabs>

<TabItem value="opts" label="Options">

```lua
opts = {}
```

</TabItem>


<TabItem value="code" label="Full Spec">

```lua
{
  "nvim-treesitter/nvim-treesitter-textobjects",
  branch = "main",
  event = "VeryLazy",
  opts = {},
  keys = function()
    local moves = {
      goto_next_start = { ["]f"] = "@function.outer", ["]c"] = "@class.outer", ["]a"] = "@parameter.inner" },
      goto_next_end = { ["]F"] = "@function.outer", ["]C"] = "@class.outer", ["]A"] = "@parameter.inner" },
      goto_previous_start = { ["[f"] = "@function.outer", ["[c"] = "@class.outer", ["[a"] = "@parameter.inner" },
      goto_previous_end = { ["[F"] = "@function.outer", ["[C"] = "@class.outer", ["[A"] = "@parameter.inner" },
    }
    local ret = {} ---@type LazyKeysSpec[]
    for method, keymaps in pairs(moves) do
      for key, query in pairs(keymaps) do
        local desc = query:gsub("@", ""):gsub("%..*", "")
        desc = desc:sub(1, 1):upper() .. desc:sub(2)
        desc = (key:sub(1, 1) == "[" and "Prev " or "Next ") .. desc
        desc = desc .. (key:sub(2, 2) == key:sub(2, 2):upper() and " End" or " Start")
        ret[#ret + 1] = {
          key,
          function()
            require("nvim-treesitter-textobjects.move")[method](query, "textobjects")
          end,
          desc = desc,
          mode = { "n", "x", "o" },
          silent = true,
        }
      end
    end
    return ret
  end,
  config = function(_, opts)
    require("nvim-treesitter-textobjects").setup(opts)
  end,
}
```

</TabItem>

</Tabs>

<!-- plugins:end -->
