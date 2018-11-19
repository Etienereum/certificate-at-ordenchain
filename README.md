# certificate-at-ordenchain
//Issuing Certificates which are blockchain based for easy verification of valid documentation

const sha256 = require('sha256')

class Block {
	constructor (index, timestamp, data, previousHash){
		this.index = index;
		this.timestamp = timestamp;
		this.data = data;
		this.previousHash = "0";
		this.hash = this.calculateHash();		
	}
	
	calculateHash(){
		return sha256(this.index + this.timestamp + this.previousHash + this.data).toString();
		
	}
	
}

class Blockchain{
	constructor(){
		this.chain = [this.createGenesis()];
		}
	
	createGenesis(){
		return new Block(1, Date.now(), "Genesis block", "0")
		}
	
	
	latestBlock(){
		return this.chain[this.chain.length - 1]
	}
	
	addBlock(newBlock){
		newBlock.previousHash = this.latestBlock().hash;
		newBlock.hash = newBlock.calculateHash();
		this.chain.push(newBlock);
	}
	
	checkValid(){
		for(let i = 1; i < this.chain.length; i++){
			const currentBlock = this.chain[i];
			const previousBlock = this.chain[i-1];
			
			if (currentBlock.hash !== currentBlock.calculateHash()){
				return false;
			}
			if (currentBlock.previousHash !== previousBlock.hash){
				return false;
			}
		}
		
		return true;
	}
	
}

	let jsChain = new Blockchain();
	jsChain.addBlock (new Block(Date.now(),{name : 'Mike John'}));
	jsChain.addBlock (new Block(Date.now(),{name : 'Sam Andrew'}));
	
	console.log(JSON.stringify(jsChain, null, 5));
	console.log("Is blokchain valid?" + jsChain.checkValid());
    
