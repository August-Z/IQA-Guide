## 指令名 APC
**APC 全称 `Application Programming Command`，是轻文演绘面向开发者暴露的一系列脚本指令。**  
**<font color=#ea6a6e>开发者可根据自身实际开发需要，渐进使用指令丰富演绘演出，详见[指令大全](../指令大全/README.md)</font>**
```ruby
; 例：在下方脚本中，按顺序分别声明调用的 APC 为 avg.create、play、flag
@avg.create ROLE_1
@play BGM1 url=轻文主题曲
@flag LINK_1
```
<font color=gray>Tips: avg.create 可能看上去和其他指令有点不同... 它实则上是 avg 模块中的 create 指令，因为一些历史研发遗留原因，我们目前仅能对 v3 新指令采用模块化声明，在之后的版本中我们会陆续将其他 APC 陆续模块化。</font>


## 指令对象 Target
**<font color=#ea6a6e>Target 是对 APC 的对象（目标）选取声明，告知 IQA 引擎该指令对谁生效。</font>**  
**目前并非支持所有指令，比如对于书写指令、少许 v2 指令仍然不适用，具体请确认各指令的具体说明。**
```ruby
; 我们沿着上方的例子来看，下方脚本中，按顺序分别声明的 Target 为：
; ROLE_1 => avg.create
; BGM1 => play
; LINK_1 => flag

@avg.create ROLE_1
@play BGM1 url=轻文主题曲
@flag LINK_1
```
