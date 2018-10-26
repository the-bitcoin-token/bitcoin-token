# Bitcoin Token

BitcoinToken is a toolkit for building blockchain enabled web applications on top of Bitcoin Cash. It consists of three tools: a wallet, a database built on the blockchain, and a token solution. These can be combined by the developer to adapt to the application. BitcoinToken runs in the browser and in node.

More information is at <a href="http://www.bitcointoken.com">www.bitcointoken.com</a> and the documentation is at <a href="http://www.bitcointoken.com/docs">www.bitcointoken.com/docs</a>.


## Install

To install BitcoinToken run

```
npm i bitcointoken
```

To test the library, you also need to install and run a [Bitcoin non-standard server](https://github.com/the-bitcoin-token/bitcoin-non-standard-server) in a separate terminal window.


## Example usage

You can use BitcoinToken to issue a tokens in Javascript. You can find more examples in the <a href="http://www.bitcointoken.com/docs">BitcoinToken docs</a>.

Create a new project using ```npm init``` to create a ```package.json``` file. Run ```npm i bitcointoken```. Then create a file ```index.js``` with the following code. The code creates a new hd private key, initialize an BitcoinWallet from it, and logs the hd private key and the deposit address.

```
const Bitcoin = require('bitcointoken')

const BitcoinWallet = Bitcoin.Wallet
const hdPrivateKey = BitcoinWallet.getHdPrivateKey()
console.log(`hdPrivateKey: ${hdPrivateKey}`)

const wallet = BitcoinWallet.fromHdPrivateKey(hdPrivateKey)
const address = wallet.getAddress()
console.log(`deposit address: ${address}`)
```

Then run ```node --experimental-repl-await index.js``` to execute the code. You should see an output similar to this:

```
hdPrivateKey: tprv8ZgxMBicQKsPfAaN1AC1WmjTNdapPu7QNQ4z3e9XZasXoeXxVQnAzP7pNxJp9aaqoYxvwRXnfT1LqqNLno1Noq2a6gAGWzDUKsGWWnxzM9Z
deposit address: mgMdp3kjjfTJTEVMqA3XxNytFa4GC88E3Z
```

To use the wallet, you need to fund it first. Copy the address and paste it into a <a href="">Bitcoin Cash Testnet Faucet</a>. The wallet you just generated is now funded.

If you run the same code again you will generate a differnt wallet and not the one you funded (try it, it will log a different address every time). To generate the same wallet again change ```index.js``` to generate the wallet you just funded.

```
const Bitcoin = require('bitcointoken')
const BitcoinWallet = Bitcoin.Wallet
const wallet = BitcoinWallet.fromHdPrivateKey('tprv8ZgxMBicQKsPfAaN1AC1WmjTNdapPu7QNQ4z3e9XZasXoeXxVQnAzP7pNxJp9aaqoYxvwRXnfT1LqqNLno1Noq2a6gAGWzDUKsGWWnxzM9Z')

console.log(`deposit address: ${wallet.getAddress()}`)
```

Run the file several times, you will see that the same address is logged every time. You can now use your wallet to generate a BitcoinDb and a BitcoinToken. Change ```index.js``` to

```
const Bitcoin = require('bitcointoken')

const BitcoinWallet = Bitcoin.Wallet
const wallet = BitcoinWallet.fromHdPrivateKey('tprv8ZgxMBicQKsPfAaN1AC1WmjTNdapPu7QNQ4z3e9XZasXoeXxVQnAzP7pNxJp9aaqoYxvwRXnfT1LqqNLno1Noq2a6gAGWzDUKsGWWnxzM9Z')
const address = wallet.getAddress()

const BitcoinDb = Bitcoin.Db
const db = new BitcoinDb(wallet)

;(async function() {
  const id = await db.put({hello: 'world'})
  console.log(id)
  
  const data = await db.get(id)
  console.log(data)
})()
```

You can now build blockchain enabled apps. See the <a href="http://www.bitcointoken.com/docs">BitcoinToken Docs</a> for more details.
