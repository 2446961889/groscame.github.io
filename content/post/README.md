<<<<<<< HEAD:content/post/README.md
+++
title = "学习git命令行，顺便建立个人网站"
author = ["wacl"]
draft = false
+++

## Lua 闷声发大财的胶水语言 {#lua-闷声发大财的胶水语言}


### rime {#rime}

> 双拼是我最近半年学习的最有用的东西了，世界上有一些基础技能是只要你投入一定固定时间，一次学会然后就可终身受益，比如学习一门外语，比如驾驶，比如练习打字速度，比如学习双拼。把时间花在这些事情上面是最有价值的投资。

因大佬的这句话，我开始学习双拼，在上网课期间双拼是我经历疫情后最大的收获，而 Rime 则放大了这个收获。
<https://github.com/baopaau/rime-lua-collection/blob/master/calculator_translator.lua>

举个例子，在前辈们的肩膀上，可以非常轻松地自定义需要的函数，例如我随便刷到的一个求具体日子对应的星期的 zeller 算法，就可以无缝的插入到这个文件里，只需要知道 Lua 文件的基本结构 table，甚至不需要知道 table 表，只用字符串匹配也可以，反正剩下的让 AI 来帮你实现具体的算法，补全剩下的代码。

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

例如，

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

可是这样一来，就会产生新的问题，比方说，我在 snippets 里面使用 python 记录，但是我可能忘了，最后使用 py 查找，就以为自己没有没有记录有关 python 的，
最直白的解决方案是使用

```yaml
'/help': [ '/http', '/git', '/fd', '/rg', '/ffmepg', '/find', '/awk', '/maxima', '/python', '/js', '/mail', '/ab'  ]
```

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

<div class="html">

<https://github.com/yanhuacuo/98wubi-tables/wiki/%E8%84%B1%E7%A6%BB%E5%80%99%E9%80%89%E6%A1%86#%E5%88%9B%E4%BD%9C>

</div>

<div class="verse">

五笔的实际使用等价于书写，与铺纸书写一样，仅当你遇到不会写的字时，才会停顿，在此之前，行云流水，心手合一。<br>
<br>
所以，五笔的优势不仅仅是速度，更是因为它是形码――是触摸着汉字风骨的文字编码方案。它的低重效率和优雅风度是从原理层面取胜的，当你走到这一步时，你基本已经可以感受到「形码」的韵味了。<br>
<br>
接下来，应当多进行文字创作，因为形码用户的遣词习性，与拼音用户是有区别的，不要辜负了这身道行。<br>
<br>
当我们用形码写汉字时，那是真的在书写，不依赖输入法去识别的你我的话语，每一枚字，都有着唯一的编码映射，确定、真实、高效，优雅――我欢欣于像匠人一样，一刀刀刻出我的句子，这是我对汉字的感情。<br>
<br>
那些长期依赖拼音输入法的人，不管他是否承认，他的大脑里都是存在着「词库依赖」的，即所谓「一脑袋口水词」。<br>
<br>
好吧，看到这里，你已经跟我一样有品味了。<br>

</div>
=======
#+TITLE: 学习git命令行，顺便建立个人网站
>>>>>>> f347888 (add lua.org):README.org
