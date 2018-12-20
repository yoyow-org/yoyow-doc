# 从0开始创建YOYOW平台账户

本文将以测试网环境为例，介绍创建平台所需要的操作，比如在网页钱包中注册账号，在cli中通过命令行创建平台。
所需资源：   
**正式网：**  
网页钱包地址： [https://wallet.yoyow.org](https://wallet.yoyow.org).  
正式网CLI下载： [https://github.com/yoyow-org/yoyow-core/releases](https://github.com/yoyow-org/yoyow-core/releases)

**测试网：**   
网页钱包测试地址： [http://demo.yoyow.org:8000](http://demo.yoyow.org:8000).  
测试网CLI下载： [https://github.com/yoyow-org/yoyow-core-testnet/releases/](https://github.com/yoyow-org/yoyow-core-testnet/releases/).


## 1. 创建账号，获取密钥
以测试网为例，访问[测试网网页钱包地址](http://demo.yoyow.org:8000 "yoyow钱包测试网") 注册或登录账号：

![创建测试网账号](/images/sdk/step1.png)

平台所有者的各权限私钥获取方式 登录钱包 --> 左侧菜单设置 --> 账号 --> 查看权限 --> 在对应权限密钥的右侧点击显示私钥 --> 输入密码显示私钥 --> 将看到的私钥拷贝进配置中.
    
![获取对应私钥](/images/sdk/step3.png)

## 2. 创建平台

    创建平台商需要最少 11000 YOYO，其中10000 为最低抵押押金，1000为创建平台手续费（测试网络注册赠送12000 测试币）

### 2.1 启动cli钱包
以测试网为例，从[测试网CLI下载](https://github.com/yoyow-org/yoyow-core-testnet/releases/)下载相应环境下的yoyow-client。
#### 2.1.1 带参数启动

以Ubuntu为例，连接测试网的node节点（ws://47.52.155.181:10011）

    ./yoyow_client -s ws://47.52.155.181:10011 

如若提示权限不足 

    sudo chmod a+x * 

  

#### 2.1.2 以配置文件启动

cli 钱包 同路径下创建wallet.json 文件

写入

    {
      "chain_id": "3505e367fe6cde243f2a1c39bd8e58557e23271dd6cbf4b29a8dc8c44c9af8fe",
      "pending_account_registrations": [],
      "pending_witness_registrations": [],
      "labeled_keys": [],
      "blind_receipts": [],
      "ws_server": "ws://47.52.155.181:10011",
      "ws_user": "",
      "ws_password": ""
    }

然后可以直接运行

    ./yoyow_client

### 2.2 设置钱包密码

    连接成功出现

    Please use the set_password method to initialize a new wallet before continuing

    new >>>

    执行

    new >>> set_password 你的密码

    返回

    set_password 你的密码
    null
    locked >>> 

    执行

    locked >>> unlock 你的密码

    返回

    unlock 123
    null
    unlocked >>>

    表示解锁成功

### 2.3 导入资金私钥

    unlocked >>> import_key yoyow账号uid 资金密钥

    例:

    unlocked >>> import_key 120252179 5JwREzpwb62iEcD6J6WXs2fbn1aSKWQWvGLNCqAEYwS31EHD7i4

    返回

    1937037ms th_a       wallet.cpp:820                save_wallet_file     ] saving wallet to file wallet.json
    true

    如果没返回true，请检查你的uid和私钥是否正确

### 2.4 创建平台

    unlocked >>> create_platform yoyow账号uid "平台名称" 抵押金额 货币符号 "平台url地址" "平台拓展信息json字符串" true

    例:

    unlocked >>> create_platform 235145448 "myPlatform" 10000 YOYO "www.example.com" "{}" true

    返回

    {
      "ref_block_num": 33094,
      "ref_block_prefix": 2124691028,
      "expiration": "2018-02-07T08:40:30",
      "operations": [[
          20,{
            "fee": {
              "total": {
                "amount": 1029296,
                "asset_id": 0
              },
              "options": {
                "from_csaf": {
                  "amount": 1029296,
                  "asset_id": 0
                }
              }
            },
            "account": 235145448,
            "pledge": {
              "amount": 1000000000,
              "asset_id": 0
            },
            "name": "myPlatform",
            "url": "www.example.com",
            "extra_data": "{}"
          }
        ]
      ],
      "signatures": [
        "1f08b704dd5ccf7e05e5dec45b06ad41e6382f5dd528e3f644d52ff4fb29c2040507544d5e94b84d77d70edcd68bb35b0cded0db87816ae64979ba98eeb641d5d7"
      ]
    }

### 2.5 更新平台

    unlocked >>> update_platform yoyow账号uid "平台名称" 抵押金额 货币符号 "平台url地址" "平台拓展信息json字符串" true

    例:

    unlocked >>> update_platform 235145448 "newplatformname" 10000 YOYO null null true

    返回与创建平台一样

    平台名称、平台url地址和平台拓展信息如没有变动则填入null，如示例操作，不会改变平台url地址和拓展信息

### 2.6 平台拓展信息协议

    平台属性中 extra_data 的拓展信息为选填内容，YOYOW登录的相关协议中规定拓展信息为JSON对象格式字符串格式，并遵循如下格式 

    {

    "login":"http://example/login" 平台扫码登录请求接口

    "description":"平台说明"  平台描述

    "image":"http://example.image.jpg" 平台头像，yoyow app 1.1 中，显示的平台头像

    "h5url":"http://exampleH5.com" 平台h5地址，用于在无app可跳转的情况下，跳转到h5页面

    "packagename":"com.example.app" 平台android 跳转

    "urlscheme":"example://"  平台ios跳转

    }
