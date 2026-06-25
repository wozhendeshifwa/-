几个常见概念：
JSON : 系统之间交换的结构化数据格式

```java
@RestController  绑定的类中的返回值以JSON格式传给前端
@Controller  返回值渲染页面路径或视图
```

Maven 项目结构：

- mvnw和mvnw.cmd：这是Maven包装器（wrapper）脚本。借助这些脚本，即便你的机器上没有安装Maven，也可以构建项目。
- pom.xml：这是Maven构建规范，随后我们将会深入介绍该文件。
- Application.java：这是Spring Boot 主类，也叫引导类
- application.properties：初始为空，但是它会为我们提供指定配置属性的方法。
- static：在这个文件夹下，存放任意为浏览器提供服务的静态内容（images、css、js 文件）
- templates：存放用来渲染内容到浏览器上的模板文件。初始为空，配合Thymeleaf模板使用
- ApplicationTests.java：简单测试类，确保Spring应用上下文成功加载



```yaml
任务：
    描述：生成后端项目骨架
    要点：springboot、maven
    依赖：Spring Web、themeleaf、devtools

任务：
    描述:生成前端项目骨架
    要点：vue3
    依赖：Pinia、router、vite

任务:
	描述:创建导航栏组件
	导航:logo、"对战"、"对局列表"、"排行榜"、"个人中心"

任务：
	描述：绘制logo
	背景：king of battle
	格式：svg

任务:
	描述:创建404单页面组件

任务:
	秒速:导航“个人中兴”添加下拉菜单
	下拉选项："个人中心"，"退出"
```

