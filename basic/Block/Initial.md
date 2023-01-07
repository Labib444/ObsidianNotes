```javascript
const SHA256 = require('crypto-js/sha256');

class Block
{
	constructor(index, timestamp, data, previousHash = '')
	{
		this.index = index;
		this.timestamp = timestamp;
		this.data = data;
		this.previousHash = previousHash;
		this.hash = this.calculateHash();
	}

	calculateHash()
	{
		return SHA256(this.index + this.previousHash + this.timestamp + JSON.stringify(this.data)).toString();
	}
}

class Blockchain
{
	constructor()
	{
		this.chain = [this.createGenesisBlock()];
	}

	createGenesisBlock()
	{
		return new Block(0, "01/01/2017", "Genesis block", "0");
	}

	getLatestBlock()
	{
		return this.chain[this.chain.length - 1];
	}

	addBlock(newBlock)
	{
		newBlock.previousHash = this.getLatestBlock().hash;
		newBlock.hash = newBlock.calculateHash();
		this.chain.push(newBlock);
	}
	
	isChainValid()
	{
		for(let i = 1; i < this.chain.length; i++)
		{
			const currentBlock = this.chain[i];
			const previousBlock = this.chain[i - 1];

			if(currentBlock.hash !== currentBlock.calculateHash())
			{
				return false;
			}

			if(currentBlock.previousHash !== previousBlock.hash)
			{
				return false;
			}
		}
		
		return true;
	}

}

let savjeeCoin = new Blockchain();
savjeeCoin.addBlock(new Block(1, "10/07/2017", { amount: 4 }));
savjeeCoin.addBlock(new Block(2, "10/07/2017", { amount: 4 }));

console.log('Is blockchain valid?' + savjeeCoin.isChainValid());
console.log(JSON.stringify(savjeeCoin, null, 4))

savjeeCoin.chain[1].data = { amount: 100 };
console.log('Is blockchain valid?' + savjeeCoin.isChainValid());

//Lets be clever
savjeeCoin.chain[1].data = { amount: 100 };
savjeeCoin.chain[1].hash = savjeeCoin.chain[1].calculateHash();
console.log('Is blockchain valid?' + savjeeCoin.isChainValid());

//Blocks can be added but never changed else it will invalidate the blockchain.
//If any block has invalidated the chain then you have to rollback the changes.
```

- Index means: Where the block is
- timestamp means: When the block was created
- data means: data, transaction data, money amount, sender, receiver
- previousHash means: the hash of the previous block

```npm
npm install --save crypto-js
```

- First block in a block chain is called jenesis block

```cli
node main.js
```

### Output of BlockChain 

![[Pasted image 20230107160337.png]]

- Proof of Work
- Peer to Peer Connection with other miners
- No validation if you have the funds to transact

### Proof of Work

![[Recording 20230107162722.webm]]

```javascript
const SHA256 = require('crypto-js/sha256');

class Block
{
	constructor(index, timestamp, data, previousHash = '')
	{
		this.index = index;
		this.timestamp = timestamp;
		this.data = data;
		this.previousHash = previousHash;
		this.hash = this.calculateHash();
		this.nonce = 0; 
		//This(nonce) is required to create new blocks because we cannot change 
		//other values(index, timestamp, data etc) to generate new hash values.
	}

	calculateHash()
	{
		return SHA256(this.index + this.previousHash + this.timestamp + JSON.stringify(this.data) + this.nonce).toString();
	}

	mineBlock(difficulty)
	{
		while(this.hash.substring(0, difficulty) !== Array(difficulty + 1).join("0") )
		{
			this.nonce++;
			this.hash = this.calculateHash();	
		}
		console.log("Block mined: " + this.hash);
	}
}

class Blockchain
{
	constructor()
	{
		this.chain = [this.createGenesisBlock()];
		this.difficulty = 2;
	}

	createGenesisBlock()
	{
		return new Block(0, "01/01/2017", "Genesis block", "0");
	}

	getLatestBlock()
	{
		return this.chain[this.chain.length - 1];
	}

	addBlock(newBlock)
	{
		newBlock.previousHash = this.getLatestBlock().hash;
		//newBlock.hash = newBlock.calculateHash();
		newBlock.mineBlock(this.difficulty);
		this.chain.push(newBlock);
	}
	
	isChainValid()
	{
		for(let i = 1; i < this.chain.length; i++)
		{
			const currentBlock = this.chain[i];
			const previousBlock = this.chain[i - 1];

			if(currentBlock.hash !== currentBlock.calculateHash())
			{
				return false;
			}

			if(currentBlock.previousHash !== previousBlock.hash)
			{
				return false;
			}
		}
		
		return true;
	}

}

let savjeeCoin = new Blockchain();

console.log('Mining block 1...')
savjeeCoin.addBlock(new Block(1, "10/07/2017", { amount: 4 }));

console.log('Mining block 2...')
savjeeCoin.addBlock(new Block(2, "10/07/2017", { amount: 8 }));
```

**Output:**
![[Pasted image 20230107163530.png]]
- Increase difficulty which will take more time to create new blocks

### Mining rewards and transactions

```javascript
const SHA256 = require('crypto-js/sha256');

class Transaction
{
	constructor(fromAddress, toAddress, amount)
	{
		this.fromAddress = fromAddress;
		this.toAddress = toAddress;
		this.amount = amount;
	}
}

class Block
{
	constructor(timestamp, transactions, previousHash = '')
	{
		this.timestamp = timestamp;
		this.transactions = transactions;
		this.previousHash = previousHash;
		this.hash = this.calculateHash();
		this.nonce = 0; 
		//This(nonce) is required to create new blocks because we cannot change 
		//other values(index, timestamp, data etc) to generate new hash values.
	}

	calculateHash()
	{
		return SHA256(this.previousHash + this.timestamp + JSON.stringify(this.data) + this.nonce).toString();
	}

	mineBlock(difficulty)
	{
		while(this.hash.substring(0, difficulty) !== Array(difficulty + 1).join("0") )
		{
			this.nonce++;
			this.hash = this.calculateHash();	
		}
		console.log("Block mined: " + this.hash);
	}
}

class Blockchain
{
	constructor()
	{
		this.chain = [this.createGenesisBlock()];
		this.difficulty = 2;
		this.pendingTransactions = [];
		this.miningReward = 100;
	}

	createGenesisBlock()
	{
		return new Block("01/01/2017", "Genesis block", "0");
	}

	getLatestBlock()
	{
		return this.chain[this.chain.length - 1];
	}

	minePendingTransactions(miningRewardAddress)
	{
		//block cannot exceed more than 1MB
		//miners choose which pending transaction will  they keep
		//Every 10 minutes a new block is created in bitcoin.
		//Within that interval transactions which haven't been added
		//to the block becomes pending transaction.
		let block = new Block(Date.now(), this.pendingTransaction);
		block.mineBlock(this.difficulty);
		console.log('Block successfully mined!');
		this.chain.push(block);

		//Reset the pending transactions and create the new transaction which 
		//is for rewarding the miner.
		this.pendingTransaction = [
			new Transaction(null, miningRewardAddress, this.miningReward);
		];
	}

	createTransaction(transaction)
	{
		this.pendingTransactions.push(transaction);
	}

	getBalanceOfAddress(address)
	{
		let balance = 0;
		for( const block of this.chain )
		{
			for( const trans of block.transactions )
			{
				if(trans.fromAddress === address)
				{
					balance -= trans.amount;
				}

				if(trans.toAddress === address)
				{
					balance += trans.amount;
				}
			}
		}
	}

	isChainValid()
	{
		for(let i = 1; i < this.chain.length; i++)
		{
			const currentBlock = this.chain[i];
			const previousBlock = this.chain[i - 1];

			if(currentBlock.hash !== currentBlock.calculateHash())
			{
				return false;
			}

			if(currentBlock.previousHash !== previousBlock.hash)
			{
				return false;
			}
		}
		
		return true;
	}

}

let savjeeCoin = new Blockchain();
savjeeCoin.createTransaction(new Transaction('address1','address2', 100));
savjeeCoin.createTransaction(new Transaction('address2','address1', 50));

console.log('\n Starting the miner...')
savjeeCoin.minePendingTransactions('xaviers-address');
console.log('\nBalance of xavier is', savjeeCoin.getBalanceOfAddress('xaviers-address'))

console.log('\n Starting the miner again...')
savjeeCoin.minePendingTransactions('xaviers-address');
console.log('\nBalance of xavier is', savjeeCoin.getBalanceOfAddress('xaviers-address'))


```

### Signing Transactions

