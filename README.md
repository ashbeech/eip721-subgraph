# Install

`yarn install`

Install `subgraph-deploy`

`yarn add -D subgraph-deploy`

Modify

• subgraph.yaml

Deploy

`graph auth --studio c387xxx8fa` <-- insert your studio CLI auth

`graph codegen && graph build` <-- build

`graph deploy --studio xxx` <-- deploy

Check studio to makesure it's syncing…

### Optional

in your package.json you can add a script to deploy that subgraph in your running graph-node

```json
{
  "scripts": {
    "deploy:eip721-subgraph": "subgraph-deploy -s ashbeech/eip721-subgraph -f eip721-subgraph -i http://localhost:5001/api -g http://localhost:8020"
  }
}
```

```javascript
  .requiredOption('-s, --subgraph <subgraphName>', 'name of the subgraph')
  .requiredOption('-f, --from <folder or npm package>', 'folder or npm package where compiled subgraphe exist')
  .requiredOption('-i, --ipfs <url>', 'ipfs node api url')
  .requiredOption('-g, --graph <url>', 'graph node url');
```

# Example graphQL query

```
  # Get all token data.
query tokenFireHose {
  tokens {
    id
    contract {
      id
    }
    tokenID
    tokenURI
    owner {
      id
    }
    mintTime
    minter {
      id
    }
  }
}

# Get specific token data by ID (minting contract address + token ID)
query tokenSelector {
  token( id:"0xe9b6db0bb21e6df1fe6f8c5e076740eebf4d81eb_2763" ) {
    id
    contract {
      id
    }
    tokenID
    tokenURI
    owner {
      id
    }
    mintTime
    minter {
      id
    }
  }
}

# Get owner's tokens (first 10)
query getOwbersTokenByOwner {
  owner(id:"0xb71147d12e2ec640decc103bd126911d23ae2fba") {
    id
    tokens (first: 10, orderBy: mintTime, orderDirection: asc) {
      id
      tokenID
    }
    numTokens
    numMints
  }
}

# Get owner's tokens (paginated e.g. page 2, next 10)
query getOwbersTokenByOwner {
  owner(id:"0xb71147d12e2ec640decc103bd126911d23ae2fba") {
    id
    tokens (first: 10, skip: 10, orderBy: mintTime, orderDirection: asc) {
      id
      tokenID
    }
    numTokens
    numMints
  }
}

query filterOwnersTokens {
  owner(id:"0xb71147d12e2ec640decc103bd126911d23ae2fba") {
    id
    tokens (where: {tokenID: 2763}) {
      id
      tokenID
    }
    numTokens
    numMints
  }
}

query getTokensByMinter {
  tokens(first: 1, where: {minter: "0xb71147d12e2ec640decc103bd126911d23ae2fba"}) {
    id
    contract {
      id
    }
    tokenID
    tokenURI
    owner {
      id
    }
    mintTime
    minter {
      id
    }
  }
}

# Get all tokens under contract. Returns unique token IDs
query getTokensByContract {
  tokenContracts {
    id
    name
    symbol
    doAllAddressesOwnTheirIdByDefault
    supportsEIP721Metadata
    tokens {
      id
      tokenID
    }
    numTokens
    numOwners
  }
}

# Hosted Studio Note: Avoid multiple queries (https://github.com/graphprotocol/graph-node/issues/934)
```
