# kratos - Post interface based on light node

This document mainly describes all the chain-based rpc post interfaces of kratos, including constructing transactions, sending transactions, serializing msg, etc. The specific information will be explained in detail in each interface below.

### Transaction related interface

##### 1.  Send a transaction (broadcast and write on blockchain)

Query path: /txs  (POST)

Query example:

```
curl --request POST --url http://localhost:1317/txs \
     --header 'accept: application/json' \
     --data '{"tx":{"msg":[{"type":"account/createMsg","value":{"KuMsg":{"auth":["kratos1fhqjhs22s4cwvjxrvlcyst3h4pvw7x49jvk0ux"],"from":"acc1","to":"acc2","amount":[],"router":"account","action":"create","data":"RJzo+ewKEwoRAQEEBDDhAAAAAAAAAAAAAAASEwoRAQEEBDDiAAAAAAAAAAAAAAAaFE3BK8FKhXDmSMNn8EguN6hY7xql"}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"20"}],"gas":"15000","payer":"acc1"},"signatures":[{"signature":"JSTwCDsT2l7qIduRcIVGP1PIm0hGrXIS/w7Bbf3PQB9EmJsDxA6+RCpNVB4r7oOV7sLAS5GZWRe5RR7cM1tQ2g==","pub_key":{"type":"tendermint/PubKeySecp256k1","value":"A6oSgwj0G6hooEDjYr4OuMdrwG9g1P1uEQfokxDohl+t"}}],"memo":"send via kratos"},"mode":"sync"}'
```

Return example:

```bash
{"height":"0","txhash":"BADEDFD7396EA94AAC1FFD67A008563B50C066B5978C5C5F2B0753D8E9F9079F","raw_log":"[]"}
```

##### 2.  Serializae message

Query path: /sign_msg/encode  (POST)

Query example:

```
curl --request POST --url http://localhost:1317/sign_msg/encode \
     --header 'accept: application/json' \
     --data '{"chain_id":"testing","account_number":"1","sequence":"1","msg":[{"type":"account/createMsg","value":{"KuMsg":{"auth":["kratos1fhqjhs22s4cwvjxrvlcyst3h4pvw7x49jvk0ux"],"from":"acc1","to":"acc2","amount":[],"router":"account","action":"create","data":"RJzo+ewKEwoRAQEEBDDhAAAAAAAAAAAAAAASEwoRAQEEBDDiAAAAAAAAAAAAAAAaFE3BK8FKhXDmSMNn8EguN6hY7xql"}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"100"}],"gas":"200000","payer":"acc1"},"memo":"send via kratos"}'
```

Return example:

```
{"msg":"eyJhY2NvdW50X251bWJlciI6IjEiLCJjaGFpbl9pZCI6InRlc3RpbmciLCJmZWUiOnsiYW1vdW50IjpbeyJhbW91bnQiOiIxMDAiLCJkZW5vbSI6Imt1Y2hhaW4va2NzIn1dLCJnYXMiOiIyMDAwMDAiLCJwYXllciI6ImFjYzEifSwibWVtbyI6InNlbmQgdmlhIGt1Y2hhaW4iLCJtc2ciOlt7ImFjdGlvbiI6ImNyZWF0ZSIsImFtb3VudCI6W10sImF1dGgiOlsia3VjaGFpbjFmaHFqaHMyMnM0Y3d2anhydmxjeXN0M2g0cHZ3N3g0OWp2azB1eCJdLCJkYXRhIjoiUkp6bytld0tFd29SQVFFRUJERGhBQUFBQUFBQUFBQUFBQUFTRXdvUkFRRUVCRERpQUFBQUFBQUFBQUFBQUFBYUZFM0JLOEZLaFhEbVNNTm44RWd1TjZoWTd4cWwiLCJmcm9tIjoiYWNjMSIsInJvdXRlciI6ImFjY291bnQiLCJ0byI6ImFjYzIifV0sInNlcXVlbmNlIjoiMSJ9"}
```

### Account related interface

*Note that this is only constructing a transaction in json, not sending a transaction*

##### 1.  Constructing a transaction of creating account 

Query path: /account/create  (POST)

Query example:

```bash
curl --request POST --url http://localhost:1317/account/create \
     --header 'accept: application/json' \
     --data '{"base_req":{"memo":"Sent via kratos","chain_id":"testing","account_number":"0","sequence":"1","gas":"200000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}]},"creator":"validator","account":"acc1","account_auth":"kratos1vd9tu3k7thsf3saeegd5x3ldetqf0s4xtarjpx"}'
```

Return example:

```
{"type":"kratos/Tx","value":{"msg":[{"type":"account/createMsg","value":{"KuMsg":{"auth":["kratos1fg3svnqcv9wcln0q5cwdcm0uk8a4hwqvng26sh"],"from":"validator","to":"acc1","amount":[],"router":"account","action":"create","data":"RJzo+ewKEwoRAQEJWBMJEBUPSAAAAAAAAAASEwoRAQEEBDDhAAAAAAAAAAAAAAAaFGNKvkbeXeCYw7nKG0NH7crAl8Km"}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"200000","payer":""},"signatures":null,"memo":"Sent via kratos"}}
```

##### 2.  Constructing a transaction of updating auth

Query path: /account/update_auth  (POST)

Query example:

```bash
curl --request POST --url http://localhost:1317/account/update_auth \
     --header 'accept: application/json' \
     --data '{"base_req":{"memo":"Sent via kratos","chain_id":"testing","account_number":"0","sequence":"1","gas":"200000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}],"payer":"acc1"},"account":"acc1","new_account_auth":"kratos1vd9tu3k7thsf3saeegd5x3ldetqf0s4xtarjpx"}'
```

Return example:

```
{"type":"kratos/Tx","value":{"msg":[{"type":"account/upAuth","value":{"KuMsg":{"auth":["kratos1fhqjhs22s4cwvjxrvlcyst3h4pvw7x49jvk0ux"],"from":"","to":"","amount":[],"router":"account","action":"updateauth","data":"L2N5QbEKEwoRAQEEBDDhAAAAAAAAAAAAAAASFGNKvkbeXeCYw7nKG0NH7crAl8Km"}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"200000","payer":"acc1"},"signatures":null,"memo":"Sent via kratos"}}
```

### Assets related interface

*Note that this is only constructing a transaction in json, not sending a transaction*

##### 1.  Constructing a transaction of transfer

Query path: /assets/transfer (POST)

Query example:

```bash
curl --request POST --url http://localhost:1317/assets/transfer \
     --header 'accept: application/json' \
     --data '{"base_req":{"memo":"Sent via kratos","chain_id":"testing","account_number":"0","sequence":"1","gas":"200000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}]},"from":"validator","to":"acc1","amount":"10000kratos/kts"}'
```

Return example:

```bash
{"type":"kratos/Tx","value":{"msg":[{"type":"kratos/msg","value":{"auth":["kratos1fg3svnqcv9wcln0q5cwdcm0uk8a4hwqvng26sh"],"from":"validator","to":"acc1","amount":[{"denom":"kratos/kts","amount":"10000"}],"router":"asset","action":""}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"200000","payer":""},"signatures":null,"memo":"Sent via kratos"}}
```

##### 2.  Constructing a transaction of creating a coin

Query path: /assets/create (POST)

Query example:

```bash
curl --request POST --url http://localhost:1317/assets/create \
     --header 'accept: application/json' \
     --data '{"base_req":{"chain_id":"testing","memo":"send via kratos","gas":"2000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}]},"creator":"acc1","symbol":"eosc","max_supply":"1000000000acc1/eosc","can_issue":"1","can_lock":"1","issue_to_height":"10000","init_supply":"100000acc1/eosc"}'
```

Return example:

```bash
{"type":"kratos/Tx","value":{"msg":[{"type":"asset/create","value":{"KuMsg":{"auth":["kratos1fhqjhs22s4cwvjxrvlcyst3h4pvw7x49jvk0ux"],"from":"","to":"","amount":[],"router":"asset","action":"create","data":"Y9xELf4KEwoRAQEEFPTDAAAAAAAAAAAAAAASEwoRAQEEBDDhAAAAAAAAAAAAAAAaFwoJYWNjMS9lb3NjEgoxMDAwMDAwMDAwIAEoATCQTjoTCglhY2MxL2Vvc2MSBjEwMDAwMA=="}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"2000","payer":""},"signatures":null,"memo":"send via kratos"}}
```

##### 3.  Constructing a transaction of issuing a coin

Query path: /assets/issue (POST)

Query example:

```bash
curl --request POST --url http://localhost:1317/assets/issue \
     --header 'accept: application/json' \
     --data '{"base_req":{"memo":"Sent via kratos","chain_id":"testing","account_number":"0","sequence":"1","gas":"200000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}]},"creator":"validator","symbol":"btc","amount":"100000validator/btc"}'
```

Return example:

```bash
{"type":"kratos/Tx","value":{"msg":[{"type":"asset/issue","value":{"KuMsg":{"auth":["kratos1fg3svnqcv9wcln0q5cwdcm0uk8a4hwqvng26sh"],"from":"","to":"","amount":[],"router":"asset","action":"issue","data":"R0gpOa0KEwoRAQEDCUDAAAAAAAAAAAAAAAASEwoRAQEJWBMJEBUPSAAAAAAAAAAaFwoNdmFsaWRhdG9yL2J0YxIGMTAwMDAw"}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"200000","payer":""},"signatures":null,"memo":"Sent via kratos"}}
```

##### 4.  Constructing a transaction of locking

Query path: /assets/lock (POST)

Query example: 

```bash
curl --request POST --url http://localhost:1317/assets/lock \
     --header 'accept: application/json' \
     --data '{"base_req":{"memo":"Sent via kratos","chain_id":"testing","account_number":"0","sequence":"1","gas":"200000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}]},"account":"validator","unlock_block_height":"10000","amount":"100000validator/btc"}'
```

Return example:

```bash
{"type":"kratos/Tx","value":{"msg":[{"type":"asset/lock","value":{"KuMsg":{"auth":["kratos1fg3svnqcv9wcln0q5cwdcm0uk8a4hwqvng26sh"],"from":"","to":"","amount":[],"router":"asset","action":"coin.lock","data":"NZ9hO5gKEwoRAQEJWBMJEBUPSAAAAAAAAAASFwoNdmFsaWRhdG9yL2J0YxIGMTAwMDAwGJBO"}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"200000","payer":""},"signatures":null,"memo":"Sent via kratos"}}```
```

##### 5.  Constructing a transaction of unlocking

Query path: /assets/unlock (POST)

Query example: 

```bash
curl --request POST --url http://localhost:1317/assets/unlock \
     --header 'accept: application/json' \
     --data '{"base_req":{"memo":"Sent via kratos","chain_id":"testing","account_number":"0","sequence":"1","gas":"200000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}]},"account":"validator","amount":"100000validator/btc"}'
```

Return example:

```bash
{"type":"kratos/Tx","value":{"msg":[{"type":"asset/unlock","value":{"KuMsg":{"auth":["kratos1fg3svnqcv9wcln0q5cwdcm0uk8a4hwqvng26sh"],"from":"","to":"","amount":[],"router":"asset","action":"coin.unlock","data":"MmA3dc0KEwoRAQEJWBMJEBUPSAAAAAAAAAASFwoNdmFsaWRhdG9yL2J0YxIGMTAwMDAw"}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"200000","payer":""},"signatures":null,"memo":"Sent via kratos"}}```
```

### Staking related interface

*Note that this is only constructing a transaction in json, not sending a transaction*

##### 1.  Constructing a transaction of delegation

Query path: /staking/delegators/delegations  (POST)

Query example:

```bash
curl --request POST --url http://localhost:1317/staking/delegations \
     --header 'accept: application/json' \
     --data '{"base_req":{"memo":"Sent via kratos","chain_id":"testing","account_number":"0","sequence":"1","gas":"200000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}]},"delegator_acc":"validator","validator_acc":"validator","amount":"50000kratos/kts"}'
```

Return example:

```bash
{"type":"kratos/Tx","value":{"msg":[{"type":"kratos/KuMsgDelegate","value":{"KuMsg":{"auth":["kratos1fg3svnqcv9wcln0q5cwdcm0uk8a4hwqvng26sh"],"from":"validator","to":"kustaking","amount":[{"denom":"kratos/kts","amount":"50000"}],"router":"kustaking","action":"delegate","data":"Pu+AYjUKEwoRAQEJWBMJEBUPSAAAAAAAAAASEwoRAQEJWBMJEBUPSAAAAAAAAAAaDgoFc3Rha2USBTUwMDAw"}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"200000","payer":""},"signatures":null,"memo":"Sent via kratos"}}
```

##### 2.  Constructing a transaction of unbonding delegation

Query path: /staking/unbonding_delegations  (POST)

Query example:

```bash
curl --request POST --url http://localhost:1317/staking/unbonding_delegations \
     --header 'accept: application/json' \
     --data '{"base_req":{"memo":"Sent via kratos","chain_id":"testing","account_number":"0","sequence":"1","gas":"200000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}]},"delegator_acc":"validator","validator_acc":"validator","amount":"50000kratos/kts"}'
```

Return example:

```bash
{"type":"kratos/Tx","value":{"msg":[{"type":"kratos/KuMsgUnbond","value":{"KuMsg":{"auth":["kratos1fg3svnqcv9wcln0q5cwdcm0uk8a4hwqvng26sh"],"from":"","to":"","amount":[],"router":"kustaking","action":"beginunbonding","data":"PosAJVwKEwoRAQEJWBMJEBUPSAAAAAAAAAASEwoRAQEJWBMJEBUPSAAAAAAAAAAaDgoFc3Rha2USBTUwMDAw"}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"200000","payer":""},"signatures":null,"memo":"Sent via kratos"}}
```

##### 3.  Constructing a transaction of redelegation

Query path: /staking/redelegations (POST)

Query example:

```bash
curl --request POST --url http://localhost:1317/staking/redelegations \
     --header 'accept: application/json' \
     --data '{"base_req":{"memo":"Sent via kratos","chain_id":"testing","account_number":"0","sequence":"1","gas":"200000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}]},"delegator_acc":"validator","validator_src_acc":"validator","validator_dst_acc":"validator","amount":"50000kratos/kts"}'
```

Return example:

```bash
{"type":"kratos/Tx","value":{"msg":[{"type":"kratos/KuMsgRedelegate","value":{"KuMsg":{"auth":["kratos1fg3svnqcv9wcln0q5cwdcm0uk8a4hwqvng26sh"],"from":"","to":"","amount":[],"router":"kustaking","action":"beginredelegate","data":"U3g38I0KEwoRAQEJWBMJEBUPSAAAAAAAAAASEwoRAQEJWBMJEBUPSAAAAAAAAAAaEwoRAQEJWBMJEBUPSAAAAAAAAAAiDgoFc3Rha2USBTUwMDAw"}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"200000","payer":""},"signatures":null,"memo":"Sent via kratos"}}
```

### Governance related interface

*Note that this is only constructing a transaction in json, not sending a transaction*

##### 1.  Constructing a transaction of proposing a text proposal

Query path: /gov/proposals  (POST)

Query example:

```bash
curl --request POST --url http://localhost:1317/gov/proposals \
     --header 'accept: application/json' \
     --data '{"base_req":{"memo":"Sent via kratos","chain_id":"testing","account_number":"0","sequence":"1","gas":"200000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}]},"title":"title","description":"des","proposal_type":"Text","initial_deposit":"50000kratos/kts","proposer_acc":"validator"}'
```

返回示例:

```
{"type":"kratos/Tx","value":{"msg":[{"type":"kratos/kuMsgSubmitProposal","value":{"KuMsg":{"auth":["kratos1fg3svnqcv9wcln0q5cwdcm0uk8a4hwqvng26sh"],"from":"validator","to":"kugov","amount":[{"denom":"kratos/kts","amount":"50000"}],"router":"kugov","action":"submit_proposal","data":"KRKS/B4KDgoFc3Rha2USBTUwMDAwEhMKEQEBCVgTCRAVD0gAAAAAAAAA"},"content":{"type":"kratos/TextProposal","value":{"title":"title","description":"des"}}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"200000","payer":""},"signatures":null,"memo":"Sent via kratos"}}
```

##### 2.  Constructing a transaction of depositing for a proposal

Query path: /gov/deposits  (POST)

Query example:

```bash
curl --request POST --url http://localhost:1317/gov/deposits \
     --header 'accept: application/json' \
     --data '{"base_req":{"memo":"Sent via kratos","chain_id":"testing","account_number":"0","sequence":"1","gas":"200000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}]},"proposal_id":"1","depositor":"validator","amount":"1000kratos/kts"}'
```

返回示例:

```
{"type":"kratos/Tx","value":{"msg":[{"type":"kratos/kuMsgDeposit","value":{"KuMsg":{"auth":["kratos1fg3svnqcv9wcln0q5cwdcm0uk8a4hwqvng26sh"],"from":"validator","to":"kugov","amount":[{"denom":"kratos/kts","amount":"1000"}],"router":"kugov","action":"deposit","data":"Kpusx9EIARITChEBAQlYEwkQFQ9IAAAAAAAAABoNCgVzdGFrZRIEMTAwMA=="}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"200000","payer":""},"signatures":null,"memo":"Sent via kratos"}}
```

##### 3.  Constructing a transaction of voting for a proposal

Query path: /gov/votes  (POST)

Query example:

```bash
curl --request POST --url http://localhost:1317/gov/votes \
     --header 'accept: application/json' \
     --data '{"base_req":{"memo":"Sent via kratos","chain_id":"testing","account_number":"0","sequence":"1","gas":"200000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}]},"proposal_id":"1","voter":"validator","option":"yes"}'
```

返回示例:

```
{"type":"kratos/Tx","value":{"msg":[{"type":"kratos/kuMsgVote","value":{"KuMsg":{"auth":["kratos1fg3svnqcv9wcln0q5cwdcm0uk8a4hwqvng26sh"],"from":"","to":"","amount":[],"router":"kugov","action":"vote","data":"HYVlETMIARITChEBAQlYEwkQFQ9IAAAAAAAAABgB"}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"200000","payer":""},"signatures":null,"memo":"Sent via kratos"}}
```

##### 4.  Constructing a transaction of proposing a param-change proposal

Query path: /gov/proposals/param_change  (POST)

Query example:

```bash
curl --request POST --url http://localhost:1317/gov/proposals/param_change \
     --header 'accept: application/json' \
     --data '{"base_req":{"memo":"Sent via kratos","chain_id":"testing","account_number":"0","sequence":"1","gas":"200000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}]},"title":"test title","description":"test","initial_deposit":"100000kratos/kts","proposer_acc":"acc1","param_changes":[{"subspace":"voting_params","key":"voting_period","value":"12000000000"}]}'
```

返回示例:

```
{"type":"kratos/Tx","value":{"msg":[{"type":"kratos/kuMsgSubmitProposal","value":{"KuMsg":{"auth":["kratos1fhqjhs22s4cwvjxrvlcyst3h4pvw7x49jvk0ux"],"from":"acc1","to":"kugov","amount":[{"denom":"kratos/kts","amount":"100000"}],"router":"kugov","action":"submit_proposal","data":"MBKS/B4KFQoLa3VjaGFpbi9rY3MSBjEwMDAwMBITChEBAQQEMOEAAAAAAAAAAAAAAA=="},"content":{"type":"kratos/ParameterChangeProposal","value":{"title":"test title","description":"test","changes":[{"subspace":"voting_params","key":"voting_period","value":"12000000000"}]}}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"200000","payer":""},"signatures":null,"memo":"Sent via kratos"}}
```


### Distribution related interface

*Note that this is only constructing a transaction in json, not sending a transaction*

##### 1.  Constructing a transaction of rewarding all assets

Query path: /distribution/delegators/rewards  (POST)

Query example:

```bash
curl --request POST --url http://localhost:1317/distribution/delegators/rewards \
     --header 'accept: application/json' \
     --data '{"base_req":{"memo":"Sent via kratos","chain_id":"testing","account_number":"0","sequence":"1","gas":"200000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}]},"delegator_acc":"validator"}'
```

返回示例:

```bash
{"type":"kratos/Tx","value":{"msg":[{"type":"kratos/MsgWithdrawDelegationReward","value":{"KuMsg":{"auth":["kratos1fg3svnqcv9wcln0q5cwdcm0uk8a4hwqvng26sh"],"from":"","to":"","amount":[],"router":"kudistribution","action":"withdrawdelreward","data":"LlNLSWQKEwoRAQEJWBMJEBUPSAAAAAAAAAASEwoRAQEJWBMJEBUPSAAAAAAAAAA="}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"200000","payer":""},"signatures":null,"memo":"Sent via kratos"}}
```

##### 2.  Constructing a transaction of rewarding for specified validator

Query path: /distribution/delegators_validator/rewards  (POST)

Query example:

```bash
curl --request POST --url http://localhost:1317/distribution/delegators_validator/rewards \
     --header 'accept: application/json' \
     --data '{"base_req":{"memo":"Sent via kratos","chain_id":"testing","account_number":"0","sequence":"1","gas":"200000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}]},"delegator_acc":"validator","validator_acc":"validator"}'
```

返回示例:

```bash
{"type":"kratos/Tx","value":{"msg":[{"type":"kratos/MsgWithdrawDelegationReward","value":{"KuMsg":{"auth":["kratos1fg3svnqcv9wcln0q5cwdcm0uk8a4hwqvng26sh"],"from":"","to":"","amount":[],"router":"kudistribution","action":"withdrawdelreward","data":"LlNLSWQKEwoRAQEJWBMJEBUPSAAAAAAAAAASEwoRAQEJWBMJEBUPSAAAAAAAAAA="}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"200000","payer":""},"signatures":null,"memo":"Sent via kratos"}}
```

##### 3.  Constructing a transaction of setting withdraw account

Query path: /distribution/delegators/withdraw_account  (POST)

Query example:

```bash
curl --request POST --url http://localhost:1317/distribution/delegators/withdraw_account \
     --header 'accept: application/json' \
     --data '{"base_req":{"memo":"Sent via kratos","chain_id":"testing","account_number":"0","sequence":"1","gas":"200000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}]},"delegator_acc":"validator","withdraw_acc":"kratos14jcnt278qwuyx2kk6nqw0xluwgy8exahrjrkav"}'
```

返回示例:

```bash
{"type":"kratos/Tx","value":{"msg":[{"type":"kratos/MsgSetWithdrawAccountId","value":{"KuMsg":{"auth":["kratos1fg3svnqcv9wcln0q5cwdcm0uk8a4hwqvng26sh"],"from":"","to":"","amount":[],"router":"kudistribution","action":"withdrawcccid","data":"MiaJ7JsKEwoRAQEJWBMJEBUPSAAAAAAAAAASFwoVAqyxNavHA7hDKtbUwOeb/HIIfJu3"}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"200000","payer":""},"signatures":null,"memo":"Sent via kratos"}}
```

##### 4.  Constructing a transaction of validator rewarding

Query path: /distribution/validators/rewards  (POST)

Query example:

```bash
curl --request POST --url http://localhost:1317/distribution/validators/rewards \
     --header 'accept: application/json' \
     --data '{"base_req":{"memo":"Sent via kratos","chain_id":"testing","account_number":"0","sequence":"1","gas":"200000","gas_adjustment":"1.2","fees":[{"denom":"kratos/kts","amount":"50"}]},"validator_acc":"validator"}'
```

返回示例:

```bash
{"type":"kratos/Tx","value":{"msg":[{"type":"kratos/MsgWithdrawDelegationReward","value":{"KuMsg":{"auth":["kratos1fg3svnqcv9wcln0q5cwdcm0uk8a4hwqvng26sh"],"from":"","to":"","amount":[],"router":"kudistribution","action":"withdrawdelreward","data":"LlNLSWQKEwoRAQEJWBMJEBUPSAAAAAAAAAASEwoRAQEJWBMJEBUPSAAAAAAAAAA="}}}],"fee":{"amount":[{"denom":"kratos/kts","amount":"50"}],"gas":"200000","payer":""},"signatures":null,"memo":"Sent via kratos"}}
```
