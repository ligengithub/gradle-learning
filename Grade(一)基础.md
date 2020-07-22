#### Gradle

java 环境  JDK6以上

自带groovy库


#### projects tasks

- projects 项目
- tasks 任务

每一个构建都是由一个或者多个projects组成，一个project代表一件想做的事情

每一个project 由一个或者多个tasks 构成，一个task 表示更加细化的工作，可能是编译classes 创建一个jar。生成javaDoc，或者生成某个目录的压缩文件。


#### gradle 命令

运行一个gradle命令会在当前的目录下寻找一个build.grade的文件，

build.grade  构建脚本，定义了project 和task

- grade -q

表示quilt，它表示不打印gradle的日志信息。

#### 定义task

~~~
task hello {
    doLast {
        println 'Hello world!'
    }
}




// 测试了6.5.1  已经不支持如下写法（<< 是doLast 的简称），将 << 换成 doLast
// 错误
task hello2 << {

}

// 正确
task hello2 doLast {
    String s = '123123'
        println s.toUpperCase()
}
~~~

####  任务依赖
taskB依赖taskA ，taskA 不需要提前定义，testA 可以定义在taskB 之前也可以定义在taskB之后。
~~~
task taskB(dependsOn: 'taskA') {
    doLast {
        println "I'm Gradle"
    }
}


task taskA {
    doLast {
        println 'Hello world!'
    }
}
~~~
#### 动态任务

 使用gradle -q task1  或者 gradle -q task1.... 执行，注意使用 $ 占位符，需要使用 "",而不能使用''
~~~

4.times {counter->
    task "task$counter" doLast {
        println "task $counter"

}}

~~~

#### 给创建的任务添加额外的依赖

~~~
// 添加额外的依赖
4.times {counter->
    task "taskMore$counter" doLast {
        println "taskMore $counter"

    }}

taskMore1.dependsOn(taskMore2,taskMore3)
~~~
输出结果，taskMore1 执行前会先执行依赖的任务
~~~
taskMore 2
taskMore 3
taskMore 1
~~~

####  给已经存在的任务添加行为
~~~
//  给已经存在的任务添加行为

task addBehavior doLast {
    println 'behavior'
}

addBehavior.doFirst {
    println(" befor2")
}
addBehavior.doFirst {
    println(" befor1")
}

addBehavior.doLast {
    println(" last1")
}
addBehavior.doLast {
    println(" last2")
}
~~~
结果:
~~~
 befor1
 befor2
behavior
 last1
 last2
 
~~~

#### 短标记法 $ 
 
 短标记$ 符号，可以访问一个已经存在的任务
 
 ~~~
 task taskShortFlag doLast {
     println "taskShortFlag"
 }
 
 taskShortFlag.doLast {
     println "name = $taskShortFlag.name ...."
 }
 ~~~
 结果:
 ~~~
 taskShortFlag
 name = taskShortFlag ....
 ~~~
 
 #### 自定义任务属性
 
 ~~~
 task customProperty{
     ext.myProperty = "value 1"
 }
 
 customProperty.doFirst {
     println(customProperty.myProperty)
 }
 ~~~
 结果
 ~~~
 value 1
 ~~~
 
 #### 调用Ant任务

Ant 任务是 Gradle 的一等公民，Gradle 通过Groovy 集成了Ant 任务，Groovy 自带了一个AntBuilder。

~~~
task loadfile doLast {
    def files = file('./').listFiles()
    files.each { File file ->
        if (file.isFile()) {
            ant.loadfile(srcFile: file, property: file.name)
            println " *** $file.name ***"
            println "${ant.properties[file.name]}"
        }
    }
    
}
~~~
结果，将当前目录下文件名和文件内容在控制台输出。

#### 抽取方法

~~~
task methodInvoke doLast {
    println(method1())
    println("方法调用完成")
}

String method1(){
    return "method1"
}

~~~

输出
~~~
method1
方法调用完成
~~~


