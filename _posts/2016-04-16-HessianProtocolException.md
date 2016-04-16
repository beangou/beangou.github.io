---
layout: default
title: hessian.io.HessianProtocolException: '￿' is an unknown code
---


使用hessian时出现这个错误，一般情况下，不是程序问题，考虑是不是hessian客户端和服务端的类（接口）版本是否一致？
是否增加属性、方法等？如果使用了maven，看看hessian服务的版本是否保持一致？解决办法，可以强制更新下项目依赖的jar包。 Update maven project