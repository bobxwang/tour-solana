#####  Account

- 程序账户
- 数据账户
- PDA（程序派生地址）
  - 存储程序的状态，即程序执行过程中存储的数据
  - 由程序的`program_id`跟种子`seed`派生而来，没有对应的私钥

```rust
pub struct Account {
    // 账户中的lamports数量
    pub lamports: u64,  
    // 此账户持有的数据
    #[serde(with = "serde_bytes")]
    pub data: Vec<u8>,
    // 拥有此账户的程序。如果可执行，加载此账户的程序。
    pub owner: Pubkey,
    // 此账户的数据包含已加载的程序（现在为只读）
    pub executable: bool,
    // 此账户下一次欠租金的纪元
    pub rent_epoch: Epoch,
}
```

##### 账号签名

- Ed25519，一种不对称加密算法，我们用户理解的Solana账号，其实就是一串Ed25519的私钥，而用户账号的地址则是这私钥对应的公钥，为可读性进行Base58编码，变成`Dkrreywj4EPwNpQ2jusegyU2j6uwzxB5t9qgv16rrmnV`，两个放到一起形成Keypair，或者叫公私钥对

##### 交易

交易就是链外数据和链上数据产生的一次交互。交易是对多个指令的打包

```rust
pub struct Transaction {
    #[wasm_bindgen(skip)]
    #[serde(with = "short_vec")]
    pub signatures: Vec<Signature>,
 
    #[wasm_bindgen(skip)]
    pub message: Message,
}

pub struct Message {
   // 消息头，标识已签名和只读的`account_keys`。
   // 头部值只描述静态的`account_keys`，它们不描述通过地址表查找加载的任何额外账户键。
   pub header: MessageHeader,

   // 此交易加载的账户列表。
   #[serde(with = "short_vec")]
   pub account_keys: Vec<Pubkey>,

   // 最近区块的区块哈希。
   pub recent_blockhash: Hash,

   // 指令调用一个指定程序，按顺序执行，
   // 如果全部成功，则在一个原子交易中提交。
   //
   // # 注意
   //
   // 程序索引必须索引到消息`account_keys`列表中，因为程序id不能从查找表中动态加载。
   //
   // 账户索引必须索引到通过以下三个键列表的连接构造的地址列表中：
   //   1) 消息`account_keys`
   //   2) 从`writable`查找表索引加载的键的有序列表
   //   3) 从`readable`查找表索引加载的键的有序列表
   #[serde(with = "short_vec")]
   pub instructions: Vec<CompiledInstruction>,

   // 用于加载此交易的额外账户的地址表查找列表
   #[serde(with = "short_vec")]
   pub address_table_lookups: Vec<MessageAddressTableLookup>,
}

pub enum VersionedMessage {
   Legacy(LegacyMessage),
   V0(v0::Message),
}

pub struct VersionedTransaction {
   // 签名列表
   #[serde(with = "short_vec")]
   pub signatures: Vec<Signature>,
   // 要签名的消息。
   pub message: VersionedMessage,
}
```

##### 指令

```rust
pub struct CompiledInstruction {
    // 交易键数组中的索引，指示执行此指令的程序账户。
    pub program_id_index: u8,
    // 有序的索引到交易键数组中，指示哪些账户传递给程序。
    #[serde(with = "short_vec")]
    pub accounts: Vec<u8>,
    // 程序输入数据。
    #[serde(with = "short_vec")]
    pub data: Vec<u8>,
}
```

##### 合约

- 普通合约，用户开发并部署
- 系统合约，节点在部署的时候生成，普通用户无法更新
  - System Program，创建账号，转账等作用
  - BPF Loader Program，部署和更新合约
  - Vote Program，创建并管理用户POS代理投票的状态跟奖励

##### 租约

##### 租金率

##### 免租