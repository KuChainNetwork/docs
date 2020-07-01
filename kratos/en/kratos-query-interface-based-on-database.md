# kratos - Query interface based on datebase

This document mainly describes kratos's datebase-based interfaces. These interfaces do not directly request full node, but request the relational database mounted on the full node. The full node will synchronize the data to the database in real time.


### 1.  Query basic information of KuChian

Query path: /v1/plugin/chain_info  (GET)

Query example: 

```bash
curl http://localhost:22222/v1/plugin/chain_info
```

Return example:

```
{
    "status_code": "200",
    "status_msg":"ok",
    "data": {
        "node_num": "33",
	    "block_height": "1234",
	    "total_account": "100",
	    "total_txs": "250"
    }
	
}
```

### 2.  Query block list

Query path: /v1/plugin/block_list  (GET)

Query parameters: 

1. proposer: account ID of proposer
2. page
3. limit

Query example: 

```bash
curl http://localhost:22222/v1/plugin/block_list?page=1&limit=2
```

Return example:

```
{
    "status_code":200,
    "status_msg":"query block list succ",
    "data":{
        "total_page_size":"46",
        "block_list":[
            {
                "block_hash":"pjKTiQRR2r6+K789dxPz9sCEHIITDiBEiTuabYY20YA=",
                "proposer":"validator",
                "chain_id":"testing",
                "block_height":"46",
                "block_time":"2020-06-24 13:32:17.40443341 +0000 UTC",
                "last_block_hash":"fKXH69kH/fyZaRW4Ng2LH1HFsmoKToKHzGDAXamdY+c=",
                "next_validators_hash":"bC4VNV6lEm/oGPCOjZBmfbVSOhaXiQo01/LyI46nAqg=",
                "consensus_hash":"BICRvH3cKD93v7+R1zxE2ljD34qcvIZ0Bdi389qtoi8="
            },
            {
                "block_hash":"fKXH69kH/fyZaRW4Ng2LH1HFsmoKToKHzGDAXamdY+c=",
                "proposer":"validator",
                "chain_id":"testing",
                "block_height":"45",
                "block_time":"2020-06-24 13:32:12.385274916 +0000 UTC",
                "last_block_hash":"iROP8t2s4X54QjRWfYUvfYuTiFFeiiqv9s31tRE7xfc=",
                "next_validators_hash":"bC4VNV6lEm/oGPCOjZBmfbVSOhaXiQo01/LyI46nAqg=",
                "consensus_hash":"BICRvH3cKD93v7+R1zxE2ljD34qcvIZ0Bdi389qtoi8="
            }]
    }
}
```

### 3.  Query delegation list

Query path: /v1/plugin/delegation_list  (GET)

Query parameters: 

1. validator: account ID of validator
2. page
3. limit

Query example: 

```bash
curl http://localhost:22222/v1/plugin/delegation_list?validator=validator&page=1&limit=2
```

Return example:

```bash
{
    "status_code":"200",
    "status_msg":"ok",
    "data":{
        "total_page_size":"12",
        "total_delegation":"100000000000000000000000000000000000000",
        "delegation_list":[
            {
                "delegator":"validator",
                "validator":"validator",
                "delegation":"100000000000000000000000000000000000000"
            },
            {
                "delegator":"acc1",
                "validator":"validator",
                "delegation":"100000000000000000000000000000000000000"
            }]
    }
}
```

### 4.  Query delegation change list

Query path: /v1/plugin/delegation_changes  (GET)

Query parameters: 

1. validator: account ID of validator
2. page
3. limit

Query example: 

```bash
curl http://localhost:22222/v1/plugin/delegation_changes?validator=validator&page=1&limit=2
```

Return example:

```bash
{
    "status_code": "200",
    "status_msg":"ok",
    "data": {
        "total_page_size": "12",
        "delegation_change_list": [
            {
                "height": "234",
                "action": "redelegation",
                "hash": "048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F",
                "delegator": "acc1",
                "change": "10000000000000000000000000000",
                "fee": "100",
                "time": "2020-05-31T04:40:26.091493Z"
            },
            {
                "height": "235",
                "action": "unbonding_delegation",
                "hash": "048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F",
                "delegator": "validator",
                "change": "10000000000000000000000000000",
                "fee": "100",
                "time": "2020-05-31T04:40:26.091493Z"
            }
        ]
    }
}
```

### 5.  Query coin list

Query path: /v1/plugin/coin_list  (GET)

Query parameters: 

1. creator: account ID of whom creating the coin
2. page
3. limit

Query example: 

```bash
curl http://localhost:22222/v1/plugin/coin_list?creator=acc1&page=1&limit=2
```

Return example:

```bash
{
    "status_code": "200",
    "status_msg":"ok",
    "data": {
        "total_page_size": "12",
        "coin_list": [
            {
                "symbol": "kcs",
                "creator": "acc1",
                "total": "100000000000000000000000000000000000000",
                "current": "10000000000000000000000000000000000",
                "create_time": "2020-05-31T04:40:26.091493Z",
                "holder_num": "111",
                "coin_txs":"111"
            },
            {
                "symbol": "btc",
                "creator": "acc1",
                "total": "100000000000000000000000000000000000000",
                "current": "10000000000000000000000000000000000",
                "create_time": "2020-05-31T04:40:26.091493Z",
                "holder_num": "111",
                "coin_txs":"111"
            }
        ]
    }
}
```

### 6.  Query account list of coin holder

Query path: /v1/plugin/coin_info  (GET)

Query parameters: 

1. symbol(required): symbol of coin
2. creator(required): account ID of whom creating the coin
3. page
4. limit


Query example: 

```bash
curl http://localhost:22222/v1/plugin/coin_info?symbol=kcs&creator=kratos&page=1&limit=2
```

Return example:

```bash
{
    "status_code": "200",
    "status_msg":"ok",
    "data": {
        "total_page_size": "12",
        "total_coins": "1000000000000000000000000000000",
        "account_list": [
            {
                "account": "acc1",
                "amount": "12345000000000000000000"
            },
            {
                "account": "acc2",
                "amount": "12345000000000000000000"
            }
        ]
    }
}
```

### 7. Query account info

Query path: /v1/plugin/account_info  (GET)

Query parameters: 

1. account(required): account ID

Query example: 

```bash
curl http://localhost:22222/v1/plugin/account_info?account=acc1
```

Return example:

```bash
{
	"status_code": "200",
	"status_msg":"ok",
	"data": {
	    "account_id": "acc1",
	    "auth": "kratos1znrs0ma4kt9pu5nl6f5rr2p4ty06dhmuylnqee",
	    "asset_list": [{
				"denom": "kratos/kts",
				"amount": "1000000000",
				"vesting": "2000000"
			},
			{
				"denom": "validator1/btc",
				"amount": "1000000000",
				"vesting": "2000000"
			}],
	    "tx_num": "200",
	    "delegation": "1000000",
	    "time": "2020-05-31T04:40:26.091493Z"
	}
}
```

### 8. Query node list

Query path: /v1/plugin/node_list  (GET)

Query parameters: 

1. type: node type (1-normal node | 2-jailed node | 3-validator)
2. page
3. limit

Query example: 

```bash
curl http://localhost:22222/v1/plugin/node_list?type=3&page=1&limit=2
```

Return example:

```bash
{
    "status_code": "200",
    "status_msg":"ok",
    "data": {
        "node_list": [
            {
                "operator_account": "acc1",
                "consensus_pubkey": "kratosvalconspub1zcjduepq03jlsrujhh07t4y993f4fkfysupkwxygn3lsvwrpla9fejm9g87qmf3jan",
                "status": "3",
                "delegator_shares": "99999999999999999900000000000000000000",
                "description":{
                    "details":" ",
                    "moniker":" testing",
                    "website":" ",
                    "identity":" ",
                    "security_contact":" "
                },
                "commission": {
                    "rate": "0.100000000000000000",
                    "max_rate": "1.000000000000000000",
                    "max_change_rate": "1.000000000000000000",
                    "update_time": "2020-05-22T06:37:08.521635Z",
                    "commissionrates":""
                }
            },
            {
                "operator_account": "acc2",
                "consensus_pubkey": "kratosvalconspub1zcjduepq03jlsrujhh07t4y993f4fkfysupkwxygn3lsvwrpla9fejm9g87qmf3jan",
                "status": "3",
                "delegator_shares": "99999999999999999900000000000000000000",
                "description":{
                    "details":" ",
                    "moniker":" testing",
                    "website":" ",
                    "identity":" ",
                    "security_contact":" "
                },
                "commission": {
                    "rate": "0.100000000000000000",
                    "max_rate": "1.000000000000000000",
                    "max_change_rate": "1.000000000000000000",
                    "update_time": "2020-05-22T06:37:08.521635Z",
                    "commissionrates":""
                }
            }]
    }
}
```


### 9. Query transaction list

Query path: /v1/plugin/tx_list  (GET)

Query parameters: 

1. msg_type: type of this message
2. msg_sender: account ID of message sender
3. msg_coin_creator: creator of coin in this message
4. msg_coin_symbol: symbol of coin in this message
5. block_height: transaction in this block
6. page
7. limit

Query example: 

```bash
curl http://localhost:22222/v1/plugin/tx_list?msg_sender=acc1&page=1&limit=2
```
Return example:

```
{
    "status_code": "200",
    "status_msg":"ok",
    "data": {
        "tx_list": [
            {
                "type": "cosmos-sdk/StdTx",
                "value": {
                    "msg": [
                        {
                            "type": "account/createMsg",
                            "value": {
                                "KuMsg": {
                                    "auth": [
                                        "kratos103g6xmg08ws48d0rma6rxt0w5w8vs8el5jxhr8"],
                                    "from": "validator",
                                    "to": "acc1",
                                    "amount": [
                                    ],
                                    "router": "account",
                                    "action": "create",
                                    "data": "eyJ0eXBlIjoiYWNjb3VudC9jcmVhdGVEYXRhIiwidmFsdWUiOnsiY3JlYXRvciI6InZhbGlkYXRvciIsIm5hbWUiOiJhY2MxIiwiYXV0aCI6Imt1Y2hhaW4xdnNxbjk1ajJ5a3JnOWNsMHI4cXR2Nnkwc3UzNjAyemw3ODZ0NW0ifX0="
                                }
                            }
                        }],
                    "fee": {
                        "amount": [
                        ],
                        "gas": "200000"
                    },
                    "signatures": [
                        {
                            "pub_key": {
                                "type": "tendermint/PubKeySecp256k1",
                                "value": "AwXNXNKgyNN1LqgzymyJ46qccktPWO/Zak3y+DLEu+Up"
                            },
                            "signature": "LhdLmDLGfQr1RJALYyjCdHN+G4WHTqiraKzoHungKdJFK2VQzblbkAQj5HFFkorl+PLXtHjBdA1SBAKIFGm01Q=="
                        }],
                    "memo": ""
                }
            },
            {
                "type": "cosmos-sdk/StdTx",
                "value": {
                    "msg": [
                        {
                            "type": "account/createMsg",
                            "value": {
                                "KuMsg": {
                                    "auth": [
                                        "kratos103g6xmg08ws48d0rma6rxt0w5w8vs8el5jxhr8"],
                                    "from": "validator",
                                    "to": "acc1",
                                    "amount": [
                                    ],
                                    "router": "account",
                                    "action": "create",
                                    "data": "eyJ0eXBlIjoiYWNjb3VudC9jcmVhdGVEYXRhIiwidmFsdWUiOnsiY3JlYXRvciI6InZhbGlkYXRvciIsIm5hbWUiOiJhY2MxIiwiYXV0aCI6Imt1Y2hhaW4xdnNxbjk1ajJ5a3JnOWNsMHI4cXR2Nnkwc3UzNjAyemw3ODZ0NW0ifX0="
                                }
                            }
                        }],
                    "fee": {
                        "amount": [
                        ],
                        "gas": "200000"
                    },
                    "signatures": [
                        {
                            "pub_key": {
                                "type": "tendermint/PubKeySecp256k1",
                                "value": "AwXNXNKgyNN1LqgzymyJ46qccktPWO/Zak3y+DLEu+Up"
                            },
                            "signature": "LhdLmDLGfQr1RJALYyjCdHN+G4WHTqiraKzoHungKdJFK2VQzblbkAQj5HFFkorl+PLXtHjBdA1SBAKIFGm01Q=="
                        }],
                    "memo": ""
                }
            }]
    }
}
```
