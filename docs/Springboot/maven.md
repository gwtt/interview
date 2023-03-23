### maven的依赖原则

> 当一个项目中出现重复引用依赖jar包时，maven一般有如下三种原则处理jar
>
> 1.最短路径原则
>
> ```java
> A -> B -> C -> D(V1)
> F -> G -> D(V2)
> 这个时候项目中就出现了两个版本的D，这时maven会采用最短路径原则，选择V2版本的D，因为V1版本的D是由A包间接依赖的，整个依赖路径长度为3，而V2版本的D是由F包间接依赖的，整个依赖路径长度为2。
> ```
>
> 2.优先声明原则
>
> ```java
> A -> B -> D(V1)
> F -> G -> D(V2)    
> 如果两个jar包版本路径深度相同，则使用优先声明的版本
> ```
>
> 3.多次直引不同版本的jar时，使用最后声明的版本)本人实测)
>
> ```java
> <dependency>
>     <groupId>org.springframework</groupId>
>     <artifactId>spring-core</artifactId>
>     <version>4.3.17.RELEASE</version>
> </dependency>
> 
> <dependency>
>     <groupId>org.springframework</groupId>
>     <artifactId>spring-core</artifactId>
>     <version>4.3.20.RELEASE</version>
> </dependency>
>     
> 如果再pom文件中，同时引用了如上两个版本，则会使用4.3.20.RELEASE版本
> ```

