+++
title = "Lua 闷声发大财的胶水语言"
author = ["wacl"]
draft = false
+++

## [Rime](https://rime.im/) 聰明的輸入法懂我心意 {#rime-聰明的輸入法懂我心意}


### 好处 {#好处}

网上有很多关于它的好处，这里我说说我为什么选择它

1.  开源，没有隐私泄露问题，拿最近很火的人工智能来说，你也不想自己问 ChatGPT 的问题经过几个中间人的倒卖吧
2.  Mac, Windows, Android, iPad 都有免费的软件
3.  支持脚本语言 Lua，几乎拥有无限可能
4.  因为是开源生态，GitHub 有许多词库，任意选择

，比方我使用的就是 GitHub 上 star 最多的<https://github.com/iDvel/ice>


### 缺点 {#缺点}

1.  半全平台同步，Mac 和 Windows 还好，关键是 iPad 和安卓，两个都没有权限，同步是个非常大的问题
2.  Windows 下的软件管理包 scoop 有点问题，我下了 5 次都没成功，差点放弃，另外使用包管理器有更新延迟
3.  配置过于复杂，就算你不使用 Lua，yaml 格式当时也让我折腾了很久，另外，你以为成功配置好了，其实是上次的遗留文件在起作用，失败也没有消息提醒，你要自己去对应的文件夹里看


### 起因 {#起因}

当我使用 iPad 的时候，我发现 iPad 定义的缩写(Text Replacement)无法导出，要知道无法导出可是个很大的问题，
我不仅要从零开始一个个的手动添加，修改、删除操作也要，
苹果向来以封闭生态著称，我总不能指望有一天它大发慈悲的开启这个功能吧，万一又取消了怎么办，还是说可以在 Mac 端有什么奇技淫巧，虽然最后我还是买了 Mac，
但当时如此恶心的强买政策让我下定了决心找一款全平台通用的输入法。

> 双拼是我最近半年学习的最有用的东西了，世界上有一些基础技能是只要你投入一定固定时间，一次学会然后就可终身受益，比如学习一门外语，比如驾驶，比如练习打字速度，比如学习双拼。把时间花在这些事情上面是最有价值的投资。

当我在互联网上寻寻觅觅时，偶然间在知乎上看到了大佬的这句话，我大受震撼，于是开始学习双拼。
那是在 2022 年 11、12 月，关于要不要解封吵得很热闹。
最后的结果是我提前在 12 月中旬放假回家，第一次把寒假过成了暑假。
当然最大的收获不是这个，而是我在在家上网课期间，一遍听高电压老师讲课，一遍使用小鹤双拼将老师说的话打下来，就这样我学会了双拼，
完全可以说双拼是我经历疫情后最大的收获，而双拼搭配 Rime 更是如虎添翼。

现在提一下双拼的好处，我没用过五笔输入， [但是五笔大佬是这样说的：](https://github.com/yanhuacuo/98wubi-tables/wiki/%E8%84%B1%E7%A6%BB%E5%80%99%E9%80%89%E6%A1%86#%E5%88%9B%E4%BD%9C)

<div class="verse">

五笔的实际使用等价于书写，与铺纸书写一样，仅当你遇到不会写的字时，才会停顿，在此之前，行云流水，心手合一。<br>
所以，五笔的优势不仅仅是速度，更是因为它是形码――是触摸着汉字风骨的文字编码方案。它的低重效率和优雅风度是从原理层面取胜的，当你走到这一步时，你基本已经可以感受到「形码」的韵味了。<br>
接下来，应当多进行文字创作，因为形码用户的遣词习性，与拼音用户是有区别的，不要辜负了这身道行。<br>
当我们用形码写汉字时，那是真的在书写，不依赖输入法去识别的你我的话语，每一枚字，都有着唯一的编码映射，确定、真实、高效，优雅――我欢欣于像匠人一样，一刀刀刻出我的句子，这是我对汉字的感情。<br>
那些长期依赖拼音输入法的人，不管他是否承认，他的大脑里都是存在着「词库依赖」的，即所谓「一脑袋口水词」。<br>
好吧，看到这里，你已经跟我一样有品味了。<br>

</div>

看完这段话，我是真的感到了一股文化传承的精气神，只能说不愧是用五笔的大佬，在这个快节奏的时代，我还是强烈推荐使用双拼，据说速度次于五笔，但是比五笔更容易。

例如，
当我学习符号计算软件 maxima 时，虽然我可能看过了一些函数，但是记不住里面的一些函数，毕竟编程语言的函数命名太像了，而且差一个字符就报错，但是我把它放在了我的输入法里面，当我需要临时看看它的使用方法时，输入/maxima 就能及时查看对应代码，文件路径
你可能会说文件路径我使用 zoxide 快速跳转，代码也有历史记录，但是，邮箱，电话等等，都借助开源的力量在全平台隐秘同步。

{{< figure src="/ox-hugo/maxima.png" >}}

```yaml
'/maxima': [
'sum(1/x^2, x, 1, 10000);sum (1/2^i, i, 1, n), simpsum;让 Maxima 尝试化简',
'函数的定义和使用:f(x) := 3*x^2 + 5; f(2);f(x, y) := sin(x) − cos(2∗y);',
'`integrate(x^2, x)` 的积分，',
'kill(all) 更是把我们定义过的变数，函数全部删除。',
'瑕积分用到无穷大的部份 Maxima 是以 inf 表示:integrate(%e^(-x^2),x,0,inf); ',
'trigexpand 利用和差化积公式展开 sin2x sinx 的高阶',
'trigreduce 利用积化和差公式变成 sin 或 cos 的和 高转低不化简  ',
'trigsimp 利用 sin2(x) + cos2(x) = 1 等公式简化  ',
'trigrat 简化分数形式，分子分母为 sin 和 cos 的线性函数',
'm2: matrix ([6, 7], [3, 8]);entermatrix (n, n)',
'sqrt (x^2);  assume (x<0);  sqrt (x^2);',
'ratsimp 用来进行通分操作:',
'factor 进行因式分解:(%i9)',
'solve([%o6, %o7, %o8], [a, b, c]);',
'求 f 对 x 的不定积分:integrate (f, x);',
'定积分:integrate (1/x^2, x, 1, inf);',
'双曲正弦函数 sinh(k*x);',
'在 x=0 点展开为泰勒级数(高达 3 阶):taylor (g, x, 0, 3);',
'当 x 趋向于 0 时 g 的极限计算如下:limit (g, x, 0);',
'’diff(y, x);引号操作符表示(不求值)。',
'%, numer;',
'现在假定我们想把上面表达式中的 x 用 5/z 替代(%i3) %o2, x=5/z;',
"''%i1;",
'bfloat(1/3);函数来提供任意高的的精度',
'Maxima 的变量 fpprec 显示的有效数字的位数由控制，它的缺省值是 16:(%i6) fpprec;重置 fpprec 以产生 100 个有效数字:fpprec: 100;',
'powerseries(1/(1-x^2, x, 0));',
'taylor(sqrt(1-sin(x)), x, 0, 5);',
'M-x remove-hook RET inferior-maxima-mode-hook RET my-imaxima-hook RET',
'solve([x^2+y^2=1,(x-2)^2+(y-3)^2=9],[x,y]);'
]
```

而且这还是保存在一个文本 YAML 文件里面，无论是 git 同步，还是修改都非常方便。


### 使用 Lua ： {#使用-lua}

<https://github.com/hchunhui/librime-lua/wiki/Scripting#%E8%84%9A%E6%9C%AC%E5%BC%80%E5%8F%91%E6%8C%87%E5%8D%97> 真的是让你的输入法何必是输入法


### 例子 {#例子}

<https://github.com/baopaau/rime-lua-collection/blob/master/calculator_translator.lua>
举个 Lua 简单的例子，在前辈们的肩膀上，可以非常轻松地自定义需要的函数，用我看 Java 练习题：
一个求具体日子对应的星期的 zeller 算法，就可以无缝的插入到这个文件里，
只需要知道 Lua 文件的基本结构 table，甚至不需要知道 table 表，只用字符串匹配也可以，反正剩下的让 AI 来帮你实现具体的算法，补全剩下的代码。

```lua
zeller = function (datatable)
  local year = datatable[1]
  local month = datatable[2]
  local day = datatable[3]
  if month < 3 then
    month = month + 12
    year = year - 1
  end

  local century = math.floor(year / 100)
  local yearOfCentury = year % 100

  local h = (day + math.floor((13 * (month + 1)) / 5) + yearOfCentury +
             math.floor(yearOfCentury / 4) + math.floor(century / 4) - (2 * century)) % 7

  local days = {[0]="Saturday","Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", }
  return "The day of the week for " .. datatable[1] .. "-" .. datatable[2] .. "-" .. datatable[3] .. " is: " .. days[h]
end
```


### 进阶 Lua {#进阶-lua}

刚才的 maxima 如果只有一个，可是如果一多起来这样一来，就会产生新的问题，比方说，我在 snippets 里面使用 python 记录，但是我可能忘了，最后使用 py 查找，就以为自己没有没有记录有关 python 的，
最直白的解决方案是使用

```yaml
'/help': [ '/http', '/git', '/fd', '/rg', '/ffmepg', '/find', '/awk', '/maxima', '/python', '/js', '/mail', '/ab'  ]
```

{{< figure src="/ox-hugo/python.gif" >}}

可是这样一来，每次要先看一遍 help 然后在找 python，绝不是什么长久之计，
在强大的人工智能的帮助下，我实现了使用 Lua 来帮我自动格式化输出，
使用相对路径没有成功，另外，在 iPad 上就算使用绝对路径，好像因为权限问题反正也失败了

```lua
local file_path
-- [[你的绝对路径 --]]
local possible_paths = {
  "../snippets.yaml",
  "./snippets.yaml",
  "C:/Users/example/rime-ice/snippets.yaml",
  "/storage/emulated/0/rime/snippets.yaml",
  "/Users/example/Library/Rime/snippets.yaml",
  "/private/var/mobile/Library/Mobile Documents/iCloud~dev~fuxiao~app~hamsterapp/Documents/sync/hamster/snippets.yaml",
}

for _, path in ipairs(possible_paths) do
  if io.open(path, "r") then
    file_path = path
    break
  end
end

local file = assert(io.open(file_path, "r"))

if file then
  local file_content = file:read("*all")
  file:close()

  local pattern = "%s+'/([^']+)'"
  local matches = {}

  for line in file_content:gmatch("[^\r\n]+") do
    local match = line:match(pattern)
    if match then
      local replacement = line:match("[%[%]] #(.+)") or "未设置"
      if replacement == "未设置" then
        if matches[match] then
          matches[match] = matches[match] .. replacement .. match .. "重复成功"
        else
          matches[match] = match
        end
      else
        matches[match] = replacement .. match
      end
    end
  end

  local function generate_candidates(input)
    local candidates = {}
    for match, replacement in pairs(matches) do
      if match:sub(1, #input) == input then
        candidates[match] = replacement
      end
    end
    return candidates
  end

  local function mytranslator(input, seg)
    if input:sub(1, 1) == "/" then
      local candidates = generate_candidates(input:sub(2))
      for match, replacement in pairs(candidates) do
        local candidate = Candidate("snippets", seg.start, seg._end, match, replacement)
        candidate.quality = 100
        yield(candidate)
      end
    end
  end

  return mytranslator
else
  file:close()
  local function mytranslator(input, seg)
  end
  return mytranslator
end
```

如果看到这里，你想使用 rime，推荐使用 star 最多的那个 git 仓库，一定要去看那个网站，这不是一条轻易的路线。


## doublecmd {#doublecmd}

自从发现了 Lua 的强大，我发现自己使用的许软件都在使用 Lua 当作胶水语言，比方大名鼎鼎的 Neovim，我使用的是 Nvchard 配置，基本没用 Lua，只用 table 表添加了一个自己定义的 copilot 包，
但是这个 doublecmd 不一应，我几乎每天都在使用，如果不是它，买了 Mac 的我几乎是要被 Mac 自带的 Finder 难用到气死。
<https://doublecmd.github.io/doc/en/index.html> 也能完美的支持 Lua 脚本，

不知道你是否遇到过要在一个文件夹里面交换两个文件的名字，明明这在任何语言里面就如同交换变量名一样简单，但是我还是用一个 tmp 文件重复了多年，知道我使用了 doublecmd 的 lua，

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

这里我使用了一个小技巧，doublecmd 毕竟是图形软件，使用的时候除非你按官方文件自定义窗口，不然也没显示，所以我使用了文件来记录到底执行了哪些命令
如果你很自信，7行代码就能搞定。

{{< figure src="/ox-hugo/doublecmd.png" >}}

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

例如我使用它提供的 Clipboard，将自己的剪贴板里的文字 bark 到 iPad 上，我知道有 LocalSend 等开源局域网传递文件的工具，再不济还有微信，QQ，但是还是没有直接点击一键发送来的快，另外，它还提供了许多有用的好功能，比方说 Rename Tool，

这里比较难的就是传给 doublecmd 的参数是什么，也可以使用我刚才那个 log 记录的方式记录