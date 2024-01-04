# Axum路由

在`Axum`中要使用路由就要使用到`Router`这个结构体，最常使用的是这个结构体的以下方法：
* new  创建一个新的`Router`
* route  设置路由
* route_service  设置路由服务
* nest   使用嵌套路由
* nest_service   使用嵌套路由服务
* merge  组合路由
* layer  把`tower::Layer`应用到所有路由上

## 请求参数

使用 *axum* 的路由系统的时候，在一些情况下，需要获取请求中的参数,这些请求参数大致可以分为 _Query_, _Path_ 和 _Json_ 等个
类型，下面就分别就这些类型进行说明：

### *Query* 类型的参数

*Query* 类型的参数是以 *key=value* 的形式出现在请求地址中的，形如： *https://example.com?name=tim* ，这里的请求参数就是 *name=time* .

如何在 *axum* 中获取这个参数呢？我们可以参考下面这个例子:

```rust
use serde::Deserialize;
use axum::routing::get;
use axum::Router;
use axum::extract::Query;

#[derive(Debug, Deserialize)]
struct Example {
  name: String
}

fn example_route() -> Router {
  Router::new()
    .route("/example", get(example_handler))
}

async fn example_handler(Query(example):Query<Example>) {
  let name = example.name;
  println!("name is: {}", name);
}
```

### *Path* 类型的参数

*Path* 类型的参数是以 */goods/1234* 的形式出现在请求地址中的，形如: *https://example.com/goods/1234* 部分要得到 *1234* 的内容。这个一般会用在详情页之类的页面地址中。

如果要在 *axum* 中获取这个参数，可以参考下面的例子:

```rust
use axum::routing::get;
use axum::Router;
use axum::extract::Path;

fn example_route() -> Router {
  Router::new()
  .route("/goods/:id", get(example_handler))
}

async fn example_handler(Path(id): Path<String>) {
  println!("id is: {}", id);
}
```

### *Json* 类型的参数

*Json* 类型的参数是以请求体的方式存在于请求中的，一般会出现在 *put*,*post* 等请求中。例如下面的例子：

```rust
use axum::{
    extract::Json,
    routing::post,
    Router,
};
use serde::Deserialize;

#[derive(Deserialize)]
struct CreateUser {
    email: String,
    password: String,
}

fn example_route() -> Router {
  Router::new()
  .route("/users", post(create_user))
}

async fn create_user(Json(payload): Json<CreateUser>) {
    // payload is a `CreateUser`
}

```
