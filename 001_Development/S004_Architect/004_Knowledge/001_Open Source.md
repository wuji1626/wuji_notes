#1 9个主流的开源许可协议
关于开源许可  
现今存在的开源协议很多，而经过Open Source Initiative 组织通过批准的开源协议目前有60多种（http://www.opensource.org/licenses/alphabetical ）。我们在常见的开源协议如BSD, GPL, LGPL,MIT等都是OSI批准的协议。  
基本概念
1.Contributors 和 Recipients
Contributors（贡献者） ——指的是对某个开源软件或项目提供了代码（包括最初的或者修改过）的人或实体（退队、公司、组织等）。
按照贡献的先后可分为"创始人"（an initial Contributor）和"参与者"（subsequent Contributors）。

Recipients（获取者） ——指的是开源软件或项目的使用者。
显然，subsequent Contributors也属于Recipients之列。

2.Source Code 和 Object Code
Source Code ——指的是由各种语言写成的源代码 。
Object Code ——指的是Source Code经过编译后，生成的类似“类库”一样的，提供了各种接口供他人使用的目标代码 （就如,DLL、JAR等）。

3.Derivative Module 和 Separate Module
Derivative Module（衍生模块） ——指的是，依托或包含“最初的”或者“从别人处获取的”开源代码而产生的代码，是对“源代码模块”的增强、改善和延续。
Separate Module（独立模块） ——指的是，参考或借助“源代码”开发出来的独立的，不包含、不依赖于原“源代码模块”的功能模块。

在OSI网站上被列为主流及被广泛使用的许可 有：
■Apache License, 2.0 (Apache-2.0)  
■BSD 3-Clause "New" or "Revised" license (BSD-3-Clause)  
■BSD 3-Clause "Simplified" or "FreeBSD" license (BSD-2-Clause)  
■GNU General Public License (GPL)  
■GNU Library or "Lesser" General Public License (LGPL)  
■MIT license (MIT)  
■Mozilla Public License 1.1 (MPL-1.1)  
■Common Development and Distribution License (CDDL-1.0)  
■Eclipse Public License (EPL-1.0)  
注：原Common Public License 1.0已被Eclipse Public License (EPL-1.0)替代。  


##Apache License, 2.0 (Apache-2.0 )  
Apache Lience允许使用者修改和重新发布代码(以其他协议形式)，允许闭源商业发布和销售。  

Apache Lience鼓励代码共享和尊重原作者的著作权。  

使用Apache Licence协议，需要遵守以下规则：
1.需要给代码的用户一份Apache Lience；
2.如果你修改了代码，需要在被修改的文件中说明；
3.在延伸的代码中（修改或衍生的代码）需要带有原来代码中的协议、商标、专利声明和其他原来作者规定需要包含的说明。
4.如果再发布的产品中包含了Notice文件，则需要在Notice文件中带有Apache Lience。你可以在Notice中增加自己的许可，但不可以表现为对Apache Lience构成更改。

	Apache Licence是对商业应用友好的许可。使用者也可以在需要的时候修改代码来满足需要并作为开源或商业产品发布/销售。


##BSD开源协议（Berkerley Software Distribution）( BSD 3-Clause , BSD 2-Clause )
目前分为BSD 3-Clause和BSD 2-Clause。顾名思义，3-Clause包含3个条款，2-Clause只有两个。

BSD允许使用者修改和重新发布代码(以其他协议形式)，允许闭源商业发布和销售。

BSD鼓励代码共享的同时，要求尊重代码作者的著作权。

使用BSD协议，需要遵守以下规则（2-Clause则不带第3条）：
1.如果再发布的产品中包含源代码，则在源代码中必须带有原来代码中的BSD协议；
2.如果再发布的只是二进制类库/软件，则需要在类库/软件的文档那个和版权声明中包含原来代码中的BSD协议；
3.不可以用开源代码的“作者/机构的名字”或“原来产品的名字”做市场推广。

要点：商业软件可以使用，也可以修改使用BSD协议的代码。


##GPL ( GNU General Public License )
GPL的出发点是代码的开源/免费使用和引用/修改/衍生代码的开源/免费使用，但不允许修改后和衍生的代码做为闭源的商业软件发布和销售。

GPL具有“传染性”，只要在一个软件中使用(“使用”指类库引用,修改后的代码或者衍生代码)GPL协议的产品,则该软件产品必须也采用 GPL协议,既必须也是开源和免费。

GPL对商业发布的限制（引自Java视线论坛的Robbin）:
“GPL是针对软件源代码的版权,而不是针对软件编译后二进制版本的版权.你有权免费获得软件的源代码,但是你没有权力免费获得软件的二进制发行版本.GP对软件发行版本唯一的限制就是:你的发行版本必须把完整的源代码一同提供.”

使用GPL协议，需要遵守以下规则：
1、确保软件自始至终都以开放源代码形式发布，保护开发成果不被窃取用作商业发售。任何一套软 件，只要其中使用了受 GPL 协议保护的第三方软件的源程序，并向非开发人员发布时，软件本身也就自动成为受 GPL 保护并且约束的实体。也就是说，此时它必须开放源代码。
2、GPL 大致就是一个左侧版权（Copyleft，或译为“反版权”、“版权属左”、“版权所无”、“版责”等）的体现。你可以去掉所有原作的版权 信息，只要你保持开源，并且随源代码、二进制版附上 GPL 的许可证就行，让后人可以很明确地得知此软件的授权信息。GPL 精髓就是，只要使软件在完整开源 的情况下，尽可能使使用者得到自由发挥的空间，使软件得到更快更好的发展。
3、无论软件以何种形式发布，都必须同时附上源代码。例如在 Web 上提供下载，就必须在二进制版本（如果有的话）下载的同一个页面，清楚地提供源代码下载的链接。如果以光盘形式发布，就必须同时附上源文件的光盘。
4、开发或维护遵循 GPL 协议开发的软件的公司或个人，可以对使用者收取一定的服务费用。但还是一句老话——必须无偿提供软件的完整源代码，不得将源代码与服务做捆绑或任何变相捆绑销售。

由于GPL严格要求使用了GPL类库的软件产品必须使用GPL协议，所以商业软件就不适合采用使用GPL协议的开源代码。
要点：商业软件不能使用GPL协议的代码。


##LGPL ( GNU Library or "Lesser" General Public License )
与GPL的强制性开源不同的是，LGPL允许商业软件通过类库引用(link)的方式使用LGPL类库而不需要开源商业软件的代码。

但是如果修改LGPL协议的代码或者衍生,则所有修改的代码,涉及修改部分的额外代码和衍生的代码都必须采用LGPL协议。因此LGPL协议的开源代码很适合作为第三方类库被商业软件引用,但不适合希望以LGPL协议代码为基础,通过修改和衍生的方式做二次开发的商业软件采用。

要点：商业软件可以使用，但不能修改LGPL协议的代码。

##MIT ( MIT license )
[MIT许可证之名源自麻省理工学院（Massachusetts Institute of Technology, MIT），又称「X条款」（X License）或「X11条款」（X11 License）]
MIT是和BSD一样宽范的许可协议,作者只想保留版权,而无任何其他了限制.也就是说,你必须在你的发行版里包含原许可协议的声明,无论你是以二进制发布的还是以源代码发布的。
要点：商业软件可以使用，也可以修改MIT协议的代码，甚至可以出售MIT协议的代码。

##MPL ( Mozilla Public License 1.1 ) 
MPL协议允许免费重发布、免费修改，但要求修改后的代码版权归软件的发起者 。这种授权维护了商业软件的利益，它要求基于这种软件的修改无偿贡献版权给该软件。这样，围绕该软件的所有代码的版权都集中在发起开发人的手中。但MPL是允许修改，无偿使用得。MPL软件对链接没有要求。
要点：商业软件可以使用，也可以修改MPL协议的代码，但修改后的代码版权归软件的发起者。


##CDDL (Common Development and Distribution License ) 
CDDL（Common Development and Distribution License，通用开发与销售许可）开源协议，是MPL（Mozilla Public License）的扩展协议，它允许公共版权使用，无专利费，并提供专利保护，可集成于商业软件中，允许自行发布许可。
要点：商业软件可以使用，也可以修改CDDL协议的代码。


##Common Public License 1.0 (CPL-1.0 )（已废弃）
CPL是IBM提出的开源协议，主要用于IBM或跟IBM相关的开源软件/项目中（例如，Eclipse、Open Laszlo等）。


##EPL (Eclipse Public License 1.0 ) 
EPL允许Recipients任意使用、复制、分发、传播、展示、修改以及改后闭源的二次商业发布。

使用EPL协议，需要遵守以下规则：
1. 当一个Contributors将源码的整体或部分再次开源发布的时候,必须继续遵循EPL开源协议来发布,而不能改用其他协议发布.除非你得到了原“源码”Owner 的授权；
2. EPL协议下,你可以将源码不做任何修改来商业发布.但如果你要发布修改后的源码,或者当你再发布的是Object Code的时候,你必须声明它的Source Code是可以获取的,而且要告知获取方法；
3. 当你需要将EPL下的源码作为一部分跟其他私有的源码混和着成为一个Project发布的时候,你可以将整个Project/Product以私人的协议发布,但要声明哪一部分代码是EPL下的,而且声明那部分代码继续遵循EPL；
4. 独立的模块(Separate Module),不需要开源。

要点：商业软件可以使用，也可以修改EPL协议的代码，但要承担代码产生的侵权责任。
