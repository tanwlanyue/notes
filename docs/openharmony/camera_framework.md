# OpenHarmony C++ 编程实践 相机框架

## 多线程



lock mutex 



shared_ptr weak_ptr

c++ 可能出现的内存问题有

1. 缓冲区溢出
2. 悬垂指针，野指针
3. 重复释放
4. 内存泄露
5. 不配对的new[]/delete
6. 内存碎片

1-5 只能指针都能解决



智能指针作为对象的成员 ，对象的析构函数不能是默认或内联的



观察者模式 在update过程中调用了register unregister 会死锁    无解 

使用栈上指针的copy 来操作数据 避免临界区太大



shard_ptr 传入lambda或function 造成对象的生命周期延长

shard_ptr 的拷贝成本比裸指针高 可以用const reference传递



```c++
#include <iostream>
#include <cassert>
#include <memory>
#include <mutex>
#include <map>

class Stock {
public:
    Stock(const std::string& key) : key_(key) {}
    const std::string& GetKey() const { return key_; }
private:
    std::string key_;
};

class StockFactory : public std::enable_shared_from_this<StockFactory> {
public:
    std::shared_ptr<Stock> Get(const std::string& key) {
        std::shared_ptr<Stock> pStock;
        std::lock_guard<std::mutex> lock(factoryMutex_);
        std::weak_ptr<Stock>& wkStock = stocks_[key];
        pStock = wkStock.lock();
        if (!pStock) {
            std::weak_ptr<StockFactory> wkFactory = shared_from_this();
            pStock.reset(new Stock(key), [wkFactory](Stock* p) { 
                auto pFactory = wkFactory.lock();
                if (pFactory) {
                    pFactory->deleteStock(p);
                }
            });
            wkStock = pStock;
        }
        return pStock;
    }
private:
    void deleteStock(Stock* stock) {
        if (!stock) {
            std::lock_guard<std::mutex> lock(factoryMutex_);
            stocks_.erase(stock->GetKey());
        }
    }
private:
    std::mutex factoryMutex_;
    std::map<std::string, std::weak_ptr<Stock>> stocks_;
};

int main() {
    return 0;
}
```

## shared_ptr  copy on write



taskqueue   

produce consumter queue

count down latch



## Go服务端

读nng和百度的brpc，其中brpc的文档值得一读，会涉及到工业界真正想解决的问题。TCP通信部分了解到这个程度就够了，毕竟是早就成熟的领域。2. 读完ddia和它附录里有意思的论文，读leveldb和redis，然后了解下lucene。这样对存储方面有初步认知。有空再去读innodb。3. 读腾讯的inference框架ncnn，了解如何做计算优化。4. 读intel tbb的work stealing用户级线程调度。5. 我觉的应届生很难有时间写出有意义的完整项目，但可以写一些有意义的数据结构：可以用C++实现类似redis但又支持泛型的radix tree，用C++实现fit in内存且吞吐吊打lucene的倒排索引。

作者：不谙世事的吴同学
链接：https://www.zhihu.com/question/394704611/answer/1245846184
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



进程有独立的地址空间

线程共享地址空间





p84
