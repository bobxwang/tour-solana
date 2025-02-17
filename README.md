##### install solana cli

```sh
sh -c "$(curl -sSfL https://release.anza.xyz/stable/install)"
solana --version
agave-install update
```

##### install anchor cli

anchor is a framework for developing Solana programs, and avm is the version manage tool 

```shell
cargo install --git https://github.com/coral-xyz/anchor avm --force
avm --version
avm install latest
avm use latest
anchor --version
```

##### create new project

```sh
anchor init <new-project-name>
```

##### about config

```sh
solana config get
solana config set -um    # For mainnet-beta
solana config set -ud    # For devnet
solana config set -ul    # For localhost
solana config set -ut    # For testnet
```

##### Local Validator

```sh
solana-test-validator
```

##### about build

```sh
cargo build-sbf -- -Znext-lockfile-bump
```

##### about deploy

```sh
solana program deploy ./target/deploy/hello_world.so
# return a program id: 75HWfQ6FD4F5FthBDgXAQ7mDFCgBmCGDVBU6o1rUQUEf
```

##### create wallet

```shell
solana-keygen new --force
solana address
```

##### about solana cmd

```sh
solana airdrop 5
solana balance
# 给某个地址转0.05
solana transfer --allow-unfunded-recipient Dkrreywj4EPwNpQ2jusegyU2j6uwzxB5t9qgv16rrmnV 0.05  
# 查看某个交易信息
solana confirm -v 34K3X41n8ZrcRLbArzJ8PLPgCfkr7qQ7hgSPSVUscUAKkdNAMZrie9YkZXF1z262TpEVk1biLqWmZF1hq7kNgjZL
# 检测状态
solana ping
```

##### 账户

- 程序账户，可执行账户，存储不可变的数据，主要存储程序代码，是无状态的
- 数据账户，不可执行账户，存储可变的数据，主要是存储程序的状态。如果一个程序账户是某个数据账户的所有者那么它可修改数据账户中的状态

##### Transaction && Instruction

交易是一组原子性的操作，代表链状态的一系列更改，包括转账代币，调用程序，更新账户状态等，具有唯一签名并由一个或多个指令组成。而指令是交易中的一条具体指令，包含执行指令所需的具体数据，例如执行指令的程序唯一标识 program_id，账户列表，指令参数，配置信息等。多个指令组成的交易可以实现不同的操作形成一个原子性的事务。

##### 交易费用

执行一个交易需要Compute Unit(cu)，类似EVM中的gas fee。引入CU还可以减少网络垃圾，为网络提供长期的经济稳定性。每笔交易设定了最大的CU限制，以确保单笔交易数据量不会过大，从而避免网络拥堵。
