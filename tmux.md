终端复用器 （`terminal multiplexer`）

结构： 会话（`session`）-> 窗口（`windows`） -> 面板（`pane`）

前缀键 (`profix key`) : 按住 ctrl + b 之后，松开

会话管理（Session）：

创建新会话 ： tmux new -s <会话名>

会话分离/挂起：前缀键 + d                                 

会话连接：tmux a -t <会话名>

会话切换：tmux switch -t <会话名>

会话选择：前缀键 + s 

面板管理(Pane)：
水平分割面板： 前缀键 + %
竖直分割面板： 前缀键 + “
面板关闭：前缀键 + s
移动面板范围：前缀键 + 方向键









