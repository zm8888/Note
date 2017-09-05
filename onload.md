## 详解浏览器渲染页面过程

[TOC]

###1.解析HTML文件，创建DOM树

- 自上而下，遇到任何样式（link、style）与脚本（script）都会阻塞（外部样式不阻塞后续外部脚本的加载）。

### 2.解析CSS

- 优先级：浏览器默认设置<用户设置<外部样式<内联样式<HTML中的style样式；
- 特定级：id数*100+类或伪类数*10+tag名称*1

### 3.将CSS与DOM合并，构建渲染树（renderingtree）

- DOM树与HTML一一对应，渲染树会忽略诸如head、display:none的元素

### 4.布局和绘制，重绘（repaint）和重排（reflow）

- 重排：若渲染树的一部分更新，且尺寸变化，就会发生重排；
- 重绘：部分节点需要更新，但不改变其他集合形状。如改变某个元素的颜色，就会发生重绘。

#### 1.重绘和重排何时会发生：

（1）增加或删除DOM节点；

（2）display:none（重排并重绘）；visibility:hidden（重排）；

（3）移动页面中的元素；

（4）增加或修改样式；（5）用户改变窗口大小，滚动页面等。

#### 2.如何减少重绘和重排以提升页面性能：

（1）不要一个个修改属性，应通过一个class来修改
错误写法：div.style.width="50px";div.style.top="60px";
正确写法：div.className+=" modify";
（2）clone节点，在副本中修改，然后直接替换当前的节点；
（3）若要频繁获取计算后的样式，请暂存起来；
（4）降低受影响的节点：在页面顶部插入节点将影响后续所有节点。而绝对定位的元素改变会影响较少的元素；
（5）批量添加DOM：多个DOM插入或修改，应组成一个长的字符串后一次性放入DOM。使用innerHTML永远比DOM操作快。（特别注意：innerHTML不会执行字符串中的嵌入脚本，因此不会产生XSS漏洞）。