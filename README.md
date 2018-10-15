# study-OOCSS-BEM
learn OOCSS BEM



OOCSS、SMACSS及BEM都是有关css的方法论（准确地说，其中BEM应该是一个完整的前端开发理论，不仅限于css），可作为实现优秀css架构（css architecture）的指南。

css易于理解，但应用和维护并不简单。在各种开发情景下，css都可能成为一个问题点。因此，我们编写和组织css应认真、用心。
OOCSS

OOCSS（Object Oriented CSS），字面意思是面向对象的CSS，是由Nicole Sullivan提出的css理论，其主要的两个原则是：

    Separate structure and skin（分离结构和主题）
    Separate container and content（分离容器和内容）

用一个例子来说明。请看下面这样的图文排列：

CSS Code复制内容到剪贴板 <div class="media media-shadow">        <div class="media-image-container">            <img class="media-image" src="rean.jpg" alt="">        </div>        <div class="media-body">            <p class="media-text">本作的主角，帝国北部地方贵族施瓦泽男爵的养子，也是托尔兹士官学校特科班“Ⅶ组”的成员。</p>        </div>    </div>       .media{        padding: 10px;    }    .media:after{        display: table;        clear: both;        content: " ";    }    .media-image-container{        float: left;        margin-right: 10px;    }    .media-image{        display: block;    }    .media-body{        overflow: hidden;    }    .media-shadow{        box-shadow: 1px 1px 3px rgba(0, 0, 0, .5);    }  
上面这段代码用media表示了这种图文排列的页面元素。如果把构成它的html、css及javascript（如果有）看做一个整体，那就相当于这是一个元件，或者说对象（object）。它可以在站点的任何地方被重用。

这样是如何体现OOCSS的两个原则的呢？
Separate structure and skin

分离结构和主题是在于将一些视觉样式效果（例如background、color）作为单独的“主题”来应用。在上面的例子中的阴影效果，没有被直接写在media的样式规则内，而是被单独写在了一个名为media-shadow的class中。因此，它成为了可选择、可拆分的主题。如果不需要对应主题，什么也不要加，如果需要，加上对应的class，就是这样的思路。
Separate container and content

分离容器和内容要求使页面元素不依赖于其所处位置。在上面的例子中，css的选择符都很短，无继承选择符（例如.header .media { }），所以，这个图文排列的元件，可以在任何地方使用，且会有一致的外观。

如果需要在特定的地方让这个元件看起来不一样一些，继续为这个元件增加class，将“不一样的部分”作为可配置的选项。元件的外观仍不依赖其所处位置。
操作指南

可以看出，OOCSS风格的css可以描述为两点：

    增加class
    不使用继承选择符

OOCSS追求元件的复用，其class命名比较抽象，一般不体现具体内容。
SMACSS

SMACSS（Scalable & Modular Architecture for CSS），是由Jonathan Snook提出的css理论。其主要原则有3条：

    Categorizing CSS Rules（为css分类）
    Naming Rules（命名规则）
    Minimizing the Depth of Applicability（最小化适配深度）

这些原则分别是什么意思呢？
Categorizing CSS Rules

这一点是SMACSS的核心。SMACSS认为css有5个类别，分别是：

    Base
    Layout（Major Components）
    Module（Minor Components）
    State
    Theme

Base Rules，基础样式，描述的是任何场合下，页面元素的默认外观。它的定义不会用到class和ID。css reset也属于此类。

Layout Rules，布局样式。它和后面的Module Rules一同，描述的是页面中的各类具体元素。元素是有层次级别之分的，Layout Rules属于较高的一层，它可以作为层级较低的Module Rules元素的容器。左右分栏、栅格系统等都属于布局样式。

Module Rules，模块样式。它可以是一个产品列表，一个导航条。一般来说，Module Rules定义的元素放置于前面说的Layout Rules元素之内。模块是独立的，可以在各种场合重用。

State Rules，状态样式，描述的是任一元素在特定状态下的外观。例如，一个消息框可能有success和error两种状态，导航条中的任一项都可能有current状态。

继续OOCSS中的例子，下面新增的让元素不显示的is-hidden就属于State Rules：

CSS Code复制内容到剪贴板 <div class="media media-shadow is-hidden">        ...    </div>       .is-hidden{        display: none;    }  
Theme Rules，主题样式，描述了页面主题外观，一般是指颜色、背景图。Theme Rules可以修改前面4个类别的样式，且应和前面4个类别分离开来（便于切换，也就是“换肤”）。SMACSS的Theme Rules不要求使用单独的class命名，也就是说，你可以在Module Rules中定义.mod { }然后在Theme Rules中也用.mod { }来定义需要修改的部分。
Naming Rules

Naming Rules是说在想class等的命名时，考虑用命名体现样式对应的类别。

按照前面5种的划分，Layout Rules用l-或layout-这样的前缀，例如：.l-header、.l-sidebar。

Module Rules用模块本身的命名，例如图文排列的.media、.media-image。

State Rules用is-前缀，例如：.is-active、.is-hidden。

Theme Rules如果作为单独class，用theme-前缀，例如.theme-a-background、.theme-a-shadow。

Base Rules不会用到class和ID，是以标签选择符为主的样式，例如p、a，无需命名。

命名规则不需要严格遵守，可以根据实际情况和自身喜好做其他的约定。记录自己的约定（写文档），然后遵守，就是可行的。
Minimizing the Depth of Applicability

字面翻译是最小化适配深度。通过一个简单的描述来说明：

CSS Code复制内容到剪贴板 /* depth 1 */   .sidebar ul h3 { }       /* depth 2 */   .sub-title { }  
上下两端css的区别在于html和css的耦合度。可以想到，由于上面的样式规则使用了继承选择符，因此对于html的结构实际是有一定依赖的。如果把h3元素搬到另一个位置，就有可能不再具有这些样式。对应的，下面的样式规则只有一个选择符，因此不依赖于特定html结构，只要为元素添加class，就可以获得对应样式。

当然，继承选择符是有用的，它可以减少因相同命名引发的样式冲突（常发生于多人协作开发）。但是，我们不应过度使用，在不造成样式冲突的允许范围之内，尽可能使用短的、不限定html结构的选择符。这就是SMACSS的最小化适配深度的意义。

看起来，这一点和OOCSS的分离容器和内容的原则非常相似。
主要目标

SMACSS着力于实现两个主要目标：

    更语义化的html和css
    降低对特定html结构的依赖

BEM

BEM，即Block，Element，Modifier，是由Yandex（俄罗斯最著名的互联网企业）的开发团队提出的前端开发理论。BEM通过Block、Element、Modifier来描述页面。

Block是页面中独立存在的区块，可以在不同场合下被重用。每个页面都可以看做是多个Block组成。

Element是构成Block的元素，只有在对应Block内部才具有意义，是依赖于Block的存在。

Modifier是描述Block或Element的属性或状态。同一Block或Element可以有多个Modifier。

这三部分结合在一起，可以体现在class命名上，从而为开发者提供更友好、更有意义的css组织方式。其形式是：

CSS Code复制内容到剪贴板 .block { }    .block_modifier { }    .block__element { }    .block__element_modifier { }  
再回到前面OOCSS的那个图文排列的例子，对应用BEM的写法的话就是：

CSS Code复制内容到剪贴板 <div class="media media_shadow">        <div class="media__image-container">            <img class="media__image" src="rean.jpg" alt="">        </div>        <div class="media__body">            <p class="media__text">本作的主角，帝国北部地方贵族施瓦泽男爵的养子，也是托尔兹士官学校特科班“Ⅶ组”的成员。</p>        </div>    </div>  
这样的写法的好处是，在class命名上以约定的形式携带了更多有用信息。在多人合作的时候，新接手这个项目的人，也可以很容易从class命名上分辨出来，哪些部分是Block，哪些是对应的Element，哪些是Modifier，并进一步推断出哪部分html可以独立使用。

BEM是完整的前端开发理论，这里只是提到了它采用的css的class命名规则。可以看出，BEM的命名规则可以使代码更易于维护。
