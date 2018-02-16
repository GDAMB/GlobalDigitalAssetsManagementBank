# EOS.IO Development Update

Our team has been working around the clock to make our EOS.IO software the best it can be. Those following along on GitHub should see some substantial improvements in the structure of the code as we implement many of the things we discussed in the last update.

EOSIO BIOS
A computer's BIOS is built into the hardware and is the first thing a computer loads prior to starting the operating system. This week we continue with the operating system metaphor to make the EOSIO blockchain bootstrap process as simple as possible, like a computer BIOS. The blockchain now starts up with a very simple initial state:

a single account (eosio.system)
a single private key
a single block producer
This initial account is like the root account on linux systems, it has unlimited power until it yields this power to a higher level operating system smart contract. From this initial state, the @eosio.system account will upload the operating system smart contract that implements the following:

staking for voting, network bandwidth, cpu bandwidth, ram, and storage.
producer and proxy vote creation
You can view this initial state as an embryonic stem cell capable of adapting an EOSIO based blockchain to any number of use cases and governance structures all of which can be updated and tweaked without requiring any hard forks.

We gain many benefits from this approach because it makes the core EOSIO software simpler and easier to test.

Dynamic Number of Block Producers
The primary outcome of this is that EOSIO blockchains now support a dynamic number of block producers which can be changed with a simple update to the @eosio.system smart contract. We will still default this to 21 producers, but this is no longer hard coded.

The primary reason for making it dynamic is because for many private blockchains, 21 producers is beyond overkill. Enterprise use of private blockchains may prefer to have just a couple of producers and test networks might want only a single producer.

Metering
Historically we have indicated that each transaction would have at most 1ms of runtime as measured subjectively by the block producer. We realized there is a demand for some transactions which could take up to 50ms to run and also want to incentivize efficiency by encouraging developers to design transactions that take less than 50us to run. Under our original model all transactions utilized the same CPU whether 50us or 1ms meaning there is no incentive to optimize below 1ms.

Because runtime is subjective and can vary depending upon other activities running on the same computer, it is not possible to generate an objective and reproducible measure of runtime.

We realized that at no additional cost, we could modify our existing time-based rate limiter to a limiter that would calculate an objective estimate of the number of WASM instructions executed. This is similar to how Ethereum measures gas consumption. With this new objective measure we can rate limit CPU just like we rate limit bandwidth.

The block producers will use the same "dynamic oversubscription" algorithm for CPU usage that they use with network bandwidth. This means that while the network has spare CPU capacity users can get more CPU-per-staked token than they would be guaranteed to get during full congestion.

The block producers would still implement a subjective runtime limit in addition to the CPU instruction counting. This subjective limit would protect the network from those who would abuse the metering algorithm by using the most time-expensive operations more than the less time-expensive operations.

Separation of CPU and Network Bandwidth
In previous updates we indicated that we would separate out RAM, Storage, and Bandwidth where CPU/Network were both considered part of bandwidth. We realized that some applications, like Steem, might have high network bandwidth (for posts) and low CPU bandwidth, whereas other applications might have low network bandwidth (exchange orders), but higher CPU bandwidth (order matching). This means that one-size-fits-all pricing and/or staking does not make sense.

To keep things simple, the user interface can still bundle these things together for normal users; however, power users now have more price flexibility.

Transaction Compression
In the process of adding support for the c++ STL library we noticed that smart contracts could get quite large (50kb) and would therefore consume significant network bandwidth. It is conceivable that more complex contracts might grow to be over 200kb. We also realized that many applications, such as Steem, bundle very compressible content into transactions.

We added support for zlib compression of transactions which can provide a 60% or more reduction in bandwidth usage for smart contract uploads and potentially higher for Steem-like content.

Network Updates
The P2P network team has been busy updating the code to enhance performance and stability. This week they made significant progress on the following:

block summary - when a block is broadcast only the transaction IDs are included rather than retransmitting all the transactions in the block. This will reduce bandwidth usage by almost 50%.

large message support - broadcasting large messages (like 50kb smart contracts) needs a different network protocol than small messages (like 200 byte transfers).

Conclusion
Our development team is working to make EOSIO the most efficient, general purpose, and flexible platform to date.
