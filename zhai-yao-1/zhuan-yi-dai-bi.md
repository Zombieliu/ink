---
description: 因此，在这一点上，我们有一个拥有合同所有代币的用户。 但是，除非您可以将其转移给其他人，否则它并不是真正有用的令牌...让我们开始吧！
---

# 转移代币

### 传递函数

转账功能完全可以实现您的期望：允许签订合约的用户将自己拥有的部分资金转给另一位用户。

您会在我们的模板代码中注意到，有一个公共函数传输和一个内部函数transfer\_from\_to。 我们之所以这样做，是因为将来，当我们启用第三方补贴并以代收形式支出时，我们将重用令牌转移的逻辑。



### transfer\_from\_to\(\)

```text
fn transfer_from_to(&mut self, from: AccountId, to: AccountId, value: Balance) -> bool {/* --snip-- */}
```

将在不进行任何授权检查的情况下构建transfer\_from\_to函数。 因为它是一个内部函数，所以我们完全控制何时调用它。 但是，它将对管理帐户之间的余额进行所有逻辑检查。

确实，我们只需要检查一件事：确保发件人帐户中有足够的资金可发送到收件者帐户：

```text
if balance_from < value {
    return false
}
```

请记住，传递函数和其他公共函数返回布尔值以表示成功。 如果发件人帐户中的余额不足以支付转帐，我们将提早退出并返回false，而不会对合约状态进行任何更改。 我们的transfer\_from\_to将简单地将“成功”布尔转发到调用它的函数。

transfer\(\)

```text
#[ink(message)] 
fn transfer(&mut self, to: AccountId, value: Balance) -> bool {/* --snip-- */}
```

最后，传递函数将简单地将from参数自动设置为self.env（）。caller（）调用transfer\_from\_to。 这是我们的“授权检查”，因为合约调用者始终被授权转移自己的资金。

### 转移数学

关于令牌传输中执行的简单数学确实没有什么可说的。

1.首先，我们获得from和to帐户的当前余额，

2.确保使用我们的balance\_of\_or\_zero（）获取器。

3.然后，我们进行上述逻辑检查，以确保余额中有足够的资金来传递价值。

4.最后，我们从from余额中减去该值，然后将其添加到balance中，然后将这些新值重新插入。

### 到你了

遵循模板中的操作。

请记住要进行 cargo +nightly test 去测试您的工作。

