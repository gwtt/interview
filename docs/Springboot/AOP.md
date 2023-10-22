### Spring的AOP的是什么？底层是怎么实现的？

> AOP面向切面编程，通过预编译方式和运行期间动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。
>
> 然后AOP的底层原理就是使用了动态代理
>
> 主要分为两种情况
>
> （1）有接口，使用了JDK的动态代理
>
> （2）没有接口，使用了CGLIB代理



### AOP采用了代理模式，说说什么是代理模式？给定一个场景说说代理类该怎么写？

> 代理模式就是指为某个对象提供一个代理对象，并且由代理对象控制对原对象的访问。
>
> 说说代理类怎么写是吧。
>
> 最容易想到的例子就是律师和平民类。
>
> 因为jdk动态代理用到了两个重要的类和接口，一个是`InvocationHandler`接口、另一个则是 `Proxy`类，这个类和接口是实现我们动态代理所必须用到的。`InvocationHandler`接口是给动态代理类实现的，负责处理被代理对象的操作的，而`Proxy`是用来创建动态代理类实例对象的，因为只有得到了这个对象我们才能调用那些需要代理的方法。
>
> 第一步 构建你这个类，实现接口，重写对应的功能
>
> ```java
> public class You implements ILawSuit {
>     @Override
>     public void submit(String proof) {
>         System.out.println(String.format("证据如下：%s",proof));
>     }
> 
>     @Override
>     public void defend() {
>         System.out.println(String.format("我无罪"));
>     }
> }
> ```
>
> 第二步 构建一个动态代理类实现InvocationHandler接口
>
> ```java
> public class DynProxyLawyer implements InvocationHandler {
>     private Object target;//被代理的对象
>     public DynProxyLawyer(Object obj){
>         this.target=obj;
>     }
>     @Override
>     public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
>         System.out.println("案件进展："+method.getName());
>         Object result=method.invoke(target,args);
>         return result;
>     }
> }
> ```
>
> 第三步 实现工厂方法，利用proxy类实例化代理对象
>
> ```java
> public class ProxyFactory {
>     public static Object getDynProxy(Object target) {
>         InvocationHandler handler = new DynProxyLawyer(target);
>         return Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(), handler);
>     }
> }
> ```
>
> 第四步 客户端的调用
>
> ```java
>     public static void main(String[] args) {
>         ILawSuit proxy= (ILawSuit) ProxyFactory.getDynProxy(new You());
>         proxy.submit("工资流水在此");
>         proxy.defend();
>     }
> ```
>
> 如果要求简单说个静态代理就行了，去看下自己写的设计模式这边。还是很好理解的。



### **拦截器与过滤器的区别**

> **相同点**：
>
> 1、拦截器与过滤器都是体现了AOP的思想，对方法实现增强，都可以拦截请求方法。
>
> 2、拦截器和过滤器都可以通过Order注解设定执行顺序
>
> **不同点**：
>
> 1、**过滤器属于Servlet级别，拦截器属于Spring级别** Filter是在javax.servlet包中定义的，要依赖于网络容器，因此只能在web项目中使用。
>
> Interceptor是SpringMVC中实现的，归根揭底拦截器是一个Spring组件，由Spring容器进行管理。
>
> 2、**过滤器和拦截器的执行顺序不同**：
>
> 下面通过一张图展示Filter和Interceprtor的执行顺序
>
> **拦截器的应用场景**：权限控制，日志打印，参数校验
>
> **过滤器的应用场景**：跨域问题解决，编码转换



### 跨域问题是什么

> **当一个资源去访问另一个不同域名或者同域名不同端口的资源时，就会发出跨域请求。 如果此时另一个资源不允许其进行跨域资源访问，那么访问就会遇到跨域问题**。
