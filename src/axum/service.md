# Axum中的服务

## 静态文件服务

当我们需要在 *axum* 中使用静态文件服务的时候，可以参考下面的例子：

```rust
use std::net::SocketAddr;
use tower_http::services::ServeDir;

#[tokio::main]
fn main() {
  let addr = SocketAddr::from([127, 0, 0, 1], 8080);
  let listener = tokio::net::TcpListener::bind(addr).await.unwrap();
  let app = using_server_dir();
  axum::serve(listener,app)
    .await
    .unwrap();
}

fn using_server_dir() -> Router {
  Router::new()
  .nest_service("/assets", ServeDir::new("assets"))
}
```

> 注意：要使用静态文件服务，需要安装 *tower_http* 并启用 *fs* 特性. [更多示例](https://github.com/tokio-rs/axum/tree/main/examples/static-file-server)
