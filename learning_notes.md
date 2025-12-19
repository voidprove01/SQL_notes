# Metrics and Rates（指标类 SQL 的安全写法）

很多 SQL 面试题最终并不是要一个 table，
而是要你算一个 metric，比如：

- conversion rate
- retention rate
- click-through rate (CTR)
- engagement rate

⚠️ 虽然最终 output 可能只是一个 scalar，
但 **unit of analysis 并没有消失，只是被折叠进 aggregation 里了**。

---

## 1. Always define the denominator first（先想分母）

在写任何 SQL 之前，先明确一个问题：

> What is **one unit** in the denominator?

换句话说就是：  
👉 **这个 rate 是在“谁的基础上”算的？**

Examples:
- Conversion rate → one row per *visitor*
- Retention rate → one row per *user active in base period*
- CTR → one row per *impression*

📌 很多面试题错，不是 SQL 写错，是 **分母想错了**。

---

## 2. Explicitly construct the denominator population（显式构造人群）

即使最终只输出一个数字，我也倾向于：

- 先构造一个 intermediate table
- 每一行 = 分母中的一个 unit
- 用一个 binary flag 表示是否发生了 numerator event

Conceptually:
