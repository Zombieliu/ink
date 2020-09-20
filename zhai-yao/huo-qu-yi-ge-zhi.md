---
description: 现在我们已经创建并初始化了一个存储值，我们将开始与它交互！
---

# 获取一个值

### 合约功能

正如您在合约模板中所看到的，所有合约功能都是合约模块的一部分。

```text
impl MyContract {
    // Public and Private functions can go here
}
```

### 公共和私有功能

在Rust中，您可以根据需要进行多种实现。 作为一种样式选择，我们建议您为私有和公共功能分解实现定义：

```text
impl MyContract {
    /// Public function
    #[ink(message)]
    fn my_public_function(&self) {
        /* --snip-- */
    }

    /// Private function
    fn my_private_function(&self) {
        /* --snip-- */
    }

    /* --snip-- */
}
```

您也可以选择拆分内容，但是最适合您的项目。

请注意，所有公共功能都必须使用＃\[ink（message）\]属性。

### 存值API 

无需过多讨论，存储值就是基础知识的一部分！ 核心层。 在后台，他们使用更原始的单元格类型，其中包含Option 。 当我们尝试从存储中获取值时，我们会解析该值，这就是为什么如果未初始化它就会panics！

```text
impl<T> Value<T>
where
    T: scale::Codec,
{
    /// Returns an immutable reference to the wrapped value.
    pub fn get(&self) -> &T {
        self.cell.get().unwrap()
    }

    /// Returns a mutable reference to the wrapped value.
    pub fn get_mut(&mut self) -> &mut T {
        self.cell.get_mut().unwrap()
    }

    /// Sets the wrapped value to the given value.
    pub fn set(&mut self, val: T) {
        self.cell.set(val);
    }
}
```

在同一文件中，您可以找到存储值公开的其他API，但是这三个是最常用的。

### 获取一个值

我们已经向您展示了初始化存储值时如何使用set。 获取值非常简单：

```text
impl MyContract {
    #[ink(message)]
    fn my_getter(&self) -> u32 {
        let number = *self.my_number.get();
        number
    }
}
```

您应该注意，get API返回对该值的引用，因此，要实际获取该值，您需要用星号（\*）对其进行取消引用。 在Rust中，如果函数中的最后一个表达式没有分号，则它将作为返回值。

您还可以删除.get（）以隐式获取值：

### 到你了

遵循模板中的操作。

请记住要进行 cargo +nightly test 去测试您的工作。

