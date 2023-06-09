+----------------------------------------------------+
+          名称：    Ultra String Reference          +
+          版本：    v0.10                           +
+          作者：    罗聪                            +
+          日期：    2004-07-15(yyyy-mm-dd)          +
+----------------------------------------------------+



[简介]
Ultra String Reference 是一个 OllyDbg 的插件（Plugin），OllyDbg的串式参考（String Reference）对中文的支持比较差，有感于此，我写了这个插件，支持对 GB2312 中文的串式参考，希望可以改善这种状况。


[系统需求]
OllyDbg 1.10，否则该插件不会被装载。


[安装]
把 ustrref.dll 拷贝至 OllyDbg 的 Plugin 目录下即可。


[使用方法]
第一步，用 OllyDbg 载入 PE 文件。
第二步，选择主菜单的“Plugins->Ultra String Reference”，或者右键单击并选择反汇编窗口中弹出的“Ultra String Reference”菜单，然后选择“Find ASCII”或者“Find UNICODE”。具体使用哪个请自行决定，一般来说 VB 的程序应该使用“Find UNICODE”，非 VB 的程序应该使用“Find ASCII”。


[原理]
通过对PE文件的代码段进行反汇编，找出以 push、mov、lea 开头的 3 种形式的代码。例如：
;--------------------
push [address]
;--------------------
或者
;----------------------------------------
mov [register], offset [address]
;----------------------------------------
我们知道，push 很可能是进行参数的压栈操作；mov offset 和 lea offset 则很可能是紧跟一个字符串的偏移地址。因此如果对程序进行反汇编，并进行一些相应的地址转换后，得到的就非常可能是字符串。根据多次尝试后，我最终采取的方法就是这样——对代码段进行反汇编，然后转换地址，再筛选掉没用的垃圾字符，从而找出有用的字符串。


[特点]
1、能正确找出中文字符串。
   这点 OllyDbg 就无法做到，大部分的中文字符串都会被它漏掉。

2、与 OllyDbg 原有的 String Reference 功能兼容。
   操作步骤和界面都是一样的，基本上不会造成使用上的困难。


[缺点]
1、有部分中文字符串不被支持。
   为了尽可能地过滤垃圾字符串，目前仅支持双字节 2 区的 6763 个常用简体中文汉字（即 GB2312 标准的汉字），以及双字节 1 区和双字节 5 区的图形符号。然而对于大多数程序来说，这些常用字已经足够了。

2、会存在一定的垃圾字符串。
   有部分垃圾字符串的组合编码会与 GB2312 简体中文汉字和图形符号的码位范围落在同一个区间内，这样就会导致识别错误。不过这是很难避免的，其实 W32Dasm 也存在同样的问题。


[ToDo]
对生成的串式参考字符串进行搜索。


[联系方式]
姓名：罗聪
主页：http://www.luocong.com
邮件：admin@luocong.com
