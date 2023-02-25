# pg16
A book about the internals of PostgreSQL open source database. It is written by using simplified Chinese language.
Any suggestion please send to itgotousa@gmail.com. Thx!


## 自序

在IT行业浪迹了二十多年以后，我决定写人生第一本IT技术方面的书，那就是你面前的这本《PostgreSQL老鸟之路 - 管理篇》。也许是因为命运的安排，十多年前我来到美国后，阴差阳错地进入了数据库管理员DBA（Database Administrator）的行业，从此我的职业生涯的大部分时间都是和Oracle/PostgreSQL等数据库软件打交道，所以本书的内容是深入学习著名的开源数据库PostgreSQL运维管理方面的相关技术，大约类似“某某某从入门到精通”之流，但绝对不会教你如何从删库到跑路。

写书有三种类型，编书，著书和编著。此三者的区别显而易见。所谓编书，就是把能够找到的相关资料汇编成册，内容是别人的，自己做的工作是汇集整理。所谓著书，就是把自己消化理解后的知识和经验写出来，把茶壶里的饺子倒出来。编著则是居于“编”和“著”中间的层次。很显然，“编书”层次最低，“著书”层次最高。著书的工作花费精力和心血最多，是一个良心活。曹氏曰：“满纸荒唐言，一把辛酸泪。都云作者痴，谁解其中味”，此语真乃著书的天花板。我亦想在PostgreSQL领域整齐百家杂语，成一家之言，竭尽自己有限的才智来“著”一本书，然后纳于大麓，藏之名山，俟后世君子。至于能否达到此种我希冀的境界，只能说，尽吾志而不能至者，无悔矣。

从二十世纪中后期到二十一世纪上半期，是IT技术革命大爆发的时代，此乃人类之幸事。但科技的快速发展也带来了某些副作用。其中一个副作用就是劳动者需要更长的时间和精力来学习，才有能力从事专业领域的工作。君不见，学士，硕士，博士不断内卷，直逼壮士，猛士，乃至烈士。等到博士毕业后，蓦然发现自己已经宿舍明镜悲白发，可怜依然单身狗。这导致当代年轻人晚婚，不婚，少子化，躺平等社会问题越来越普遍。在IT技术领域，随着技术复杂度的提高，进入其中的壁垒越来越高。后来的学习者想登堂入室，难度越来越大。譬如Linux内核的开发，对于初学者来说，学习曲线异常陡峭，除非投入巨大的精力和时间成本，否则连老鸟的谈话都很难听懂。如果没有足够的利益吸引，想进入技术领域的年轻人只会越来越少。老鸟终究会慢慢凋零，而新鲜血液补充不足，是各种IT技术发展过程中普遍面临的问题。我写本书的目的就是希望能够在学习者进入PostgreSQL数据库领域的过程中，为他们降低学习门槛做一些力所能及的微薄贡献。另一方面也是对自己职业生涯的一个负责任的总结。

或云：坊间相关PostgreSQL的书籍早已汗牛充栋，你写的这本书又有什么独特的价值？答曰：因为我对目前所有的关于PostgreSQL的书籍都不甚满意。不满是技术进步的动力。在我学习PostgreSQL的过程中，充满了痛苦。很多初级读物流于肤浅，而深入的书籍因为前导概念和知识的缺乏，又让初学者不知所云。我不得不像一只勤劳的小蜜蜂，不断在互联网的世界里飞来飞去，四处艰难地采集我寻找的干货。自己要花费大量时间，不断做实验，分析源码，做学习笔记，才能够有一点点收获。我的学习笔记上寥寥数语心得的背后，都是一把辛酸泪。我不想后学者再反复重复这个痛苦过程，就想写一本书，循序渐进地把基本知识和深入知识融合，让读者只要按顺序学习本书，就能够以较少的痛苦代价，达到一定程度的“登堂入室”的境界，为进一步提高技术水平打下良好的基础。不让别的学习者像自己一样痛苦，是我写本书的强烈冲动。

## 本书写作遵循的原则
- **说自己的话** ：既然是著书，就是倾倒自己肚子里面货真价实的饺子。不说抄来的话，力争每一句话都是自己完全理解的，然后再传递给读者。这是对读者负责，也是对自己有益。所以我力争本书中的每一句话都是自己的所思所想，是自己的语言，是能够理解的人话。要对得起天地，对得起读者，对得起自己的良心。

- **循序渐进** ： 人类学习知识的过程肯定是循序渐进的。如果概念B是建立在概念A的基础上，那么学习的时候必须先学习搞懂概念A，然后才能学习概念B。这当然是废话。然而很多书籍和教程往往忽视这个根本性的原则，导致读者如鲠在喉，想吐槽却无处说理。我亦深受其苦，所以“循序渐进”是本书遵循的最高原则。我的目标是，只要读者按照顺序反复学习一本书，就能够从门外汉到登堂入室。整个过程非常流畅，学习时候的痛苦感亦减少很多。

- **内外兼修** ： 当我们学习Oracle, SQL Server等商业闭源软件的时候，只可能学习“外在”的功能性知识，了解这个软件的功能，却不知道它的内部是如何实现的。这就是老话说的“知其然而不知其所以然”。我从事Oracle DBA十几年了，但当你问我对Oracle数据库技术掌握到什么程度，我只能默然，脸红，承认自己懂的太少。这是因为Oracle的源代码是商业机密，不可能公布天下的。所以无论我多么努力，学习闭源软件的技术的时候，总有一种隔靴挠痒的感觉。我们知道，如果感觉自己彻底掌握了一个技术，就会给自己带来巨大的成就感和自信心，这是人性。这种成就感和自信心在开源软件身上才能够获得。所以本书在讲解“外在”的PostgreSQL各种功能知识的同时，会引导读者阅读背后的关键源码，让读者能够内外兼修，通过源代码和软件功能的互相验证，达到“知其然且知其所以然”的境界，增加彻底掌握的满足感和幸福感，从而进一步增加学习的兴趣。

- **化繁为简** ： 大道至简，计算机的终极道理是0和1的关系。无论一个计算机技术多么复杂，包括现在当红炸子鸡的ChatGPT等AI技术，对于一个真正掌握此技术的人来说，都可以浅显易懂的方式传递给后学者。面对庞大繁杂的源代码，用简明扼要的图来描述其关键思想，再加上关键代码的展示配合展示重要细节要点，是帮助读者迅速理解技术背后秘密的重要途径。本书所有的图都是我花费大量心血手工绘制的，而且我确保每张图都尽可能简单，易于理解和记忆，这是本书一大特色。

- **中英文夹杂** ： IT技术是西方文明的舶来品，大量高质量的知识依然存在于英语世界。很多概念，可以翻译成中文，但是不如直接使用英文更好。举一个例子：PostgreSQL的内存管理，有几个概念，MemoryContext, Block, Chunk，可以分别翻译成“内存上下文”，“内存块”，“内存切片”。但是我在阅读别人的IT技术文章的过程中觉得专业概念直接引用英文单词，比用中文翻译的概念更加清晰，通用概念如进程(Process)/线程(Thread)等除外。IT从业者几乎百分百都具备一定的英文阅读能力，所以本书在表述上用中文，而涉及的专业概念直接用英文单词，中英文夹杂，不会影响读者的阅读理解，反而更加清晰准确地阐述所要表达的内容。我在本书中秉承着专业概念直接使用英文单词，不进行翻译的原则。

未来PostgreSQL的发展势头肯定会越来越好，市面上也出现越来越多的相关高薪工作岗位。当你觉得自己不是走管理路线，当老板的料的时候，那么如果能够通过自己的勤奋学习，“彻底”掌握某种高技术，收获了满足感，又拿到比富土康的全蛋兄更高薪的工作，无需站流水线，可以在家上班，这也是不错的人生选择。我相信在每一个普通人的内心深处肯定有一种共识：穷尽毕生钻研一门技术到极精深的境界，做一枚优秀的匠人，衣食无忧，亦是一种幸福的人生。青青子衿，悠悠我心，此何人哉？

是为序！

***

## 本书的提纲和内容概述

这部分内容可以放在附录中，不要放在前言部分。

- 第一章，当然讲解如何安装PostgreSQL，搭建自己的学习和实验环境，自不待言。这也是循序渐进原则的开始。
- 第二章，介绍PostgreSQL的体系结构，让读者对这个数据库软件有一个大致的理解。这也是非常自然而然的事情。
- 第三章，下一步当然是要学习数据库的备份和恢复。手中有粮，心中不慌。作为一个DBA，掌握数据库备份和恢复的基本能力，是第一要务。数据库性能慢一点，不会死人。数据丢失才是导致你跑路的根本原因。当时为了学习备份和恢复，就需要掌握前导的知识和概念，所以本章介绍WAL(Write Ahead Log)和数据文件页面结构的基本知识。WAL是深入理解备份和恢复的核心概念，必须深入理解才能够顺利地学习后面的知识。
- 第四章，备份和恢复，无需废话。
- 第五章，Standby数据库的搭建。这是建立在第三和第四章至上的高级应用，也是面试和工作中必须掌握的知识。
- 第六章，逻辑复制技术，依然是前三章的延续的高级知识。
- 第七章， 。。。。

待续。。。。



