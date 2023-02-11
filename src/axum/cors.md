# 设置Axum跨源资源共享

## 引入依赖

在需要使Axum支持跨源资源共享的时候，需要引入`tower-http`模块,并开启`cors`功能。如：
```toml
[dependencies]
tower-http = { version = "0.3.5", features = ["cors"]}
```

## 代码示例

```rust
use axum::Router;
use tower::http::CorsLayer;
#[tokio::main]
async fn main() {
    let app = Router::new()
        .route("/",get())
        .layer(
            CorsLayer::new()
                .allow_origin(Any)
                .allow_methods(Any)
                .allow_headers(Any),
        );
}
```

示例代码讲解：

在上面的代码中，使用了`tower`的`layer`作为中间件对`Axum`的路由进行的设置。`CorsLayer`的作用就是设置`CORS跨源资源共享`的一个`Layer`。
方法如下：

* new 创建一个`CorsLayer`结构体
* permissive  生成一个`CorsLayer`结构体，其`allow_origin`,`allow_methods`,`allow_headers`,`expose_headers`的值都为`Any`
* very_permissive 
* allow_origin 指定允许跨源资源共享的`origin`
* allow_methods 指定能纹跨源资源共享的请求方法
* allow_headers 指定允许跨源资源共享的请求标头
* allow_credentials 指定允许的认证信息
* max_age   指定跨源资源共享的最大缓存时间
* expose_headers 指定那些响应标头可以暴露给浏览器中运行的脚本(指可以在浏览器端可以使用脚本文件获取的响应标头的字段)
* vary  指定Vary字段
