# HFHE v2 Mask Ledger 笔记

这个仓库换成了独立名字和独立说明，不再按旧仓库同名镜像发布。它的定位是“mask 账本”：只记录哪些公开信息可能影响 `R`、`PC opening`、或者目标钱包密钥。

## 当前一句话结论

还没解出。现有公开信息能排除很多路线，但还不能把每层的隐藏 mask 补齐。

## 目标

```text
octC5eR9pLGKbpzTbDgHowkFt8HW7LZYb2gzehzxHamxuAZ
```

最新观察：余额 `500001.000001`，nonce `0`，`has_public_key=false`，交易数 `5`。

## 为什么卡住

每个公开 numerator 只是被隐藏的 `R` 乘过。wrapped text 一块需要两个 base layer 的 mask 才能还原。源码里实际 `R` 又是三段相乘：

```text
R = PRF_R1 * PRF_R2 * PRF_R3
```

公开 LPN side samples 只覆盖 `pvac.prf.r.1`。缺 `r.2`、`r.3`，也缺 `sk.prf_k`，所以不能直接算出完整 `R`。

## 已经排除的方向

- `secret.ct` 后面没有明显追加隐藏块。
- 没发现 ASCII 明文口袋。
- base layer seed/nonce 没复用。
- PC 点没复用，也不是 opening。
- `pk.bin` 的 H 矩阵没有明显零列/重复列/秩坍塌。
- Kubo/smoke-ui 的 Day 6 结果是负向代数结论，不是解密结果。

## 后续只重点看什么

1. `PRF_R2` / `PRF_R3` 是否泄漏；
2. `sk.prf_k` 或 Toeplitz stream 是否泄漏；
3. PC 的 `value` / `blind` opening 是否出现；
4. 是否有人给出能本地验证的钱包私钥或明文；
5. 官方 repo 是否出现新 artifact / ref / release / comment；
6. 目标地址链上状态是否变化。

其它只有“我解了”但没 artifact 的推文，暂时只做低优先级记录。
