# BNAQ — Agent ID 专属打新（BSC）

BNAQ（BN + 安全拼音 AQ），简介：代表币安安全。社区 Meme 项目，与币安官方无关。

公开打新页：https://bnaq-agent.github.io/bnaq/

## 打新规则

- 链：BNB Smart Chain（chainId 56）
- 单价：1.2 USDT / 份
- 每份可得：10,000 BNAQ
- 总量：10,000 份
- 每地址限购：10 份
- 门槛：买家须持有 SPACE ID Agent ID（注册表 balanceOf > 0），否则 buy 会被拒绝
- 付款即时发币（USDT transferFrom 后同笔交易转入 BNAQ）

## 合约地址（BSC 主网）

- 代币合约 BNAQ：`0x1aD6B1D49BF83D2660A528B124Bb61634592dd7c`
  - BscScan：https://bscscan.com/address/0x1aD6B1D49BF83D2660A528B124Bb61634592dd7c
- 打新合约 BNAQSale：`0xaecdf0a06eccc7997c4f8114abaf7142f6c30f0c`
  - BscScan：https://bscscan.com/address/0xaecdf0a06eccc7997c4f8114abaf7142f6c30f0c
- USDT（BEP-20）：`0x55d398326f99059fF775485246999027B3197955`
- SPACE ID Agent ID 注册表：`0x60B9262456dE011F7d15a54CdD8c48bAaa8B022F`

两份合约源码均已在 Sourcify exact-match 验证；代币经币安官方 token audit 检测为 LOW 风险（等级 2），买卖税 0。


## 给 AI Agent 的调用说明

1. 读进度：对打新合约调用 `soldShares()`（selector `0x67153ab4`）得已售份数；`TOTAL_SHARES()` 得总量 10000；`saleOpen()` 得销售状态。
2. 查门槛：对注册表调用 `balanceOf(买家地址)`，须 > 0。
3. 打新：先对 USDT 调用 `approve(打新合约, 份数 × 1.2 USDT)`（精确金额，勿无限授权），再对打新合约调用 `buy(uint256 份数)`（selector `0xd96a094a`），份数 1–10 且该地址累计不超过 10 份。
4. 机器可读配置：见同目录 `bnaq.json`，网页版见 `index.html`（含 JSON 配置块）。

打新合约 owner 可随时提取合约内 USDT（withdrawUSDT 为公开标准函数），提款记录链上可查。链上交易不可撤销，参与前请自行核对合约地址与风险。
