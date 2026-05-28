thrift 接口定义

```
namespace cpp match_service

// 结构体：user 属性：id(int) name score(int)
struct User{
	1:i32 id,
	2:string name,
	3:i32 score,
}

// 远程接口定义 User 增 删
service Match{
	// 增 
	i32 add_user(1: User user 2:string info),
	// 删
	i32 remove_user(1:User user 2: string info),
}

```

自动生成 c++ 代码

```bash
# 根据 thrift 文件递归生成 python 代码
thrift -r --gen cpp xxx.thrift
```

编译 链接 

```bash
# 编译 
g++ -c main.cpp *.cpp
# 链接 并引入 Thrift库
g++ *.o -o main -lthrift
# 用到线程，则添加上 -pthread
g++ *.o -o main -lthrift -pthread
```

多人开发 c++ 源文件 尽量不要加上 using namespace std; 避免命名冲突

```cpp
// 手动 std
std::cout << “hello world” << std::endl;
```

自动生成 python 代码

```bash
thrift -r --gen py thrift/xxx.thrift
```

客户端 python 代码

```python
# stdin 循环读入op id name score
# 检查字段数、输入合法性
```

多线程 + 锁机制

```thrift
// 互斥访问message_que 添加元素，并唤醒阻塞进程
// 互斥访问message_que 为空时，阻塞进程；否则消耗元素
// 根据op，pool增或删
```

升级服务器模式

```cpp
// 简单服务模式
int port = 9090;
  ::std::shared_ptr<MatchServiceHandler> handler(new MatchServiceHandler());
  ::std::shared_ptr<TProcessor> processor(new MatchServiceProcessor(handler));
  ::std::shared_ptr<TServerTransport> serverTransport(new TServerSocket(port));
  ::std::shared_ptr<TTransportFactory> transportFactory(new TBufferedTransportFactory());
  ::std::shared_ptr<TProtocolFactory> protocolFactory(new TBinaryProtocolFactory()

// 多线程服务模式
#include <xxx>
class CalculatorCloneFactory : virtual public CalculatorIfFactory {
 public:
  ~CalculatorCloneFactory() override = default;
  CalculatorIf* getHandler(const ::apache::thrift::TConnectionInfo& connInfo) override
  {
    std::shared_ptr<TSocket> sock = std::dynamic_pointer_cast<TSocket>(connInfo.transport);
    
    return new CalculatorHandler;
  }
  void releaseHandler( ::shared::SharedServiceIf* handler) override {
    delete handler;
  }
};
                                                     
TThreadedServer server(
    std::make_shared<CalculatorProcessorFactory>(std::make_shared<CalculatorCloneFactory>()),
    std::make_shared<TServerSocket>(9090), //port
    std::make_shared<TBufferedTransportFactory>(),
    std::make_shared<TBinaryProtocolFactory>());                                                   
```



