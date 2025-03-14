# CORS

CORS middleware can be used for [Cross-Origin Resource Sharing](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CORS).

Since the browser will send `Method::OPTIONS` requests, it is necessary to increase the handling of such requests. You can add `empty_handler` to the root `Router` to handle this situation once and for all.

## Config Cargo.toml

```toml
salvo = { version = "*", features = ["cors"] }
```

## Sample Code

```rust
use salvo::prelude::*;
use salvo::cors::Cors;

#[handler]
async fn hello() -> &'static str {
    "hello"
}

#[tokio::main]
async fn main() {
    tracing_subscriber::fmt().init();

    let cors_handler = Cors::builder()
        .allow_origin("https://salvo.rs")
        .allow_methods(vec!["GET", "POST", "DELETE"])
        .build();

    let router = Router::with_hoop(cors_handler).get(hello).options(empty_handler);
    Server::new(TcpListener::bind("127.0.0.1:7878")).serve(router).await;
}
```
