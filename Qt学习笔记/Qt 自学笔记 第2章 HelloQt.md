## 编译
很多编程都是从HelloWorld开始第一个程序的，我们就从Hello Qt开始。Qt编写代码/编译主要有两种，一种使用你喜欢的编辑器编译代码，然后通过命令行编译，另一种就是直接使用QtCreator。这里先介绍下使用命令行编译，后面的章节都使用QtCreator.


```c++
#include<QApplication>
#include<QLabel>
int main(int argc , char** argv)
{
    QApplication app(argc,argv);
    QLabel* label = new QLabel("Hello Qt");
    label->show();
    return app.exec();
}
```
这是<C++ Qt4 Gui编程>中的第一个例子，在你喜欢的目录下，新建一个HelloQt.cpp文件，然后把上面的代码复制进去，这些代码的含义稍后详述，这里先介绍下如何编译Qt代码。

**请确定第1章里，要求添加的环境变量已经添加了，否则无法通过命令行编译**

1. 进入目录，目录下应该有个已经写好的HelloQt.cpp文件，我这里的目录名称为p2,即有文件“~/p2/HelloQt.cpp”

2. 执行命令
```shell
qmake -project
```
这时候目录下应该已经多出了一个p2.pro的文件，该.pro文件为自动生成，而文件名会被命名为当前目录的名称，例如我的目录名为p2。

3. 使用你喜欢的编译器（可以是vim,VS Code,Notepad++或者其他，但千万别用win自带的记事本，记事本经常会无法显示.pro文件的换行符）打开.pro文件，然后在该文件里加上一句
```shell
QT += widgets
```
然后保存并推出，注意这句代码是区分大小写的

4. 输入命令
```shell
qmake p2.pro
```
这时候目录里应该会多出一些文件，以及两个子目录，debug和release,在自动生成的文件里会有一个Makefile,Qt编译需要区分debug和release模式，这取决于Qt的初始设置究竟是哪一种。如果你不在乎，就可以不用理这个文件，如果你需要编译指定debug或者release模式，用你喜欢的编译器打开这个Makefile文件，在文件开始有三行
```shell
first: release
install: release-install
uninstall: release-uninstall
```
我们可以看到他默认编译的是release模式，如果你希望编译debug模式，需要把这三行里的release改成debug,说明下，Qt Study Notes的例子都是使用debug模式

5. 输入命令
```shell
mingw32-make
```
我这里使用的是MingW编译器，如果你安装的是VS编译器，这里需要输入的命令可能会是
```shell
nmake
```
然后程序很快就编译好了，进入debug子目录，里边有个"p2.exe"(文件名没有设置的话，默认是目录名)，运行这个exe程序，差不多是这个样子

![](https://jxf2008-1302581379.cos.ap-nanjing.myqcloud.com/QtNotes/2-1.png)

样子挺丑的，当然你可以把窗体拉伸下

![](https://jxf2008-1302581379.cos.ap-nanjing.myqcloud.com/QtNotes/2-2.png)

>兴奋ing..

我不知道诸位看到这个按钮（其实是个标签）是什么感觉，反正我第一次见到这个标签时，那个激动啊。。。。

我是自学编程的，第一次接触编程，是我从书店买了一本<C++ primer plus>,然后看着这本书，按照书上一行一行的敲代码，我花了半年的时间学完这本一千多页的书的时候，发现自己始终工作与一个黑框框（shell)内，当我学完<C++ primer plus>后开始学习Qt的时候，当我第一次看到黑框框以外的世界，就如同一个失明许久的人终于获得了光明，整个人都激动的想大喊。。。。。。

## 解析Hello Qt

在度过了最初的激动后，我们需要仔细的分析下这段来自<C++ Qt4 Gui编程>的代码。我们需要逐行分析这段看似非常短的代码

>第1-2行

代码包含了两个头文件，QLabel和QApplication，关于QLabel就是我们看到的标签，在Qt中，绝大多数窗体都是一个类，Qt的命名习惯是**字母Q+窗体种类名称**，而窗体的各种功能大多数通过类的成员函数来实现。这个程序中我们需要用到QLabel和QApplication这两个类，所以用#include来包含，至于QApplication类的作用下述。

>第3行

C++的标准主函数,整个《Qt Study Notes》，我假设你已经熟悉了c++基本的语法，希望你对这个函数不会感到陌生


>第5行

生成了一个QApplication的对象app，这个程序中有关这个类的代码一共有3行，先暂时跳过，我们最后再讨论这个类

>第6-7行

生成了一个QLabel的对象label，该类接受一个字符作为参数，该字符就是显示在标签上的字符。Qt窗体在创建时默认是隐藏的，所以第7行调用QLabel的成员函数show()来显示这个窗体，这样你就可以看见他，此时你也许猜到了，他还有个成员函数hide()，作用是隐藏窗体，有兴趣的朋友可以在第7行下面加上一句label->hide()来看看效果

>第8行

这个程序第三行的有关QApplication的代码，在解释这个类作用前，可能有人会问，这个程序代码执行完了，为什么窗体还在这里？事实上这个程序代码并没有执行完成，对象（第5行生成）app的成员函数exec()是个无限循环，这保证了这个程序代码一直在执行，也就是这个标签为什么一直在这里的原因，当然直到你手动关闭掉这个标签，无限循环结束。那么这个类的作用就比较清晰了，使用他有3个步骤

1. 包含QApplication头文件

2. 生成一个对象

3. 调用成员函数exec()使得窗体一直存在
        
>关于QApplication类

在《Qt Study Notes》所有的例子中都只会用到这3行代码，虽然死记硬背不提倡，但对于这样公式化的东西背一下未尝不是好版本，至于这个QApplication类的作用细节已经超出了范畴，用建议兴趣的同学看完整个《Qt Study Notes》后再来探索

 好了，通过上面的解释，我们可以把这段Hello Qt的代码翻译成“伪代码”

```c++
包含QApplication头文件
包含QLabel(标签)头文件 

int main(int argc , char** argv)
{
    生成一个QApplication对象
    生成一个QLabel(标签)对象
    调用QLabel的show()函数使得标签可见
    调用QApplication对象的exec()函数使得窗体一直结束（程序处于一直运行）
}
```

>关于内存泄漏

很多对内存敏感的朋友应该看出来这个程序存在的问题了，程序中使用new创建了一个对象（QLabel对象），但没有对应的delete,关于这一点，在书本上（C++GUI Qt4编程）中的解释是“这样一点内存泄漏无关大局”，事实上，对于很多C/C++程序员来说，任何内存泄漏都是无法容忍的，有人会问，Qt是否不考虑内存泄漏的问题？

答案是否定的，Qt自带了一套非常不错的内存管理机制，这使得Qt对于内存的管理相对于一般的C++内存管理有很大不同，Qt更加的智能，更加的自动化，关于Qt如何管理内存，我们将在稍后的篇章里详细讨论这个问题，因为讨论Qt内存的问题需要一些稍后介绍的内容，出于排版上的考虑，放在了稍后几章，在正式讨论这个问题之前，先让我们“放肆“一下，暂时忽略内存泄漏的问题。
