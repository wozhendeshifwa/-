thrift 流程

```
namespace cpp match_service

// User 结构体
struct User{
	1:i32 id,
	2:string name,
	3:i32 score,
}

// 远程服务接口
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
# 编译 c++ 文件
g++ -c main.cpp *.cpp

# 将所有的.o 文件链接输出成可执行文件 main
# 并引入 Thrift C++ 库
# 并引入 Thrift C++ 库
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
# 根据 thrift 文件递归生成 python 代码
thrift -r --gen py thrift/xxx.thrift
```

客户端 python 代码

```python

```

锁机制

```thrift
namespace cpp match_service

struct User {
    1: i32 id,
    2: string name,
    3: i32 score,
}

service Match {
    i32 add(1: User user, 2: string name),
    i32 remove(1: User user, 2: string name),
}
```

```python
'''
thrift 接口定义
namespace cpp match_service
struct User {
    1: i32 id, 
    2: string name,
    3: i32 score,
}
service Match {
    i32 add(1: User user, 2: string name),
    i32 remove(1: User user, 2: string name),
}
'''
from gen_py.match import Match
from gen_py.match.ttypes import User

from thrift import Thrift
from thrift.transport import TSocket
from thrift.transport import TTransport
from thrift.protocol import TBinaryProtocol
from sys import stdin

def operate(op, id, name, score):
    # Make socket
    transport = TSocket.TSocket('localhost', 9090)

    # Buffering is critical. Raw sockets are very slow
    transport = TTransport.TBufferedTransport(transport)

    # Wrap in a protocol
    protocol = TBinaryProtocol.TBinaryProtocol(transport)

    # Create a client to use the protocol encoder
    client = Match.Client(protocol)  # 修正：应该是 Match 而不是 Calculator

    # Connect!
    transport.open()

    '''
    根据函数参数新建对象
    并根据op调用服务端函数
    '''
    user = User(id=id, name=name, score=score)
    if op == "add":
        client.add(user, name)
    elif op == "remove":
        client.remove(user, name)
    # Close!
    transport.close()

'''
主函数：main 
循环 stdin 读入一行op,id,name,score，按空格分开
'''
def main():
    for line in stdin:
        line = line.strip()
        if not line:
            continue
        op, id_str, name, score_str = line.split()
        id = int(id_str)
        score = int(score_str)
        operate(op, id, name, score)

#执行main函数
if __name__ == "__main__":
    main()
```

```cpp
/*
	thrift 相关接口定义
namespace cpp match_service
struct User {
    1: i32 id, 
    2: string name,
    3: i32 score,
}
service Match {
    i32 add(1: User user, 2: string name),
    i32 remove(1: User user, 2: string name),                                                          
}
*/
#include "gen_cpp/Match.h"
#include <thrift/protocol/TBinaryProtocol.h>
#include <thrift/server/TSimpleServer.h>
#include <thrift/transport/TServerSocket.h>                                                                                                                                                       
#include <thrift/transport/TBufferTransports.h>
#include <iostream>
#include <vector>
#include <queue>
#include <mutex>
#include <condition_variable>
#include <thread>

using namespace ::apache::thrift;
using namespace ::apache::thrift::protocol;
using namespace ::apache::thrift::transport;
using namespace ::apache::thrift::server;
using namespace ::match_service;

// 类:Pool 全局对象:pool
// 私有属性：User数组:users
// 公共方法，1.增 2.删 3.users弹出两个元素进行匹配，并输出结果（简易匹配就行）
class Pool {
private:
    std::vector<User> users;
public:
    void add(const User& user) {
        users.push_back(user);
    }
    void remove(const User& user) {
        for (auto it = users.begin(); it != users.end(); ++it) {
            if (it->id == user.id) {
                users.erase(it);
                break;
            }
        }
    }
    
    // 从 users 中弹出两个元素进行匹配，并输出结果
    void match() {
        if (users.size() >= 2) {
            User user1 = users[0];
            User user2 = users[1];
            users.erase(users.begin());
            users.erase(users.begin());
            std::cout << "Match result: " << user1.name << " vs " << user2.name << std::endl;
        }
    }
};

// Task结构体
// 属性User，op(操作类型："add" 或 "remove")
struct Task {
    User user;
    std::string op;
};

// 结构体：Message_que 全局对象:message_que
// 队列元素类型:Task 
// 互斥信号量+条件变量 实现锁机制
struct Message_que {
    std::queue<Task> tasks;
    std::mutex mtx;
    std::condition_variable cv;
} message_que;

class MatchHandler : virtual public MatchIf {
public:
    MatchHandler() {
        // Your initialization goes here
    }

    int32_t add(const User& user, const std::string& name) {
        // Your implementation goes here
        printf("add request from %s\n", name.c_str());
        
        // 互斥访问 message_que 并添加元素
        {
            std::lock_guard<std::mutex> lock(message_que.mtx);
            Task task;
            task.user = user;
            task.op = "add";
            message_que.tasks.push(task);
            message_que.cv.notify_one();
        }
        return 0;
    }
	
    int32_t remove(const User& user, const std::string& name) {
        // Your implementation goes here
        printf("remove request from %s\n", name.c_str());
        
       	// 互斥访问 message_que 并添加元素
        {
            std::lock_guard<std::mutex> lock(message_que.mtx);
            Task task;
            task.user = user;
            task.op = "remove";
            message_que.tasks.push(task);
            message_que.cv.notify_one();
        }
        
        return 0;
    }
};

// 全局对象
Pool pool;

// 方法：consume_task
// 持续不断地访问message_que，并在message_que为空时，阻塞
// message_que不空，则消费一个元素，并根据操作类型(op)， pool进行增删
// 每次处理完一个任务后，调用 pool.match() 进行匹配
void consume_task() {
    while (true) {
        std::unique_lock<std::mutex> lock(message_que.mtx);
        message_que.cv.wait(lock, []{ return !message_que.tasks.empty(); });
        
        Task task = message_que.tasks.front();
        message_que.tasks.pop();
        lock.unlock();
        
        if (task.op == "add") {
            pool.add(task.user);
        } else if (task.op == "remove") {
            pool.remove(task.user);
        }
        
        // pool 不断进行匹配
        pool.match();
    }
}

int main(int argc, char **argv) {
    int port = 9090;
    ::std::shared_ptr<MatchHandler> handler(new MatchHandler());
    ::std::shared_ptr<TProcessor> processor(new MatchProcessor(handler));
    ::std::shared_ptr<TServerTransport> serverTransport(new TServerSocket(port));
    ::std::shared_ptr<TTransportFactory> transportFactory(new TBufferedTransportFactory());
    ::std::shared_ptr<TProtocolFactory> protocolFactory(new TBinaryProtocolFactory());

    TSimpleServer server(processor, serverTransport, transportFactory, protocolFactory);
    std::cout << "Starting Match Server on port " << port << "..." << std::endl;
    
    // 线程化 consume_task
    std::thread consumer_thread(consume_task);
    consumer_thread.detach();  // 分离线程，让它在后台运行
    
    std::cout << "Match Server is running..." << std::endl;
    server.serve();
  	
    
    return 0;
}
```

```

```

