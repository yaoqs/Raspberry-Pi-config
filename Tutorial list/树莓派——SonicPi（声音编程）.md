# 树莓派——SonicPi（声音编程）

Sonic Pi是一个开源的编程环境，设计初衷是为了通过创建声音制作音乐的流程来进行编程概念的探索和教学。

Sonic Pi的执行的代码基于Ruby，这是一种漂亮又简洁的编程语言。这意味着你可以写很多代码而不需要考虑太多关于语法和大小括号的问题——尽管对于复杂程序这些很重要。

# 1、GETTING STARTED（开始学习）

你可以在Raspbin系统的applications菜单的Education中找到Sonic Pi程序。打开这个程序，你将会看到类似下面的窗口：

![\](http://www.2cto.com/uploadfile/Collfiles/20140923/201409230922226.png)
 这就是Sonic Pi的程序界面。窗口中主要包含三个部分。最大的部分是你写代码的地方，我们叫这里为编程面板。右上角的控制窗口为输出面板，这里会显示你的程序执行过程中的信息。右下方的窗口是错误面板，当你的代码中有错误的时候会在这里显示错误信息。

# 2、MAKING SOUNDS（制作声音）

在Workspace 1中输入以下内容：

```
play 60
```

现在点击Play按钮，这个音符将会执行。MIDI将值60指定为C音。

尝试将**play 60**更改为**pley 60**看看会有什么错误。

现在输入以下代码：

```
play 60
play 67
play 69
```

然后在点击Play按钮一次。

这些音符执行的很快，以至于听起来好像是同时发声一样。使用sleep在音符之间添加暂停：

```
play 60
sleep 1
play 67
sleep 2
play 69
```

# 3、A TUNE：FRERE JACQUES（一首曲子：雅克兄弟）

雅克兄弟这首曲子的开头为：

C D E C 或者在MIDI中的音符为60 62 64 60。

**音符与MIDI音符值对照：**

| C    | D    | E    | F    | G    | A    | B    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 60   | 62   | 64   | 65   | 67   | 69   | 71   |

以下是曲子：

```
play 60
sleep 0.5
play 62
sleep 0.5
play 64
sleep 0.5
play 60
sleep 0.5
```

# 4、LOOPING（循环）

为了重复执行一系列指令，你可以使用循环。以下是Ruby语言循环的示例：

```
2.times do
  play 60
  sleep 0.5
  play 62
  sleep 0.5
  play 64
  sleep 0.5
  play 60
  sleep 0.5
end
```

# 5、FUNCTIONS（方法）

你可以在一个方法中定义一系列执行，以后可以多次调用这个方法以避免复制黏贴多行代码：

```
def frere
  play 60
  sleep 0.5
  play 62
  sleep 0.5
  play 64
  sleep 0.5
  play 60
  sleep 0.5
end
```

以后就可以输入frere来调用这个方法。例如在一个循环中调用：

```
4.times do
  frere
end
```

# 6、SYNTHS（合成器）

合成器能够让play方法发出不同的声音效果。默认的合成器是“pretty_bell”，但是你可以自己更改：

```
"pretty_bell"
"dull_bell"
"beep"
"saw_beep"
"fm"
```

尝试一下不同的合成器：

```
with_synth "fm"
2.times do
  play 60
  sleep 0.5
  play 62
  sleep 0.5
end
```

# 7、THREADS（线程）

你可以使用线程来同时播放两首曲子。类似于循环，它也是以end关键字为结尾的代码块：

```
in_thread do
  with_synth "saw_beep"
  2.times do
    play 60
    sleep 0.5
    play 67
    sleep 0.5
  end
end
```

# 8、WORKSPACES（工作空间）

 你可以在Sonic Pi程序窗口中使用多个工作空间。这意味着你可以在其它工作空间中试用你的代码块，而不需要删除当前的代码。建议你使用其它的工作空间来试用代码，做实现或沙盒测试。 

# 9、SONIC PI FILES（Sonic Pi文件）

 如果你选择保存为文件，你就可以以后再返回来或者分享给别人。Sonic Pi文件是一个简单的文本文件，因此你可以在其它计算机上查看和编辑，也可以在其他树莓派上运行。						



from https://www.2cto.com/os/201409/336553.html