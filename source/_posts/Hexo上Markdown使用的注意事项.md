---
title: Hexo上Markdown使用的注意事项
date: 2017-04-10 15:31:15
tags:
---
## #0:代码块的引用
代码块引用的语法是: 
> (此处有1个空格)\`\`\`language    
> code  
> (此处有1个空格)\`\`\`

language指的是语言类型，加了的话可以代码高亮（如果支持）。如果不在开头打一个空格，在某些环境下会出意想不到的问题，我也不知道为什么。  
开头加空格！开头加空格！开头加空格！重要事情说三遍！！！<!--more-->

## #1:Latex多行公式
行内公式是用\$夹住公式，如\$公式\$这样，比如\$\frac{1}{x}\$这样会出现$\frac{1}{x}$。  
行外公式就是用两个\$夹住,可以得到下面这种效果：$$\frac{d}{dx}y=x$$
还有多行公式，用{equation}加{split},但是{aligned}用不了，不知道为什么。‘\\’用来换行，‘&’用来对齐（参见LaTex语法），还有需要注意的是在Hexo上因为会对'\'转义，所以要使用四个反斜杠来换行，此处参考了[这篇博文](http://kubicode.me/2016/03/18/Hexo/The-Trick-about-Hexo-Support-MutliLine-Equation-using-Mathjax/?utm_source=tuicool&utm_medium=referral)效果如下：$$\begin{equation}\begin{split}&\quad a+b\\\\
&=c+d\\\\
&=e+f\end{split}\end{equation}$$