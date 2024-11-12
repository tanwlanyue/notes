# 第2章 `auto`

**CHAPTER 2 `auto`**

从概念上来说，`auto`要多简单有多简单，但是它比看起来要微妙一些。使用它可以存储类型，当然，它也会犯一些错误，而且比之手动声明一些复杂类型也会存在一些性能问题。此外，从程序员的角度来说，如果按照符合规定的流程走，那 `auto`类型推导的一些结果是错误的。当这些情况发生时，对我们来说引导 `auto`产生正确的结果是很重要的，因为严格按照说明书上面的类型写声明虽然可行但是最好避免。

本章简单的覆盖了 `auto`的里里外外。

## 条款五：优先考虑 `auto`而非显式类型声明

**Item 5: Prefer `auto` to explicit type declarations**

哈，开心一下：

````cpp
int x;
````

等等，该死！我忘记了初始化 `x`，所以 `x`的值是不确定的。它可能会被初始化为0，这得取决于工作环境。哎。

别介意，让我们转换一个话题， 对一个局部变量使用解引用迭代器的方式初始化：

````cpp
template<typename It>           //对从b到e的所有元素使用
void dwim(It b, It e)           //dwim（“do what I mean”）算法
{
    while (b != e) {
        typename std::iterator_traits<It>::value_type
        currValue = *b;
        …
    }
}
````

嘿！`typename std::iterator_traits<It>::value_type`是想表达迭代器指向的元素的值的类型吗？我无论如何都说不出它是多么有趣这样的话，该死！等等，我早就说过了吗？

好吧，声明一个局部变量，类型是一个闭包，闭包的类型只有编译器知道，因此我们写不出来，该死!

该死该死该死，C++编程不应该是这样不愉快的体验。

别担心，它只在过去是这样，到了C++11所有的这些问题都消失了，这都多亏了 `auto`。`auto`变量从初始化表达式中推导出类型，所以我们必须初始化。这意味着当你在现代化C++的高速公路上飞奔的同时你不得不对只声明不初始化变量的老旧方法说拜拜：

````cpp
int x1;                         //潜在的未初始化的变量
auto x2;                        //错误！必须要初始化
auto x3 = 0;                    //没问题，x已经定义了
````

而且即使使用解引用迭代器初始化局部变量也不会对你的高速驾驶有任何影响

````cpp
template<typename It>           //如之前一样
void dwim(It b,It e)
{
    while (b != e) {
        auto currValue = *b;
        …
    }
}
````

因为使用[Item2](../1.DeducingTypes/item2.md)所述的 `auto`类型推导技术，它甚至能表示一些只有编译器才知道的类型：

````cpp
auto derefUPLess = 
    [](const std::unique_ptr<Widget> &p1,       //用于std::unique_ptr
       const std::unique_ptr<Widget> &p2)       //指向的Widget类型的
    { return *p1 < *p2; };                      //比较函数
````

很酷对吧，如果使用C++14，将会变得更酷，因为*lambda*表达式中的形参也可以使用 `auto`：

````cpp
auto derefLess =                                //C++14版本
    [](const auto& p1,                          //被任何像指针一样的东西
       const auto& p2)                          //指向的值的比较函数
    { return *p1 < *p2; };
````

尽管这很酷，但是你可能会想我们完全不需要使用 `auto`声明局部变量来保存一个闭包，因为我们可以使用 `std::function`对象。没错，我们的确可以那么做，但是事情可能不是完全如你想的那样。当然现在你可能会问，`std::function`对象到底是什么。让我来给你解释一下。

`std::function`是一个C++11标准模板库中的一个模板，它泛化了函数指针的概念。与函数指针只能指向函数不同，`std::function`可以指向任何可调用对象，也就是那些像函数一样能进行调用的东西。当你声明函数指针时你必须指定函数类型（即函数签名），同样当你创建 `std::function`对象时你也需要提供函数签名，由于它是一个模板所以你需要在它的模板参数里面提供。举个例子，假设你想声明一个 `std::function`对象 `func`使它指向一个可调用对象，比如一个具有这样函数签名的函数，

````cpp
bool(const std::unique_ptr<Widget> &,           //C++11
     const std::unique_ptr<Widget> &)           //std::unique_ptr<Widget>
                                                //比较函数的签名
````

你就得这么写：

````cpp
std::function<bool(const std::unique_ptr<Widget> &,
                   const std::unique_ptr<Widget> &)> func;
````

因为*lambda*表达式能产生一个可调用对象，所以我们现在可以把闭包存放到 `std::function`对象中。这意味着我们可以不使用 `auto`写出C++11版的 `derefUPLess`：

````cpp
std::function<bool(const std::unique_ptr<Widget> &,
                   const std::unique_ptr<Widget> &)>
derefUPLess = [](const std::unique_ptr<Widget> &p1,
                 const std::unique_ptr<Widget> &p2)
                { return *p1 < *p2; };
````

语法冗长不说，还需要重复写很多形参类型，使用 `std::function`还不如使用 `auto`。用 `auto`声明的变量保存一个和闭包一样类型的（新）闭包，因此使用了与闭包相同大小存储空间。实例化 `std::function`并声明一个对象这个对象将会有固定的大小。这个大小可能不足以存储一个闭包，这个时候 `std::function`的构造函数将会在堆上面分配内存来存储，这就造成了使用 `std::function`比 `auto`声明变量会消耗更多的内存。并且通过具体实现我们得知通过 `std::function`调用一个闭包几乎无疑比 `auto`声明的对象调用要慢。换句话说，`std::function`方法比 `auto`方法要更耗空间且更慢，还可能有*out-of-memory*异常。并且正如上面的例子，比起写 `std::function`实例化的类型来，使用 `auto`要方便得多。在这场存储闭包的比赛中，`auto`无疑取得了胜利（也可以使用 `std::bind`来生成一个闭包，但在[Item34](../6.LambdaExpressions/item34.md)我会尽我最大努力说服你使用*lambda*表达式代替 `std::bind`)

使用 `auto`除了可以避免未初始化的无效变量，省略冗长的声明类型，直接保存闭包外，它还有一个好处是可以避免一个问题，我称之为与类型快捷方式（type shortcuts）有关的问题。你将看到这样的代码——甚至你会这么写：

````cpp
std::vector<int> v;
…
unsigned sz = v.size();
````

`v.size()`的标准返回类型是 `std::vector<int>::size_type`，但是只有少数开发者意识到这点。`std::vector<int>::size_type`实际上被指定为无符号整型，所以很多人都认为用 `unsigned`就足够了，写下了上述的代码。这会造成一些有趣的结果。举个例子，在**Windows 32-bit**上 `std::vector<int>::size_type`和 `unsigned`是一样的大小，但是在**Windows 64-bit**上 `std::vector<int>::size_type`是64位，`unsigned`是32位。这意味着这段代码在Windows 32-bit上正常工作，但是当把应用程序移植到Windows 64-bit上时就可能会出现一些问题。谁愿意花时间处理这些细枝末节的问题呢？

所以使用 `auto`可以确保你不需要浪费时间：

````cpp
auto sz =v.size();                      //sz的类型是std::vector<int>::size_type
````

你还是不相信使用 `auto`是多么明智的选择？考虑下面的代码：

````cpp
std::unordered_map<std::string, int> m;
…

for(const std::pair<std::string, int>& p : m)
{
    …                                   //用p做一些事
}
````

看起来好像很合情合理的表达，但是这里有一个问题，你看到了吗？

要想看到错误你就得知道 `std::unordered_map`的*key*是 `const`的，所以*hash table*（`std::unordered_map`本质上的东西）中的 `std::pair`的类型不是 `std::pair<std::string, int>`，而是 `std::pair<const std::string, int>`。但那不是在循环中的变量 `p`声明的类型。编译器会努力的找到一种方法把 `std::pair<const std::string, int>`（即*hash table*中的东西）转换为 `std::pair<std::string, int>`（`p`的声明类型）。它会成功的，因为它会通过拷贝 `m`中的对象创建一个临时对象，这个临时对象的类型是 `p`想绑定到的对象的类型，即 `m`中元素的类型，然后把 `p`的引用绑定到这个临时对象上。在每个循环迭代结束时，临时对象将会销毁，如果你写了这样的一个循环，你可能会对它的一些行为感到非常惊讶，因为你确信你只是让成为 `p`指向 `m`中各个元素的引用而已。

使用 `auto`可以避免这些很难被意识到的类型不匹配的错误：

````cpp
for(const auto& p : m)
{
    …                                   //如之前一样
}
````

这样无疑更具效率，且更容易书写。而且，这个代码有一个非常吸引人的特性，如果你获取 `p`的地址，你确实会得到一个指向 `m`中元素的指针。在没有 `auto`的版本中 `p`会指向一个临时变量，这个临时变量在每次迭代完成时会被销毁。

前面这两个例子——应当写 `std::vector<int>::size_type`时写了 `unsigned`，应当写 `std::pair<const std::string, int>`时写了 `std::pair<std::string, int>`——说明了显式的指定类型可能会导致你不想看到的类型转换。如果你使用 `auto`声明目标变量你就不必担心这个问题。

基于这些原因我建议你优先考虑 `auto`而非显式类型声明。然而 `auto`也不是完美的。每个 `auto`变量都从初始化表达式中推导类型，有一些表达式的类型和我们期望的大相径庭。关于在哪些情况下会发生这些问题，以及你可以怎么解决这些问题我们在[Item2](../1.DeducingTypes/item2.md)和[6](../2.Auto/item6.md)讨论，所以这里我不再赘述。我想把注意力放到你可能关心的另一点：使用auto代替传统类型声明对源码可读性的影响。

首先，深呼吸，放松，`auto`是**可选项**，不是**命令**，在某些情况下如果你的专业判断告诉你使用显式类型声明比 `auto`要更清晰更易维护，那你就不必再坚持使用 `auto`。但是要牢记，C++没有在其他众所周知的语言所拥有的类型推导（*type inference*）上开辟新土地。其他静态类型的过程式语言（如C#、D、Sacla、Visual Basic）或多或少都有等价的特性，更不必提那些静态类型的函数式语言了（如ML、Haskell、OCaml、F#等）。在某种程度上，这是因为动态类型语言，如Perl、Python、Ruby等的成功；在这些语言中，几乎没有显式的类型声明。软件开发社区对于类型推导有丰富的经验，他们展示了在维护大型工业强度的代码上使用这种技术没有任何争议。

一些开发者也担心使用 `auto`就不能瞥一眼源代码便知道对象的类型，然而，IDE扛起了部分担子（也考虑到了[Item4](../1.DeducingTypes/item4.md)中提到的IDE类型显示问题），在很多情况下，少量显示一个对象的类型对于知道对象的确切类型是有帮助的，这通常已经足够了。举个例子，要想知道一个对象是容器还是计数器还是智能指针，不需要知道它的确切类型。一个适当的变量名称就能告诉我们大量的抽象类型信息。

事实是显式指定类型通常只会引入一些微妙的错误，无论是在正确性还是效率方面。而且，如果初始化表达式的类型改变，则 `auto`推导出的类型也会改变，这意味着使用 `auto`可以帮助我们完成一些重构工作。举个例子，如果一个函数返回类型被声明为 `int`，但是后来你认为将它声明为 `long`会更好，调用它作为初始化表达式的变量会自动改变类型，但是如果你不使用 `auto`你就不得不在源代码中挨个找到调用地点然后修改它们。

**请记住：**

+ `auto`变量必须初始化，通常它可以避免一些移植性和效率性的问题，也使得重构更方便，还能让你少打几个字。
+ 正如[Item2](../1.DeducingTypes/item2.md)和[6](../2.Auto/item6.md)讨论的，`auto`类型的变量可能会踩到一些陷阱。

## 条款六：`auto`推导若非己愿，使用显式类型初始化惯用法

**Item 6: Use the explicitly typed initializer idiom when `auto` deduces undesired types**

在[Item5](../2.Auto/item5.md)中解释了比起显式指定类型使用 `auto`声明变量有若干技术优势，但是有时当你想向左转 `auto`却向右转。举个例子，假如我有一个函数，参数为 `Widget`，返回一个 `std::vector<bool>`，这里的 `bool`表示 `Widget`是否提供一个独有的特性。

````cpp
std::vector<bool> features(const Widget& w);
````

更进一步假设第5个*bit*表示 `Widget`是否具有高优先级，我们可以写这样的代码：

````cpp
Widget w;
…
bool highPriority = features(w)[5];     //w高优先级吗？
…
processWidget(w, highPriority);         //根据它的优先级处理w
````

这个代码没有任何问题。它会正常工作，但是如果我们使用 `auto`代替 `highPriority`的显式指定类型做一些看起来很无害的改变：

````cpp
auto highPriority = features(w)[5];     //w高优先级吗？
````

情况变了。所有代码仍然可编译，但是行为不再可预测：

````cpp
processWidget(w,highPriority);          //未定义行为！
````

就像注释说的，这个 `processWidget`是一个未定义行为。为什么呢？答案有可能让你很惊讶，使用 `auto`后 `highPriority`不再是 `bool`类型。虽然从概念上来说 `std::vector<bool>`意味着存放 `bool`，但是 `std::vector<bool>`的 `operator[]`不会返回容器中元素的引用（这就是 `std::vector::operator[]`可返回**除了 `bool`以外**的任何类型），取而代之它返回一个 `std::vector<bool>::reference`的对象（一个嵌套于 `std::vector<bool>`中的类）。

`std::vector<bool>::reference`之所以存在是因为 `std::vector<bool>`规定了使用一个打包形式（packed form）表示它的 `bool`，每个 `bool`占一个*bit*。那给 `std::vector`的 `operator[]`带来了问题，因为 `std::vector<T>`的 `operator[]`应当返回一个 `T&`，但是C++禁止对 `bit`s的引用。无法返回一个 `bool&`，`std::vector<bool>`的 `operator[]`返回一个**行为类似于** `bool&`的对象。要想成功扮演这个角色，`bool&`适用的上下文 `std::vector<bool>::reference`也必须一样能适用。在 `std::vector<bool>::reference`的特性中，使这个原则可行的特性是一个可以向 `bool`的隐式转化。（不是 `bool&`，是**`bool`**。要想完整的解释 `std::vector<bool>::reference`能模拟 `bool&`的行为所使用的一堆技术可能扯得太远了，所以这里简单地说隐式类型转换只是这个大型马赛克的一小块）

有了这些信息，我们再来看看原始代码的一部分：

````cpp
bool highPriority = features(w)[5];     //显式的声明highPriority的类型
````

这里，`features`返回一个 `std::vector<bool>`对象后再调用 `operator[]`，`operator[]`将会返回一个 `std::vector<bool>::reference`对象，然后再通过隐式转换赋值给 `bool`变量 `highPriority`。`highPriority`因此表示的是 `features`返回的 `std::vector<bool>`中的第五个*bit*，这也正如我们所期待的那样。

然后再对照一下当使用 `auto`时发生了什么：

````cpp
auto highPriority = features(w)[5];     //推导highPriority的类型
````

同样的，`features`返回一个 `std::vector<bool>`对象，再调用 `operator[]`，`operator[]`将会返回一个 `std::vector<bool>::reference`对象，但是现在这里有一点变化了，`auto`推导 `highPriority`的类型为 `std::vector<bool>::reference`，但是 `highPriority`对象没有第五*bit*的值。

这个值取决于 `std::vector<bool>::reference`的具体实现。其中的一种实现是这样的（`std::vector<bool>::reference`）对象包含一个指向机器字（*word*）的指针，然后加上方括号中的偏移实现被引用*bit*这样的行为。然后再来考虑 `highPriority`初始化表达的意思，注意这里假设 `std::vector<bool>::reference`就是刚提到的实现方式。

调用 `features`将返回一个 `std::vector<bool>`临时对象，这个对象没有名字，为了方便我们的讨论，我这里叫他 `temp`。`operator[]`在 `temp`上调用，它返回的 `std::vector<bool>::reference`包含一个指向存着这些*bit*s的一个数据结构中的一个*word*的指针（`temp`管理这些*bit*s），还有相应于第5个*bit*的偏移。`highPriority`是这个 `std::vector<bool>::reference`的拷贝，所以 `highPriority`也包含一个指针，指向 `temp`中的这个*word*，加上相应于第5个*bit*的偏移。在这个语句结束的时候 `temp`将会被销毁，因为它是一个临时变量。因此 `highPriority`包含一个悬置的（*dangling*）指针，如果用于 `processWidget`调用中将会造成未定义行为：

````cpp
processWidget(w, highPriority);         //未定义行为！
                                        //highPriority包含一个悬置指针！
````

`std::vector<bool>::reference`是一个代理类（*proxy class*）的例子：所谓代理类就是以模仿和增强一些类型的行为为目的而存在的类。很多情况下都会使用代理类，`std::vector<bool>::reference`展示了对 `std::vector<bool>`使用 `operator[]`来实现引用*bit*这样的行为。另外，C++标准模板库中的智能指针（见[第4章](../4.SmartPointers/item18.md)）也是用代理类实现了对原始指针的资源管理行为。代理类的功能已被大家广泛接受。事实上，“Proxy”设计模式是软件设计这座万神庙中一直都存在的高级会员。

一些代理类被设计于用以对客户可见。比如 `std::shared_ptr`和 `std::unique_ptr`。其他的代理类则或多或少不可见，比如 `std::vector<bool>::reference`就是不可见代理类的一个例子，还有它在 `std::bitset`的胞弟 `std::bitset::reference`。

在后者的阵营（注：指不可见代理类）里一些C++库也是用了表达式模板（*expression templates*）的黑科技。这些库通常被用于提高数值运算的效率。给出一个矩阵类 `Matrix`和矩阵对象 `m1`，`m2`，`m3`，`m4`，举个例子，这个表达式

````cpp
Matrix sum = m1 + m2 + m3 + m4;
````

可以使计算更加高效，只需要使让 `operator+`返回一个代理类代理结果而不是返回结果本身。也就是说，对两个 `Matrix`对象使用 `operator+`将会返回如 `Sum<Matrix, Matrix>`这样的代理类作为结果而不是直接返回一个 `Matrix`对象。在 `std::vector<bool>::reference`和 `bool`中存在一个隐式转换，同样对于 `Matrix`来说也可以存在一个隐式转换允许 `Matrix`的代理类转换为 `Matrix`，这让表达式等号“`=`”右边能产生代理对象来初始化 `sum`。（这个对象应当编码整个初始化表达式，即类似于 `Sum<Sum<Sum<Matrix, Matrix>, Matrix>, Matrix>`的东西。客户应该避免看到这个实际的类型。）

作为一个通则，不可见的代理类通常不适用于 `auto`。这样类型的对象的生命期通常不会设计为能活过一条语句，所以创建那样的对象你基本上就走向了违反程序库设计基本假设的道路。`std::vector<bool>::reference`就是这种情况，我们看到违反这个基本假设将导致未定义行为。

因此你想避开这种形式的代码：

````cpp
auto someVar = expression of "invisible" proxy class type;
````

但是你怎么能意识到你正在使用代理类？应用他们的软件不可能宣告它们的存在。它们被设计为**不可见**，至少概念上说是这样！每当你发现它们，你真的应该舍弃[Item5](../2.Auto/item5.md)演示的 `auto`所具有的诸多好处吗？

让我们首先回到如何找到它们的问题上。虽然“不可见”代理类都在程序员日常使用的雷达下方飞行，但是很多库都证明它们可以上方飞行。当你越熟悉你使用的库的基本设计理念，你的思维就会越活跃，不至于思维僵化认为代理类只能在这些库中使用。

当缺少文档的时候，可以去看看头文件。很少会出现源代码全都用代理对象，它们通常用于一些函数的返回类型，所以通常能从函数签名中看出它们的存在。这里有一份 `std::vector<bool>::operator[]`的说明书：

````cpp
namespace std{                                  //来自于C++标准库
    template<class Allocator>
    class vector<bool, Allocator>{
    public:
        …
        class reference { … };

        reference operator[](size_type n);
        …
    };
}
````

假设你知道对 `std::vector<T>`使用 `operator[]`通常会返回一个 `T&`，在这里 `operator[]`不寻常的返回类型提示你它使用了代理类。多关注你使用的接口可以暴露代理类的存在。

实际上， 很多开发者都是在跟踪一些令人困惑的复杂问题或在单元测试出错进行调试时才看到代理类的使用。不管你怎么发现它们的，一旦看到 `auto`推导了代理类的类型而不是被代理的类型，解决方案并不需要抛弃 `auto`。`auto`本身没什么问题，问题是 `auto`不会推导出你想要的类型。解决方案是强制使用一个不同的类型推导形式，这种方法我通常称之为显式类型初始器惯用法（*the explicitly typed initialized idiom*)。

显式类型初始器惯用法使用 `auto`声明一个变量，然后对表达式强制类型转换（*cast*）得出你期望的推导结果。举个例子，我们该怎么将这个惯用法施加到 `highPriority`上？

````cpp
auto highPriority = static_cast<bool>(features(w)[5]);
````

这里，`features(w)[5]`还是返回一个 `std::vector<bool>::reference`对象，就像之前那样，但是这个转型使得表达式类型为 `bool`，然后 `auto`才被用于推导 `highPriority`。在运行时，对 `std::vector<bool>::operator[]`返回的 `std::vector<bool>::reference`执行它支持的向 `bool`的转型，在这个过程中指向 `std::vector<bool>`的指针已经被解引用。这就避开了我们之前的未定义行为。然后5将被用于指向*bit*的指针，`bool`值被用于初始化 `highPriority`。

对于 `Matrix`来说，显式类型初始器惯用法是这样的：

````cpp
auto sum = static_cast<Matrix>(m1 + m2 + m3 + m4);
````

应用这个惯用法不限制初始化表达式产生一个代理类。它也可以用于强调你声明了一个变量类型，它的类型不同于初始化表达式的类型。举个例子，假设你有这样一个表达式计算公差值：

````cpp
double calcEpsilon();                           //返回公差值
````

`calcEpsilon`清楚的表明它返回一个 `double`，但是假设你知道对于这个程序来说使用 `float`的精度已经足够了，而且你很关心 `double`和 `float`的大小。你可以声明一个 `float`变量储存 `calEpsilon`的计算结果。

````cpp
float ep = calcEpsilon();                       //double到float隐式转换
````

但是这几乎没有表明“我确实要减少函数返回值的精度”。使用显式类型初始器惯用法我们可以这样：

````cpp
auto ep = static_cast<float>(calcEpsilon());
````

出于同样的原因，如果你故意想用整数类型存储一个表达式返回的浮点数类型的结果，你也可以使用这个方法。假如你需要计算一个随机访问迭代器（比如 `std::vector`，`std::deque`或者 `std::array`）中某元素的下标，你被提供一个 `0.0`到 `1.0`的 `double`值表明这个元素离容器的头部有多远（`0.5`意味着位于容器中间）。进一步假设你很自信结果下标是 `int`。如果容器是 `c`，`d`是 `double`类型变量，你可以用这样的方法计算容器下标：

````cpp
int index = d * c.size();
````

但是这种写法并没有明确表明你想将右侧的 `double`类型转换成 `int`类型，显式类型初始器可以帮助你正确表意：

````cpp
auto index = static_cast<int>(d * size());
````

**请记住：**

+ 不可见的代理类可能会使 `auto`从表达式中推导出“错误的”类型
+ 显式类型初始器惯用法强制 `auto`推导出你想要的结果
