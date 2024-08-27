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



#### 使用以下 cURL 命令直接从水龙头服务器请求代币：
```
curl --location --request POST 'https://faucet.devnet.sui.io/gas' \
--header 'Content-Type: application/json' \
--data-raw '{
    "FixedAmountRequest": {
        "recipient": "0xd498a1b4a9ffa22a55456f8dac635f7dbed3f5d36c374b1d84f0720a9cb56bbb"
    }
}'
```

------------
## 编写 Dapp

### 创建包
首先，在你计划存储包的位置打开一个终端或控制台。使用 sui move new 命令创建一个名为 my_first_package 的空 Move 包：
`sui move new my_first_package`


### move 语言结构介绍
```
module my_first_package::my_module {

    // Part 1: Imports
    use sui::object::{Self, UID};
    use sui::transfer;
    use sui::tx_context::{Self, TxContext};

    // Part 2: Struct definitions
    struct Sword has key, store {
        id: UID,
        magic: u64,
        strength: u64,
    }

    struct Forge has key, store {
        id: UID,
        swords_created: u64,
    }

    // Part 3: Module initializer to be executed when this module is published
    fun init(ctx: &mut TxContext) {
        let admin = Forge {
            id: object::new(ctx),
            swords_created: 0,
        };
        // Transfer the forge object to the module/package publisher
        transfer::transfer(admin, tx_context::sender(ctx));
    }

    // Part 4: Accessors required to read the struct attributes
    public fun magic(self: &Sword): u64 {
        self.magic
    }

    public fun strength(self: &Sword): u64 {
        self.strength
    }

    public fun swords_created(self: &Forge): u64 {
        self.swords_created
    }

    // Part 5: Public/entry functions (introduced later in the tutorial)

    // Part 6: Private functions (if any)

}
```

第一部分：导入 - 在现代编程中，代码重用是必需的。Move 通过允许模块使用在其他模块中声明的类型和函数的导入来支持这一概念。在这个例子中，模块从 object、transfer 和 tx_content 模块导入。这些模块在包中可用，因为 Move.toml 文件定义了 Sui 依赖项（以及它们被定义的 sui 命名地址）。

第二部分：结构声明 - 结构定义了模块可以创建或销毁的类型。结构定义可以包括使用 has 关键字提供的能力。例如，此示例中的结构具有 key 能力，表示这些结构是可以在地址之间转移的 Sui 对象。结构上的 store 能力提供了在其他结构字段中出现并自由传输的能力。

第三部分：模块初始化器 - 一个特殊的函数，在模块发布时确切地调用一次。

第四部分：访问器函数 - 这些函数允许从其他模块读取模块结构的字段。

