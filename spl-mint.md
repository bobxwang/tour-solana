在以太坊中，普通代币被一个叫做`ERC20`的提案定了规范，而在Solana的世界里就是SPL代币。SPL Token是`Solana Program Library`中的一个组成部分，叫做`Token Program`，简称SPL Token。

所有的代币都有这个合约来管理，该合约代码地址是 `https://github.com/solana-labs/solana-program-library/tree/master/token`

以太坊中一个代币就是一个合约，而在SPL Token中，一个代币仅仅是一个归Token合约管理的普通Account对象，这个对象里面的二进制数据定义了这个代币的基本属性

```rust
pub struct Mint {
    pub mint_authority: COption<Pubkey>,
    pub supply: u64,   // 供应量
    pub decimal: u8,   // 精度
    pub is_initialized: bool,
    pub freeze_authority: COption<Pubkey>,
}
```

