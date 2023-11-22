+++
title = "doublecmd"
author = ["groscame"]
draft = false
+++

自从我发现了 Lua 的强大之处，我意识到许多我使用的软件都在使用 Lua 作为胶水语言，例如 MPV、Cheat Engine、Wireshark，还有大名鼎鼎的编辑器 Neovim。尽管我平时使用 doublecmd 和 Neovim，但我使用的是 Nvchad 配置，基本上没有使用 Lua，只是通过表（table）添加了一个自定义的 copilot 包。但是 doublecmd 不同，我几乎每天都在使用它，如果没有它，买了 Mac 的我几乎是要被 Mac 自带的 Finder 难用到气死。

<https://doublecmd.github.io/doc/en/index.html> 支持多种扩展，并且能完美支持 Lua 脚本。

不知道你是否遇到过在一个文件夹中交换两个文件的名称，明明在任何语言中，这就像交换变量名一样简单，但是我仍然：

1.  先复制一个文件（Ctrl+C）
2.  使用 F2 进行重命名
3.  逐个编辑文件名
4.  删除多余的文件

多年来，我使用一个临时文件来重复这个过程，直到我开始使用 doublecmd 的 Lua 脚本功能。

下面是修改过的 Lua 代码，用于在 doublecmd 中交换两个选中文件的名称：

```lua
-- %"0%ps1
-- %"0%ps2
-- 这是doublecmd接受的参数，详情请Read The Fucking Manual
local params = {...}
local file1 = params[1]:gsub("\\", "/")
local file2 = params[2]:gsub("\\", "/")
local file1_tmp = file1 .. '.tmp'
local file = io.open("D:/WINDOWS/TEMP/help.txt", "w")

os.rename(file1, file1_tmp)
os.rename(file2, file1)
os.rename(file1_tmp, file2)

file:write(file1 .. " TO " .. file1_tmp .. "成功✌️\n")
file:write(file2 .. " TO " .. file1 .. "成功✌️\n")
file:write(file1_tmp .. " TO " .. file2 .. "成功✌️\n")
file:close()
```

这段代码中比较困难的部分是将参数传递给 doublecmd。你也可以使用日志记录的技巧来记录 doublecmd 的 Lua 到底执行了什么命令。由于 doublecmd 是一个图形化软件，在使用过程中，除非按照官方文档自定义窗口，否则也像 Rime 那样不会有显示。因此，我使用 help 文件来记录执行了哪些命令。

另外，Lua 的路径问题自 Rime 以来就让我无从下手。如果你很自信并且不需要消息记录，那么这段代码其实 7 行就能搞定。

{{< figure src="/ox-hugo/doublecmd.png" >}}

再举个例子，借助 doublecmd 提供的 Clipboard 功能，你可以将剪贴板中的文本内容快速发送到 iPad 的 bark 上。尽管我知道有一些开源的局域网文件传输工具，比如 LocalSend，以及一些即时通讯工具如微信、QQ，但它们并不能提供直接点击一键发送的快捷方式。
另外，它们也各有自己的限制，localsend 要在同一个局域网，微信，QQ 要扫码。

此外，doublecmd 还提供了许多其他有用的功能，比如 Rename Tool、符号链接创建、内置 Shell 等。更详细的信息请参阅官方文档。

```lua
local folders = {
  'C:/Users/[你的用户名]/.emacs.d.spacemacs',
  'C:/Users/[你的用户名]/.emacs.d.centaur',
  'C:/Users/[你的用户名]/.emacs.d.purcell',
  'C:/Users/[你的用户名]/.emacs.d.redguardtoo',
  'C:/Users/[你的用户名]/rime-ice',
  'C:/Users/[你的用户名]/AppData/Local/nvim',
  'C:/Users/[你的用户名]/.SpaceVim',
  'C:/Users/[你的用户名]/.SpaceVim.d',
  'C:/Users/[你的用户名]/.emacs.d/emacs-application-framework',
  'D:/python/stable-diffusion-webui',
  'C:/Users/wacl/.emacs.d',
  'C:/Users/wacl/AppData/Local/nvim.nvchad/lua/custom',
  'D:/WINDOWS/TEMP/auto-add-routes',
  -- 添加更多文件夹路径
}
local file = io.open("D:/WINDOWS/TEMP/help.txt", "w")

-- 遍历文件夹列表
for _, folderPath in ipairs(folders) do
  -- 构建 Git 命令
  local gitCommand = "cd " .. folderPath .. " && git pull"

  local result = os.execute(gitCommand)

  -- 检查执行结果
  if result then
    file:write(gitCommand .. "成功✌️\n")
  else
    file:write(gitCommand .. "Git pull 失败：" .. folderPath)
  end
end
file:close()
```

)
  end
end
<close()>
\#+end_src

    <write>(gitCommand .. "成功✌️\n")
  else
    <write>(gitCommand .. "Git pull 失败：" .. folderPath)
  end
end
<close()>
\#+end_src
