# 储存一个值

我们要对合约模板进行的第一件事是引入一些存储值。

这是在存储器中存储一些简单值的方式：



```text
#[ink(storage)]
struct MyContract {
    // Store a bool
    my_bool: storage::Value<bool>,
    // Store some number
    my_number: storage::Value<u32>,
}
/* --snip-- */
```

### 支持的类型

合同存储（如storage :: Value ）允许通过奇偶校验编解码器进行编码和解码的类型通用，其中包括最常见的类型，例如bool，u {8,16,32,64,128}，i {8， 16,32,64,128}，字符串，元组和数组。

ink！ 提供智能合约特定于底物的类型，例如AccountId，Balance和Hash，就好像它们是原始类型一样。 ink！ 通过存储模块提供存储类型，以进行更精细的存储交互：

```text
use ink_core::storage::{Value, Vec, HashMap, BTreeMap, Heap, Stash, Bitvec};
```

 这是有关如何存储AccountId和Balance的示例：

```text
// We are importing the default PALETTE types
use ink_lang as ink;

#[ink::contract(version = "0.1.0")]
impl MyContract {

    // Our struct will use those default PALETTE types
    #[ink(storage)]
    struct MyContract {
        // Store some AccountId
        my_account: storage::Value<AccountId>,
        // Store some Balance
        my_balance: storage::Value<Balance>,
    }
    ...
}
```

您可以在core / env / types.rs中找到所有受支持的Substrate类型。

### 初始化存储

重要说明：本节很重要。 阅读两次。 然后喝茶，再读一遍。

在与ink中的任何存储项目进行交互之前！ 合同，您必须确保它们已初始化！ 如果您没有初始化存储项目并尝试访问它，那么您的合同调用将不会成功，并且由事务引起的任何状态更改都将被还原。 （尽管如此，仍将收取燃料费用！）

对于上述存储值，我们可以使用以下方法设置初始值：

```text
self.my_bool.set(false);
self.my_number.set(42);
self.my_account.set(AccountId::from([0x1; 32]));
self.my_balance.set(1337);
```

这可以在我们的合同逻辑中的任何地方完成，但是最常见的是通过使用＃\[ink（constructor）\]属性来完成。

### 合同部署

每种ink！ 智能合约必须具有一个构造器，该构造器在创建合约时运行一次。 ink！ 智能合约可以具有多个不同的构造函数：

```text
use ink_lang as ink;

#[ink::contract(version = "0.1.0")]
impl MyContract {
    #[ink(storage)]
    struct MyContract {
        number: storage::Value<u32>,
    }

    impl MyContract {
        #[ink(constructor)]
        fn new(&mut self, init_value: u32) {
            self.number.set(init_value);
        }

        #[ink(constructor)]
        fn default(&mut self) {
            self.new(0)
        }
    /* --snip-- */
    }
}
```

### 轮到你！

 遵循模板中的操作。

请记住要进行 cargo +nightly test 去测试您的工作。

