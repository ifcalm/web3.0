更新rust: `rustup update stable`

sui中文文档: https://docs-zh.sui-book.com/guides/developer/getting-started/sui-install/


### sui cli 基础用法
- 验证您的系统是否已安装 CLI: `sui --version`
- 升级sui: `cargo install --locked --git https://github.com/MystenLabs/sui.git --branch devnet sui`
- 列出当前所有网络别名: `sui client envs`
- 切换网络: `sui client switch --env mainnet`
- 查询当前保存了密钥的地址: `sui client addresses`
- 查询当前启用的地址: `sui client active-address`
- 列出所拥有的 gas objects: `sui client gas`

### Sui CLI 包括以下五种顶级命令，对于尚未记录的命令，请使用help标志。例如，sui validator --help。

- Sui Client CLI: 使用 `sui client` 命令与Sui网络交互。文档: https://docs-zh.sui-book.com/references/cli/client
- Sui Console CLI: 使用 `sui console` 打开一个与当前活动网络交互的交互式控制台。
- Sui Keytool CLI: 使用 `sui keytool` 命令访问密码工具。
- Sui Move CLI: 使用 `sui move` 命令执行与Move编程语言相关的命令。
- Sui Validator CLI: 使用 `sui validator` 命令访问对Sui验证节点有用的工具。

`sui client` 命令所有可用子命令的列表
Sui CLI的console命令提供了命令级别的访问，通过将 Sui Client CLI 命令包装在类似shell的功能中，与Sui网络进行交互。此命令启动一个新进程，并为用户提供一个运行所有可用 Sui Client CLI 命令的环境。此外，它还提供命令历史记录支持。

Sui CLI的keytool命令提供了多个命令级别的访问，用于管理和生成地址，以及处理私钥、签名或zkLogin。例如，用户可以使用 sui keytool import [...] 命令从Sui钱包导出私钥，并将其导入到本地Sui CLI钱包。

在命令中加入 --json 标志可以将输出格式从默认的、更易于人类阅读的 Sui CLI 格式转换为 JSON 格式。这对于处理大型数据集特别有帮助，因为在小屏幕上展示这些大量数据可能会遇到问题。在这种情况下，使用 --json 标志是一个不错的选择。

### 连接到sui网络
Sui 提供 Mainnet、Devnet 和 Testnet 网络。你可以使用其中一个测试网络，Devnet 或 Testnet，来尝试在该网络上运行的 Sui 版本。你还可以启动一个本地 Sui 网络进行本地开发。


更新sui版本: `cargo install --locked --git https://github.com/MystenLabs/sui.git --branch devnet sui`

国内github下载慢问题，可以通过 ping github.com, 然后在 /etc/hosts 配置ip 域名进行加速。

