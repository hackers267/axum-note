# 在Axum中使用Stream

当我们需要在web服务中传输大文件的时候，如何我们使用传统的先读取文件内容，然后再传输给客户端的方法，一个方面会导致内存使用变大，
另一个方面在读取文件的时候会花费大量的时候，客户端不会立即获得响应，等待时间过长。在这个时候，我们就需要使用 _stream(流)_ 的
方式来解决这个问题。

## 使用 _stream_ 下载文件

首先，我们要考虑的是下载文件的情况。要使用 _stream_ 下载文件，我们首先需要添加 *tokio-util*, 并启用 _io_ 的 _feature(特性)_，
在控制台执行如下的命令： `cargo add tokio_util --features io`;

然后使用如下的示例代码即可：

```rust
use axum::{
    body::Body,
    http::{header::CONTENT_DISPOSITION, HeaderMap, HeaderValue, StatusCode},
    response::{IntoResponse, Response},
};
use tokio_util::io::ReaderStream;

pub(crate) async fn get_file() -> Response {
    let path = "Cargo.toml"; // 需要读取的文件路径
    let filename = "Cargo.toml"; // 下载的默认文件名
    let file = match tokio::fs::File::open(&path).await {
        Ok(file) => file,
        Err(err) => {
            return (StatusCode::NOT_FOUND, format!("File not found,{}", err)).into_response()
        }
    };
    let stream = ReaderStream::new(file);
    let body = Body::from_stream(stream);
    let mut header = HeaderMap::new();
    header.insert(
        CONTENT_DISPOSITION,
        HeaderValue::from_str(&format!("attachment;filename='{}'", filename)).unwrap(),
    );
    (header, body).into_response()
}
```
