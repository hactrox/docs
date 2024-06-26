# invokefunction 方法

使用给定的操作和参数，通过合约脚本哈希调用智能合约之后返回结果。

> [!Note]
>
> - 此方法用于测试你的虚拟机脚本，调用时只是在 RPC 对应的节点试运行脚本并返回结果，不会对区块链数据产生影响。
> - 此方法由插件提供，需要安装 [RpcServer](https://github.com/neo-project/neo-modules/releases) 插件才可以调用

## 参数说明

- scripthash：智能合约脚本哈希。

- operation：操作名称（字符串）。

- params：传递给智能合约操作的参数，可选。

- signers: 签名账户列表，可选。
  * account: 签名账户
  * scopes: 签名的作用域，定义用户签名的可用范围，防止未经授权的合约随意使用用户签名。允许的值为：
    * None：空，只对交易签名，不允许任何合约使用该签名
    * CalledByEntry：只作用链式调用的入口，比如用户调用 A 合约，那么只有 A 合约可以使用签名，A 合约再调用 B 合约，在 B 合约中是不能使用用户签名的。建议作为钱包的默认签名作用。
    * CustomContracts：自定义合约，在指定的合约中可以使用该签名。可与 CalledByEntry 配合使用。
    * CustomGroups：自定义合约组，在指定的合约组中可以使用该签名。可与 CalledByEntry 配合使用。
    * Global：全局。风险极高，合约有可能会转移地址中的所有资产，仅在极其信任该合约时选择。
  * allowedcontracts: 如果 scopes 是 CustomContracts，该字段是签名生效的合约 Hash 列表
  * allowedgroups: 如果 scopes 是 CustomGroups，该字段是签名生效的公钥列表。
- use diagnostic：是否返回模拟的调用信息和存储区变化信息，默认为 `false`。

> [!Note]
>
> 你需要根据传入地址的数据类型，使用正确的字节序格式。如果数据类型为 Hash160，输入大端序 scripthash；如果数据类型为 ByteArray，则输入小端序 scripthash。

例如：

```json
{
  "type": "String",
  "value": "Hello"
}

{
  "type": "Hash160",
  "value": "0xf621168b1fce3a89c33a5f6bcf7e774b4657031c"
}

{
  "type": "ByteArray",
  "value": "7472616e73666572"
}
```

## 调用示例

请求正文：

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "invokefunction",
  "params": [
    "0xa1a375677dded85db80a852c28c2431cab29e2c4",
    "transfer",
    [
            {
                "type": "Hash160",
                "value": "0xfa03cb7b40072c69ca41f0ad3606a548f1d59966"
            },
            {
                "type": "Hash160",
                "value": "0xebae4ab3f21765e5f604dfdd590fdf142cfb89fa"
            },
            {
                "type": "Integer",
                "value": "10000"
            },
            {
                "type": "String",
                "value": ""
            }
        ],
        [
            {
                "account": "0xfa03cb7b40072c69ca41f0ad3606a548f1d59966",
                "scopes": "CalledByEntry",
                "allowedcontracts": [],
                "allowedgroups": []
            }
        ],
    true
  ]
}

```

响应正文：

```json
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "script": "DAABECcMFPqJ+ywU3w9Z3d8E9uVlF/KzSq7rDBRmmdXxSKUGNq3wQcppLAdAe8sD+hTAHwwIdHJhbnNmZXIMFMTiKascQ8IoLIUKuF3Y3n1ndaOhQWJ9W1I=",
        "state": "HALT",
        "gasconsumed": "1490312",
        "exception": null,
        "notifications": [
            {
                "eventname": "Transfer",
                "contract": "0xa1a375677dded85db80a852c28c2431cab29e2c4",
                "state": {
                    "type": "Array",
                    "value": [
                        {
                            "type": "ByteString",
                            "value": "ZpnV8UilBjat8EHKaSwHQHvLA/o="
                        },
                        {
                            "type": "ByteString",
                            "value": "+on7LBTfD1nd3wT25WUX8rNKrus="
                        },
                        {
                            "type": "Integer",
                            "value": "10000"
                        }
                    ]
                }
            }
        ],
        "diagnostics": {
            "invokedcontracts": {
                "hash": "0x9cac876fcc1646f1f017aa49b1fbcf87bd37b043",
                "call": [
                    {
                        "hash": "0xa1a375677dded85db80a852c28c2431cab29e2c4",
                        "call": [
                            {
                                "hash": "0xa1a375677dded85db80a852c28c2431cab29e2c4"
                            },
                            {
                                "hash": "0xa1a375677dded85db80a852c28c2431cab29e2c4"
                            },
                            {
                                "hash": "0xa1a375677dded85db80a852c28c2431cab29e2c4"
                            },
                            {
                                "hash": "0xa1a375677dded85db80a852c28c2431cab29e2c4"
                            },
                            {
                                "hash": "0xa1a375677dded85db80a852c28c2431cab29e2c4"
                            },
                            {
                                "hash": "0xa1a375677dded85db80a852c28c2431cab29e2c4"
                            },
                            {
                                "hash": "0xa1a375677dded85db80a852c28c2431cab29e2c4",
                                "call": [
                                    {
                                        "hash": "0xa1a375677dded85db80a852c28c2431cab29e2c4"
                                    },
                                    {
                                        "hash": "0xa1a375677dded85db80a852c28c2431cab29e2c4"
                                    }
                                ]
                            },
                            {
                                "hash": "0xa1a375677dded85db80a852c28c2431cab29e2c4",
                                "call": [
                                    {
                                        "hash": "0xa1a375677dded85db80a852c28c2431cab29e2c4"
                                    },
                                    {
                                        "hash": "0xa1a375677dded85db80a852c28c2431cab29e2c4"
                                    }
                                ]
                            },
                            {
                                "hash": "0xfffdc93764dbaddd97c48f252a53ea4643faa3fd"
                            }
                        ]
                    }
                ]
            },
            "storagechanges": [
                {
                    "state": "Changed",
                    "key": "BgAAAAEBZpnV8UilBjat8EHKaSwHQHvLA/o=",
                    "value": "8CTJ5wda"
                },
                {
                    "state": "Added",
                    "key": "BgAAAAEB+on7LBTfD1nd3wT25WUX8rNKrus=",
                    "value": "ECc="
                }
            ]
        },
        "stack": [
            {
                "type": "Boolean",
                "value": true
            }
        ],
        "tx": "AOaXOgSIvRYAAAAAAKzgAQAAAAAAesUGAAFmmdXxSKUGNq3wQcppLAdAe8sD+gEAWQwAARAnDBT6ifssFN8PWd3fBPblZRfys0qu6wwUZpnV8UilBjat8EHKaSwHQHvLA/oUwB8MCHRyYW5zZmVyDBTE4imrHEPCKCyFCrhd2N59Z3WjoUFifVtSAUIMQMTS2HRIO9gDxq/U/lqIB77dLBzVHT4cwKdvqoGOZqm4IoGqHbYzBSYHOPHWGNutWvkjCgIQGQFKK1JGyOR16LwoDCEDrQCtTQQyXXSsHZm3oRiqiAzP00uFPaW9tICYC3D7Bm9BVuezJw=="
    }
}
```

响应说明：

- script：合约的调用脚本，在 Neo 官网上可以解析 https://neo.org/converter

   ```
    PUSHDATA1
    PUSHINT16 10000
    PUSHDATA1 0xebae4ab3f21765e5f604dfdd590fdf142cfb89fa
    PUSHDATA1 0xfa03cb7b40072c69ca41f0ad3606a548f1d59966
    PUSH4
    PACK
    PUSH15
    PUSHDATA1 transfer
    PUSHDATA1 0xa1a375677dded85db80a852c28c2431cab29e2c4
    SYSCALL System.Contract.Call
   ```
   
- state：虚拟机状态， `HALT` 表示虚拟机执行成功，`FAULT` 表示虚拟机执行时遇到异常退出。
- gasconsumed：调用智能合约时消耗的系统手续费。
- exception： 合约执行过程中的异常信息，无异常时为 `null`。
- notifications：合约执行过程中产生的 event 信息，交易上链后可以通过 [getapplicationlog](./getapplicationlog.md) 来查看。
- diagnostics：合约调用的过程信息和存储区改变情况，该请求只是测试调用，不会改变链上真实存储区，这里显示的存储区改变是模拟交易发送上链后的存储区变化情况。
- stack：合约执行结果，其中 value 如果是字符串或 ByteArray，则是 Base64 编码后的结果。
- tx：本次调用合约交易的 Base64 编码，需要打开钱包并且传入正确的签名账户参数，否则返回结果不包含该字段。
- pendingsignature：签名未完成的交易，如果该交易需要多个签名而当前打开的钱包中不包含所有签名账户时，返回结果包含该字段。

## 特别说明

当合约执行结果里包含迭代器 Iterator 类型时，会根据 `RpcServer` 插件的 `config.json` 文件中 `SessionEnabled` 字段的值来决定是否返回 `session`，如果 `SessionEnabled` 为 `true`，返回结果中会包含 `session` 字段，用来在 [traverseiterator](./traverseiterator.md) 方法中进一步获取 Iterator 的详细内容，如果 `SessionEnabled` 为 `false`，则返回结果中不包含 `session`，无法进一步获取 Iterator 的详细内容。

如下示例是 `SessionEnabled` 为 `true` 时的 Iterator 类型返回结果：

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "script": "wh8MBnRva2VucwwU/+Ha24YygjLl9RpzOqiVrDhOCyVBYn1bUg==",
        "state": "HALT",
        "gasconsumed": "247083",
        "exception": null,
        "notifications": [],
        "stack": [
            {
                "type": "InteropInterface",
                "interface": "IIterator",
                "id": "bfc3ccf4-d814-4497-9d68-eb50806c3b7a"
            }
        ],
        "session": "77c20bc6-6c6a-40dc-87b9-0be461c04831"
    }
}
```

获取 Iterator 请参考 [traverseiterator](traverseiterator.md) 方法。
