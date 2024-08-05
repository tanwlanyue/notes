# [The Definitive C++ Book Guide and List](https://stackoverflow.com/questions/388242/the-definitive-c-book-guide-and-list)

这个提问试图从每年出版的众多质量不高的C++书籍中筛选出少数几本精品。

与其他许多编程语言不同，通常可以通过互联网上的教程边做边学，很少有人能够不通过阅读一本写得好的C++书籍就快速掌握C++。C++的庞大和复杂性使得这几乎不可能。事实上，C++的庞大和复杂性导致市面上存在许多非常糟糕的C++书籍。我们所说的糟糕，不仅仅是风格问题，而是诸如明显的事实错误和推广极其糟糕的编程风格之类的问题。

---

## Beginner

### Introductory, no previous programming experience

| Book                                                         | Author(s)                                                    | Description                                                  | review                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [*C++ Primer*](https://rads.stackoverflow.com/amzn/click/com/0321714113)*  * Not to be confused with [*C++ Primer Plus*](https://rads.stackoverflow.com/amzn/click/com/0672326973) (Stephen Prata), with a significantly less favorable [review](https://accu.org/bookreviews/2002/glassborow_1744). | Stanley Lippman, Josée Lajoie, and Barbara E. Moo (**updated for C++11**) | Coming at 1k pages, this is a very thorough introduction into C++ that covers just about everything in the language in a very accessible format and in great detail. The fifth edition (released August 16, 2012) covers C++11. | [[Review\]](https://accu.org/bookreviews/2012/glassborow_1848) |
| [*Programming: Principles and Practice Using C++*](https://rads.stackoverflow.com/amzn/click/com/0138308683) | Bjarne Stroustrup, 3rd Edition - April 22, 2024 (**updated for C++20/C++23**) | An introduction to programming using C++ by the creator of the language. A good read, that assumes no previous programming experience, but is not only for beginners. |                                                              |

### Introductory, with previous programming experience

| Book                                                         | Author(s)                                                    | Description                                                  | review                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [*A Tour of C++*](https://rads.stackoverflow.com/amzn/click/com/0136816487) | Bjarne Stroustrup (**[2nd edition for C++17](https://rads.stackoverflow.com/amzn/click/com/0134997832)**, **[3rd edition for C++20](https://rads.stackoverflow.com/amzn/click/com/0136816487)**) | The “tour” is a quick (about 180 pages and 14 chapters) tutorial overview of all of standard C++ (language and standard library, **and using C++11**) at a moderately high level for people who already know C++ or at least are experienced programmers. This book is an extended version of the material that constitutes Chapters 2-5 of The C++ Programming Language, 4th edition. |                                                              |
| [*Accelerated C++*](https://rads.stackoverflow.com/amzn/click/com/020170353X) | Andrew Koenig and Barbara Moo, 1st Edition - August 24, 2000 | This basically covers the same ground as the *C++ Primer*, but does so in a quarter of its space. This is largely because it does not attempt to be an introduction to *programming*, but an introduction to *C++* for people who've previously programmed in some other language. It has a steeper learning curve, but, for those who can cope with this, it is a very compact introduction to the language. (Historically, it broke new ground by being the first beginner's book to use a modern approach to teaching the language.) Despite this, the C++ it teaches is purely C++98. | [[Review\]](https://accu.org/bookreviews/2000/glassborow_1185) |

### Best practices

| Book                                                         | Author(s)                                | Description                                                  | review                                                       |
| ------------------------------------------------------------ | ---------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [*Effective C++*](https://rads.stackoverflow.com/amzn/click/com/0321334876) | Scott Meyers, 3rd Edition - May 22, 2005 | 这本书的写作目标是成为C++程序员应该阅读的最好的第二本书，而且它成功了。早期版本的目标是面向从C语言转过来的程序员，第三版改变了这一点，面向的是像Java这样的语言的程序员。它以一种非常易于接近（并且令人愉快）的风格，呈现了大约50条容易记住的经验法则及其理由。对于C++11和C++14，例子和一些问题已经过时，应该优先选择《Effective Modern C++》。 | [[Review\]](https://accu.org/bookreviews/1998/glassborow_700) |
| [*Effective Modern C++*](https://rads.stackoverflow.com/amzn/click/com/1491903996) | Scott Meyers                             | 这本书面向的是正在从C++03过渡到C++11和C++14的C++程序员。这本书可以被视为《Effective C++》中某些部分的延续和“纠正”。它们没有涵盖相同的内容，但保持了类似的条目化主题。 | [[Review\]](https://accu.org/bookreviews/2019/floyd_1937)    |
| [*Effective STL*](https://rads.stackoverflow.com/amzn/click/com/0201749629) | Scott Meyers                             | 这本书的目标是针对标准库中的STL部分做到《Effective C++》对整个语言所做的事情：它提供了经验法则及其理由。 |                                                              |

------

## Intermediate

| Book                                                         | Author(s)                                 | Description                                                  | review                                                       |
| ------------------------------------------------------------ | ----------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [*More Effective C++*](https://rads.stackoverflow.com/amzn/click/com/020163371X) | Scott Meyers                              | Even more rules of thumb than *Effective C++*. Not as important as the ones in the first book, but still good to know. |                                                              |
| [*Exceptional C++*](https://rads.stackoverflow.com/amzn/click/com/0201615622) | Herb Sutter                               | 这本书以一系列谜题的形式呈现，它对C++中适当的资源管理和异常安全性进行了最佳和深入的讨论，特别是通过资源获取即初始化（RAII）的方式，除此之外，还深入涵盖了包括pimpl惯用法、名称查找、良好的类设计以及C++内存模型在内的多种其他主题。 | [[Review\]](https://accu.org/bookreviews/2000/griffiths_209) |
| [*More Exceptional C++*](https://rads.stackoverflow.com/amzn/click/com/020170434X) | Herb Sutter                               | 除了在《Exceptional C++》中未涵盖的异常安全性主题之外，这本书还讨论了C++中有效的面向对象编程以及正确使用 STL 标准模板库。 | [[Review\]](https://accu.org/bookreviews/2002/glassborow_784) |
| [*Exceptional C++ Style*](https://rads.stackoverflow.com/amzn/click/com/0201760428) | Herb Sutter                               | 这本书讨论了泛型编程、优化和资源管理；此外，它还出色地阐述了如何通过使用非成员函数和单一职责原则来编写C++中的模块化代码。 | [[Review\]](https://accu.org/bookreviews/2005/goodliffe_107) |
| [*C++ Coding Standards*](https://rads.stackoverflow.com/amzn/click/com/0321113586) | Herb Sutter and Andrei Alexandrescu       | 这里的“编码标准”并不意味着“我应该用多少空格来缩进我的代码？” 这本书包含了101个最佳实践、惯用法和常见陷阱，可以帮助你编写正确、易于理解且高效的C++代码。 | [[Review\]](https://accu.org/bookreviews/2004/glassborow_1439) |
| [*C++ Templates: The Complete Guide*](https://rads.stackoverflow.com/amzn/click/com/0201734842) | David Vandevoorde and Nicolai M. Josuttis | 这是一本关于C++11之前存在的模板的书籍。它涵盖了从非常基础的内容到一些最高级模板元编程的所有内容，并解释了模板如何工作的每一个细节（无论是概念上还是在实现上），并讨论了许多常见的陷阱。在附录中对单一定义规则（ODR）和重载解析有出色的总结。[第二版](https://rads.stackoverflow.com/amzn/click/com/0321714121)已经出版，涵盖了C++11、C++14和C++17的内容。 | [[Review\]](https://accu.org/bookreviews/2020/floyd_1946)    |
| [*C++ 17 - The Complete Guide*](https://leanpub.com/cpp17)   | Nicolai M. Josuttis                       | 这本书描述了C++17标准中引入的所有新特性，涵盖了从简单的如“内联变量”、“constexpr if”一直到“多态内存资源”和“带有过度对齐数据的new和delete”的所有内容。 | [[Review\]](https://accu.org/bookreviews/2020/floyd_1943)    |
| [*C++ 20 - The Complete Guide*](https://leanpub.com/cpp20)   | Nicolai M. Josuttis                       | 这本书介绍了C++20的所有新语言和库特性。它涵盖了每个新特性的动机和背景，包括示例和背景信息。重点在于这些特性如何影响日常编程，如何将它们结合起来，以及如何在实践中从C++20中获益。（请注意，这本书是**分步出版**的，第一版现在已经完成。） |                                                              |
| [*C++ in Action*](http://www.worldcolleges.info/sites/default/files/C++_In_Action.pdf) | Bartosz Milewski                          | 这本书通过从头构建一个应用程序来解释C++及其特性。            | [[Review\]](https://eli.thegreenplace.net/2003/09/12/book-review-c-in-action-by-bartosz-milewski) |
| [*Functional Programming in C++*](https://www.manning.com/books/functional-programming-in-c-plus-plus) | Ivan Čukić                                | 这本书向现代C++（C++11及更高版本）介绍了函数式编程技术。对于那些想要将函数式编程范式应用于C++的人来说，这是一本非常好的读物。 |                                                              |

------

## Advanced

| Book                                                         | Author(s)                           | Description                                                  | review                                                       |
| ------------------------------------------------------------ | ----------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [*Modern C++ Design*](https://rads.stackoverflow.com/amzn/click/com/0201704315) | Andrei Alexandrescu                 | 一本关于高级泛型编程技术的开创性书籍。介绍了基于策略的设计、类型列表和基本的泛型编程惯用语，然后解释了如何使用泛型编程高效、模块化和清晰地实现许多有用的设计模式（包括小型对象分配器、仿函数、工厂、访问者和多态方法）。 | [[Review\]](https://accu.org/bookreviews/2001/glassborow_979) |
| [*C++ Template Metaprogramming*](https://rads.stackoverflow.com/amzn/click/com/0321227255) | David Abrahams and Aleksey Gurtovoy |                                                              |                                                              |
| [*C++ Concurrency In Action*](https://rads.stackoverflow.com/amzn/click/com/1933988770) | Anthony Williams                    | 一本涵盖C++11并发支持的书籍，包括线程库、原子操作库、C++内存模型、锁和互斥量，以及设计和调试多线程应用程序的问题。[第二版](https://rads.stackoverflow.com/amzn/click/com/1617294691)已经出版，它涵盖了C++14和C++17的内容。 | [[Review\]](https://accu.org/bookreviews/2012/glassborow_1850) |
| [*Advanced C++ Metaprogramming*](https://rads.stackoverflow.com/amzn/click/com/1460966163) | Davide Di Gennaro                   | 这是一本C++11之前的模板元编程（TMP）技巧手册，更侧重于实践而非理论。这本书中有很多代码片段，其中一些由于类型特性的出现而变得过时，但这些技巧仍然值得了解。如果你能忍受那些古怪的排版/编辑，它比Alexandrescu的书更容易阅读，而且可以说，更有收获。对于更有经验的开发者来说，你很可能会发现一些C++中的特殊或不寻常的特性，这通常只有通过广泛的经验才能获得。 |                                                              |
| [*Large Scale C++ volume I, Process and architecture*](https://www.pearson.com/store/p/large-scale-c-volume-i-process-and-architecture/P100001343596/9780201717068) (2020) | John Lakos                          | 这是三部分系列中的第一部分，扩展了较早的书籍《Large Scale C++ Design》。Lakos解释了经过实战检验的技术，用于管理非常大的C++软件项目。如果你在一个大的C++软件项目中工作，这是一本很好的读物，详细阐述了物理结构和逻辑结构之间的关系，组件的策略以及它们的重用。 | [[Review\]](https://accu.org/bookreviews/2020/bruntlett_1953/) |

------

## Reference Style - All Levels

| Book                                                         | Author(s)                                 | Description                                                  | review                                                       |
| ------------------------------------------------------------ | ----------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [*The C++ Programming Language*](https://rads.stackoverflow.com/amzn/click/com/0321958322) | Bjarne Stroustrup (**updated for C++11**) | 这是C++的创始人所写的C++经典入门书籍。它旨在与经典的K&R（Kernighan & Ritchie，即C语言的经典教程《The C Programming Language》的作者）相提并论，读起来确实非常相似，涵盖了从核心语言到标准库、编程范式到语言哲学的几乎所有内容。 | [[Review\]](https://accu.org/bookreviews/2014/lenton_1853) Note: All releases of the C++ standard are tracked in the question "*[Where do I find the current C or C++ standard documents?](https://stackoverflow.com/questions/81656/where-do-i-find-the-current-c-or-c-standard-documents)*". |
| [*C++ Standard Library Tutorial and Reference*](https://rads.stackoverflow.com/amzn/click/com/0321623215) | Nicolai Josuttis (**updated for C++11**)  | C++标准库的介绍和参考书籍。第二版（发布于2012年4月9日）涵盖了C++11的内容。 | [[Review\]](https://accu.org/bookreviews/2012/glassborow_1849) |
| [*The C++ IO Streams and Locales*](https://rads.stackoverflow.com/amzn/click/com/0201183951) | Angelika Langer and Klaus Kreft           | 关于这本书，除了要说如果你想了解有关 streams 和 locales 的任何事情，那么这里就是找到权威答案的地方之外，几乎没什么可说的。 | [[Review\]](https://accu.org/bookreviews/2000/glassborow_200) |

**C++11/14/17/… References:**

- [Working Draft, Standard for Programming Language C++](http://eel.is/c++draft/) generated from [LaTeX sources published on GitHub](https://github.com/cplusplus/draft).
- [C++ Standard Papers](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/), latest standard working draft: [ISO working draft](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2020/n4868.pdf)
- *The C++[11](https://www.iso.org/standard/50372.html)/[14](https://www.iso.org/standard/64029.html)/[17](https://www.iso.org/standard/68564.html) Standard (INCITS/ISO/IEC 14882:2011/2014/2017)* This, of course, is the final arbiter of all that is or isn't C++. Be aware, however, that it is intended purely as a reference for *experienced* users willing to devote considerable time and effort to its understanding. The C++17 standard is released in electronic form for 198 Swiss Francs.
- The C++17 standard is available, but seemingly not in an economical form – [directly from the ISO](https://www.iso.org/standard/68564.html) it costs 198 Swiss Francs (about $200 US). For most people, the [final draft before standardization](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2017/n4659.pdf) is more than adequate (and free). Many will prefer an [even newer draft](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/n4778.pdf), documenting new features that are likely to be included in C++20.
- [C++20 draft](https://github.com/cplusplus/draft/releases/download/n4868/n4868.pdf) is available on GitHub as [some older too](https://github.com/cplusplus/draft/releases).
- [*Overview of the New C++ (C++11/14) (PDF only)*](https://www.artima.com/shop/overview_of_the_new_cpp) (Scott Meyers) (**updated for C++14**) These are the presentation materials (slides and some lecture notes) of a three-day training course offered by Scott Meyers, who's a highly respected author on C++. Even though the list of items is short, the quality is high.
- The [*C++ Core Guidelines (C++11/14/17/…)*](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md) (edited by Bjarne Stroustrup and Herb Sutter) is an evolving online document consisting of a set of guidelines for using modern C++ well. The guidelines are focused on relatively higher-level issues, such as interfaces, resource management, memory management, and concurrency affecting application architecture and library design. The project was [announced at CppCon'15 by Bjarne Stroustrup and others](https://isocpp.org/blog/2015/09/bjarne-stroustrup-announces-cpp-core-guidelines) and welcomes contributions from the community. Most guidelines are supplemented with a rationale and examples as well as discussions of possible tool support. Many rules are designed specifically to be automatically checkable by static analysis tools.
- The [*C++ Super-FAQ*](https://isocpp.org/faq) (Marshall Cline, Bjarne Stroustrup, and others) is an effort by the Standard C++ Foundation to unify the C++ FAQs previously maintained individually by Marshall Cline and Bjarne Stroustrup and also incorporating new contributions. The items mostly address issues at an intermediate level and are often written with a humorous tone. Not all items might be fully up to date with the latest edition of the C++ standard yet.
- [*cppreference.com (C++03/11/14/17/…)*](https://cppreference.com/) (initiated by Nate Kohl) is a wiki that summarizes the basic core-language features and has extensive documentation of the C++ standard library. The documentation is very precise but is easier to read than the official standard document and provides better navigation due to its wiki nature. The project documents all versions of the C++ standard and the site allows filtering the display for a specific version. The project was [presented by Nate Kohl at CppCon'14](https://isocpp.org/blog/2015/07/cppcon-2014-cppreference.com-documenting-cpp-one-edit-at-a-time-nate-kohl).

------

## Classics / Older

**注意：** 这些书中包含的某些信息可能已不是最新的，或不再被认为是最佳实践。

- [*The Design and Evolution of C++*](https://rads.stackoverflow.com/amzn/click/com/0201543303) (Bjarne Stroustrup) If you want to know *why* the language is the way it is, this book is where you find answers. This covers everything *before the standardization* of C++.
- [*Ruminations on C++*](https://rads.stackoverflow.com/amzn/click/com/0201423391) - (Andrew Koenig and Barbara Moo) [[Review\]](https://accu.org/bookreviews/1997/glassborow_776)
- [*Advanced C++ Programming Styles and Idioms*](https://rads.stackoverflow.com/amzn/click/com/0201548550) (James Coplien) A predecessor of the pattern movement, it describes many C++-specific “idioms”. It's certainly a very good book and might still be worth a read if you can spare the time, but quite old and not up-to-date with current C++.
- [*Large Scale C++ Software Design*](https://rads.stackoverflow.com/amzn/click/com/0201633620) (John Lakos) Lakos explains techniques to manage very big C++ software projects. Certainly, a good read, if it only was up to date. It was written long before C++ 98 and misses on many features (e.g. namespaces) important for large-scale projects. If you need to work on a big C++ software project, you might want to read it, although you need to take more than a grain of salt with it. Not to be confused with the extended and later book series Large Scale C++ volume I-III.
- [*Inside the C++ Object Model*](https://rads.stackoverflow.com/amzn/click/com/0201834545) (Stanley Lippman) If you want to know how virtual member functions are commonly implemented and how base objects are commonly laid out in memory in a multi-inheritance scenario, and how all this affects performance, this is where you will find thorough discussions of such topics.
- [*The Annotated C++ Reference Manual*](https://rads.stackoverflow.com/amzn/click/com/0201514591) (Bjarne Stroustrup, Margaret A. Ellis) This book is quite outdated in the fact that it explores the 1989 C++ 2.0 version - Templates, exceptions, namespaces, and new casts were not yet introduced. Saying that however, this book goes through the entire C++ standard of the time explaining the rationale, the possible implementations, and features of the language. This is not a book to learn programming principles and patterns on C++, but to understand every aspect of the C++ language.
- [*Thinking in C++*](https://rads.stackoverflow.com/amzn/click/com/0139798099) (Bruce Eckel, 2nd Edition, 2000). Two volumes; is a tutorial-style *free* set of intro-level books. Downloads: [vol 1](https://ia800100.us.archive.org/10/items/TICPP2ndEdVolOne/TICPP-2nd-ed-Vol-one.zip), [vol 2](https://ia800108.us.archive.org/24/items/TICPP2ndEdVolTwo/TICPP-2nd-ed-Vol-two.zip). Unfortunately, they're marred by a number of trivial errors (e.g. maintaining that temporaries are automatic `const`), with no official errata list. A partial 3rd party errata list is available at http://www.computersciencelab.com/Eckel.htm, but it is apparently not maintained.
- [*Scientific and Engineering C++: An Introduction to Advanced Techniques and Examples*](https://rads.stackoverflow.com/amzn/click/com/0201533936) (John Barton and Lee Nackman) It is a comprehensive and very detailed book that tried to explain and make use of all the features available in C++, in the context of numerical methods. It introduced at the time several new techniques, such as the Curiously Recurring Template Pattern (CRTP, also called Barton-Nackman trick). It pioneered several techniques such as dimensional analysis and automatic differentiation. It came with a lot of compilable and useful code, ranging from an expression parser to a Lapack wrapper. The code is [still available online](https://www.informit.com/store/scientific-and-engineering-c-plus-plus-an-introduction-9780201533934). Unfortunately, the books have become somewhat outdated in style and C++ features, however, it was an incredible tour-de-force at the time (1994, pre-STL). The chapters on dynamics inheritance are a bit complicated to understand and not very useful. An updated version of this classic book that includes move semantics and the lessons learned from the STL would be very nice.

---

1. **@G Rassovsky**: 所有承诺在Y小时内教会X的书籍都应该避免，例如"24小时内学会C++"。- akhil_mittal, 评论于2014年12月29日
2. **用户1630889**: 尽管我尊重Bruce Eckel将他的材料免费在线发布，但我不推荐他的《Thinking in C++》。书中的观点表明了相对糟糕或效果不佳的C++和"面向对象"编程的使用，类似于GoF设计模式的不当应用。我发现它是一本有趣的通用编程入门书籍，但随着某人对编程和（尤其是）计算机科学整体的熟悉，我发现纯粹以"经典"OOP术语思考的书籍对教育有害。- 用户1630889, 评论于2015年1月16日
3. **Zaphod Beeblebrox**: 在accu.org网站上，有一个书籍评论部分和评级。你可以搜索C++的评论。它们中的许多被评为"不推荐"。- Zaphod Beeblebrox, 评论于2016年1月26日
4. **haccks**: 我有一个反推荐："Let Us C++ by Yashavant Kanetkar"。这是一本完全的垃圾书。我请求所有初学者/程序员不要读这本书。读这本书就像是在章节中教你2+2=4，然后在练习中让你计算宇宙的面积。非常令人沮丧。- haccks, 评论于2022年4月3日
5. **Myrddin Krustowski**: 可以添加Nicolai Josuttis的《C++ Move Semantics - The Complete Guide》到列表中吗？- Myrddin Krustowski, 评论于2022年6月14日
6. **ThisClark**: 关于Meyers的《Effective C++》"这是以成为C++程序员应该阅读的最好的第二本书为目标而写的，并且它成功了。"...那么C++程序员应该阅读的最好的第一本书是什么？- ThisClark, 评论于2022年12月14日
7. **Autechre**: 我认为Marc Gregoire的《Professional C++》怎么样？- Autechre, 评论于2023年2月10日
8. **Autechre**: 数组中有一个小错误。《C++ Primer》并没有涵盖C++的所有内容，因为它根本没有涵盖多线程。- Autechre, 评论于2023年2月10日
9. **Joe**: Steve Oualline的《Practical C++ Programming》是一本很棒的书。我发现它对指针和引用特别有帮助，尤其是对于初学者。- Joe, 评论于2023年3月13日
10. **Wayne DSouza**: 评论Prata是有偏见的。它假设了某种模式并寻找一种思想流派，未能考虑到这本书是试图理解语言的人的一个很好的工具。- Wayne DSouza, 评论于2023年6月15日
11. **Reed Hedges**: 我想知道如果这个表格包括每本书的出版日期（每个版本的出版日期）以及涵盖的C++语言版本，会不会有帮助？我可以添加这些吗？- Reed Hedges, 评论于2023年7月11日
12. **Cubbi**: 我浏览了那本书，它比链接的评论建议的还要糟糕。- Cubbi, 评论于2023年8月15日
13. **Justme0**: 增加一本中文书籍：《STL源码剖析》侯捷著 : ) - Justme0, 评论于2023年8月24日
14. **Ryan Lundy**: 我对《C++ Primer》如此受推荐感到有点惊讶。它可能很全面，但也像泥土一样枯燥无味。我不得不强迫自己阅读它。Stroustrup的书更易读。- Ryan Lundy, 评论于2023年9月13日
15. **akuzminykh**: 我们可以添加一列，说明一本书是否有练习吗？- akuzminykh, 评论于4月14日18:35
16. **Meenohara**: Robert Lafore的《Object-Oriented Programming in C++》用于基本概念。- Meenohara, 评论于5月1日9:40

---

https://github.com/MeouSker77/Cpp17?tab=readme-ov-file