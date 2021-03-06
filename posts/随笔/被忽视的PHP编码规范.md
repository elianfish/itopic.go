```
{
    "url": "php-ignore-items",
    "time": "2014/01/06 11:30",
    "tag": "随笔,吐槽,PHP"
}
```

说编程是一门艺术一点都不过分，无论从代码中体现的思维还是给人的视觉感受都无可挑剔。当我们看别人的代码时，第一眼感受到的就是作者的编码风格，根据编码风格大致可以感受到程序的好坏。良好的编码风格看起来很有艺术的感觉，读起来容易，易维护，出问题的几率也小。而不良的编码习惯导致代码很难读，思维不够清晰，维护成本高，可能潜藏着更多的BUG。

艺术是用来欣赏的，但实际项目中需求的随意不间断无规则变动导致项目不可能成为艺术。虽然成不了艺术，但也不该让人一眼就看到肮脏错乱无规则的代码，下面都是从实际项目中看到的一些问题，这些问题不是需要有多好的基础、多好的技巧才能做到，只要留心注意下，每个人都可以做到，这里提出来一起探讨。

# 1、页面过长

![](/static/uploads/ignore-php-items-1.png)

上面的截图为某个控制器的长度，还是删除过不少的，2211行是个什么概念，如果一屏可以显示40行代码，也就是要翻50多屏才翻完。想要找到某个片段并不容易，也可以想象到其中要么是方法太长，要么是函数很多。如果是方法太长，说明耦合性很高，可以把一些片段写成方法独立出去，也方便其他地方调用；如果是方法太多，则说明可以将功能进一步按模块细分，一个控制器文件做一个模块的事情，如果模块太大则细分为更小的模块。

# 2、排版混乱

![](/static/uploads/ignore-php-items-2.png)

先看看上图，看能不能发现什么问题？如果要给session加一个配置信息，很有可能写在uri的上面，可仔细一看，session的数组结束于uri上面。不怪他人不小心，只怪这括号太有诱惑力。还有需要注意下tab键的个数，至少通过排版可以清楚的看到层级关系。还有一些代码写一行空两行，空格时而三个时而五个，TAB错综复杂，注释一片一片等等，这些直接影响到程序的阅读。想要写好程序，先从排版开始吧，找一些好的开源类库看看错落有致、整洁规范是个什么样子。

# 3、注释杂乱无章

![](/static/uploads/ignore-php-items-3.png)

排版注释这些都是分不开的，注释不好也直接影响到程序的阅读，我们常习惯拷贝，很多时候直接连注释也一并拷贝过来，可注释在这里根本就不适用，那这些注释就是没用的、无效的；还有一些注释就是一些废话，太简单的语句一看就懂的语句就不必加注释了，注释太多容易导致废话太多，适可而止就好；还有一些注释可能根本就是错误的，看到想骂人的那种。。。

上面的代码一看就知道是饱经磨砺，很多调试的程序被注释了，这是可以看到的。还有很多调试的注释可能还在系统中运行，每天产生一些日志文件、日志信息等。其实类似的这些垃圾代码是可以删掉的，留在上面影响程序的阅读，代码看起来很乱，影响阅读者的心情。 

# 4、奇特的命名

![](/static/uploads/ignore-php-items-4.png)

写程序时给变量、函数、常量等命名那是很平常的事，常用的就大小驼峰、下划线分隔等，基本上采用英文单词来表单名称的含义，直接用拼音、拼音加英文的方式来命名应该是少数情况，感觉还是有点难理解，中间得停顿几下才知道表达的什么意思。还有一些常量采用大写字母这应该属于国际标准吧，可还是看到直接使用小写字母的情况，一下子都没反应过来。如果要找的话，各种奇葩的名称应该都可以找到。

命名是一种学问，个人可以慢慢去研究。简单说下自己的命名方式，函数名采用小写字母下划线分隔方式；类属性、类方法采用小驼峰；类名根据系统中大部分情况来，有下划线分隔的，有大驼峰的，也有其他方式的；变量统一采用小写字母下划线分隔方式。单词个数不要过多，超过3个就有点多了，超过5个那就太多了。类名可以用名词，方法可以用动词。 

# 5、循环嵌套

![](/static/uploads/ignore-php-items-5.png)

嵌套的情况可能很多人存在，因为这种逻辑判断是顺利成章的事，IF什么条件，ELSE什么条件，满足条件后还有条件就继续嵌套，这样子越嵌越深、越写越复杂，想要找个某个块并不容易，修改起来也很困难。
其实这个问题可以换种思维来写，比如要判断是否为AJAX提交：
```
if(! $this->input->is_ajax_request()) {
    //这里写ELSE的内容
}
//这里就表示该请求为AJAX请求
```
按照这种思维写下去可以减少条件嵌套，嵌套的耦合性比较高，嵌套的越多出问题的可能性越大，也可以尝试通过一些其他的方式来对嵌套解耦。

# 6、多环境问题，配置问题

配置的问题存在很多地方，数据库配置直接写在文件中，类的一些配置信息直接写在属性中，当要修改时是十分不方便的。以及多环境的配置，测试环境请求测试地址，生产环境配置生产环境配置，能灵活的做到不影响。关于多环境、初始化等在本博客CodeIgniter初始化中有提到过，思维对所有项目应该都适用。

# 7、生产环境上的测试方法、空方法、没用的方法、大规模注释的代码

![](/static/uploads/ignore-php-items-6.png)

看看生产环境，运行久了之后少不了有些测试方法、大量注释的代码留在上面，是什么原因呢？上面的方法直接就可以看出是测试方法，也许有些时候并不是那么好分辨是否为测试方法，是否为有用的方法，后面的人接手后也糊里糊涂的，更加不敢动，特别是在一些比较重要的系统中，迷糊的情况下宁愿让它继续留在上面也不敢直接干掉它。
有些时候，事情比较急的时候我们可能会直接去注释掉某个片段，写个测试方法等，但事情处理完之后能不能把垃圾清理干净？不要想着以后会再去处理，99%都不会了。

# 8、日志记录问题

为了调试，最常用的手段还是打印，而生成环境不方便打印，所以变成了记日志。记日志有两个需要考虑的地方，一是记录在哪里？很多项目是直接记录在根目录，比较危险的，直接就可以访问到了，所以不要把日志记录在根目录下。二是怎么记？常用的方法就直接file_put_contents，然后不断的追加，最后一个日志文件加到几G。。。产生的原因还是跟上面一样，这些信息只增不减，慢慢的日志文件越来越多,直到产生问题。

# 9、不做任何判断

在开的发时候常常从数据库读取出记录后，不管是否取数据成功就直接用；获取到的变量不管有没有设置，有没有值也就直接用，不做任何判断性的逻辑，一个接口不成功也返回成功，一个操作提示操作成功实际却失败了。这个问题是很常见的，直接影响到程序的健壮性。做程序的不能这样子，我们得告诉机器成功该怎么处理，取不到数据该怎么处理，这是程序员最基本的要求。

简单的列举了近段时间在项目中看到的一些问题，这里不针对具体的人，主要还是抛出问题，引起重视。也许被这样的问题困扰过、吃过亏之后就记住了。

总之，不论自己还是他人的代码，读代码的时间总是要比写代码的时间多得多。读的过程很有可能碰到上面这些问题，于是心底一遍一遍的谴责作者，如果不想被谴责，就注意一下这些问题吧。