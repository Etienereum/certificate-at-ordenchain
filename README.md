# certificate-at-ordenchain
//Issuing Certificates which are blockchain based for easy verification and documentation.

const sha256 = require('sha256');

class Block {
	constructor (index, timestamp, data, previousHash, hash, calculateHash){
		this.index = index;
		this.timestamp = timestamp;
		this.data = data;
		this.previousHash = "0";
		this.hash = this.calculateHash();
		}
		
	calculateHash(){
		return sha256(this.index + this.timestamp + this.data + this.previousHash).toString();	
		}
}

class Blockchain{
	constructor(){
		this.chain= [this.createGenesis()];
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
			else
				return "Yes, it is valid";
		}
	}
	
}

	let certChain = new Blockchain();
	certChain.addBlock(new Block(2, Date.now(),{"Certificate Name": "Emeka Nduka"}));
	certChain.addBlock(new Block(3, Date.now(),{"Certificate Name": "Okoronkwo Jude"}));
	certChain.addBlock(new Block(4, Date.now(),{"Certificate Name": "Adebayo Adejo"}));
	certChain.addBlock(new Block(5, Date.now(),{"Certificate Name": "Mallam Muhammadu"}));
	
	console.log(JSON.stringify(certChain, null, 4));
	console.log("Is blokchain valid? " + certChain.checkValid());
   
