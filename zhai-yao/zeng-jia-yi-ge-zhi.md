---
description: 是时候让我们的用户修改存储了！
---

# 增加一个值

### 可变和不变的功能

您可能已经注意到，功能模板包括self作为合约功能的第一个参数。 通过自我，您可以访问所有合约功能和存储项目。

如果您只是从合约存储中读取内容，则只需要通过＆self。 但是，如果要修改存储项目，则需要将其显式标记为可变的＆mut self。

```text
impl MyContract {
    #[ink(message)]
    fn my_getter(&self) -> u32 {
        *self.my_number
    } 

    #[ink(message)]
    fn my_setter(&mut self, new_value: u32) {
        self.my_number.set(new_value);
    }
}
```

### 修改存储值

您始终可以通过再次调用set来更新存储项目的值。

但是，如果您知道该值已设置，则可以以更符合人体工程学的方式修改该值：

```text
impl MyContract {
    #[ink(message)]
    fn my_setter(&mut self, new_value: u32) {
        self.my_number.set(new_value);
    }

    #[ink(message)]
    fn my_adder(&mut self, add_value: u32) {
        self.my_number += add_value;
    }
}
```

但是，如果未初始化该值，则您的合约可以正常编译，但是在合约执行期间会出现恐慌！ 我们真的不能低估以这种方式犯错的容易程度。

### 到你了

遵循模板中的操作。

请记住要进行 cargo +nightly test 去测试您的工作。

