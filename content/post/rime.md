+++
title = "Rime"
author = ["groscame"]
draft = false
+++

## 优缺点 {#优缺点}

[聰明的輸入法懂我心意 ](https://rime.im/) ，Rime 是一个强大的输入法工具，网上有很多关于它好处的介绍，我不想拾人牙慧[^fn:1]，这里就说说我为什么选择它。


### 优点： {#优点}

1.  Rime 是开源软件，不存在隐私泄露问题。拿最近非常热门的 ChatGPT 为例，你也不希望自己的对话记录被第三方非法获取和采集吧。现在大数据这么强，更应该随时提防自己的数据是否被滥用，防止中间人攻击。与此同时，每次与 ChatGPT 交互的问题都可以记录在你自己的词库中，打造属于你个人的 AI 体验。
2.  Rime 支持 Mac、Windows、Android 和 iPad 平台，并且这些平台上的软件都是免费的。
3.  Rime 支持 Lua 脚本语言，这意味着它拥有近乎无限的可能。
4.  作为一个开源软件，Rime 借助强大的开源生态系统，GitHub 上有许多可用的词库供使用，例如我使用的是收藏 star 数最多的词库<https://github.com/iDvel/ice>。


### 缺点： {#缺点}

1.  Rime 在不同平台之间的同步存在问题，尤其是在 iPad 和安卓平台上没有同步权限，导致同步是一个很痛苦的问题。
2.  在 Windows 平台上，使用软件包管理器 Scoop 安装 Rime 会遇到一些问题。我重装了至少 5 次仍然没有成功，这让我差点放弃。此外，使用软件包管理器更新软件也存在延迟的问题，推荐官网安装使用。
3.  Rime 的配置相对复杂，即使你不使用 Lua 脚本，配置 YAML 格式也可能花费较长的时间。此外，如果你以为配置成功了，实际上可能是上次遗留的文件在起作用，而且失败时也没有及时的错误提示，你需要自己检查对应的文件夹来排查问题。


## 起因 {#起因}

```text
如果允许人类自由迁徙，那人流的方向，就是文明的方向。
```

当我使用 iPad 时，我发现 iPad 定义的文本替换(Text Replacement)无法导出，无法导出对我来说可是个很大的问题，无法导出文本替换意味着我不仅要从零开始手动添加每一个缩写，修改、删除操作也要一个一个地弄。
苹果以封闭的生态环境著称，所以我不觉得它会突然大发慈悲开放此功能。更重要的是,一旦开放可能随时又取消,到时候该如何是好？
因此,我下定决心找一款全平台通用的输入法解决此问题。

> 双拼是我最近半年学习的最有用的东西了，世界上有一些基础技能是只要你投入一定固定时间，一次学会然后就可终身受益，比如学习一门外语，比如驾驶，比如练习打字速度，比如学习双拼。把时间花在这些事情上面是最有价值的投资。

后来，当我在互联网上寻寻觅觅时，偶然间在知乎上看到了大佬的这句话，我大受震撼，于是开始学习双拼。

那是在 2022 年 11、12 月，关于要不要解封讨论得很热烈。同时，我开始研究双拼输入法。最后我提前在 12 月中旬回家过寒假，第一次把寒假过成暑假。
当然最重要的收获不是这个，而是我在在家上网课期间，一遍听高电压老师讲课，一遍使用小鹤双拼将老师说的话打下来，就这样我成功掌握了小鹤双拼输入法。

随后搭配 Rime 输入引擎，双拼输入给了我前所未有的便利。它可以说是我在疫情期间获得的最大收获。

现在提一下双拼的好处，我没用过五笔输入， [但是五笔大佬是这样说的：](https://github.com/yanhuacuo/98wubi-tables/wiki/%E8%84%B1%E7%A6%BB%E5%80%99%E9%80%89%E6%A1%86#%E5%88%9B%E4%BD%9C)

<div class="verse">

五笔的实际使用等价于书写，与铺纸书写一样，仅当你遇到不会写的字时，才会停顿，在此之前，行云流水，心手合一。<br />
所以，五笔的优势不仅仅是速度，更是因为它是形码――是触摸着汉字风骨的文字编码方案。它的低重效率和优雅风度是从原理层面取胜的，当你走到这一步时，你基本已经可以感受到「形码」的韵味了。<br />
接下来，应当多进行文字创作，因为形码用户的遣词习性，与拼音用户是有区别的，不要辜负了这身道行。<br />
当我们用形码写汉字时，那是真的在书写，不依赖输入法去识别的你我的话语，每一枚字，都有着唯一的编码映射，确定、真实、高效，优雅――我欢欣于像匠人一样，一刀刀刻出我的句子，这是我对汉字的感情。<br />
那些长期依赖拼音输入法的人，不管他是否承认，他的大脑里都是存在着「词库依赖」的，即所谓「一脑袋口水词」。<br />
好吧，看到这里，你已经跟我一样有品味了。<br />

</div>

说真的看完这段话，我是真的感到了一股文化传承的魅力，只能说不愧是用五笔的大佬。
我使用自己的 Rime 词库，可以一定程度的缓解"一脑袋口水词"的症状，尤其是 `坤` 和 `🐔` 。
最重要是它能免费提高你与 ChatGPT 对话的速度。


## Rime 和双拼 {#rime-和双拼}

使用输入法来管理一些临时需要查阅的信息确实非常方便。

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

举个例子，当我学习符号计算软件 maxima&nbsp;[^fn:2] 时，有些函数虽然我可能已经看过，但记不太清楚。
编程语言的函数命名往往非常相似，差一个字符就可能导致错误。因此，我将这些函数添加到我的输入法中。当我需要临时查看它们的使用方法时，只需输入 ~/maxima~，就能立即查看对应的代码。
对于文件路径，您可能会提到使用 zoxide 进行快速跳转，或者在命令行中有历史记录可供查阅。
然而，对于像邮箱、电话号码等相对密码来说安全性不高，但也非常常用的信息，你怎么办呢。

这时借助开源输入法 Rime 的全平台同步功能。可以隐秘地将这些信息存储在输入法中，任何设备都可随时取用，而不会泄露隐私。

而且这还是保存在一个文本 YAML 文件里面，无论是 git 同步，还是修改都非常方便，另外如果我问了一次人工智能，就可以把它放在这个文件里面，需要的时候直接在输入法里面找，毫不夸张的说，这是在打造属于自己的一款词库，里面住着独属于你的 AI。


## Rime 和 Lua {#rime-和-lua}

[Lua 简单例子](https://github.com/baopaau/rime-lua-collection/blob/master/calculator_translator.lua)

举个例子，在前辈们的肩膀上，可以轻松自定义所需函数。拿我了解的 Java 练习题来说，比如要实现一个 **根据具体日期计算星期** 的 Zeller 算法，可以无缝地将其插入到这个 Lua 文件中。你只需了解 Lua 文件的基本结构，甚至不需要了解表（table）的概念，只需进行字符串匹配即可。剩下的部分就交给 AI 来帮你实现具体的算法，完善余下的代码。

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


## 进阶 Lua {#进阶-lua}

[你的输入法何必是输入法](https://github.com/hchunhui/librime-lua/wiki/Scripting#%E8%84%9A%E6%9C%AC%E5%BC%80%E5%8F%91%E6%8C%87%E5%8D%97)
刚才提到的 maxima 可能只有一个实例，但如果涉及多个实例，就会引发新的问题。例如，我在 snippets yaml 文件代码片段中使用 python 进行记录，但可能会忘记，并最终使用 "py" 进行搜索，误以为自己没有记录与 Python 相关的内容。

最直白的解决方案是使用

```yaml
'/help': [ '/http', '/git', '/fd', '/rg', '/ffmepg', '/find', '/awk', '/maxima', '/python', '/js', '/mail', '/ab'  ]
```

可是这样一来，每次要先看一遍 help 然后在去找 python，这强大的心智负担绝不是什么长久之计。
在强大的人工智能的帮助下，我实现了使用 Lua 来帮我自动格式化输出，我实现了使用 Lua 来自动格式化输出。不过，我在尝试使用相对路径时遇到了问题，而在 iPad 上即使使用绝对路径，也可能由于权限问题而失败。

{{< figure src="/ox-hugo/python.gif" >}}

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

最后总结一下，双拼加快了我与 AI 对话的能力，Rime 让我能够记录与 AI 的对话，Lua 使我自定义我想要的功能。
到这一步，Rime 已经有一点像编辑器了。如果你对使用 Rime 感兴趣并且有充足的自由时间，我推荐选择 star 数最多的 Git 仓库开始，并务必查看相关网站，这并非一条轻松的道路。

[^fn:1]: 比喻拾取别人的一言半语当作自己的话。
[^fn:2]: 不推荐使用，除非你没的选。