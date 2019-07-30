# 小节
这里对几个栗子进行运行时分析：  

### Demo A
```sql
@dianame 乔
@play VO1 url=7-qiao-1027
「好啊！安雨姐有很多户外演唱会吧？我超想挑战那种的！」
@p
```
Runtime Translate:
```ruby
1.同步运行 -> 指令(dianame) <- 对象(乔)
2.*异步运行 -> 指令(play) <- 对象(VO1) <- 参数{url: 7-qiao-1027}
3.*异步运行 -> 引擎内部指令(print)，用于渲染文本 <- 「好啊！安雨姐有很多户外演唱会吧？我超想挑战那种的！」
4.同步运行 -> 指令(p)

; (2)与(3)是同时运行的
; p指令不支持异步模式，所以实际运行时会自动转化为同步模式
```

### Demo B
```sql
@alias avg.animation cmd=avg.ani
#avg.ani BG1 alpha=0 time=1800
@avg.clear BG1

#avg.set BG1 url=大学-房间-晚上 y=-200 alpha=0
@avg.ani BG1 alpha=1 time=3000
@avg.ani BG1 x=-400 time=8000
@play BGM1 url=给沉默的世界1 time=800 loop=always
```
Runtime Translate：
```ruby
1.同步运行 -> 指令(alias) <- 对象(avg.animation) <- 参数{cmd: avg.ani}
2.同步运行 -> 指令(avg.ani) <- 对象(BG1) <- 参数{...}
3.同步运行 -> 指令(avg.clear) <- 对象(BG1)
4.同步运行 -> 指令(avg.set) <- 对象(BG1) <- 参数{...}
5.*异步运行 -> 指令(avg.ani) <- 对象(BG1) <- 参数{alpha: 1, time: 3000}
6.*异步运行 -> 指令(avg.ani) <- 对象(BG1) <- 参数{x: -400, time: 8000}
7.*异步运行 -> 指令(play) <- 对象(BGM1) <- 参数{url: 给沉默的世界1, time: 800, loop: always}

; (4) avg.set 在同步模式下等待图片资源加载成功并渲染到图层上
; (5)、(6)、(7)是同时运行的，BG1 并列启动了2个动画行为
```
经过分析后，我们可以写出与上方等价的演出脚本，**让异同运行直观展示**，便于开发时的设计与后续维护：
```sql
#alias avg.animation cmd=avg.ani
#avg.ani BG1 alpha=0 time=1800
#avg.clear BG1
#avg.set BG1 url=大学-房间-晚上 y=-200 alpha=0

@avg.ani BG1 alpha=1 time=3000
@avg.ani BG1 x=-400 time=8000
@play BGM1 url=给沉默的世界1 time=800 loop=always
```
