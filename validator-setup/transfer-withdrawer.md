# Transfer Withdrawer to another entity

### 1. Change authorized withdrawer
- create a nonce account if you don’t have one:
```
solana-keygen new -o nonce-keypair.json
```
You will get output that looks like this:
```
Generating a new keypair

For added security, enter a BIP39 passphrase

NOTE! This passphrase improves security of the recovery seed phrase NOT the
keypair file itself, which is stored as insecure plain text

BIP39 Passphrase (empty for none): 

Wrote new keypair to nonce-keypair.json
==================================================================================
pubkey: CQwaLVSMDNPNFxrTmMJKBjLwCZP2cEQZFqmCy5xzaB2r
==================================================================================
Save this seed phrase and your BIP39 passphrase to recover your new keypair:
plastic flame beauty together small hidden run planet faculty august weird arrange
==================================================================================
```
Copy the value that you see for pubkey. It will be referred to as PUBKEY henceforth.
Next, let's generate a nonce account with the newly created key pair:
```
solana create-nonce-account nonce-keypair.json 1
```
- print out nonce info:
```
  solana nonce-account nonce-keypair.json
```
You will get output that looks like this:
```
Balance: 0.1 SOL
Minimum Balance Required: 0.00144768 SOL
Nonce blockhash: 9MKPRoeZiuUwXxpXyRBifLNFAzLjLWh3cezCuXE7TXKc
Fee: 5000 lamports per signature
Authority: 4uzi7zTHEJvbSwcUtwjBpmdSgr5ahNkq4QPPWNHXmqR9
```
Note the nonce blockhash value. It will be referred to as BLOCKHASH.
- use nonce info from above
```
solana vote-authorize-withdrawer-checked --sign-only --nonce <PUBKEY> --nonce-authority nonce-keypair.json --blockhash <BLOCKHASH> DeNodee9LR1WPokmRqidmAQEq8UbBqNCv6QfFTvU6k69 DNodeaLZFq2PaJvJZTE6CikdknDzPC7wbEzdu4DNpY <your-new-authorized-withdraw-keypair>
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
