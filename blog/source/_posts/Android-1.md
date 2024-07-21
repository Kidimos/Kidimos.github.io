---
title: Android-1
date: 2023-07-24 17:23:00
tags:
- 安卓开发
- 学习
- 编程
categories:
- 学习
- 编程
- 安卓开发

---

学习目的：实习（大概也许能算是）期间被分配到了安卓开发的任务，需要学习Kt以及安卓项目的开发。

## 项目结构

### 分类

#### 分为app（代表app模块）、Gradle Scripts

##### app：

分为manifest、java和res

manifest子目录下面只有一个XML文件，是App的运行配置文件

Java子目录下面存放当前模块的java源代码以及测试

res子目录存放当前模块的资源文件：

1. drawable目录存放图形描述文件与图片文件
2. layout目录存放App页面的布局文件
3. mipmap目录存放App的启动图标
4. values目录存放一些常量定义文件，例如字符串常量strings.xml等

**特别注意**：application下面还有个activity节点，它是活动页面的注册声明，只有在mainfest中正确配置了activity节点，才能在运行时候访问对应的活动页面。

##### Gradle Scripts：

主要是工程的编译配置文件

## 开发

#### 文本显示

##### 设置文本内容

两种方式来设置文本内容：

* 在XML文件中通过属性android:text来设置文本
* 在Java代码中调用文本视图对象的setText方法设置文本

##### 引用字符串资源

两种方式引用字符串资源

* 在XML文件中引用（@string/xxx）
* 在Java代码中引用（R.string.xxx）

#### 跳转页面步骤

1. ###### 首先，在layout包编写代表页面布局的xml文件

2. ###### 然后，创建继承了BaseActivity类的新Activity类。

3. ###### 在manifest里，创建一个activity的元素，在元素里声明：

   {% codeblock%}
   <manifest>
       <activity android:name=".activity.TestActivity"
                     android:exported="true">
               <intent-filter>
                   <action android:name="android.intent.action.test"/>
                   <category android:name="android.intent.category.DEFAULT"/>
               </intent-filter>
           </activity>
   </manifest>
   {% endcodeblock %}

4. ###### 然后在要跳转到那个页面的Activity类中

   {% codeblock [lang:Kotlin] %}
   testButton.setOnClickListener {
               showToast("This is a test button",Toast.LENGTH_LONG)
               //Test功能目前自定义为可以跳转到一个新的页面
               var intent = Intent(this,TestActivity::class.java);
               startActivity(intent);
   }
   {% endcodeblock %}

5. ###### 即可完成跳转页面。

(这逆天排版我是真受不了了)