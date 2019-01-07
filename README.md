### Youchallenge

To start your Phoenix server:

  * Install dependencies with `mix deps.get`
  * Create and migrate your database with `mix ecto.setup`
  * Install Node.js dependencies with `cd assets/frontend && npm install`
  * Run `source .env` inside `youchallenge` directory (the account inside that file is provided for your convenience, but could be replaced with your own account. It is used for contract deployments.)
  * Start Phoenix endpoint with `mix phx.server`
  * (In a separate terminal window) start Node.js server with `cd assets/frontend && npm start`

Now you can visit [`localhost:3000`](http://localhost:3000) from your browser.

### Implementation and decisions

Please keep in mind that the submitted code is nowhere near the production state and does not represent a fully-functional application. This prototype has been created for demonstration purposes only (to explore the idea explained below).

Youchallenge implementation differs from regular dApps. First, it consists of 2 separate parts: the first one is the API - it serves several purposes, including contract deployments, contract state changes and other (possible, but not included) functions. The rationale behind this "traditional" backend is that while web3 dApps are great for many use-cases (such as transparency, immutability and security, among others), it might be useful to utilize a mixture of both. This way the "normal" backend can efficiently handle operations it was built for (such as iterating over large collections, quickly resolve relationships between different entities, make batch changes to sets of data, etc) while web3 will be responsible for operations that reap the benefits of the Ethereum blockchain.

The second part is just a regular dApp front-end. It serves as normal web3 wrapper around Metamask, contract functions execution (both call and transaction) and as a web2 client that "talks" to the backend server. This way the client relies on data from the Ethereum blockchain AND traditional web app / database.

While both parts interact with blockchain in their own way, it's certainly less trivial to sync both of them so that they reflect the most up-to-date state. In a traditional dApp there's no concept of a backend server running behind the scenes - instead, contracts, technologies like IPFS and other platforms serve that purpose. While they are being used in production, they are certainly not as stable and battle-tested as normal databases, in-memory storage engines and web app servers.
One might argue that by utilizing traditional infrastructure technologies the benefits of the blockchain platform crumble. Indeed, without careful planning and solid understanding of trade-offs it certainly might be the case. However, if the web2 stack is used for non-critical business logic (that does not require the benefits of the Ethereum blockchain) and is supported by redundant and fault-tolerant infrastructure, it might provide benefits listed above. Another challenge that becomes very real when using web2 stack for interactions with smart contracts is the key management. There are solutions like Hashicorp Vault (among others), but its integration with existing apps presents a new set of challenges (deep understanding of security protocols, legal challenges, etc). And while it's certainly not easy to merge these 2 very different stacks, it might be worth the time and effort to explore that path.

### Workflow:

* Create new challenge from account 1 and use account 2 as a contender
* Switch to account 2 (the contender)
* The newly created challenge should have now have the "accept" button available
* Accept the challenge. After a few seconds the address should be confirmed and appear at the top of the challenge list item
* Account 1 will now have the "contribute" button available. This will trigger a transaction via Metamask with some default values pre-filled (can be changed via the edit menu in Metamask)
* After submitting the transaction, the balance values should change after a few moments (after transaction is confirmed)
* Account 2 can complete the challenge by clicking the "complete" button
* Account 1 can verify and finish the challenge by clicking "finish" button. This will trigger contract's balance transfer to the contender's address (can be verified in the Metamask account)
* In case the contender hasn't finished the challenge in time, the challenge expires and the balance is flushed to the funding account (the one that's used for deployments)
