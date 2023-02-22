# pg16
A book about the internals of PostgreSQL open source database

## 前言

在IT行业浪迹了二十多年以后，我想写人生第一本关于IT技术的书，那就是本书。也许是因为命运的安排，十多年前我来到美国后，阴差阳错地进入了数据库管理员DBA（Database Administrator）的行业，自己职业生涯的大部分时间是和Oracle等数据库打交道，所以本书是关于著名的开源数据库PostgreSQL的书，是类似“某某某从入门到精通”之类的内容，但绝对不会教你从删库到跑路。

写书有三种类型，编书，著书和编著。其中的区别是显而易见的。所谓编书，就是把能够找到的相关资料汇编成册，内容是别人的，自己做的工作是汇集整理。所谓著书，就是把自己理解的知识和经验写出来，把肚子里面的饺子倒出来。编著则是居于“编”和“著”中间的层次。很显然，“著书”层次最高，它是花费精力和心血最多的工作，产生的书也最容易得到读者的赏识。曹雪芹说过：“满纸荒唐言，一把辛酸泪。都云作者痴，谁解其中味”。这估计是著书的天花板了。我亦想用自己最大的心血来“著”这本书，至于能不能得到读者的认同，任由读者去判断，我自不多言。

从二十世纪中后期到二十一世纪前期，是信息技术革命大爆发的时代。这自然是一件好事，但是也带来了一些副作用。其中一个典型的副作用就是技术壁垒越来越高，导致后来的学习者想登堂入室的难度越来越大，也导致技术发展的新鲜血液补充不足。譬如Linux内核的开发，对于初学者来说，学习曲线异常陡峭，除非投入巨大的学习精力和时间成本，否则很难听懂Linux内核老鸟的谈话。老鸟们终究会老去，新鲜血液补充不足，是各种技术面临的普遍问题。劳动者需要更长的学习时间才能从事某一个专业领域，从而导致当代年轻人不婚，少子化，躺平等社会问题。这不能不说是对技术进步的讽刺，而且这个问题基本无解。我写本书的目的当然不是为了，也无法解决这些社会问题，而是希望能够在学习者进入PostgreSQL数据库领域的过程中，为他们降低学习门槛做一些力所能及的贡献。另一方面也是对自己职业生涯的一个负责任的总结。

或云：坊间相关PostgreSQL的书籍早已汗牛充栋，你写的这本书又有什么价值？答曰：因为我对目前所有的关于PostgreSQL的书籍不甚满意。不满是技术进步的动力。在我学习PostgreSQL的过程中，充满了痛苦。很多初级读物流于肤浅，而深入的书籍因为前导概念和知识的缺乏，又让初学者不知所云。我不得不像一只勤劳的小蜜蜂，不断在互联网的世界里飞来飞去，四处艰难地采集我寻找的干货知识点，加上自己不断做笔记和做实验，分析源码，花费了大量时间，才能够有一点点收获。我不想后学者再反复重复这个痛苦过程，就想循序渐进地把基本知识和深入知识融合在一起，让读者只要按顺序学习本书，就能够以较少的痛苦代价，达到一定程度的“登堂入室”的境界，为进一步提高技术水平打下良好的基础。不让别的学习者像自己一样痛苦，是我写本书的强烈冲动。希望本书能够达到我的这个目标。

## 本书写作遵循的原则
- **循序渐进的原则** ： 人类学习知识的过程肯定是循序渐进的。如果概念B是建立在概念A的基础上，那么学习的时候必须先学习搞懂概念A，然后才能学习概念B。这当然是废话。然而很多书籍和教程往往忽视这个根本性的原则，导致读者如鲠在喉，想吐槽却无处说理。我亦深受其苦，所以“循序渐进”是本书遵循的最高原则。只要读者按照顺序学习本书，整个过程是非常流畅的，学习时候的痛苦感减少很多。

- **基本知识和深入知识互相验证的原则** ： 当我们学习Oracle, SQL Server等商业闭源软件的时候，只可能学习“外在”的功能性知识，了解这个软件的功能，却不知道它的内部是如何实现的，就是老话说的“知其然而不知其所以然”。我从事Oracle DBA十几年了，但是当你问我对Oracle数据库掌握到什么程度，我只能默然，脸红，承认自己懂的太少。感觉自己彻底掌握一个技术会带来巨大的成就感和自信心。这种成就感和自信心只可能在开源软件身上才能够获得。所以本书在讲解基本的PostgreSQL知识的同时，会引导读者阅读背后的关键源码，让读者能够内外互相验证，知其然而且知其所以然，增加彻底掌握的满足感，进一步增加学习的兴趣。目前PostgreSQL的发展势头非常好，导致市面上出现越来越多的相关高薪工作岗位。既能够“彻底”掌握技术，把自己变成大国工匠，又很容易获得高薪工作，何乐而不为呢？让我们一起努力吧。

- **中英文夹杂原则** ： IT技术是西方文明的舶来品，大量高质量的知识依然存在于英语世界。很多概念，可以翻译成中文，但是不如直接使用英文更好。举一个例子：PostgreSQL的内存管理，有几个概念，MemoryContext, Block, Chunk，可以分别翻译成“内存上下文”，“内存块”，“内存切片”。但是我在学习的过程中觉得直接引用英文单词，比用中文翻译的概念更加清晰。IT从业者几乎百分百都具备一定的英文阅读能力，所以在行文上用中文，而涉及的概念直接用英文，导致中英文夹杂，不仅不是缺点，反而更加清晰准确地阐述所要表达的内容。所以我在本书中坚持专业概念直接使用英文单词的方式。
- **说自己的话的原则** ：既然是著书，就是倾倒自己肚子里面货真价实的饺子，所以不说抄来的话。力争每一句话都是自己完全理解的，然后再传递给读者。这是对读者负责，也是对自己负责。所谓教学相长，作者努力让读者明白自己传达的内容的过程中，也是自己不断提高的过程。说自己理解的话，对作者和读者都是大大的好事，所以我坚持本书中的每一句话都是自己的所思所想的原则。

## 本书的提纲和内容概述
- 第一章，当然讲解如何安装PostgreSQL，搭建自己的学习和实验环境，自不待言。这也是循序渐进原则的开始。
- 第二章，介绍PostgreSQL的体系结构，让读者对这个数据库软件有一个大致的理解。这也是非常自然而然的事情。
- 第三章，下一步当然是要学习数据库的备份和恢复。手中有粮，心中不慌。作为一个DBA，掌握数据库备份和恢复的基本能力，是第一要务。数据库性能慢一点，不会死人。数据丢失才是导致你跑路的根本原因。当时为了学习备份和恢复，就需要掌握前导的知识和概念，所以本章介绍WAL(Write Ahead Log)和数据文件页面结构的基本知识。WAL是深入理解备份和恢复的核心概念，必须深入理解才能够顺利地学习后面的知识。
- 第四章，备份和恢复，无需废话。
- 第五章，Standby数据库的搭建。这是建立在第三和第四章至上的高级应用，也是面试和工作中必须掌握的知识。
- 第六章，逻辑复制技术，依然是前三章的延续的高级知识。
- 第七章， 。。。。





