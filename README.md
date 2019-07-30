# IQA-Engine V3.1 指令指南 

轻文演绘引擎上层解析用户编写的游戏脚本，底层使用 JavaScript 实现。  
您可以通过一段简易的指令与若干素材即可快捷生成动态演绘。
******

## 版本更新（老用户必看）
### 新增运行符功能
A. 得益于 **v3.0 avg 模块的优秀设计**，我们发现同步动画具有高可用性，于是这个功能被抽离成了运行符，供开发者自主控制。  
运行符主要含有：**同步符号 `#`、异步符 `@` **两种模式，适用于目前所有指令。
  
>如下方可保证图层成功加载图片后、再继续加载音频（此处音频因为是异步加载，故不论加载进度是否完成，演出都将继续向下）  
大部分情况演出时效果为：背景图 iqing 加载显示完成，而后看到“Hello IQA-Engine V3”此条文本的逐渐显示，最后听到 BGM1 的淡入播放，因为不论图层与音频，都需要动态加载资源，这是一个**耗时操作**。

```ruby
@avg.create BG1 
#avg.set BG1 url=iqing
@play BGM1 url=pv time=500
Hello IQA-Eninge V3[r][p]
```   
>这里举一个使图层进行“无限抖动”的栗子🌰：  
a. 图层运动成 20% 不透明度(耗时 800ms)；  
b. 图层 x 在 0～5 之间进行运动，次数无限(每次耗时 33ms)。  
这段脚本的运行结果会是（1～3）：  
1. 图层加载资源成功后，设置坐标且完全透明；
2. 同时运行 (a) 与 (b)，对话框开始显示文字“对话框显示出来咯～”；  
3. 图层待 (a) 运行完后，图层运动成 100% 不透明度(耗时 1500ms)。  

```ruby
@avg.create FG5
#avg.set FG5 url=对话框 x=-30 y=0 alpha=0
@avg.animation FG5 alpha=0.2 time=800
@avg.animation FG5 x=0 chain alpha=1 time=1500
@avg.animation FG5 x=5 yoyo repeat=Infinity time=33
对话框显示出来咯～[p]
```

### AVG 指令模块修改
A. `avg.animation` 不再是默认同步运行
```ruby 
; v3.0 的动画如不指定 async 标签则会同步运行
@avg.animation BG1 x=100 time=1000  

; 同步运行，即 BG2 图层 x 值向 100 进行运动，耗时 1000ms 后，脚本可继续向下运行
#avg.animation BG2 x=100 time=1000

; 异步运行，不论该行指令是否运行成功、或运行完成，脚本都将继续向下运行
@avg.animation BG2 x=100 time=1000
```  
B. 图层属性名有小部分调整（详见各指令）  
C. v2 的图层指令被废弃 (如 in、action、motion)
