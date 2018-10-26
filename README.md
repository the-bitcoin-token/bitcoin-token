# Bitcoin Token

BitcoinToken is a toolkit for building blockchain enabled web applications on top of Bitcoin Cash.

It consists of three tools: a wallet, a database built on the blockchain, and a token solution. These can be combined by the developer to adapt to the application.

More information is at <a href="http://www.bitcointoken.com">www.bitcointoken.com</a> and <a href="http://www.bitcointoken.com/docs">www.bitcointoken.com/docs</a>.

<b>Note.</b> BitcoinToken is in alpha stage and contains known and unknown bugs. The wallet and the database are pretty stable, but the token solution still needs more work. DO NOT USE IN PRODUCTION YET.


## Install

To install BitcoinToken run

```
npm i bitcointoken
```

To use the library, you also need to install and run a [Bitcoin non-standard server](https://github.com/the-bitcoin-token/bitcoin-non-standard-server) in a separate terminal window. If you have [Docker](https://www.docker.com/) and [Docker Compose](https://docs.docker.com/compose/) installed run

```
  git clone https://github.com/BitcoinDB/bitcoin-non-standard-server.git
  cd bitcoin-non-standard-server/
  docker-compose up
```

Otherwise follow the instruction in the [Github repo](https://github.com/the-bitcoin-token/bitcoin-non-standard-server).

## Example usage

In this example you will create a Bitcoin Cash user account and a wallet, fund your wallet, send some Bitcoin, store data on the blockchain, and issue a token.

### Send money using BitcoinWallet

Create a new project using ```npm init``` and run ```npm i bitcointoken``` to install the software. The first thing you need is a Bitcoin Cash user account in the form of a hd private key. To have BitcoinToken generate one for you, create a file called ```index.js``` with the following code

```
const Bitcoin = require('bitcointoken')

const hdPrivatekey = Bitcoin.Wallet.getHdPrivateKey()
console.log('hdPrivatekey',  hdPrivatekey)
```

Then run ```node --experimental-repl-await index.js``` to execute the code. If you installed BitcoinToken correctly you should see output similar to this.

```
hdPrivateKey tprv8ZgxMBicQKsPfAaN1AC1WmjTNdapPu7QNQ4z3e9XZasXoeXxVQnAzP7pNxJp9aaqoYxvwRXnfT1LqqNLno1Noq2a6gAGWzDUKsGWWnxzM9Z
```

If you run the code multiple times you will see that it generates a different key every time you run the file. You can pick one of these keys as your hd private key.

To create a BitcoinWallet from your hd private key change the contents of <code>index.js</code> to this code but paste in your hd private key.
```
const Bitcoin = require('bitcointoken')

const wallet = Bitcoin.Wallet.fromHdPrivateKey('tprv8ZgxMBicQKsPfAaN1AC1WmjTNdapPu7QNQ4z3e9XZasXoeXxVQnAzP7pNxJp9aaqoYxvwRXnfT1LqqNLno1Noq2a6gAGWzDUKsGWWnxzM9Z')

const address = wallet.getAddress()
console.log('deposit address', address)
```

The output is your BitcoinCash deposit address.

```
deposit address mgMdp3kjjfTJTEVMqA3XxNytFa4GC88E3Z
```

 The wallet is generated deterministically from your hd private key, so you will see the same output if you run the file multiple times.

Before you can use the wallet, you need to fund it. Copy the address and paste it into a <a href="">Bitcoin Cash Testnet Faucet</a>to send testnet coins to it.

To check your balance and send some bitcoin change <code>index.js</code> to contain the following code. It generates a second wallet from random and sends 10000 satoshis to it.

```
const Bitcoin = require('bitcointoken')

const wallet = Bitcoin.Wallet.fromHdPrivateKey('tprv8ZgxMBicQKsPfAaN1AC1WmjTNdapPu7QNQ4z3e9XZasXoeXxVQnAzP7pNxJp9aaqoYxvwRXnfT1LqqNLno1Noq2a6gAGWzDUKsGWWnxzM9Z')

const friendsWallet = new Bitcoin.Wallet()
const friendsAddress = friendsWallet.getAddress()

;(async function() {
  const balance = await wallet.getBalance()
  console.log('balance', balance)

  const res = await wallet.send(10000, friendsAddress)
  console.log('txId', res.txid)
})()
```

The output will be your balance and the transaction id of the transaction that was broadcast. You can see that the transaction was broadcast by pasting the transaction id into a <a href="https://www.blocktrail.com/tBCC">block explorer</a>.

```
balance 780000
txId b61505ed4384f4273ca9f09db072028a1ee009a80d2af9bbd969fcc9ff7df2ad
```

### Store data using BitcoinDb

You can use your BitcoinWallet object to generate a BitcoinDb object from it. You can then use the BitcoinDb object to store data on the blockchain. Bitcoin Db will encode your data into a Bitcoin Cash transaction and broadcast it to the blockchain using BitcoinWallet. Data stored on the blockchain propagates around the globe in seconds. So data stored in BitcoinDb will be visible to all users almost instantly.

```
const Bitcoin = require('bitcointoken')

const wallet = Bitcoin.Wallet.fromHdPrivateKey('tprv8ZgxMBicQKsPfAaN1AC1WmjTNdapPu7QNQ4z3e9XZasXoeXxVQnAzP7pNxJp9aaqoYxvwRXnfT1LqqNLno1Noq2a6gAGWzDUKsGWWnxzM9Z')
const db = new Bitcoin.Db(wallet)

;(async function() {
  const id = await db.put({hello: 'world'})
  console.log('id', id)
  
  const res = await db.get(id)
  console.log('res', res)
})()
```

### Issue a token using BitcoinToken

You can use BitcoinToken to issue, send, and receive token. You can generate a BitcoinToken object from a BitcoinDb object by passing it into the constructor. To issue a token call <code>token.create()</code>. To send the token to a friend call <code>token.send()</code>.


```
const Bitcoin = require('bitcointoken')

const wallet = Bitcoin.Wallet.fromHdPrivateKey('tprv8ZgxMBicQKsPfAaN1AC1WmjTNdapPu7QNQ4z3e9XZasXoeXxVQnAzP7pNxJp9aaqoYxvwRXnfT1LqqNLno1Noq2a6gAGWzDUKsGWWnxzM9Z')
const db = new Bitcoin.Db(wallet)
const token = new Bitcoin.Token(db)

const friendsWallet = new Bitcoin.Wallet()
const friendsPublicKey = friendsWallet.getPublicKey()

;(async function() {
  const tokenId = await token.create({
    balance: '10',
    name: 'my-token',
    url: 'www.mytoken.com',
  })
  console.log('tokenId', tokenId)
  
  const res = await token.send(1, friendsPublicKey)
  console.log('res', res)

  const balance = await token.getBalance()
  console.log('balance', balance)
})()

```

You can now build blockchain enabled apps. See the <a href="http://www.bitcointoken.com/docs">BitcoinToken Docs</a> for more information about BitcoinToken.
