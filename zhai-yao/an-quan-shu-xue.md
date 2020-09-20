---
description: ink！ 提供在我们的Cargo.toml文件上启用的内置溢出保护。 建议将其保持为安全机制。
---

# 安全数学



```text
[profile.release]
panic = "abort"           <-- Panics shall be treated as aborts: reduces binary size
lto = true                <-- enable link-time-optimization: more efficient codegen
opt-level = "z"           <-- Optimize for small binary output
overflow-checks = true    <-- Arithmetic overflow protection
```

