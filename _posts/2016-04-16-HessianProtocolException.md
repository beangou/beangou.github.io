---
layout: default
title: hessian.io.HessianProtocolException
---
<pre><code>
	org.springframework.remoting.RemoteAccessException: Cannot access Hessian remote service 	at [http://xxx.xxx.xxx.xxx:8080/orderservice/createOrder]; nested exception is 	com.caucho.hessian.io.HessianProtocolException: '￿' is an unknown code
	at  org.springframework.remoting.caucho.HessianClientInterceptor.convertHessianAccessException(Hes 	sianClientInterceptor.java:293)
	at 			org.springframework.remoting.caucho.HessianClientInterceptor.invoke(HessianClientInterceptor.java:261)
	at 	org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179)
	at org.springframework.aop.framework.JdkDynamicAopProxy.invoke(JdkDynamicAopProxy.java:207)
	at com.sun.proxy.$Proxy20.createOrder(Unknown Source)
	at com.store59.seckill.api.Order.testCreateOrder(Order.java:111)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.junit.runners.model.FrameworkMethod$1.runReflectiveCall(FrameworkMethod.java:50)
	at org.junit.internal.runners.model.ReflectiveCallable.run(ReflectiveCallable.java:12)
	at org.junit.runners.model.FrameworkMethod.invokeExplosively(FrameworkMethod.java:47)
	at org.junit.internal.runners.statements.InvokeMethod.evaluate(InvokeMethod.java:17)
	at org.junit.runners.ParentRunner.runLeaf(ParentRunner.java:325)
	at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:78)
	at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:57)
	at org.junit.runners.ParentRunner$3.run(ParentRunner.java:290)
	at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:71)
	at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:288)
	at org.junit.runners.ParentRunner.access$000(ParentRunner.java:58)
	at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:268)
	at org.junit.runners.ParentRunner.run(ParentRunner.java:363)
	at org.eclipse.jdt.internal.junit4.runner.JUnit4TestReference.run(JUnit4TestReference.java:86)
	at org.eclipse.jdt.internal.junit.runner.TestExecution.run(TestExecution.java:38)
	at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.runTests(RemoteTestRunner.java:459)
	at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.runTests(RemoteTestRunner.java:675)
	at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.run(RemoteTestRunner.java:382)
	at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.main(RemoteTestRunner.java:192)
Caused by: com.caucho.hessian.io.HessianProtocolException: '￿' is an unknown code
	at com.caucho.hessian.client.HessianProxy.invoke(HessianProxy.java:220)
	at com.sun.proxy.$Proxy19.createOrder(Unknown Source)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at 	org.springframework.remoting.caucho.HessianClientInterceptor.invoke(HessianClientInterceptor.java:248)
	... 27 more
</pre></code>
使用hessian时出现这个错误，一般情况下，不是程序问题，考虑是不是hessian客户端和服务端的类（接口）版本是否一致？
是否增加属性、方法等？如果使用了maven，看看hessian服务的版本是否保持一致？解决办法，可以强制更新下项目依赖的jar包。 Update maven project