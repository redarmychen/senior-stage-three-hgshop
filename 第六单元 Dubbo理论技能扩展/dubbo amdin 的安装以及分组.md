# dubbo amdin 的安装

  直接把dubbo-admin-2.5.8 拷贝到tomcat 的webapps 目录下即可，

 此时连接的zk（注册中心是本机127.0.0.1）

如需要修改配置则修改dubbo-admin-2.5.8\WEB-INF\dubbo.properties

里边默认的文件内容是：



#注册中心的地址

dubbo.registry.address=zookeeper://127.0.0.1:2181

#用户明和密码  

dubbo.admin.root.password=123456
dubbo.admin.guest.password=guest





# 服务分组

## 概念

   通一个接口使用具有多个相同的实现，则需要服务分组。

##   提供者的写法



!-- 声明需要暴露的服务接口 -->
<dubbo:service interface="com.zhuzg1711d.group.service.DemoService"  group="shanghai" ref="demoService1" />
<dubbo:service interface="com.zhuzg1711d.group.service.DemoService"  group="shanghai"  ref="demoService2" />
<dubbo:service interface="com.zhuzg1711d.group.service.DemoService" group="beijing" ref="demoService3" />



## 调用者的写法：



    <!-- 声明需要暴露的服务接口 -->
    <dubbo:reference interface="com.zhuzg1711d.group.service.DemoService" group="shanghai" id="demoServicesh" />
    <dubbo:reference interface="com.zhuzg1711d.group.service.DemoService" group="beijing" id="demoServicebj" />
 <dubbo:reference interface="com.zhuzg1711d.group.service.DemoService" id="demoService" />



## 应用场景

​      服务就近。



# 分组合并

​	一个接口有多个实现，现在消费者需要的内容将多个服务提供者的结果需要合并到一起。

​      学习本内容需要再理解服务分组的基础之上进行。

## 自然合并：

​	对于List 、 set 、map 是自然合并的。

```xml
<dubbo:reference interface="com.mmcro.student.service.StudentService" 
 id="myStudentc" group="b,c" >
 	<dubbo:method name="getStus" merger="true"></dubbo:method>
 </dubbo:reference>
```


## 自定义分组函数

```xml
    <dubbo:reference interface="com.mmcro.student.service.StudentService" 
 id="myStudentc" group="b,c" >
 	<dubbo:method name="calTotal" merger=".myMerge"></dubbo:method>
 </dubbo:reference>
```
  这里的myMerge 是calTotal返回类型的一个函数，且其参数也是也是返回值类型。作用是将两个数据进行叠加，叠加的算法在这个函数内部实现。

 group 的作用 是将这两个分组的结果进行合并。



#     

