# Axum路由

在`Axum`中要使用路由就要使用到`Router`这个结构体，最常使用的是这个结构体的以下方法：
* new  创建一个新的`Router`
* route  设置路由
* route_service  设置路由服务
* nest   使用嵌套路由
* nest_service   使用嵌套路由服务
* merge  组合路由
* layer  把`tower::Layer`应用到所有路由上
