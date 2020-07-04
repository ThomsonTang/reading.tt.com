## 线程的创建和启动

## 术语卡——创建线程的方式
- implement `Runnable`
- extends `Thread`
- using `ThreadFactory.newThread()`

## 术语卡——对线程的操作
- start
- sleep
- interrupt
- group

## 术语卡——线程的基本属性
- 术语： 线程的attribute
- 印象：
	- ID
	- Name
	- Priority: 优先级一般用整数 1-10 来表示，1表示最低，10表示最高。一般来说我们最好不要修改线程的优先级，除非真有这样的需求。
	- Status：java里线程有六种状态，在`Thread.State`枚举类中：
		- new
		- runnable
		- blocked
		- waiting
		- timed waiting
		- terminated
- 例子：