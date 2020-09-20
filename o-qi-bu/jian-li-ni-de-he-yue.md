# 建立你的合约

运行以下命令来编译您的智能合约：

```text
cargo +nightly contract build
```

这个特殊命令将使爱上ink！ 它将项目映射到Wasm二进制文件中，然后可以将其部署到链中。 如果一切顺利，您应该看到一个包含此.wasm文件的目标文件夹。

```text
target
└── flipper.wasm
```

### 合约元数据

通过运行下一个命令，我们将生成合同元数据（也称为合同ABI）：

```text
cargo +nightly contract generate-metadata
```

您应该在同一目标目录中有一个新的JSON文件（metadata.json）：

```text
target
├── flipper.wasm
└── metadata.json
```

让我们看一下其中的结构：

```text
{
  "registry": {
    "strings": [...],
    "types": [...]
  },
  "storage": {...},
  "contract": {
    "name": ...,
    "constructors": [...],
    "messages": [...],
    "events": [],
    "docs": []
  }
}
```

您可以看到该文件描述了可用于与合同进行交互的所有接口。

Registry提供了在其余JSON中使用的字符串和字符串类型。 存储定义了合同管理的所有存储项目以及如何最终访问它们。 合同存储有关可调用函数的信息，例如构造函数和用户可以调用以与合同进行交互的消息。 它还具有有用的信息，例如合同或任何文档发出的事件。 如果仔细查看构造函数和消息，还会注意到选择器，它是函数名称的4字节哈希，用于将合同调用路由到正确的函数。

Polkadot-JS Apps使用此文件生成一个友好的界面，用于部署合同并与之交互。 :\)

在下一节中，我们将启动一个Substrate节点并配置Polkadot-JS Apps UI使其与之交互。

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 学习更多

ink！ 提供在我们的Cargo.toml文件上启用的内置溢出保护。 建议保持启用状态，以防止合同中潜在的溢出错

```text
[profile.release]
panic = "abort"           <-- Panics shall be treated as aborts: reduces binary size
lto = true                <-- enable link-time-optimization: more efficient codegen
opt-level = "z"           <-- Optimize for small binary output
overflow-checks = true    <-- Arithmetic overflow protection
```

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------

