## Lombok

首先我们要知道Lombok是什么

```java
Project Lombok is a java library that automatically plugs into your editor and build tools, spicing up your java.Never write another getter or equals method again, with one annotation your class has a fully featured builder, Automate your logging variables, and much more.
Lombok是一个Java库，能自动插入编辑器并构建工具，简化Java开发。通过添加注解的方式，不需要为类编写getter或eques方法，同时可以自动化日志变量
```

具体怎么添加依赖和怎么安装插件就不赘诉了，dddd。

```java
@Slf4j//使用日志功能
@Getter
@Setter
//不用手写setter getter方法
@NoArgsConstructor(Access = AccessLevel.PRIVATE)//无参私有构造函数，方便单例模式
@AllArgsConstructor//全参构造函数
@EqualsAndHashCode(of = {"字段名","字段名"})//指定哪些字段参与equals hashcode重写
@ToString(of = {"字段名","字段名"})//指定哪些字段打印
@Data //包含了Getter,Setter,EqualsAndHashCode,ToString,AllArgsConstructor这些
@Accessors(fluent=true,chain=true)//对应字段的 getter 方法前面就没有 get，setter 方法就不会有 set和对应字段的 setter 方法调用后，会返回当前对象
@Builder//建造者模式
@Singular//跟builder配合使用，人性化解释操作
@SneakyThrows//异常抛出
@Synchronized//自动加关键字synchronized
@Cleanu//自动释放资源
```



参考:[Lombok还能这么玩？代码效率翻倍，爱不释手_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1hv4y1U76d/?spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=564ed778d24ae60e88181db996c35664)