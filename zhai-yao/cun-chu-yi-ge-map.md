---
description: 现在，让我们将Incrementer扩展为不仅可以管理一个号码，而且可以为每个用户管理一个号码！
---

# 存储一个Map

### 储存HashMap

除了存储:::值，ink！ 还支持storage :: HashMap，它允许您将项目存储在键值映射中。

这是从用户到数字的映射的示例：

```text
#[ink(storage)]
struct MyContract {
    // Store a mapping from AccountIds to a u32
    my_number_map: storage::HashMap<AccountId, u32>,
}
```

这意味着对于给定的键，您可以存储值类型的唯一实例。 在这种情况下，每个“用户”都有自己的号码，我们可以构建逻辑，以便只有他们才能修改自己的号码。

### 存储HashMap API

您可以在ink！的[core/src/storage2/collections/hashmap](https://github.com/paritytech/ink/blob/master/core/src/storage2/collections/hashmap/impls.rs)部分中找到完整的HashMap API。

以下是您可能会使用的一些最常见的功能：

```text
    /// Inserts a key-value pair into the map.
    ///
    /// If the map did not have this key present, `None` is returned.
    ///
    /// If the map did have this key present, the value is updated,
    /// and the old value is returned.
    pub fn insert(&mut self, key: K, val: V) -> Option<V> {/* --snip-- */}

    /// Removes a key from the map, returning the value at the key if the key
    /// was previously in the map.
    pub fn remove<Q>(&mut self, key: &Q) -> Option<V> {/* --snip-- */}

    /// Returns an immutable reference to the value corresponding to the key.
    pub fn get<Q>(&self, key: &Q) -> Option<&V> {/* --snip-- */}

    /// Returns a mutable reference to the value corresponding to the key.
    pub fn get_mut<Q>(&mut self, key: &Q) -> Option<&mut V> {/* --snip-- */}

    /// Mutates the value associated with the key if any.
    /// Returns a reference to the mutated element
    pub fn mutate_with<Q, F>(&mut self, key: &Q, f: F) -> Option<&V> {/* --snip-- */}
```

### 初始化HashMap

如本教程中多次提到的，在使用存储之前不进行初始化存储是一个常见的错误，可能会破坏智能合约。 对于存储值中的每个键，需要先设置该值，然后才能使用它。 为此，我们将创建一个私有函数来处理何时设置值以及何时未设置值，并确保我们永远不会使用未初始化的存储。

因此，给定my\_number\_map，想象一下我们希望将任何给定键的默认值设为0。我们可以构建如下函数：

```text

#![cfg_attr(not(feature = "std"), no_std)]

use ink_lang as ink;

#[ink::contract(version = "0.1.0")]
mod mycontract {
    use ink_core::storage;

    #[ink(storage)]
    struct MyContract {
        // Store a mapping from AccountIds to a u32
        my_number_map: storage::HashMap<AccountId, u32>,
    }

    impl MyContract {
        /// Private function.
        /// Returns the number for an AccountId or 0 if it is not set.
        fn my_number_or_zero(&self, of: &AccountId) -> u32 {
            let balance = self.my_number_map.get(of).unwrap_or(&0);
            *balance
        }
    }
}
```

在这里，我们看到从my\_number\_map获得值之后，我们调用unwrap\_or，它将解开存储在存储中的值，或者如果没有值，则返回一些已知值。 然后，在构建与此HashMap交互的函数时，您需要始终记住要调用此函数，而不是直接从存储中获取值。

这是一个例子：

```text

#![cfg_attr(not(feature = "std"), no_std)]

use ink_lang as ink;

#[ink::contract(version = "0.1.0")]
mod mycontract {
    use ink_core::storage;


    #[ink(storage)]
    struct MyContract {
        // Store a mapping from AccountIds to a u32
        my_number_map: storage::HashMap<AccountId, u32>,
    }

    impl MyContract {
        // Get the value for a given AccountId
        #[ink(message)]
        fn get(&self, of: AccountId) -> u32 {
            let value = self.my_number_or_zero(&of);
            value
        }

        // Get the value for the calling AccountId
        #[ink(message)]
        fn get_my_number(&self) -> u32 {
            let caller = self.env().caller();
            let value = self.my_number_or_zero(&caller);
            value
        }

        // Returns the number for an AccountId or 0 if it is not set.
        fn my_number_or_zero(&self, of: &AccountId) -> u32 {
            let value = self.my_number_map.get(of).unwrap_or(&0);
            *value
        }
    }
}
```

### 合约召集人

如您在上面的示例中可能已经注意到的，我们使用了一个称为self.env（）。caller（）的特殊函数。 此功能在整个合同逻辑中都可用，并且始终会向您返回合同调用者。

注意：合同调用方与原始调用方不同。 如果用户触发合同然后调用后续合同，则第二个合同中的self.env（）。caller（）将是第一个合同的地址，而不是原始用户。

self.env（）。caller（）可以通过多种不同方式使用。 在上面的示例中，我们基本上在创建一个“访问控制”层，该层允许用户修改自己的值，但不能修改其他值。 您还可以执行诸如在合同部署期间定义合同所有者的操作：

```text

#![cfg_attr(not(feature = "std"), no_std)]

use ink_lang as ink;

#[ink::contract(version = "0.1.0")]
mod mycontract {
    use ink_core::storage;


    #[ink(storage)]
    struct MyContract {
        // Store a contract owner
        owner: storage::Value<AccountId>,
    }

    impl MyContract {
        #[ink(constructor)]
        fn new(&mut self, init_value: i32) {
            self.owner.set(self.env().caller());
        }
        /* --snip-- */
    }
}
```

然后，您可以编写允许使用的函数，以检查当前调用者是否为合约的所有者。

### 到你了

遵循模板中的操作。

请记住要进行 cargo +nightly test 去测试您的工作。

