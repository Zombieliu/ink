---
description: 我们的Incrementer合约的最后一步是允许每个用户更新自己的价值。
---

# 增加我的值

### 修改HashMap

更改HashMap的值与获取值一样敏感。 如果您尝试在初始化之前修改某些值，则您的合同会惊慌！ （您是否一直在数我们这样说的次数？）

但是不用担心，我们可以继续使用创建的my\_number\_or\_zero函数来保护我们免受这些情况的侵扰！

```text
impl MyContract {

    /* --snip-- */

    // Set the value for the calling AccountId
    #[ink(message)]
    fn set_my_number(&mut self, value: u32) {
        let caller = self.env().caller();
        self.my_number_map.insert(caller, value);
    }

    // Add a value to the existing value for the calling AccountId
    #[ink(message)]
    fn add_my_number(&mut self, value: u32) {
        let caller = self.env().caller();
        let my_number = self.my_number_or_zero(&caller);
        self.my_number_map.insert(caller, my_number + value);
    }

    /// Returns the number for an AccountId or 0 if it is not set.
    fn my_number_or_zero(&self, of: &AccountId) -> u32 {
        let value = self.my_number_map.get(of).unwrap_or(&0);
        *value
    }
}
```

在这里，我们编写了两种修改HashMap的函数。 一个简单地将值直接插入存储中，而无需先读取该值，另一个简单地修改现有值。 请注意，由于在存储中初始化了该值，因此我们始终可以毫无忧虑地插入该值，但是在获取或修改任何内容之前，我们需要调用my\_number\_or\_zero以确保我们使用的是真实值。

### 感觉疼痛（可选）

我们的合同存储空间将不会总是具有现有价值。 我们可以利用Rust Option 类型来帮助完成此任务。 如果合同存储中没有价值，我们将插入一个新值； 相反，如果存在现有值，我们将仅对其进行更新。

墨水！ HashMaps getter返回一个Option ，我们可以使用它来标识存储中是否存在现有值。

```text
let caller = self.env().caller();
match self.my_number_map.get(&caller) {
    Some(_) => {
        self.my_number_map.mutate_with(&caller, |value| *value += by);
    }
    None => {
        self.my_number_map.insert(caller, by);
    }
};
```

### 到你了

遵循模板中的操作。

请记住要进行 cargo +nightly test 去测试您的工作。

