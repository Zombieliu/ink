---
description: 跟着本教程走，你需要在你的电脑上设置一些东西.
---

# 设置



### **Substrate 必要环境**

刚开始，你需要确保你的电脑已经构建了substrate.

如果你正使用OSX或者linux发行版，你可以这么去运行

```text
curl https://getsubstrate.io -sSf | bash -s -- --fast
```

```text
rustup target add wasm32-unknown-unknown --toolchain stable
rustup component add rust-src --toolchain nightly
rustup toolchain install nightly-2020-06-01
rustup target add wasm32-unknown-unknown --toolchain nightly-2020-06-01
```

如果你正使用其他的操作系统，例如Windows,请访问[installation instructions](https://substrate.dev/docs/en/knowledgebase/getting-started/windows-users) 在Substrate开发者中心.

### **安装一个Substrate 节点**

我们需要去使用Substrate节点把合约模块嵌入进去，通过工作区我们将使用格式化去设计Substrate节点客户端

```text
cargo +nightly-2020-06-01 install node-cli --git https://github.com/paritytech/substrate.git --tag v2.0.0-rc4 --force --locked
```

### **Ink!命令行**

最后我们将安装的是ink工具!，命令行将设置Substarte 智能合约项目使其更加简单

你可以安装这个实用的工具通过Cargo

```text
cargo install cargo-contract --vers 0.6.2 --force
```

然后你可以使用cargo contract --help 去开始探索你可获得的命令.

注意: The ink! 命令行在开发中，它的很多命令尚未实现！

