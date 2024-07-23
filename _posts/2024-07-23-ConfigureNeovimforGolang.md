---
title: "Golang을 위한 Neovim 설정 방법"
description: ""
coverImage: "/assets/img/2024-07-23-ConfigureNeovimforGolang_0.png"
date: 2024-07-23 22:03
ogImage: 
  url: /assets/img/2024-07-23-ConfigureNeovimforGolang_0.png
tag: Tech
originalTitle: "Configure Neovim for Golang"
link: "https://medium.com/gitconnected/configure-neovim-for-golang-887fa8db61bd"
---


요즘 Go 쓰기에 관심이 생겨 Neovim 설정에서 Golang을 설정해야 했어요. 여기에 어떻게 설정했는지 알려드릴게요.

![이미지](/assets/img/2024-07-23-ConfigureNeovimforGolang_0.png)

저는 유지 및 계속 업데이트하는 개인 Neovim 설정이 있어요. 이 기사에서는 해당 설정에서 사용하는 플러그인을 이용할 거에요. 제 설정은 [여기](링크)에서 확인할 수 있어요.

Neovim이나 LSP 설정에 익숙하지 않은 경우, 몇 가지 개념에 익숙해지기 위해 이전 기사 몇 편을 읽어보시는 걸 추천합니다.

<div class="content-ad"></div>

일반적으로, 플러그인 설정에는 lazy.nvim을 사용하는 것이 좋습니다.

새 언어를 위해 Neovim을 구성하는 여러 측면이 있습니다.

- LSP 구성하기 (언어 서버 추가 및 연결)
- Treesitter 지원 추가 (문법 강조 및 다른 기능)
- 서식 지정 구성 (자동 저장 포함)
- 디버거 추가 및 구성

# LSP에 추가

<div class="content-ad"></div>

자, 언어 서버를 설치하고 설정해 봅시다.

## gopls 언어 서버 설치

저는 새로운 언어로 작업할 때 필요한 다양한 종속성을 설치하고 관리하는 mason.nvim을 사용합니다.

```js
{
    "williamboman/mason.nvim",
    dependencies = {
      "WhoIsSethDaniel/mason-tool-installer.nvim",
    },
    config = function()
      local mason = require("mason")
      local mason_tool_installer = require("mason-tool-installer")

      mason_tool_installer.setup({
        ensure_installed = {
          "gopls",
        },
      })
    end,
}
```

<div class="content-ad"></div>

## lsp-zero 설치 및 구성

나는 lsp zero를 사용하여 언어 서버를 내 Neovim 인스턴스에 연결합니다.

이 배포는 mason-lspconfig 및 nvim-lspconfig을 서로 통신하도록 설정하고 완성을 위해 nvim-cmp 및 스니펫 지원을 추가하는 등 다른 유용한 기능을 제공합니다.

내 키맵은 그대로 남겨 두었지만, 자유롭게 여러분에게 맞는 것으로 업데이트해 주세요.

<div class="content-ad"></div>

```json
{
  "VonHeikemen/lsp-zero.nvim",
  branch = "v2.x",
  dependencies = {
    -- LSP Support
    { "neovim/nvim-lspconfig" }, -- 필수
    { -- 선택적
      "williamboman/mason.nvim",
      build = function()
        pcall(vim.cmd, "MasonUpdate")
      end,
    },
    { "williamboman/mason-lspconfig.nvim" }, -- 선택적

    -- 자동 완성
    { "hrsh7th/nvim-cmp" }, -- 필수
    { "hrsh7th/cmp-nvim-lsp" }, -- 필수
    { "L3MON4D3/LuaSnip" }, -- 필수
    { "rafamadriz/friendly-snippets" },
    { "hrsh7th/cmp-buffer" },
    { "hrsh7th/cmp-path" },
    { "hrsh7th/cmp-cmdline" },
    { "saadparwaiz1/cmp_luasnip" },
  },
  config = function()
    local lsp = require("lsp-zero")

    lsp.on_attach(function(client, bufnr)
      local opts = { buffer = bufnr, remap = false }

      vim.keymap.set("n", "gr", function() vim.lsp.buf.references() end, vim.tbl_deep_extend("force", opts, { desc = "LSP 참조 이동" }))
      vim.keymap.set("n", "gd", function() vim.lsp.buf.definition() end, vim.tbl_deep_extend("force", opts, { desc = "LSP 정의 이동" }))
      vim.keymap.set("n", "K", function() vim.lsp.buf.hover() end, vim.tbl_deep_extend("force", opts, { desc = "LSP 툴팁" }))
      vim.keymap.set("n", "<leader>vws", function() vim.lsp.buf.workspace_symbol() end, vim.tbl_deep_extend("force", opts, { desc = "LSP 워크스페이스 심볼" }))
      vim.keymap.set("n", "<leader>vd", function() vim.diagnostic.setloclist() end, vim.tbl_deep_extend("force", opts, { desc = "LSP 오류 표시" }))
      vim.keymap.set("n", "[d", function() vim.diagnostic.goto_next() end, vim.tbl_deep_extend("force", opts, { desc = "다음 오류" }))
      vim.keymap.set("n", "]d", function() vim.diagnostic.goto_prev() end, vim.tbl_deep_extend("force", opts, { desc = "이전 오류" }))
      vim.keymap.set("n", "<leader>vca", function() vim.lsp.buf.code_action() end, vim.tbl_deep_extend("force", opts, { desc = "LSP 코드 수정" }))
      vim.keymap.set("n", "<leader>vrr", function() vim.lsp.buf.references() end, vim.tbl_deep_extend("force", opts, { desc = "LSP 참조" }))
      vim.keymap.set("n", "<leader>vrn", function() vim.lsp.buf.rename() end, vim.tbl_deep_extend("force", opts, { desc = "LSP 이름 변경" }))
      vim.keymap.set("i", "<C-h>", function() vim.lsp.buf.signature_help() end, vim.tbl_deep_extend("force", opts, { desc = "LSP 서명 도움말" }))
    end)

    require("mason").setup({})
    require("mason-lspconfig").setup({
      ensure_installed = {
        "gopls",
      },
      handlers = {
        lsp.default_setup,
        lua_ls = function()
          local lua_opts = lsp.nvim_lua_ls()
          require("lspconfig").lua_ls.setup(lua_opts)
        end,
      },
    })
  end,
}
```

# Treesitter 문법 추가

문법 표시를 활성화하고 다른 플러그인에서 사용하는 트리시터 구문 트리를 지원하기 위해 go에 대한 문법을 설치하세요.

```json
{
    "nvim-treesitter/nvim-treesitter",
    build = ":TSUpdate",
    config = function()
      local configs = require("nvim-treesitter.configs")

      configs.setup({
        ensure_installed = {
          "go",
        },
      })
    end,
}
```

<div class="content-ad"></div>

# Conform.nvim으로 서식 추가하기

이전에는 null-ls를 사용했지만 해당 플러그인이 보관되었고, 이제는 conform.nvim을 사용하여 서식을 설정하는 것을 선호합니다.

제가 개인적으로 서식을 트리거할 때 키를 누를 때 좋아하는데요, 만약 자동 저장을 선호한다면 conform.setup 내에 이 부분을 추가하세요.

```js
format_on_save = {
  - 이 옵션들은 conform.format()로 전달됩니다.
  timeout_ms = 500,
  lsp_format = "fallback",
},
```

<div class="content-ad"></div>

이제 conform.nvim을 설치하고 구성하고 Go 파일에 "gofmt"를 설정하는 플러그인 블록을 제공해 드렸습니다.

```js
{
  "stevearc/conform.nvim",
  event = { "BufReadPre", "BufNewFile" },
  config = function()
    local conform = require("conform")

    conform.setup({
      formatters_by_ft = {
        go = { { "gofmt" } },
      },
    })

    vim.keymap.set({ "n", "v" }, "<leader>l", function()
      conform.format({
        lsp_fallback = true,
        async = false,
        timeout_ms = 1000,
      })
    end, { desc = "파일 또는 범위(시각적 모드에서)를 형식화합니다." })
  end,
}
```

이제 Neovim을 다시 시작하고 Go 파일을 열어서 자동 완성, 서식 지정 및 구문 강조가 예상대로 작동하는 것을 확인할 수 있어야 합니다.

<img src="https://miro.medium.com/v2/resize:fit:400/1*-k4bwIw8oPq_545ksQ0kHw.gif" />

<div class="content-ad"></div>

# 디버거 설치 및 구성하기

이제 DAP(Debug Adapter Protocol)를 기반으로 하는 필수 디버거를 설정해보겠습니다.

## Delve 및 nvim-dap-go 설치하기

저는 Mason을 사용하여 delve를 설치했습니다. 아직 많이 사용해보지는 않았지만 좋은 평가를 들었습니다. 더 나은 디버거가 있다고 판단할 경우 이 글을 업데이트하겠습니다.

<div class="content-ad"></div>

다른 디버거를 사용하고 Delve보다 선호하는 경우 댓글로 알려주세요.

다시 mason.nvim을 사용하여 delve를 설치하세요.

```js
{
    "williamboman/mason.nvim",
    dependencies = {
      "WhoIsSethDaniel/mason-tool-installer.nvim",
    },
    config = function()
      local mason = require("mason")
      local mason_tool_installer = require("mason-tool-installer")

      mason_tool_installer.setup({
        ensure_installed = {
          "gopls",
          "delve",
        },
      })
    end,
  }
```

이제 nvim-dap을 설치하고 구성하여 특정 언어에 맞는 dap을 연결하세요. 저는 nvim-dap-go를 사용합니다.

<div class="content-ad"></div>

```js
{
    "mfussenegger/nvim-dap",
    dependencies = {
      {
        "rcarriga/nvim-dap-ui",
        "nvim-neotest/nvim-nio",
        config = function(_, opts)
          local dap = require("dap")
          local dapui = require("dapui")
          dapui.setup(opts)
          dap.listeners.after.event_initialized["dapui_config"] = function()
            dapui.open({})
          end
          dap.listeners.before.event_terminated["dapui_config"] = function()
            dapui.close({})
          end
          dap.listeners.before.event_exited["dapui_config"] = function()
            dapui.close({})
          end
        end,
      },
      {
        "leoluz/nvim-dap-go",
        config = function()
          require("dap-go").setup()
        end,
      },
      {
        "jay-babu/mason-nvim-dap.nvim",
        dependencies = "mason.nvim",
        cmd = { "DapInstall", "DapUninstall" },
        opts = {
          automatic_installation = true,
          handlers = {},
          ensure_installed = {
            "delve"
          },
        },
      },
    },
  }
```

이제 Go 파일로 들어가서 중단점을 설정하고 디버그하실 수 있어요!

```js
:lua require('dap-go').debug_test()
```

# 대안들

<div class="content-ad"></div>

만약 여러 조각이 아닌 하나의 구성을 선호한다면 vim-go를 확인해보세요.

개인적으로 저는 각 부분을 구성하는 것만큼 확장 가능하지 않다고 생각하지만, 어떤 사람들은 이 방법을 선호합니다.

# 기타 자료

만약 비디오 형식의 안내를 선호한다면 아래 비디오를 확인해보세요.

<div class="content-ad"></div>

위주제를 즐기시는 경우 저의 유튜브 채널도 마음에 드실 것 같아요. 좋은 하루 되세요!