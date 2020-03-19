### usage


## Tellor(TRB) stratum protocol

### mining.subscribe

- params: ["agent", null]
- result: [null, "nonce prefix", "nonce2 size"]
```json
request:
{
	"id": 1,
	"method": "mining.subscribe",
	"params": ["TRBminer-v1.0.0", null]
}

response:
{
	"id": 1,
	"result": [null, "537c4a39", 8],
	"error": null
}
```
nonce1 is first part of the block header nonce (in str).

By protocol, nonce is less 32 byte string. The miner will pick nonce2 str



### mining.authorize

- params: ["username", "password"]
- result: true
```json
{
	"id": 2,
	"method": "mining.authorize",
	"params": ["24b4Cf63E98B4649A96A2A488D731ACc30E3E746.worker1", "x"]
} 

{"id":2,"result":true,"error":null}
```


### mining.notify

- params: ["jobId", "challenge hash", pool eth address, diff, cleanJob]
```json
{
	"id": null,
	"method": "mining.notify",
	"params": ["1576906090_45", "73869a849914249f8a1a2e0c627659f2e5f42afd6649f504d2dcb471c659a85c",
	"34b4Cf63E98B4649A96A2A488D731ACc30E3E746" 2.9152603e+07, false]
}

```

### mining.submit
- params: [ "username", "jobId", "nonce2" ]
- result: true / false
```json
{
	"id": 102,
	"method": "mining.submit",
	"params": ["24b4Cf63E98B4649A96A2A488D731ACc30E3E746.worker1", "1576906090_45", "0102030405060708"]
}

{"id":102,"result":true,"error":null}    // accepted share response
{"id":102,"result":false,"error":[22,"mined",null]}  // rejected share response
```

nonce2 is the second part of the nonce.






```json
In this example

nonce = str2bin(nonce1) + str2bin(nonce2) = 3533376334613339 + 30313032303330343035303630373038

challengeHash = 73869a849914249f8a1a2e0c627659f2e5f42afd6649f504d2dcb471c659a85c   (32 bytes without nonce)

headerHashWithNonce = challengeHash + pool addr + nonce = 73869a849914249f8a1a2e0c627659f2e5f42afd6649f504d2dcb471c659a85c + 34b4Cf63E98B4649A96A2A488D731ACc30E3E746 + 353337633461333930313032303330343035303630373038 

resultHash = sha256(ripemd160(sha3(headerHashWithNonce)))

result = resultHash % 2.9152603e+07

```
