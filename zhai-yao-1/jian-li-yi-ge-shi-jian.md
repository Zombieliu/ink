---
description: >-
  回想一下，提交交易时，合同调用不能直接将值返回给外界。 但是，我们经常想向外界表明发生了什么事情（例如，发生了交易或已经达到某种状态）。
  我们可以使用事件提醒其他人这已经发生。
---

# 建立一个事件

宣布事件

事件可以传达任意数量的数据，以与struct类似的方式定义。 应该使用＃\[ink（event）\]属性声明事件。

例如,

```text
#[ink(event)]
struct Foo {
    #[ink(topic)]
    from: Option<AccountId>,
    #[ink(topic)]
    to: Option<AccountId>,
    value: Balance,
}
```

此Foo事件将包含三段数据-Balance类型的值和两个Option-wrapped AccountId变量，这些变量指示起始和终止帐户。 为了更快地访问事件数据，它们可以具有索引字段。 我们可以通过在该字段上使用＃\[ink（topic）\]属性标签来做到这一点。

从Option变量检索数据的一种方法是使用.unwrap\_or（）函数。 您可能还记得在该项目和Incrementer项目的my\_value\_or\_zero（）和balance\_of\_or\_zero（）函数中使用此功能。

### 发射事件

既然我们已经定义了事件中将包含哪些数据以及如何声明它们，那么该是实际发出一些事件的时候了。 我们通过调用self.env（）。emit\_event来做到这一点，并将事件作为方法调用的唯一参数。

请记住，由于from和to字段是Option，因此我们不能仅将它们设置为特定值。 假设我们要为初始部署者设置值100。 此值不来自任何其他帐户，因此from值应为None。

```text
self.env()
    .emit_event(
        Transfer {
            from: None,
            to: Some(self.env().caller()),
            value: 100,
        });
```

注意：value不需要Some（），因为value没有存储在Option中。

我们希望在每次传输时都发出一个Foo事件。 在我们一直在使用的ERC-20模板中，这发生在两个地方：第一，在新调用期间，第二，每次调用transfer\_from\_to。

### 到你了！

 每次发生令牌转移时，请按照模板代码中的ACTION发出一个Transfer事件。

请记住要进行 cargo +nightly test 去测试您的工作。

