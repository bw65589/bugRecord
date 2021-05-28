# bugRecord

# 2021.05.28


系统启动时，spring配置文件解析失败，报”cvc-elt.1: “找不到元素 'beans' 的声明”异常

解决方案来源于（https://www.cnblogs.com/longshiyVip/p/4574070.html）

现象：spring加载配置文件applicationContext.xml出错,抛出nested exception is og.xml.sax.SAXParseException; lineNumber: 12; columnNumber: 47; cvc-elt.1: 找不到元素 'beans' 的声明的异常信息。


造成该异常原因有两种：

第一,配置文件头部配置的xsd版本信息不正确，造成解析时出错。spring头部xsd或dtd校验文件的查找分两步，第一先从本地jar包中找，如果找到则用本地jar包的进行校验（可以在spring-beans.jar或spring-context.jar里的META-INF下的spring-schemas文件中找到xsd文件位置的定义），如果没有找到则进行第二步查找，它会尝试从网络中下载该文件然后校验，如果系统断网或下载不下来，则会抛出上述异常.
解决办法 ： 将applicationContext.xml中xsd文件定义的版本改为spring jar包中定义的xsd的版本，如果版本定义的太高在本地会无法找到，只能从网络上下载。
例如我的spring jar包是3.0.5版本的,但是我的xsd写的是3.2的,有时候如果系统断网或下载不下来就会报这个问题,把3.2改成3.0就解决了。


第二,在dtd中缺少 xmlns="http://www.springframework.org/schema/beans"也会出现这个问题。
解决方法:在spring配置文件中加入xmlns="http://www.springframework.org/schema/beans即可解决。
