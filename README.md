# rust-webassembly-quickstart
如果你想在 Rust 中使用 WebAssembly，以下是一个简单的例子，展示如何创建一个小的 WebAssembly 应用，该应用可以在网页中运行并提供一个简单的加法功能。

### 步骤 1: 准备工作

首先，你需要安装 Rust 的工具链和 `wasm-pack` 工具。`wasm-pack` 是一个帮助你构建和打包 Rust 代码为 WebAssembly 的工具。

- 安装 Rust：[https://www.rust-lang.org/tools/install](https://www.rust-lang.org/tools/install)
- 安装 `wasm-pack`：`cargo install wasm-pack`

### 步骤 2: 创建一个新的 Rust 项目

在命令行中运行以下命令来创建一个新的 Rust 库项目：

```bash
cargo new --lib rust_wasm_example
cd rust_wasm_example
```

### 步骤 3: 配置 Cargo.toml

修改 `Cargo.toml` 文件来指定 crate 类型为 `cdylib`，以便生成 WebAssembly 可以使用的二进制文件，并添加 `wasm-bindgen` 依赖，用于将 Rust 函数绑定到 JavaScript。

```toml
[package]
name = "rust_wasm_example"
version = "0.1.0"
edition = "2018"

[lib]
crate-type = ["cdylib"]

[dependencies]
wasm-bindgen = "0.2"
```

### 步骤 4: 编写 Rust 代码

在 `src/lib.rs` 中，使用 `wasm-bindgen` 宏来定义一个可以从 JavaScript 访问的函数：

```rust
use wasm_bindgen::prelude::*;

#[wasm_bindgen]
pub fn add(x: i32, y: i32) -> i32 {
    x + y
}
```

### 步骤 5: 构建项目

使用 `wasm-pack` 构建 WebAssembly 包：

```bash
wasm-pack build --target web
```

这将在项目的 `pkg` 目录下生成 WebAssembly 和 JavaScript 文件。

### 步骤 6: 在网页中使用

创建一个 HTML 文件来加载并使用这个 WebAssembly 模块。在 `rust_wasm_example` 目录中创建一个名为 `index.html` 的文件：

```html
<!DOCTYPE html>
<html>
<head>
    <title>Rust WebAssembly Example</title>
    <script type="module">
        import init, { add } from './pkg/rust_wasm_example.js';

        async function run() {
            await init();
            console.log("2 + 3 =", add(2, 3));
        }

        run();
    </script>
</head>
<body>
    <h1>Rust + WebAssembly Example</h1>
</body>
</html>
```

### 步骤 7: 运行

使用任何静态文件服务器来提供包含 `index.html` 和 `pkg/` 目录的文件。如果你有 Python 安装，可以使用以下命令快速启动一个服务器：

```bash
python3 -m http.server
```

然后，在浏览器中访问 `http://localhost:8000` 查看结果。

这个示例展示了如何使用 Rust 和 WebAssembly 来实现一个简单的加法功能，你可以在此基础上继续扩展和探索更复杂的功能。
