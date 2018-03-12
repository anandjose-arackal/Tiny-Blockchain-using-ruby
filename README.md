# Tiny Blockchain Implementation using ruby
Let’s Build the Tiniest Blockchain using ruby

## Blockchain
a digital ledger in which transactions made in bitcoin or another cryptocurrency are recorded chronologically and publicly.

We’ll start by first defining what our blocks will look like. In blockchain, each block is stored with a timestamp and, optionally, an index. In SnakeCoin, we’re going to store both. And to help ensure integrity throughout the blockchain, each block will have a self-identifying hash. Like Bitcoin, each block’s hash will be a cryptographic hash of the block’s index, timestamp, data, and the hash of the previous block’s hash. Oh, and the data can be anything you want.

```
class Block

  attr_reader :index
  attr_reader :timestamp
  attr_reader :data
  attr_reader :previous_hash
  attr_reader :hash

  def initialize(index, data, previous_hash)
    @index         = index
    @timestamp     = Time.now
    @data          = data
    @previous_hash = previous_hash
    @hash          = calc_hash
  end
  
end 

```

We’ll create a function that simply returns a genesis block to make things easy. 
This block is of index 0, and it has an arbitrary data value and an arbitrary value in the “previous hash” parameter.

```
 def self.first( data="Genesis" )    # create genesis (big bang! first) block
    ## uses index zero (0) and arbitrary previous_hash ("0")
    Block.new( 0, data, "0" )
 end

```

Now that we’re able to create a genesis block,
we need a function that will generate succeeding blocks in the blockchain. This function will take the previous block in the chain as a parameter,
create the data for the block to be generated, and return the new block with its appropriate data. When new blocks hash 
information from previous blocks, the integrity of the blockchain increases with each new block.
If we didn’t do this, it would be easier for an outside party to “change the past” and replace our chain with an entirely new
one of their own. This chain of hashes acts as cryptographic proof and helps ensure that once a block is added to the 
blockchain it cannot be replaced or removed.

```
  def self.next( previous, data="Transaction Data..." )
    Block.new( previous.index+1, data, previous.hash )
  end
  
```

That’s the majority of the hard work. Now, we can create our blockchain! In our case, the blockchain itself is a simple
Python list. The first element of the list is the genesis block. And of course, we need to add the succeeding blocks.
Because SnakeCoin is the tiniest blockchain, we’ll only add 20 new blocks. We can do this with a for loop.

```b0 = Block.first( "Genesis" )
b1 = Block.next( b0, "Transaction Data..." )
b2 = Block.next( b1, "Transaction Data......" )
b3 = Block.next( b2, "More Transaction Data..." )

blockchain = [b0, b1, b2, b3]

pp blockchain
```

