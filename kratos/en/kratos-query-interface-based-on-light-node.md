# kratos - Query interface based on light node

This document mainly describes all rpc query interfaces based on kratos, including querying blockchain information, querying transaction information, etc. The specific information will be explained in detail in each interface below.

Note: The interface described in this document cannot directly request the kratos full node (because all external communication data of the kratos full node has been serialized), these interfaces are exposed by the kratos light node, and the client's request will be sent to the light node. The light nodes will serialize the request and then route it to the corresponding full node handler, where a full node can be connected by multiple light nodes.

Light node startup method: For the started blockchain network, you only need to know the rpc address of one of the full nodes to start the light node. The command is as follows:

`kucli rest-server --node "tcp://localhost:26657" --trust-node`

Where `tcp://localhost:26657` is the rpc address of the full node

*This document only provides query interfaces. For account creation, transfer, voting, etc., since the transaction needs to be signed locally before the transaction is sent, these interfaces will also be integrated in js-sdk*



### Node information interface

*Query blocks, transactions and nodes*

##### 1. Query basic information of nodes, such as chain ID, node name, block height, etc.

Query path: /node_info  (GET)

Query example:

```bash
curl http://localhost:1317/node_info
```

Return example:

```
{
	"node_info": {
		"protocol_version": {
			"p2p": "7",
			"block": "10",
			"app": "0"
		},
		"id": "6671b26ac8ce68042fdba47f71e0666db53aee09",
		"listen_addr": "tcp://0.0.0.0:26656",
		"network": "testing",
		"version": "0.33.2",
		"channels": "4020212223303800",
		"moniker": "testing",
		"other": {
			"tx_index": "on",
			"rpc_address": "tcp://127.0.0.1:26657"
		}
	},
	"application_version": {
		"name": "kratos",
		"server_name": "kucd",
		"client_name": "kucli",
		"version": "0.0.1",
		"commit": "b365bd5a0ad8cdd8a8a84dd8f05ba066ec793a7c",
		"build_tags": "netgo,ledger",
		"go": "go version go1.13.3 darwin/amd64"
	}
}
```

##### 2. Query the latest block information

Query path: /blocks/latest  (GET)

Query example:

```bash
curl http://localhost:1317/blocks/latest
```

Return example:

```
{
	"block_id": {
		"hash": "2CD6BDAAFFBF94E17B307341498467655C2B11D9FB87A67021A353586B150D78",
		"parts": {
			"total": "1",
			"hash": "E21EA8313BB92945A1A7F69CE0F33F00E66F4165FBBC9D5B522305478173C2A2"
		}
	},
	"block": {
		"header": {
			"version": {
				"block": "10",
				"app": "0"
			},
			"chain_id": "testing",
			"height": "7418",
			"time": "2020-05-25T03:22:02.090174Z",
			"last_block_id": {
				"hash": "29C20BA6C8ADCF7BA4BE2EC4AE1B1FE1ABF5D0442F154D4111B5775902BBFEA1",
				"parts": {
					"total": "1",
					"hash": "F79421020D5FD0DBF73483C9A7E2AA4C0CEB991E8DD78884F2D2624AF7DFAD2F"
				}
			},
			"last_commit_hash": "D44841E64DAFAB3AEFE3AD86993809B205BDC255B7C3AF6ACC211D79C898B93B",
			"data_hash": "",
			"validators_hash": "361460B717522DB967CD57C9EC4D944928A2294622EFB0F4D31CF63CACBD3748",
			"next_validators_hash": "361460B717522DB967CD57C9EC4D944928A2294622EFB0F4D31CF63CACBD3748",
			"consensus_hash": "048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F",
			"app_hash": "56A09319322CF629EB80F6B32FBC3C09FEC2E760A8EDBE53D874F7105C71F239",
			"last_results_hash": "",
			"evidence_hash": "",
			"proposer_address": "C5BC6CAAB799AD682ADF2D5465F81417EF56AC0C"
		},
		"data": {
			"txs": null
		},
		"evidence": {
			"evidence": null
		},
		"last_commit": {
			"height": "7417",
			"round": "0",
			"block_id": {
				"hash": "29C20BA6C8ADCF7BA4BE2EC4AE1B1FE1ABF5D0442F154D4111B5775902BBFEA1",
				"parts": {
					"total": "1",
					"hash": "F79421020D5FD0DBF73483C9A7E2AA4C0CEB991E8DD78884F2D2624AF7DFAD2F"
				}
			},
			"signatures": [{
				"block_id_flag": 2,
				"validator_address": "C5BC6CAAB799AD682ADF2D5465F81417EF56AC0C",
				"timestamp": "2020-05-25T03:22:02.090174Z",
				"signature": "eC2Wlji3G31X6m5e0XagciGfS8342pf+hK7szZRzvPF3hSSgCybuwgDEMAgCmkc+4Yy473c/fGqgvFzIIA8NCw=="
			}]
		}
	}
}
```

##### 3.  Query the information of the specified height block

Query path: /blocks/{height}  (GET)

Query example:

```bash
curl http://localhost:1317/blocks/25
```

Return example: same as above


### Account information interface

##### 1.  Query the information of the specified account

Query path: /account/{name}  (GET)

Query example:

```bash
curl http://localhost:1317/account/validator
```

Return example:

```
{
	"height": "7449",
	"result": {
		"type": "kratos/Account",
		"value": {
			"id": "validator",
			"auth": "kratos103g6xmg08ws48d0rma6rxt0w5w8vs8el5jxhr8"
		}
	}
}
```

##### 2.  Qeury information about the specified auth

Query path: /account/auth/{auth}  (GET)

Query example:

```bash
curl http://localhost:1317/account/auth/kratos1fhqjhs22s4cwvjxrvlcyst3h4pvw7x49jvk0ux
```

Return example:

```
{
    "height":"2289",
    "result":{
        "name":"",
        "address":"kratos1fhqjhs22s4cwvjxrvlcyst3h4pvw7x49jvk0ux",
        "public_key":"61rphyEDqhKDCPQbqGigQONivg64x2vAb2DU/W4RB+iTEOiGX60=",
        "number":"1",
        "sequence":"4"
    }
}
```

##### 3.  Query account list by auth

Query path: /accounts/{auth}  (GET)

Query example:

```bash
curl http://localhost:1317/accounts/kratos1fhqjhs22s4cwvjxrvlcyst3h4pvw7x49jvk0ux
```

Return example:

```
{
    "height":"2289",
    "result":["acc1", "acc2"]
}
```


### Transaction query interface

##### 1.  Query the information of the specified transaction according to the hash

Query path: /txs/{hash}  (GET)

Query example:

```bash
curl http://localhost:1317/txs/1DD53A2A31A5B10012F279356E7A44992F497B0DC1449EF6133B0C7BEB9BE37A
```

Return example:

```
{
	"height": "236",
	"txhash": "644771EAC46CF64A5A7ED235B1B999CF8904E05B9E3E632244C9E0256ABD9933",
	"raw_log": "[{\"msg_index\":0,\"log\":\"\",\"events\":[{\"type\":\"create_account\",\"attributes\":[{\"key\":\"creator\",\"value\":\"validator\"},{\"key\":\"account\",\"value\":\"acc1\"},{\"key\":\"auth\",\"value\":\"kratos1vsqn95j2ykrg9cl0r8qtv6y0su3602zl786t5m\"}]},{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"create\"}]}]}]",
	"logs": [{
		"msg_index": 0,
		"log": "",
		"events": [{
			"type": "create_account",
			"attributes": [{
				"key": "creator",
				"value": "validator"
			}, {
				"key": "account",
				"value": "acc1"
			}, {
				"key": "auth",
				"value": "kratos1vsqn95j2ykrg9cl0r8qtv6y0su3602zl786t5m"
			}]
		}, {
			"type": "message",
			"attributes": [{
				"key": "action",
				"value": "create"
			}]
		}]
	}],
	"gas_used": "114814",
	"tx": {
		"type": "cosmos-sdk/StdTx",
		"value": {
			"msg": [{
				"type": "account/createMsg",
				"value": {
					"KuMsg": {
						"auth": ["kratos103g6xmg08ws48d0rma6rxt0w5w8vs8el5jxhr8"],
						"from": "validator",
						"to": "acc1",
						"amount": [],
						"router": "account",
						"action": "create",
						"data": "eyJ0eXBlIjoiYWNjb3VudC9jcmVhdGVEYXRhIiwidmFsdWUiOnsiY3JlYXRvciI6InZhbGlkYXRvciIsIm5hbWUiOiJhY2MxIiwiYXV0aCI6Imt1Y2hhaW4xdnNxbjk1ajJ5a3JnOWNsMHI4cXR2Nnkwc3UzNjAyemw3ODZ0NW0ifX0="
					}
				}
			}],
			"fee": {
				"amount": [],
				"gas": "200000"
			},
			"signatures": [{
				"pub_key": {
					"type": "tendermint/PubKeySecp256k1",
					"value": "AwXNXNKgyNN1LqgzymyJ46qccktPWO/Zak3y+DLEu+Up"
				},
				"signature": "LhdLmDLGfQr1RJALYyjCdHN+G4WHTqiraKzoHungKdJFK2VQzblbkAQj5HFFkorl+PLXtHjBdA1SBAKIFGm01Q=="
			}],
			"memo": ""
		}
	},
	"timestamp": "2020-05-22T06:58:13Z"
}
```

##### 2.  Query the information of the specified transaction according to the conditions set in the parameters

Query path: /txs  (GET)

Query parameters:

1. message.action
2. message.sender  
3. page
4. limit

Query example:

```bash
curl http://localhost:1317/txs?message.action=delegate&message.sender=validator13&page=1&limit=1
```

Return example: same as above



### Asset related interface

##### 1.  Query assets of specified account

Query path: /assets/coins/{account}  (GET)

Query example:

```bash
curl http://localhost:1317/assets/coins/validator
```

Return example:

```bash
{"height":"7889","result":[{"denom":"kratos/kts","amount":"999399888998898899000000"}]}
```


##### 2.  Query coinpower of specified account (coinpower is assets inside the module account)

Query path: /assets/coin_powers/{account}  (GET)

Query example:

```bash
curl http://localhost:1317/assets/coin_powers/validator
```

Return example:

```bash
{"height":"7915","result":[{"denom":"kratos/kts","amount":"500111001101100000000"}]}
```


##### 3.  Query total asset supply information

Query path: /supply/total  (GET)

Query example:

```bash
curl http://localhost:1317/supply/total
```

Return example:

```bash
{"height":"7930","result":[{"denom":"kratos/kts","amount":"1000000"},{"denom":"kratos/kts","amount":"1000113156378650494040858"}]}
```

##### 4.  Query the specified total asset supply information

Query path: /supply/total/{denomination}  (GET)

Query example:

```bash
curl http://localhost:1317/supply/total/kratos/kts
```

Return example:

```bash
{"height":"7954","result":"1000113499079433706690908"}
```

##### 5.  Query inflation/deflation parameter information

Query path: /minting/parameters  (GET)

Query example:

```bash
curl http://localhost:1317/minting/parameters
```

Return example:

```bash
{"height":"7958","result":{
  "mint_denom": "kratos/kts",
  "inflation_rate_change": "0.090000000000000000",
  "inflation_max": "0.120000000000000000",
  "inflation_min": "0.060000000000000000",
  "goal_bonded": "0.670000000000000000",
  "blocks_per_year": "6311520"
}}
```

##### 6.  Query lock information

Query path: /assets/coins_locked/{account} (GET)

Query example:

```bash
curl http://localhost:1317/assets/coins_locked/acc1
```

Return example:

```bash
{"height":"242","result":{"coins":[{"denom":"acc1/btc","amount":"2000"}],"locks":[{"coins":[{"denom":"acc1/btc","amount":"2000"}],"unlock_block_height":"300"}]}}
```

##### 7.  Query token status information

Query path: /assets/coin_stat/{creator}/{symbol} (GET)

Query example:

```bash
curl http://localhost:1317/assets/coin_stat/acc1/btc
```

Return example:

```bash
{
    "height":"274",
    "result":{
        "CoinStat":{
            "symbol":"btc",
            "creator":"acc1",
            "create_height":"183",
            "supply":{
                "denom":"acc1/btc",
                "amount":"100000"
            },
            "max_supply":{
                "denom":"acc1/btc",
                "amount":"1000000000"
            },
            "can_issue":true,
            "can_lock":true,
            "issue_to_height":"1000",
            "init_supply":{
                "denom":"acc1/btc",
                "amount":"500000000"
            }
        },
        "current_max_supply":{
            "denom":"acc1/btc",
            "amount":"555691554"
        }
    }
}
```


### Staking query interface

##### 1.  Query all delegation informations of the specified account

Query path: /staking/delegators/{delegatorAccount}/delegations  (GET)

Query example:

```bash
curl http://localhost:1317/staking/delegators/validator/delegations
```

Return example:

```bash
{"height":"7998","result":[
  {
    "delegator_account": "validator",
    "validator_account": "validator",
    "shares": "100000000000000000000.000000000000000000",
    "balance": {
      "denom": "kratos/kts",
      "amount": "100000000000000000000"
    }
  }
]}
```

##### 2.  Query the delegation informations of the specified delegator for the specified validator

Query path: /staking/delegators/{delegatorAccount}/delegations/{validatorAccount}  (GET)

Query example:

```bash
curl http://localhost:1317/staking/delegators/acc1/delegations/validator
```

Return example:

```bash
{"height":"8071","result":{
  "delegator_account": "validator",
  "validator_account": "validator",
  "shares": "100000000000000000000.000000000000000000",
  "balance": {
    "denom": "kratos/kts",
    "amount": "100000000000000000000"
  }
}}
```

##### 3.  Query assets that have not received after the specified delegator withdraws the delegation

Query path: /staking/delegators/{delegatorAccount}/unbonding_delegations  (GET)

Query example:

```bash
curl
http://localhost:1317/staking/delegators/validator/unbonding_delegations
```

Return example:

```bash
{"height":"8086","result":[
  {
    "delegator_account": "validator",
    "validator_account": "validator",
    "entries": [
      {
        "creation_height": "8084",
        "completion_time": "2020-06-08T04:17:58.313588Z",
        "initial_balance": "100",
        "balance": "100"
      }
    ]
  }
]}
```

##### 4. Query assets that have not been received after the specified delegator withdraws the delegation for the specified validator

Query path: /staking/delegators/{delegatorAccount}/unbonding_delegations/{validatorAccount}  (GET)

Query example:

```bash
curl
http://localhost:1317/staking/delegators/acc1/unbonding_delegations/validator
```

Return example:

```bash
{"height":"8091","result":{
  "delegator_account": "validator",
  "validator_account": "validator",
  "entries": [
    {
      "creation_height": "8084",
      "completion_time": "2020-06-08T04:17:58.313588Z",
      "initial_balance": "100",
      "balance": "100"
    }
  ]
}}
```

##### 5.  Query information about redelegation with specified parameters

Query path: /staking/redelegations  (GET)

Query description: Since there is no freezing period for redelegation, the delegation has been moved after the transaction is commited, so the interface has a high probability that the query will return null

parameters:

1. delegator
2. validator_from
3. validator_to

Query example:

```bash
curl http://localhost:1317/staking/redelegations?delegator=acc1&validator_from=validator&validator_to=validator2
```

##### 6.  Query all validators information delegated by the specified delegator

Query path: /staking/delegators/{delegatorAccount}/validators  (GET)

Query example:

```bash
curl http://localhost:1317/staking/delegators/validator/validators
```

Return example:

```bash
{"height":"8198","result":[
  {
    "operator_account": "validator",
    "consensus_pubkey": "kratosvalconspub1zcjduepq03jlsrujhh07t4y993f4fkfysupkwxygn3lsvwrpla9fejm9g87qmf3jan",
    "status": 3,
    "tokens": "99999999999999999900",
    "delegator_shares": "99999999999999999900.000000000000000000",
    "description": {
      "moniker": "testing"
    },
    "unbonding_time": "1970-01-01T00:00:00Z",
    "commission": {
      "commission_rates": {
        "rate": "0.100000000000000000",
        "max_rate": "1.000000000000000000",
        "max_change_rate": "1.000000000000000000"
      },
      "update_time": "2020-05-22T06:37:08.521635Z"
    },
    "min_self_delegation": "1"
  }
]}
```

##### 7.  Query validator information in delegation by a specified delegator for a specified validator

Query path: /staking/delegators/{delegatorAccount}/validators/{validatorAccount}  (GET)

Query example:

```bash
curl http://localhost:1317/staking/delegators/acc1/validators/validator
```

Return example:

```bash
{"height":"8214","result":{
  "operator_account": "validator",
  "consensus_pubkey": "kratosvalconspub1zcjduepq03jlsrujhh07t4y993f4fkfysupkwxygn3lsvwrpla9fejm9g87qmf3jan",
  "status": 3,
  "tokens": "99999999999999999900",
  "delegator_shares": "99999999999999999900.000000000000000000",
  "description": {
    "moniker": "testing"
  },
  "unbonding_time": "1970-01-01T00:00:00Z",
  "commission": {
    "commission_rates": {
      "rate": "0.100000000000000000",
      "max_rate": "1.000000000000000000",
      "max_change_rate": "1.000000000000000000"
    },
    "update_time": "2020-05-22T06:37:08.521635Z"
  },
  "min_self_delegation": "1"
}}
```

##### 8.  Query information of all validators

Query path: /staking/validators  (GET)

Query example:

```bash
curl http://localhost:1317/staking/validators
```

Return example:

```bash
{"height":"8232","result":[
  {
    "operator_account": "validator",
    "consensus_pubkey": "kratosvalconspub1zcjduepq03jlsrujhh07t4y993f4fkfysupkwxygn3lsvwrpla9fejm9g87qmf3jan",
    "status": 3,
    "tokens": "99999999999999999900",
    "delegator_shares": "99999999999999999900.000000000000000000",
    "description": {
      "moniker": "testing"
    },
    "unbonding_time": "1970-01-01T00:00:00Z",
    "commission": {
      "commission_rates": {
        "rate": "0.100000000000000000",
        "max_rate": "1.000000000000000000",
        "max_change_rate": "1.000000000000000000"
      },
      "update_time": "2020-05-22T06:37:08.521635Z"
    },
    "min_self_delegation": "1"
  }
]}
```

##### 9.  Query information of specified validator

Query path: /staking/validators/{validatorAccount}  (GET)

Query example:

```bash
curl http://localhost:1317/staking/validators/validator
```

Return example:

```bash
{"height":"8237","result":{
  "operator_account": "validator",
  "consensus_pubkey": "kratosvalconspub1zcjduepq03jlsrujhh07t4y993f4fkfysupkwxygn3lsvwrpla9fejm9g87qmf3jan",
  "status": 3,
  "tokens": "99999999999999999900",
  "delegator_shares": "99999999999999999900.000000000000000000",
  "description": {
    "moniker": "testing"
  },
  "unbonding_time": "1970-01-01T00:00:00Z",
  "commission": {
    "commission_rates": {
      "rate": "0.100000000000000000",
      "max_rate": "1.000000000000000000",
      "max_change_rate": "1.000000000000000000"
    },
    "update_time": "2020-05-22T06:37:08.521635Z"
  },
  "min_self_delegation": "1"
}}
```

##### 10.  Query all delegators information of the specified validator

Query path: /staking/validators/{validatorAccount}/delegations  (GET)

Query example:

```bash
curl http://localhost:1317/staking/validators/validator/delegations
```

Return example:

```bash
{"height":"8241","result":[
  {
    "delegator_account": "validator",
    "validator_account": "validator",
    "shares": "99999999999999999900.000000000000000000",
    "balance": {
      "denom": "kratos/kts",
      "amount": "99999999999999999900"
    }
  }
]}
```

##### 11. Query all assets of the specified validator that have not arrived after withdraw delegation

Query path: /staking/validators/{validatorAccount}/unbonding_delegations  (GET)

Query example:

```bash
curl http://localhost:1317/staking/validators/validator/unbonding_delegations
```

Return example:

```bash
{"height":"8244","result":[
  {
    "delegator_account": "validator",
    "validator_account": "validator",
    "entries": [
      {
        "creation_height": "8084",
        "completion_time": "2020-06-08T04:17:58.313588Z",
        "initial_balance": "100",
        "balance": "100"
      }
    ]
  }
]}
```

##### 12.  Query information of staking pool

Query path: /staking/pool  (GET)

Query example:

```bash
curl http://localhost:1317/staking/pool
```

Return example:

```bash
{"height":"8247","result":{
  "not_bonded_tokens": "100000000000000000100",
  "bonded_tokens": "99999999999999999900"
}}
```

##### 13.  Query parameters of staking module

Query path: /staking/parameters  (GET)

Query example:

```bash
curl http://localhost:1317/staking/parameters
```

Return example:

```bash
{"height":"8250","result":{
  "unbonding_time": "1209600000000000",
  "max_validators": 33,
  "max_entries": 7,
  "bond_denom": "kratos/kts"
}}
```



### Governance related interface

##### 1.  Query proposals by specified parameters

Query path: /gov/proposals  (GET)

Query description: Through some filter conditions, filter out the proposals that meet the conditions. Before querying the proposals, you should first create a proposal.

Query parameters:

1. voter
2. depositor
3. status ("deposit_period", "voting_period", "passed", "rejected")

Query example:

```bash
curl http://localhost:1317/gov/proposals?status=passed
```

Return example:

```
{"height":"7231","result":[
  {
    "content": {
      "type": "kratos/TextProposal",
      "value": {
        "title": "validator for test",
        "description": "test test test"
      }
    },
    "ProposalBase": {
      "id": "1",
      "status": "Passed",
      "final_tally_result": {
        "yes": "100000900",
        "abstain": "0",
        "no": "0",
        "no_with_veto": "0"
      },
      "submit_time": "2020-05-18T10:22:30.2112Z",
      "deposit_end_time": "2020-06-01T10:22:30.2112Z",
      "total_deposit": [
        {
          "denom": "kratos/kts",
          "amount": "600000000"
        }
      ],
      "voting_start_time": "2020-05-19T07:04:00.842831Z",
      "voting_end_time": "2020-05-19T07:28:17.65806Z"
    }
  }
]}
```

##### 2.  Query proposals by specified proposal ID

Query path: /gov/proposals/{proposalId}  (GET)

Query example:

```bash
curl http://localhost:1317/gov/proposals/1
```
Return example:

```
{"height":"6870","result":{
  "content": {
    "type": "kratos/TextProposal",
    "value": {
      "title": "validator for test",
      "description": "test test test"
    }
  },
  "ProposalBase": {
    "id": "1",
    "status": "Passed",
    "final_tally_result": {
      "yes": "100000900",
      "abstain": "0",
      "no": "0",
      "no_with_veto": "0"
    },
    "submit_time": "2020-05-18T10:22:30.2112Z",
    "deposit_end_time": "2020-06-01T10:22:30.2112Z",
    "total_deposit": [
      {
        "denom": "kratos/kts",
        "amount": "600000000"
      }
    ],
    "voting_start_time": "2020-05-19T07:04:00.842831Z",
    "voting_end_time": "2020-05-19T07:28:17.65806Z"
  }
}}
```

##### 3.  Query proposer of a specified proposal

Query path: /gov/proposals/{proposalId}/proposer  (GET)

Query example:

```bash
http://localhost:1317/gov/proposals/1/proposer
```

Return example:

```
{"height":"6630","result":{"proposal_id":"1","proposer":"validator"}}
```

##### 4.  Query current status of a specified proposal

Query path: /gov/proposals/{proposalId}/tally  (GET)

Query example:

```bash
curl http://localhost:1317/gov/proposals/1/tally
```

Return example:

```
{"height":"6630","result":{
  "yes": "100000900",
  "abstain": "50000",
  "no": "6500000",
  "no_with_veto": "0"
}}
```

##### 5.  Query deposits of a specified proposal

Query path: /gov/proposals/{proposalId}/deposits  (GET)

Query example:

```bash
curl http://localhost:1317/gov/proposals/1/deposits
```

Return example:

```
{
	"height": "0",
	"result": [{
		"proposal_id": "1",
		"depositor": "acc1",
		"amount": [{
			"denom": "kratos/kts",
			"amount": "100000"
		}]
	}, {
		"proposal_id": "1",
		"depositor": "validator",
		"amount": [{
			"denom": "kratos/kts",
			"amount": "1000000000"
		}]
	}, {
		"proposal_id": "1",
		"depositor": "validator",
		"amount": [{
			"denom": "kratos/kts",
			"amount": "100000000000"
		}]
	}, {
		"proposal_id": "1",
		"depositor": "validator",
		"amount": [{
			"denom": "kratos/kts",
			"amount": "1000000000000"
		}]
	}, {
		"proposal_id": "1",
		"depositor": "validator",
		"amount": [{
			"denom": "kratos/kts",
			"amount": "1000000000000000"
		}]
	}, {
		"proposal_id": "1",
		"depositor": "validator",
		"amount": [{
			"denom": "kratos/kts",
			"amount": "10000000000000000"
		}]
	}, {
		"proposal_id": "1",
		"depositor": "validator",
		"amount": [{
			"denom": "kratos/kts",
			"amount": "100000000000000000"
		}]
	}, {
		"proposal_id": "1",
		"depositor": "validator",
		"amount": [{
			"denom": "kratos/kts",
			"amount": "500000000000000000000"
		}]
	}]
}
```

##### 6.  Query deposit which a specified account deposit for a specified proposal

Query path: /gov/proposals/{proposalId}/deposits/{depositor} (GET)

Query example:

```bash
curl http://localhost:1317/gov/proposals/1/deposits/validator
```

Return example:

```
{"height":"0","result":{"proposal_id":"1","depositor":"validator","amount":[{"denom":"kratos/kts","amount":"1000000000"}]}}
```

##### 7.  Query voting information of a specified proposal

Query path: /gov/proposals/{proposalId}/votes (GET)

Query example:

```bash
curl http://localhost:1317/gov/proposals/1/votes
```

Return example:

```
{"height":"0","result":[{"proposal_id":"1","voter":"validator","option":"Yes"}]}
```

##### 8.  Query voting information which a specified account vote for a specified proposal

Query path: /gov/proposals/{proposalId}/votes/{voter}  (GET)

Query example:

```bash
curl http://localhost:1317/gov/proposals/1/votes/validator
```

Return example:

```
{"height":"0","result":{"proposal_id":"1","voter":"validator","option":"Yes"}}
```

##### 9.  Query parameter information of deposit

Query path: /gov/parameters/deposit  (GET)

Query example:

```bash
curl http://localhost:1317/gov/parameters/deposit
```

Return example:

```
{"height":"8491","result":{
  "min_deposit": [
    {
      "denom": "kratos/kts",
      "amount": "500000000000000000000"
    }
  ],
  "max_deposit_period": "1209600000000000"
}}
```

##### 10.  Query parameter information of voting

Query path: /gov/parameters/voting  (GET)

Query example:

```bash
curl http://localhost:1317/gov/parameters/voting
```

Return example:

```
{"height":"8497","result":{
  "voting_period": "1209600000000000"
}}
```


### Distribution related interface

##### 1.  Query rewards of a specified account

Query path: /distribution/delegators/{delegator_account}/rewards  (GET)

Query example:

```bash
curl http://localhost:1317/distribution/delegators/validator/rewards
```

Return example:

```bash
{
	"height": "56",
	"result": {
		"rewards": [{
			"validator_account": "validator",
			"reward": [{
				"denom": "kratos/kts",
				"amount": "20060384998540806800.000000000000000000"
			}]
		}],
		"total": [{
			"denom": "kratos/kts",
			"amount": "20060384998540806800.000000000000000000"
		}]
	}
}
```


##### 2.  Query the specified account's rewards for the specified validator

Query path: /distribution/delegators/{delegator_account}/rewards/{validator_account} (GET)

Query example:

```bash
curl http://localhost:1317/distribution/delegators/validator/rewards/validator
```

Return example:

```bash
{"height":"66","result":[
  {
    "denom": "kratos/kts",
    "amount": "27795282979052557300.000000000000000000"
  }
]}
```


##### 3.  Query the account of the specified account to receive the rewards

Query path: /distribution/delegators/{delegator_account}/withdraw_account  (GET)

Query example:

```bash
curl http://localhost:1317/distribution/delegators/validator/withdraw_account
```

Return example:

```bash
{"height":"49428","result":{
  "withdraw_account": "test1",
  "validator_account": "",
  "delegator_account": "test1"
}}
```


##### 4.  Query all rewards information of the specified validator (including delegator, validator, fee)

Query path: /distribution/validators/{validator_account}  (GET)

Query example:

```bash
curl http://localhost:1317/distribution/validators/validator
```

Return example:

```bash
{
	"height": "80",
	"result": {
		"operator_account": "validator",
		"self_bond_rewards": [{
			"denom": "kratos/kts",
			"amount": "40737101398353519300.000000000000000000"
		}],
		"val_commission": {
			"commission": [{
				"denom": "kratos/kts",
				"amount": "4526344599817057705.520000000000000000"
			}]
		}
	}
}
```

##### 5.  Query outstanding_rewards of a specified validator

Query path: /distribution/validators/{validatorAcc}/outstanding_rewards  (GET)

Query example:

```bash
curl http://localhost:1317/distribution/validators/validator/outstanding_rewards
```

Return example:

```bash
{"height":"87","result":{
  "rewards": [
    {
      "denom": "kratos/kts",
      "amount": "53480480892038981849.880000000000000000"
    }
  ]
}}
```

##### 6.  Query self-rewards of a specified validator

Query path: /distribution/validators/{validator_account}/rewards  (GET)

Query example:

```bash
curl http://localhost:1317/distribution/validators/validator/rewards
```

Return example:

```bash
{"height":"92","result":[
  {
    "denom": "kratos/kts",
    "amount": "53792128123685819600.000000000000000000"
  }
]}
```

##### 7.  Query parameters of distribution module

Query path: /distribution/parameters  (GET)

Query example:

```bash
curl http://localhost:1317/distribution/parameters
```

Return example:

```bash
{"height":"99","result":{
  "community_tax": "0.020000000000000000",
  "base_proposer_reward": "0.010000000000000000",
  "bonus_proposer_reward": "0.040000000000000000",
  "withdraw_addr_enabled": true
}}
```


