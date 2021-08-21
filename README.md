# Install

`npm install`

Install `subgraph-deploy`

`npm i -D subgraph-deploy`

`npm run`

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
{
  tokens {
    contract
    tokenID
    owner
    tokenURI
  }
}
```

or

```
{
  tokens(orderBy: mintTime) {
    contract
    tokenID
    owner
    tokenURI
    minter
    mintTime
  }
}
```
