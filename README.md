# pg16
A book about the internals of PostgreSQL open source database. It is written by using simplified Chinese language.
Any suggestion please send to itgotousa@gmail.com. Thx!

## 自序

在IT行业浪迹了二十多年以后，我决定写人生第一本IT技术方面的书，那就是你面前的这本《PostgreSQL老鸟之路 - 运维篇》。也许是因为命运的安排，十多年前我来到美国后，阴差阳错地进入了数据库管理员DBA（Database Administrator）的行业，从此我的职业生涯的大部分时间都是和Oracle/PostgreSQL等数据库软件打交道，所以本书的内容是对著名的开源数据库软件PostgreSQL运维管理方面等相关技术进行讲解，大约类似“某某某从入门到精通”的套路，但绝对不会教你如何从删库到跑路。

写书有三种类型，编书，著书和编著。此三者的区别显而易见：所谓编书，就是把相关资料汇编成册，内容是别人的，自己做的工作是汇集整理。所谓著书，就要把自己消化理解后的知识和经验写出来，把茶壶里的饺子倒出来。编著则是居于“编书”和“著书”中间的层次。很显然，“著书”层次最高，要花费的精力和心血最多，是个良心活。曹氏曰：“满纸荒唐言，一把辛酸泪。都云作者痴，谁解其中味”，此语真乃著书的天花板。我亦想在PostgreSQL领域整齐百家诸语，成一家之言，竭尽自己有限的才智来“著”一本书，然后藏之名山，以俟后世君子。至于能否达到此种我希冀的境界，只能说，尽吾志而不能至者，无悔矣。

从二十世纪中后期到二十一世纪上半期，是IT技术革命大爆发的时代，此乃人类之幸事。但IT科技的快速发展也带来了某些副作用，其中一个副作用就是劳动者需要更长的时间和精力来学习，才具备从事专业领域工作的能力。君不见，学士，硕士，博士不断内卷，直逼壮士，猛士，乃至烈士。等到博士毕业后，蓦然发现自己已经宿舍明镜悲白发，可怜依然单身狗。当代年轻人晚婚，不婚，少子化，躺平等社会问题越来越普遍。在IT技术领域，随着技术复杂度的提高，进入其中的壁垒越来越高，后来的学习者想登堂入室，难度越来越大。譬如Linux内核的开发，对于初学者来说，学习曲线异常陡峭，除非投入巨大的精力和时间成本，否则连老鸟的谈话都很难听懂。如果没有足够的利益吸引，想进入技术领域的年轻人只会越来越少。老鸟终究会慢慢凋零，而新鲜血液补充不足，是各种IT技术发展过程中普遍面临的问题。我写本书的目的就是希望能够在学习者进入PostgreSQL数据库领域的过程中，为他们降低学习门槛做一些微薄的贡献，另一方面也是对自己职业生涯的一个总结。

或云：坊间相关PostgreSQL的书籍早已汗牛充栋，你写的这本书又有什么独特的价值？答曰：我把本书的奋斗目标定为中文世界里帮助学习者对PostgreSQL登堂入室最好的拐杖之一。因为我对目前所有的PostgreSQL相关的技术书籍都不甚满意，而不满则是技术进步的根本动力。在我学习PostgreSQL的过程中，充满了痛苦。很多初级读物流于肤浅，而深入的书籍因为缺乏前导知识，又让初学者不知所云。我不得不像一只勤劳的小蜜蜂，不断在互联网的世界里飞来飞去，四处艰难地采集我寻找的干货。自己要花费大量时间，不断做实验，分析源码，做学习笔记，才能够有一点点收获。我的学习笔记上寥寥数语心得的背后，都是一把辛酸泪。我不想后学者再重复这个痛苦的过程，就产生了一种强烈的冲动：想写一本关于PostgreSQL的书，循序渐进地把基本知识和深入知识融合，让读者只要按顺序学习本书，就能够以较少的痛苦代价，达到一定程度的“登堂入室”的境界。

## 本书写作遵循的原则
- **说自己的话** ：既然是著书，就是倾倒自己肚子里面货真价实的饺子。不说抄来的话，力争每一句话都是自己完全理解的，然后再传递给读者。这是对读者负责，也对自己有益。所以我力争本书中的每一句话都是自己的所思所想，是自己的语言，是能够理解的人话。上要对得起天地，中要对得起读者，下要对得起自己的良心。

- **循序渐进** ：人类学习知识的过程肯定是循序渐进的：如果概念B是建立在概念A的基础上，那么学习的时候必须先学习搞懂概念A，然后才能学习概念B。这当然是废话，然而很多书籍和教程往往忽视这个根本性的原则，导致读者如鲠在喉，想吐槽却无处说理，吾亦深受其苦。所以“循序渐进”是本书遵循的最高原则。我写作时候设定的目标就是：只要读者按照顺序反复学习本书，就能够从门外汉到登堂入室，整个过程非常流畅，学习时候的痛苦感亦大为减少。

- **内外兼修** ： 当我们学习Oracle, SQL Server等商业闭源软件的时候，只可能学习“外在”的功能性知识，了解这个软件的功能，却不知道它的内部是如何实现的。这就是老话说的“知其然而不知其所以然”。我从事Oracle DBA十几年了，但当你问我对Oracle数据库技术掌握到什么程度，我只能默然，脸红，承认自己懂的太少。这是因为Oracle的源代码是商业机密，不可能公布天下的。所以无论我多么努力，学习闭源软件技术的时候，总有一种隔靴挠痒的感觉。我们知道，如果感觉自己彻底掌握了一个技术，就会带来巨大的成就感和自信心，这是人性。这种成就感和自信心在开源软件身上才能够获得。所以本书在讲解外在的PostgreSQL各种功能知识的同时，会引导读者阅读背后的关键源码，让读者能够内外兼修，通过源代码和软件功能的互相验证，达到“知其然且知其所以然”的境界，增加学习时候的满足感，甚至某种幸福感。

- **化繁为简** ： 大道至简，计算机的终极道理是0和1的关系。无论一个计算机技术多么复杂，包括现在当红炸子鸡的ChatGPT等AI技术，对于一个真正掌握此技术的人来说，都可以浅显易懂的方式传递给后学者。面对庞大繁杂的源代码，用简明扼要的图来描述其关键思想，再引用关键代码配合展示重要的细节和要点，是帮助读者迅速理解软件技术背后秘密的重要途径。本书所有的图都是我花费大量心血手工绘制的，且我确保每张图都尽可能简单，易于理解和记忆，这是本书一大特色。

- **中英文夹杂** ： IT技术诞生于西方文明世界，目前大量高质量的相关知识依然存在于英语世界。很多概念，可以翻译成中文，但是不如直接使用英文更好。举一个例子：PostgreSQL的内存管理，有几个概念，MemoryContext, Block, Chunk，可以分别翻译成“内存上下文”，“内存块”，“内存片”。但是我在阅读别人的IT技术文章的过程中觉得专业概念直接引用英文单词，比用中文翻译的概念更加清晰。夹杂在汉字中的英文单词特立独行，很容易让我把它和周围的汉字区别出来。IT从业者几乎百分百都具备一定的英文阅读能力，本书在表述上用中文，而涉及的专业概念尽量采用英文单词，这种做法不仅不会影响读者的阅读理解，反而更加清晰准确地阐述所要表达的内容。

目前PostgreSQL的发展势头越来越好，市面上也正在出现越来越多的相关高薪工作岗位。在人生的某一个阶段，你可能会沮丧地发现自己不是当老板的料。但是如果能够通过自己的勤奋学习，“彻底”掌握某种高技术，收获了满足感，薪水又超过富土康的张全蛋，甚至可以在家远程上班，这难道不是很好的人生选择吗？我相信在每个普通人的内心深处肯定有一种共识：做一枚匠人，穷尽毕生钻研一门技术到精深的境界，衣食无忧，又无需操心过多烦人的人际关系，亦是一种幸福的人生。青青子衿，悠悠我心，此何人哉？那就是你我芸芸众生啊。

是为序。
