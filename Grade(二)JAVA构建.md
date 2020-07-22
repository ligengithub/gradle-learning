#### Java插件

Gradle 对java的支持，是通过插件实现的。Java 插件是基于合约的，
这意味着插件已经给项目的许多方面定义了默认的参数, 
比如 Java 源文件的位置. 如果你在项目里遵从这些合约，那么只需要很少的配置。

#### 引入java插件
~~~
apply plugin: 'java'
~~~


java 插件在项目中添加许多任务。通常只会用到其中的一小部分任务。常用的任务有

- build 编译和测试代码，生成jar包
- clean 清除build生成的文件
- assemble 编译打包代码，但是不运行测试
- check 编译和测试代码

#### 指定仓库

~~~
// maven 仓库
repositories {
    mavenCentral()
}
~~~
#### 指定依赖
~~~
dependencies {
//    编译阶段
    compile group: 'commons-collections'
//    测试阶段
    testCompile group: 'junit', name: 'junit', version: '4.12'
}

~~~
