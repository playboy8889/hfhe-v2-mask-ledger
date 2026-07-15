# Octra HFHE Challenge v2 公共审计说明

本仓库是 Octra HFHE Challenge v2 的公开审计/解谜进展仓库，用来同步整理可公开的实验记录、外部线索 triage、负向结论和阶段性总结。

## 当前状态

- 明文：未恢复
- 目标钱包私钥：未恢复
- 目标地址：`octC5eR9pLGKbpzTbDgHowkFt8HW7LZYb2gzehzxHamxuAZ`
- 最新观察到的钱包状态：余额 `500001.000001`，nonce `0`，`has_public_key=false`，交易数 `5`

## 目前核心阻塞点

公开材料只给出了部分可分析数据，尤其是 `pvac.prf.r.1` 域的 LPN side samples。实际解密每层需要：

```text
R = PRF_R1 * PRF_R2 * PRF_R3
```

目前公开材料没有泄漏：

- `PRF_R2`
- `PRF_R3`
- `sk.prf_k`
- Toeplitz secret-derived stream material
- PC commitment openings，即 `value` / `blind`
- 明文
- 目标钱包私钥

因此现阶段的结论是：**公开材料足以做结构审计和排除很多攻击路线，但还不足以恢复明文或私钥。**

## 已完成的主要检查

1. `secret.ct` 格式边界审计：22 个 ciphertext，44 个 base layer，无 trailing bytes，无隐藏附加块。
2. bundle / endian / ASCII pocket 检查：没有发现隐藏明文或大小端解析漏洞。
3. seed / nonce / PC / edge witness 碰撞检查：无重复 `RSeed`、无 PC 复用、无零承诺、无 duplicate edge `s`。
4. `pk.bin` 公钥矩阵审计：`H` 无零列/重复列，GF(2) 行满秩，`ubk.perm` 与 `ubk.inv` 一致。
5. PRF domain 源码审计：`PRF_R1`、`PRF_R2`、`PRF_R3` 是不同 domain，公开样本只覆盖 `pvac.prf.r.1`。
6. PC commitment 表面审计：公开 `PC` 是 32-byte Ristretto commitment point，不包含 opening 的 `value` / `blind`。
7. 外部线索检查：Kubo/smoke-ui 的 Day 6 algebraic-track 是独立负向结果，不是解密结果。

## 后续优先级

优先关注以下类型的新线索：

- 泄漏或可推导 `PRF_R2` / `PRF_R3`；
- 泄漏 `sk.prf_k` 或 Toeplitz stream；
- 泄漏 PC opening；
- 可本地验证的钱包私钥/seed/plaintext；
- 官方仓库新 ref、release、artifact、issue/PR/comment 中出现可验证材料；
- 链上目标地址状态变化。

低优先级：

- 单纯声称 “solved” 但没有 artifact；
- 只重复 LPN-only solver 的路线；
- 已经被 smoke-ui 或本仓库复现为负向的代数/invariant route。

## 发布原则

本仓库只发布公开审计材料，不提交本地 token、`github.txt`、无关钱包文件或未验证的私钥/助记词。
