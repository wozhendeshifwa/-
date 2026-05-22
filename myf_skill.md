视角仅仅缩小在 xxx 周围：

尽量保持内容、结构不变，从注释内容、语法结构、功能逻辑三个方面，补全xxx

如有代码重构，请直接给出完整代码

如若添加注释，则参照原注释的模样



---

代码补全，严格语法

注释补全，少即是多

```thrift
namespace cpp match_service

// User 定义
struct User{
1:i32 id,
2:string name,
3:i32 score
}

//远程服务接口
service Match{
	// 增
	i32 add(1:User user, 2:string info),
	// 删
	i32 remove(1:User user, 2:string info),
}
```

