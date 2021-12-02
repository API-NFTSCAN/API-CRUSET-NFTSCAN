# API-CRUSET-NFTSCAN
与crust合作存储NFT元数据
# NFTSCAN API

##接口描述 

NFTSCAN对外开放NFT资产信息查询说明: 

接口可查询NFTSCAN当前收集的所有的项目合约，再通过合约地址查询某个项目下的所有NFT资产详细信息，主要数据是tokenuri，第一版只保存NFT元数据。

Crust查询NFT资产信息进行保存以后，返回对应的IPFS存储链接。由于NFT数据不规范性，部分项目的metadata未能查询成功，需要持续补充，

nft_metadata_get字段为正常的项目可以批量查询进行保存，异常的需要等nftscan补充metadata。

http://api.nftscan.com/nftmsg/ 示例

## 接口返回
````json
{
    "msg": "请求成功",
    "code": 200,
    "data": {
        "total": 4949,
        "nft_message_List": [
            {
                "nft_contract": "0xcdb7c1a6fe7e112210ca548c214f656763e13533",
                "nft_asset_id": "0x0000000000000000000000000000000000000000000000000000000000001387",
                "nft_content_uri": "https://asset.maonft.com/rpc/4999.png",
                "nft_metadata": "{\"image\":\"https://asset.maonft.com/rpc/4999.png\",\"external_url\":\"https://maonft.com/rpc/4999\",\"name\":\"Ready Player Cat #4999\",\"description\":\"Ready Player Cat (RPC) is the mascot of the MAO DAO gaming metaverse. They are only born one at a time from loot boxes, and each celebrate distinctive qualities and visual characteristics. RPC Genesis is a curated collection of 5,000 unique RPC NFTs on the Ethereum blockchain that also represent MAO DAO membership.\",\"attributes\":[{\"value\":4999,\"trait_type\":\"ID\"},{\"value\":\"Red\",\"trait_type\":\"Background\"},{\"value\":\"Olive\",\"trait_type\":\"Cloth\"},{\"value\":\"Ray Ban\",\"trait_type\":\"Eye\"},{\"value\":\"Shock Hair\",\"trait_type\":\"Hair\"},{\"value\":\"Small Hat\",\"trait_type\":\"Hat\"}]}",
                "nft_data_type": "image/png"
            },
            {
                "nft_contract": "0xcdb7c1a6fe7e112210ca548c214f656763e13533",
                "nft_asset_id": "0x0000000000000000000000000000000000000000000000000000000000001380",
                "nft_content_uri": "https://asset.maonft.com/rpc/4992.png",
                "nft_metadata": "{\"image\":\"https://asset.maonft.com/rpc/4992.png\",\"external_url\":\"https://maonft.com/rpc/4992\",\"name\":\"Ready Player Cat #4992\",\"description\":\"Ready Player Cat (RPC) is the mascot of the MAO DAO gaming metaverse. They are only born one at a time from loot boxes, and each celebrate distinctive qualities and visual characteristics. RPC Genesis is a curated collection of 5,000 unique RPC NFTs on the Ethereum blockchain that also represent MAO DAO membership.\",\"attributes\":[{\"value\":4992,\"trait_type\":\"ID\"},{\"value\":\"Grey\",\"trait_type\":\"Background\"},{\"value\":\"Styles\",\"trait_type\":\"Cloth\"},{\"value\":\"Party Time\",\"trait_type\":\"Eye\"},{\"value\":\"Crew Cut\",\"trait_type\":\"Hair\"}]}",
                "nft_data_type": "image/png"
            }
        ]
    }
}
````

## 查询NFTSCAN已收录的NFT项目

请求链接 

getNftProject

| 参数名称 | 类型 | 描述 |
| :-----:| :----: | :----: |
| chain | int | 0/ETH 1/BSC 前期只支持ETH，可不传 |
| page_size | String | 每一页返回数据条数 |
| page_index | String | 页数 |

返回参数


| 参数名称param | 类型 | 描述 |
| :-----:| :----: | :----: |
| code | int | 状态码 |
| msg | String | 消息 |
| nft_platform_num | int | NFT项目总数 |
| nft_platform_list | List | NFT项目列表 |


### nft_platform_list

| 参数名称param | 类型type | 描述 |
| :-----:| :----: | :----: |
| nft_platform_name | String | NFT项目名称 |
| nft_platform_type | String | NFT项目类型 （1155/721）|
| nft_platform_contract | String | NFT项目合约地址 |
| nft_platform_describe | String | NFT项目简介 |
| nft_metadata_status | int | 项目元数据获取状态(1正常/0异常)，三方查询数据保存时，保存状态为正常的数据|
| nft_crust_save_status | int | 元数据在Crust保存状态（0未保存/1已保存） |
| nft_crust_url_status | int | NTFSCAN是否从crust更新保存后的新连接（0未更新/1已更新）|


数据示例

```json
{
    "msg": "请求成功",
    "code": 200,
    "data": {
        "nft_platform_num": 2021,
        "nft_platform_list": [
            {
                "nft_platform_name": "UNI",
                "nft_platform_type": "1155/721",
                "nft_platform_contract": "0xc83e009c7794e8f6d1954dc13c23a35fc4d039f6",
                "nft_platform_describe": "UNI is .....",
                "nft_metadata_status": 1,
                "nft_crust_save_status": 1,
                "nft_crust_url_status": 1
            }
        ]
    }
}
```


## 根据合约地址查询NFT项目下所有资产信息 

getNftList

请求参数 

| 参数名称 | 类型 | 描述 |
| :-----:| :----: | :----: |
| nft_address | String | NFT合约地址 |
| page_size | String | 每一页返回数据条数  |
| page_index | String | 页数|



返回参数 

| 参数名称 | 类型 | 描述 |
| :-----:| :----: | :----: |
| code | int | 状态码  |
| msg | String | 消息  |
| total | int | NFT总数 |
| nft_message_List | Object | NFT详细信息列表 |

nft_message_List

| 参数名称 | 类型 | 描述 |
| :-----:| :----: | :----: |
| nft_contract| String | NFT合约地址 |
| nft_asset_id | String | NFT资产ID |
| nft_content_uri | String | NFT TokenUri |
| nft_metadata| String | NFT元数据  |
| nft_data_type | String | 数据类型（jpg,png,gif,mp4...,base64） |

数据示例 

```json
{
    "msg": "请求成功",
    "code": 200,
    "data": {
        "total": 4949,
        "nft_message_List": [
            {
                "nft_contract": "0xcdb7c1a6fe7e112210ca548c214f656763e13533",
                "nft_asset_id": "0x0000000000000000000000000000000000000000000000000000000000001387",
                "nft_content_uri": "https://asset.maonft.com/rpc/4999.png",
                "nft_metadata": "{\"image\":\"https://asset.maonft.com/rpc/4999.png\",\"external_url\":\"https://maonft.com/rpc/4999\",\"name\":\"Ready Player Cat #4999\",\"description\":\"Ready Player Cat (RPC) is the mascot of the MAO DAO gaming metaverse. They are only born one at a time from loot boxes, and each celebrate distinctive qualities and visual characteristics. RPC Genesis is a curated collection of 5,000 unique RPC NFTs on the Ethereum blockchain that also represent MAO DAO membership.\",\"attributes\":[{\"value\":4999,\"trait_type\":\"ID\"},{\"value\":\"Red\",\"trait_type\":\"Background\"},{\"value\":\"Olive\",\"trait_type\":\"Cloth\"},{\"value\":\"Ray Ban\",\"trait_type\":\"Eye\"},{\"value\":\"Shock Hair\",\"trait_type\":\"Hair\"},{\"value\":\"Small Hat\",\"trait_type\":\"Hat\"}]}",
                "nft_data_type": "image/png"
            },
            {
                "nft_contract": "0xcdb7c1a6fe7e112210ca548c214f656763e13533",
                "nft_asset_id": "0x0000000000000000000000000000000000000000000000000000000000001380",
                "nft_content_uri": "https://asset.maonft.com/rpc/4992.png",
                "nft_metadata": "{\"image\":\"https://asset.maonft.com/rpc/4992.png\",\"external_url\":\"https://maonft.com/rpc/4992\",\"name\":\"Ready Player Cat #4992\",\"description\":\"Ready Player Cat (RPC) is the mascot of the MAO DAO gaming metaverse. They are only born one at a time from loot boxes, and each celebrate distinctive qualities and visual characteristics. RPC Genesis is a curated collection of 5,000 unique RPC NFTs on the Ethereum blockchain that also represent MAO DAO membership.\",\"attributes\":[{\"value\":4992,\"trait_type\":\"ID\"},{\"value\":\"Grey\",\"trait_type\":\"Background\"},{\"value\":\"Styles\",\"trait_type\":\"Cloth\"},{\"value\":\"Party Time\",\"trait_type\":\"Eye\"},{\"value\":\"Crew Cut\",\"trait_type\":\"Hair\"}]}",
                "nft_data_type": "image/png"
            }
        ]
    }
}
```



## 更新IPFS存储链接

updateTokenUri

当Crust存储完项目的NFT资产以后，更新tokenUri

请求参数

| 参数名称 | 类型 | 描述 |
| :-----:| :----: | :----: |
| nft_address | String | NFT合约地址 |
| nft_token_id | String | NFT资产ID， |
| nft_tokenuri | String | NFT在IPFS存储后链接 |
| nft_token_order_id | String | NFT在crust的订单id |


返回参数

| 参数名称 | 类型 | 描述 |
| :-----:| :----: | :----: |
| code | int | 状态码 |
| msg | String | 消息  |


数据示例 

```json
{
    "nft_address":"0xc83e009c7794e8f6d1954dc13c23a35fc4d039f6",
    "nft_token_id":"0x3a35fc4",
    "nft_tokenuri":"QmXXX"
}
```
```json
{
    "msg": "更新成功",
    "code": 200
}
```


## 更新NFT资产信息在Crust的保存状态 

updateStatus

当Crust存储完某个项目的NFT资产以后，需要调用这个接口更新项目保存状态，NFTSCAN会根据这个状态去查询更新后的存储链接。

请求参数

| 参数名称 | 类型 | 描述 |
| :-----:| :----: | :----: |
| nft_address | String | NFT合约地址 |
| nft_save_status | int | Crust是否查询数据并进行保存（0未保存,1已保存，2部分保存） |

返回参数

| 参数名称 | 类型 | 描述 |
| :-----:| :----: | :----: |
| code | int | 状态码 |
| msg | String | 消息  |


数据示例 

```json
{
    "nft_address":"0xc83e009c7794e8f6d1954dc13c23a35fc4d039f6",
    "nft_ipfs_svae":1
}
```
```json
{
    "msg": "更新成功",
    "code": 200
}
```



## 根据（合约地址+资产ID）查询在IPFS保存后的链接 

getIpfsData

请求参数

| 参数名称 | 类型 | 描述 |
| :-----:| :----: | :----: |
| nft_contract | String | NFT合约地址 |
| nft_tokenid | int | NFT资产ID |

返回参数

待定

简版流程图


![Crust](https://user-images.githubusercontent.com/11494432/137425612-f6db90ac-ee1d-47f5-8094-b5deb8968ecc.jpg)







