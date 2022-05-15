# Change the 

### 1. Change authorized withdrawer
- create a nonce account if you don’t have one:
```
  solana-keygen new -o nonce-keypair.json
  solana create-nonce-account nonce-keypair.json 1
```
- print out nonce info:
```
  solana nonce-account nonce-keypair.json
```
- use nonce info from above
```
solana vote-authorize-withdrawer-checked --sign-only --nonce <PUBKEY> --nonce-authority <KEYPAIR> --blockhash <BLOCKHASH> DeNodee9LR1WPokmRqidmAQEq8UbBqNCv6QfFTvU6k69 DeNodee9LR1WPokmRqidmAQEq8UbBqNCv6QfFTvU6k69 <your-new-authorized-withdraw-keypair>
```
- send the current owner (Shakudo) the printout

(Owner (Shakudo) will run a command on our side like `solana vote-authorize-withdrawer-checked DeNodee9LR1WPokmRqidmAQEq8UbBqNCv6QfFTvU6k69 withdrawer.json another.json --blockhash <from-client> --signer <from-client>`)

### 2. Create staking account or set existing staking account with your withdraw keypair
To create a new staking account:
```
solana create-stake-account --from <KEYPAIR> stake-account.json <AMOUNT> \
    --stake-authority <KEYPAIR> --withdraw-authority <KEYPAIR> \
    --fee-payer <KEYPAIR>
```
To set an existing staking account:
```
solana stake-authorize <STAKE_ACCOUNT_ADDRESS> \
    --stake-authority <KEYPAIR> --new-stake-authority <PUBKEY> \
    --fee-payer <KEYPAIR>
```
