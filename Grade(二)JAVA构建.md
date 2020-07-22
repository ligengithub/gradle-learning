#### Java插件

Gradle 对java的支持，是通过插件实现的。Java 插件是基于合约的，
这意味着插件已经给项目的许多方面定义了默认的参数, 
比如 Java 源文件的位置. 如果你在项目里遵从这些合约，那么只需要很少的配置。

#### 引入java插件
~~~
apply plugin: 'java'
~~~
或者
~~~
plugins {
    id 'java'
}
~~~

然后就可以使用插件中的任务了。java 插件在项目中添加许多任务。通常只会用到其中的一小部分任务。常用的任务有

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
#### 引入依赖
~~~
dependencies {
//    编译阶段
    compile group: 'commons-collections'
//    测试阶段
    testCompile group: 'junit', name: 'junit', version: '4.12'
}

~~~

#### 定制操作
java 插件给 项目添加了一些默认属性，并赋予了初值。当然也可以根据自己的需求改变值

查看默认属性:

~~~
gradle properties
~~~
声明当前的jdk版本和指定当前项目的版本:

~~~
sourceCompatibility = 1.5
version = '1.0'
jar {
    manifest {
        attributes 'Implementation-Title': 'Gradle Quickstart', 'Implementation-Version': version
    }
}
~~~
测试阶段添加系统属性:

~~~
test {
    systemProperties 'property': 'value'
}
~~~



#### 发布jar 文件
~~~
uploadArchives {
    repositories {
        flatDir {
            // 指定生成jar包的位置
            dirs 'repos'
        }
    }
}
~~~


#### 一个完整的java构建脚本

~~~
apply plugin: 'java'
apply plugin: 'eclipse'

sourceCompatibility = 1.5
version = '1.0'
jar {
    manifest {
        attributes 'Implementation-Title': 'Gradle Quickstart', 'Implementation-Version': version
    }
}

repositories {
    mavenCentral()
}

dependencies {
    compile group: 'commons-collections', name: 'commons-collections', version: '3.2'
    testCompile group: 'junit', name: 'junit', version: '4.+'
}

test {
    systemProperties 'property': 'value'
}

uploadArchives {
    repositories {
       flatDir {
           dirs 'repos'
       }
    }
}
~~~


####  多项目构建

step1: setting.gradle 声明 项目包含的模块

~~~
include "common", "gradle-java"

~~~
step2: 添加公共配置

~~~
subprojects {
    apply plugin: 'java'

    repositories {
       mavenCentral()
    }

    dependencies {
        testCompile 'junit:junit:4.11'
    }

    version = '1.0'

    jar {
        manifest.attributes provider: 'gradle'
    }
}
~~~

