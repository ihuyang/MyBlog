---
layout: post
title:  "Java开发中的编码研究"
date:   2016-10-06 21:00:00
categories: java web
tags: featured
image: /assets/article_images/2016-10-06/desktop.JPG
image2: /assets/article_images/2016-10-06/desktop.JPG
---
### *摘要*
>　　对于每一个程序员来说，相信编码这个概念都不会陌生，因为无论使用何种开发语言都或多或少会涉及到编码，只是需要考虑的程度有所区别而已。尽管非常的重要且基础，但它也特别容易在实际的开发过程中被忽视，由此带来的问题也一直困扰着开发人员。乱码就是这些问题的代表，相信所有的程序员都被乱码折磨过吧。
<br>
>　　前段时间，我在工作中再次遇到编码问题，尽管最后解决，但还是让我决心通过查阅资料去好好研究编码问题。在本文中，我将针对这段时间自己对Java开发中编码问题的研究进行介绍，也算是自己的小结，如果你对该方面内容感兴趣，可以继续阅读下文。

# *1. 编码问题* #
　　首先，第一个问题是，我们为什么要**编码**，回答这个问题对于从深层次了解编码非常有帮助。编码是信息从一种形式转换为另一种形式的过程，在我们的生活中就有很典型的例子，不同的国家不同的民族使用的语言和文字并不一样，他们之间的交流依靠的是语言间的转换，也就是我们常说的翻译。同样的，我们知道计算机中依靠二进制0和1来存储信息，这就是计算机所能明白的“语言”，而程序员呢，我们的语言是字母、数字或是文字，我们想要计算机能理解我们的语言从而进行服务，就必须有从我们的语言转换为计算机语言的办法，这个过程就是编码。与之相对的概念是**解码**，即从计算机语言转换为我们所能理解的语言的过程，我们可能常把这两个概念都说成是编码，实际上并不一样。

　　我们知道code是代码的意思，这形成了英语与中文之间的转换关系，我们为什么会知道它们是这样转换的？回答是依靠了词典或者说是一种既定的规则。这个规则在编码中就是我们使用的**编码格式**，它告诉了我们如何将我们的语言“翻译”为计算机语言，常见的编码格式有ASCII码、ISO-8859-1、GBK、GB2312、UTF-16和UTF-8等等，后面我将着重介绍几种重要的编码格式。既然编码格式是这种转换关系的规则，那么不同的规则下，自然就会有不同的对应关系，同一个汉字在不同的编码格式下转换成的二进制序列顺序甚至是长度都不会一样。

　　计算机中存储信息的最小单元是字节，即8个bit位，共有2的10次方种表现形式，如果要和字符形成对应关系就可以表示256个字符。显然这对于人类语言来说远远不够，那么有些字符就得用更多的字节去表示。前面提到的编码格式代表了字符和字节间的转换关系，而编码格式也有它自己能转换的范围，不可能是无限的，比如GB2312就是一种双字节编码格式，它使用两个字节来表示信息，自然不会包含超过两字节所能表示的最多字符，这个范围就称作这种编码格式的**字符集**。字符集是各种文字和符号（即字符）的集合，而编码格式是这些字符转换为字节的规则，是字典的作用，两者并不一样。我们常说的UTF-8，严格意义上来说是一种编码格式，而UTF-8字符集才是一种字符集，不能混为一谈。

# *2. 常见编码格式* #
　　前面已经提到，为了满足不同的编码需求，已经产生了很多种成熟的编码格式。在这里，我将介绍这些编码格式中比较常用的几种，它们具有代表性且适用广泛。

* **ASCII码**　　最先提到的自然是最为基础的编码格式，ASCII码相信大家都不会觉得陌生。首先，它是**单字节**的编码格式。标准的ASCII码，只用这个字节的低七位来表示字符，也就是说它最多只能用于转换2的7次方个字符(128个)，这些字符（或者说是ASCII码的字符集）包括了计算机常用的打印字符和控制字符，打印字符包含了阿拉伯数字、大小写的英文字母、标点符号等等，而控制字符则包含了换行、回车等等。可以看出单字节的ASCII码并不支持中文。
* **ISO-8859-1**　　标准的ASCII码只使用了字节的低七位，能够表现的字符数目非常有限，而ISO-8859-1就是为了涵盖更多西欧语言对ASCII码进行的一种扩展。ISO-8859-1依然是单字节编码，但是它可以使用所有位，因此它最多可以表示256个字符。这里我们提到ISO-8859-1是对ASCII的扩展，因此它也能向下兼容ASCII码，换句话说使用ASCII编码后的二进制序列可以使用ISO-8859-1进行解码不会出现问题。接下来我将提到的两个编码格式也同样有这样的特点。
* **GB2312**　　GB2312是一种专门为中文设计的编码格式，是对ASCII码的**中文扩展**，它采用双字节进行编码，除了前面提到的基础字符以外，它还支持了绝大部分的简体汉字，是中文编码中非常重要的一种编码格式。
* **GBK**　　GBK全称是《汉字内码扩展规范》，和ISO-8859-1与ASCII码的关系一样，它是对GB2312的一种扩展，虽然同样是双字节编码，但是它包含更多的码位，从而可以表示更多的汉字。同样的，因为是对前者的扩展，所以它也是向下兼容的，使用GB2312编码后的二进制序列可以用GBK解码回汉字不会出现问题。
* **Unicode**　　这个必须特别介绍，Universal Code，**统一码**，正如它名字表示的那样，它想要统一我们的编码格式。其实在介绍前面几种编码格式的时候我们可能就会有疑问，为什么大家不使用同一套标准来进行编码呢，这样就不会出现各式各样的乱码问题了。Unicode产生的目的其实就是为了实现这样的场景，它希望它的字符集能够包含世界上所有的语言和符号，换句话说，每一个字符都能有唯一的二进制序列与之对应。可以想象的到，这样的字符集将多么的庞大。由此也产生了一个问题，为了节省空间，每个字符的字节长度不可能统一，有的字符会使用少量字节表示，比如英文字母使用单字节，而有的字符则需要更多，那么计算机如何能知道它用了多少个字节呢。为了更形象的理解这点，我画了下图这个例子。

![同一序列的多种解析方式](/assets/article_images/2016-10-06/2016-10-06_1.jpg "同一序列的多种解析方式")
<br>
　　从这张图中，我们可以看出Unicode想要在计算机编码中使用，还需要更为具体的存取方法或者规则。

* **UTF-8**　　对于UTF-8，我们首先需要明确一点，虽然我在这里将Unicode和UTF-8并列介绍，但实际上UTF-8只是Unicode一种具体的实现方式，它只是在Unicode的基础上规定了字符如何在计算机中存取。
<br>
　　UTF-8与前面提到的几种编码格式都不太一样，它是一种**变长**的编码格式，拥有多个编码区域，在不同的区域，字符的长度并不一样。在UTF-8中不同类型的字符可以由1到6个字节组成，这对于互联网时代的我们，非常有意义，因为网络的带宽是有限的，定长的编码格式在传输小字节字符时将造成空间的浪费，变长编码格式却很好的解决了这点。但是变长的编码格式又会出现前文中所提到的多种解析方式的问题，UTF-8是如何制定编码规则避免这个问题的呢？
<br>
　　UTF-8的编码规则非常简单，只需要逐字节判断最高几位控制位就可以了。对于一个字节，如果它最高位是0，说明这个字节应该解码为一个单字节的字符，这时的编码方式和ASCII码完全一样。（前面有提到ASCII码使用7位进行编码）；对于一个字节，如果它最高位是10，那么这个字节就不是当前字符的首字节，我们需要继续向前查找首字节；对于一个字节，如果他最高位是11，那么这个字节就是当前多字节字符的首字节，而且高位连续1的个数代表了这个字符的字节长度。最后，根据当前字符的长度，除去这些字节的控制位，剩下的就是Unicode编码了，以上就是UTF-8的编码规则。为了更好的理解，我在下图中举了一些例子。

![UTF-8的编码规则](/assets/article_images/2016-10-06/2016-10-06_2.jpg "UTF-8的编码规则")