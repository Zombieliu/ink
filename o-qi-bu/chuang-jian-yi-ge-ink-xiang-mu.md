# 创建一个ink!项目

我们将使用ink！ 命令生成Substrate智能合约项目所需的文件。

确保您在工作目录中，然后运行：

```text
cargo contract new flipper
```

该命令将创建一个名为flipper的新项目文件夹，我们将对其进行探索：

```text
cd flipper/
```

### Ink!合约项目

```text
flipper
|
+-- .cargo
|   |
|   +-- config            <-- Compiler Configuration
+-- .ink
|   |
|   +-- abi_gen 
|       |
|       +-- Cargo.toml   
|       +-- main.rs       <-- ABI Generator
|
+-- lib.rs                <-- Contract Source Code
|
+-- Cargo.toml            <-- Rust Dependencies and ink! Configuration
|
+-- .gitignore
```



### **合约源代码**

Ink 命令自动为“ Flipper”合同生成源代码，这是您可以构建的最简单的“智能”合同。

您可以通过在这里查看源代码来初步了解将要发生的事情

[Flipper Example Source Code](https://github.com/paritytech/ink/blob/master/examples/flipper/lib.rs)

Flipper合约不过是一个布尔对象，它通过flip（）函数把true变为false。 我们不会深入探讨此源代码的细节，因为我们将引导您逐步建立更高级的合约！

### 测试你的合约

您将在源代码底部看到一个简单的测试，它可以验证合约的功能。我们可以使用脱链测试环境快速测试该代码是否按预期运行！ 

在你的项目文件夹中运行:



```text
cargo +nightly test
```

您应该看到一个成功的测试完成:



```text
$ cargo +nightly test
    running 2 tests
    test flipper::tests::it_works ... ok
    test flipper::tests::default_works ... ok

    test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

现在我们对一切都充满信心，那么我们实际上可以将此合约编译为Wasm。

