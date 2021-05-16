---
title: try_catch
date: 2020-07-30
category:
tags:
---

### # try catch 心得

try{ //正常执行 }catch(e/*你感觉会出错的 错误类型*/){ // 可能出现的意外 eg:用户自己操作失误 或者 函数少条件 不影响下面的函数执行 // 有时也会用在 比如 focus() 但可恶的ie有可能会第一次没有focus事件 再让他执行一次 // 有时一些不是bug的bug 在ie上 他要求必须加上 catch 哪怕就一个空catch 以前在ie8上遇到过这个操蛋的js问题 }

1、
作者：杨光
链接：https://www.zhihu.com/question/21583373/answer/18696617

try catch的使用，永远应该放在你的控制范围之内，而不应该防范未知的错误。也就是说你很清楚知道这里是有可能”出错“的，而且你很清楚知道什么前提下会出错，你就是要故意利用报错信息来区分错误，后续的程序会解决所有的出错，让程序继续执行。
如果让用户先发现你根本没预料到的错误，而不是你先发现错误，你是失职的。

大多数情况下，try catch适用于两种场合：
1、浏览器原罪的场合：也就是兼容性场合，因为浏览器兼容性不是程序员能改正的，所以只能try catch：由于不同浏览器的报错提示是不一样的，根据捕获的浏览器的报错提示判断用户的浏览器，然后做出对应的措施，这时候使用try catch是巧妙的办法，如果用if就比较笨拙，因为if通常只能反馈真或假，不能直接反馈浏览器的报错内容。
2、考虑如下代码。window.a.b是非法的，再跟2对比就没有意义，这样非法的条件，在try catch中仍可以继续运行下去。但在if中window.a.b已经报错，整个页面都会坏掉。如果希望用if写，那么必须先判断window.a是否是合法的，window.a是合法的前提下再判断window.a.b是不是合法的，如果也是合法的，再判断window.a.b是否不等于2，这样是不是很蠢？这时就体现出try catch的妙处了，程序不但知道window.a.b !== 2是假的，而且直接可以知道究竟哪一步就已经是假的。
再想象一下，有一个变量是json.a.b.c，其中的a/b/c都可能是存在的也可能是不存在的，全看具体情况，这时候你简单的写if (json.a.b.c === 2) {...}是不行的，因为json.a.b就可能已经是非法的，所以你如果用if，就要考虑a是不是非法的、a是合法前提下b是不是非法的，b是合法前提下c是不是非法的。但是json.a.b.c === 2在try中就可以直接写，也就是说，我不关心究竟a/b/c谁是非法的，我只关心json.a.b.c到底是不是等于2，不等于2或者任何一步出错，对我来讲没有区别，反正都是不等于2，我不关心哪步出错，而且程序不会坏掉。这是一种比较省心的写法。
另外注意，try catch不能做真假判断，只能做非法判断。也就是说：try {1 === 2}，虽然1===2是假，但是是合法的，catch不会捕捉到错误，也不会告诉你1 === 2到底是真是假。所以，写在try里的应该是json.a.b.c而不是json.a.b.c === 2。究竟是不是等于2，是后面的事，是if干的事。简单说，try catch用于捕捉报错，当你不关心哪一步错误，只关心有没有错，就用try catch。

try {
  window.a.b !== 2
}
catch(err){
  alert(err) // 可执行
  alert(123) // 可执行
}

if (window.a.b !== 2) {
  alert("error") // 不执行
}
alert(123); // 不执行

最后，try catch在早期被各种语言的程序员滥用，try catch出现的场合被夸大了，事实上没那么多适用场合。如果你的几千行程序都没用到try catch也是很正常的，尤其是用了jquery。

2、
1、事情还有得挽回，换条路走
try {
执行某个逻辑
} catch (e) {
出问题鸟，换个逻辑执行
}

2、体面的退出
try {
正常流程
} catch (e) {
弹个框告诉用户不好意思出了点问题
如果是用户的错就告诉用户什么地方错了
如果是程序的错，就告诉用户不好意思没法执行
}

作者：杨益
链接：https://www.zhihu.com/question/21583373/answer/18689400

3、
偶认为，异常处理和错误处理是两个不同的概念。例如NodeJS里大多数error都是用来处理异常的，因为异常是不可避免的，例如数据库挂了，网络错误，你虽然知道有可能，但是不知道何时发生，这些异常你需要捕捉或者传给上层。而错误处理，则是一些基本的判定，可以从代码级别避免其发生，可预知可推测，如果发生了，不是系统问题，而是你的程序有bug了。

对于NodeJS来说，两种错误都时刻需要注意，特别是系统错误，因为不可预知，需要大量代码来catch错误，传递错误，最后统一处理。

而对于前端，系统错误出现的场景相对来说低得多，主要是一些io场景，大部分前端可能不太关心。而普通的错误处理，则比较常见，因为前端耦合的特定系统比较多，和这些系统操作的时候，数据和dom啊什么的大多是可预知的，跟系统错误还是要区分开的，一些错误，需要你自己去吞并和处理，如果出现错误，显然是bug，而不是不可预知。

bulabula，记事本里相关的一片文章躺了很久了，至今写了不到一半，太监了。

作者：孙芋头
链接：https://www.zhihu.com/question/21583373/answer/19938568

[try catch](https://www.cnblogs.com/yangheng/p/6018224.html)