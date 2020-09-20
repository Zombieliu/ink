---
description: 让我们从较高的角度看一下使用ink开发智能合约时可以使用到的内容。
---

# 合约模板

### ink!

ink！ eDSL是一种用Rust编程语言编写基于WebAssembly的智能合约的eDSL。

ink！ 只是标准的Rust，具有明确定义的“合同格式”，带有专门的\[\[ink（...）\]\]属性宏。 这些属性宏告诉ink！ Rust智能合约的不同部分代表什么，并最终允许ink使用！ 完成创建与Substrate兼容的Wasm字节码所需的所有魔术！

### 轮到你了！

我们将为将在本章中构建的Incrementer合同启动一个新项目。

因此，进入您的工作目录并运行：

```text
cargo contract new incrementer
```

像以前一样，这将创建一个名为增量器的新项目文件夹，在本章的其余部分中将使用该文件夹。

```text
cd incrementer/
```

在lib.rs文件中，将“ Flipper”合同源代码替换为此处提供的模板代码。

快速检查它是否可以编译并通过以下简单测试：

```text
cargo +nightly test
```

还要检查是否可以通过运行以下命令来构建Wasm文件：

```text
cargo +nightly contract build
```

如果一切看起来不错，那么我们就可以开始编程了！

