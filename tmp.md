```
  1 from match_client.match import Match
  2 from match_client.match.ttypes import User
  3 
  4 from thrift import Thrift
  5 from thrift.transport import TSocket
  6 from thrift.transport import TTransport
  7 from thrift.protocol import TBinaryProtocol
  8 
  9 '''
 10 结构体：User
 11 属性：id(int) name(string) score(int)
 12 
 13 服务端接口：add_user remove_user
 14 '''
 15 
 16 def operate(op,id,name,score):
 17     # Make socket
 18     transport = TSocket.TSocket('localhost', 9090)
 19 
 20     # Buffering is critical. Raw sockets are very slow
 21     transport = TTransport.TBufferedTransport(transport)
 22 
 23     # Wrap in a protocol
 24     protocol = TBinaryProtocol.TBinaryProtocol(transport)
 25 
 26     # Create a client to use the protocol encoder
 27     client = Calculator.Client(protocol)
 28 
 29     # Connect!
 30     transport.open()
 31 ----
 32     # 根据 op，调用服务端接口函数
 33     xxx
 34 
 35 
 36     # Close!
 37     transport.close()
 38 
 39 # 函数：main 功能：持续不断读入一行：op,id,name,score
 40 # 字段匹配、输入合法
 41 xxx                                                                                                                                                                                            
 42 
 43 # 执行main()
 xxx;
```

