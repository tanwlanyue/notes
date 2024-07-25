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











