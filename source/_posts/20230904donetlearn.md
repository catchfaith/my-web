---
title: CSharp依赖注入和读取配置文件
date: 2023-09-01 11:21:05
categories: CSharp
---
## 1.依赖注入DI
DI的全称(Dependency Injection)
这里不得不提到一另外一个知识点
### 1.1 控制反转（IOC）
控制反转(IOC 英文全称Inversion of Control)
而依赖注入就是控制反转的实现方式
通俗的理解比如在以往的.net framework当中有个User类 我们需要去User user= new User();去获取这个User类，我们所有的需要的东西需要程序员自己手动去创造，而使用了依赖注入的方式 我们只需要在程序初始化前，将所有我们可能要用到的东西都注入到DI容器当中，等到我们要用的时候我们就可以直接从容器中去获取，类似于spring中的getbean() 从代码的层面看控制反转的目的其实也就是从我该如何创建xx对象变成我要xx对象。
控制反转的两种实现方式
- 服务定位器(ServiceLocator)
```C#
//服务定位器的实现方式
IDbConnection conn = ServiceLocator.GetService<IDbConnection>();
```
- 依赖注入(Dependency Injection,简称DI)
```C#
class Demo{
public IDbConnection Conn;
public void insertDB(){
IDbCommand cmd = Conn.CreateCommand();
}
}
```
从上面两个不同的方式去看使用服务定位器的方式是管ServiceLocator去要这个IDbConnection对象，而使用了依赖注入的方式你只需要去在Demo类当中声明一个IDbConnection对象，你就可以拿着用，至于这个对象怎么获得的在Demo实例化的时候框架就会自己给这个对象赋值可以参考spring的实现原理和@AutoWrie。
市场上现在灵活的实现了控制反转的手段大多还是使用了依赖注入的方式，所以我们学习的时候也是只关注依赖注入去深究。
### 1.2 依赖注入的几个概念
1.服务：这里可以理解为一个对象，你要拿到的对象，好比spring中的bean。
2.注册服务：在程序运行前会将你想要放进容器的对象给他放进去，也好比spring中编写spring.xml文件。
3.服务容器：也就是管理注册的服务。
4.查询服务：你需要一个服务你直接拿，好比spring中的getbean()
5.对象的生命周期：
- Transient(瞬时):每次获取都会获取一个新的对象
- Scoped(作用域):在这个返回之内获取的会是同一个对象
- Singleton(单例):每次获取的都是同一个对象
### 1.3 dotnet中使用DI
dotnet的控制反转组件取名为DependencyInjection,但是它也包含了ServiceLocator的功能。
1、下载DependencyInjection nuget包
2、引入命名空间
3、使用ServiceCollection(服务集合)来构造容器对象IServiceProvider(服务提供者)。调用ServiceCollection的BuildServiceProvider()创建的ServiceProvider,可以用来获取BuildServiceProvider()之前的ServiceCollection中的对象
操作流程就是我们将对象先放到ServiceCollection中，然后ServiceCollection再去创建IServiceProvider对象，我们再去管IServiceProvider要对象
```C# 
ServiceCollect service = new ServiceCollect();
service.AddScoped<ITestService,TestService>();
//瞬时
//service.AddTransient<ITestService,TestService>();
//单例
//service.AddSingleton<ITestService,TestService>();
//使用ServiceProvider获取对象
using(ServiceProvider sp = service.BuildServiceProvider()){
    //如果没找到返回null
   ITestService sp.GetService<ITestService>();
   //使用getrequiredService如果没找到会抛出异常
    var requiredService = (ITestService)scope.ServiceProvider.GetRequiredService(typeof(ITestService));
    //在通过ServiceProvider获取service还有其他方法具体可以参考文档
}
```
4.依赖注入是由传染性的，如果1个类的对象是通过DI注入的，那么这个类的构造函数里面的说有服务类型的参数都会被DI注入，如果一个类是手动创建的，那么这个对象就和DI没什么关系，它的构造函数中声明的服务类型参数就不会被自动赋值。好比spring中会给类里面标记了@AutoWire注解的都注入进来
如下面的例子：同时注册userService 和orderService 在userService中注入的orderService，在构造userService的时候一起被注入
```C# 
using System;
using Microsoft.Extensions.DependencyInjection;

namespace Core5Test
{
    class Program
    {
        static void Main(string[] args)
        {
            ServiceCollection service = new ServiceCollection();
            service.AddScoped<UserService>();
            service.AddScoped<OrderService>();
            using (ServiceProvider sp =service.BuildServiceProvider())
            {
                OrderService orderService = sp.GetService<OrderService>();
                orderService.printIn();
            }
        }
        
    }
     class UserService
    {
        public void hello()
        {
            Console.WriteLine("hello");
        }
    }

    class OrderService
    {
        public readonly UserService Service;

        public OrderService(UserService Service)
        {
            this.Service = Service;
        }

        public void printIn()
        {
            Service.hello();
        }
    }
}
```
5.dotnet的DI默认是构造函数注入。


## 2.配置文件
### 2.1 如何使用配置文件
1.读取配置文件的时候需要下载两个nuget包
- 1.框架的Microsoft.Extensions.Configuration
- 2.读取json文件的包Microsoft.Extensions.Configuration.json
2.new new ConfigurationBuilder();这个是用来对于Configuration进行配置的
3.AddJsonFile();增加一个json配置源
4.ConfigurationBuilder.Build()得到IConfigurationRoot，IConfigurationRoot继承了IConfiguration接口通过这个IConfigurationRoot来获取配置项
5.引入Microsoft.Extensions.Configuration.Binder包可以将配置项直接加到实体类当中使用Get<T>();方法
实操代码
``` C#
using System;
using Microsoft.Extensions.Configuration;

namespace ConfigLearn
{
    class Program
    {
        static void Main(string[] args)
        {
            //读取配置文件需要down两个包
            //1.框架的Microsoft.Extensions.Configuration
            //2.读取json文件的包Microsoft.Extensions.Configuration.json
            //new ConfigurationBuilder();用来对于Configuration进行配置的
            var builder = new ConfigurationBuilder();
            //Adds a JSON configuration source to
            //进入方法一探究竟
            // path : Path relative to the base path stored in=>相对于存储在中的基本路径的路径
            //optional:Whether the file is optional=> 文件是否可选 ,其实就是文件假如没有会不会报错 true就会报错
            //reloadOnChange:Whether the configuration should be reloaded if the file changes=>文件修改后是否重载
            builder.AddJsonFile("faith.json", optional: true, reloadOnChange: false);
            IConfigurationRoot config = builder.Build();
            var name = config["name"];
            var port = config.GetSection("proxy:port").Value;
            //直接绑定类
            //安装Microsoft.Extensions.Configuration.Binder
            var proxy = config.GetSection("proxy").Get<Proxy>();
            var config1 = config.Get<Config>();
            Console.WriteLine($"{config1.name},{config1.age},{config1.gender},{config1.Proxy.port},{config1.Proxy.address}");
            Console.WriteLine($"{proxy.port},{proxy.address}");
            Console.WriteLine("name:"+name);
            Console.WriteLine("port:"+port);
            Console.WriteLine("Hello World!");
        }
    }

    public class Config
    {
        public string name { get; set; }
        public int age { get; set; }
        public string gender { get; set; }
        public Proxy Proxy { get; set; }
    }

    public class Proxy
    {
        public string port { get; set; }
        public string address { get; set; }
    }
}
```
### 2.2 进阶使用配置文件(使用IOption<T>来操作配置文件)
那么为什么要使用这种方式来操作配置文件呢？
答案是这种方式更适合依赖注入，可以将你配置文件的内容拿出来注入到容器中，那么每次用到的时候只需要在构造函数当中构造出来就可以拿到你想要的配置文件 就不用像前面的例子一样先new ConfigrationBuilder(),再AddJsonFile(),再然后一大堆了。
首先读取配置的时候，DI有3很多Option的类型
- IOptions<T> 假设reload 还是提取的上一次的配置信息，只有程序重启会刷新
- IOptionsMonitor<T> 配置文件修改就会刷新
- IOptionsSnapshot<T> 再一个作用域范围也就是同一个scope内他的配置信息都是统一的
所以一般建议使用IOptionSnapshot<T>
话不多说上代码
```C#
           //Main方法
           //注入IOptionsSnapshot
            ServiceCollection service = new ServiceCollection();
            //从IConfigrationRoot中获得 所以要先构造IConfigrationRoot
            var configurationBuilder = new ConfigurationBuilder();
            var addJsonFile = configurationBuilder.AddJsonFile("faith.json", optional: true, reloadOnChange: true);
            var configurationRoot = addJsonFile.Build();
            //AddOptions是ServiceCollect的扩展方法
            service.AddOptions<Config>().Configure(e => configurationRoot.Bind(e));
            //注入测试类
            service.AddScoped<ConfigOptionsTest>();
            using (var buildServiceProvider = service.BuildServiceProvider())
            {
                var configOptionsTest = buildServiceProvider.GetRequiredService<ConfigOptionsTest>();
                configOptionsTest.printIn();
            }
```
测试类代码
```C#
public class ConfigOptionsTest
    {
        private readonly IOptionsSnapshot<Config> _optionsSnapshot;

        public ConfigOptionsTest(IOptionsSnapshot<Config> optionsSnapshot)
        {
            _optionsSnapshot = optionsSnapshot;
        }

        public void printIn()
        {
            Console.WriteLine(_optionsSnapshot.Value.age);
            Console.WriteLine("========================");
            Console.WriteLine(_optionsSnapshot.Value.age);
        }
    }
```
### 使用管理用户机密的方式进行读取配置文件
vs中右键项目可以找到管理用户机密方式，rider中右键项目Tools中的.Net User Secret会生成一个json文件，这个文件是存储再计算机AppData文件当中，读取代码使用Microsoft.Extensions.Configuration.UserSecrets包下的AddUserSecrets扩展方法，如下
```C#
 var configurationBuilder = new ConfigurationBuilder();
            var addUserSecrets = configurationBuilder.AddUserSecrets<Program>();
            var configurationRoot = addUserSecrets.Build();
            Console.WriteLine(configurationRoot["name"]);
```
