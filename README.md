# seahorse-deposit-pool
A simple example of a Solana smart contract written in the Seahorse language. It is a pool that receives SOL and mints a SPL token. Users can then redeem their SPL tokens to retrieve their deposited SOL.


## Tutorial for newcomers

In order to start learning how to develop a smart contract in Solana using the [Seahorse language](https://seahorse-lang.org/), I recommend reading and testing the code in the [Solana playground](https://beta.solpg.io/), following the steps below. But before that, I suggest reading [the basics about Solana Accounts](https://solanacookbook.com/core-concepts/accounts.html#facts) and [this excellent medium post](https://medium.com/@jorge.londono_31005/understanding-solanas-mint-account-and-token-accounts-546c0590e8e) about Solana's Mint Account and Token Accounts.

After getting familiar with the different types of Solana accounts, we are ready to run and test this code.

## Steps

1. Start a new Seahorse project in the [Solana playground](https://beta.solpg.io/). 
2. Copy the Seahorse code of the file `programs_py/seahorsedepositpool.py` of this repository.
3. Build code.
4. Deploy.

Now we run the tests.

5. First of all, run the `init` command to create a new pool and initialize the TokenMint account. We need to use these parameters:
- owner: Public key of the user's account. You can simply choose "My address" from the drop-down menu.
- pool: From seed -> use 'dep-pool' as seed (without '')   This generates the public key of the pool's Account.
- mint: From seed -> use 'pool-token-mint' as seed (without '')  This generates the public key of the associated TokenMint account.

Before proceeding with the next step copy the generated public keys for future use.

6. Now run `initAssociatedTokenAccount` to initialize the token account associated to the user's account. We need to use the following parameters:
-newTokenAccount: from seeds -> use 'pool-token-mint'  as seed (without '') and add user's account public key as the second seed (compare with what is written in the Seahorse code). Again, copy the generated public key for future use.
-mint: public key of the associated TokenMint account.
-signer: public key of the user's account ("My Address").

7. Now we have to create the token account associated to the pool. We can not use `initAssociatedTokenAccount` as in the previous item since we do not own the pool's account. Thus, we will use the `solana-cli` instead.  In the command line run

`spl-token create-account --owner PoolPubkey TokenMintPubKey`

Observe that the program prints:

> Creating account [ACertainPublicKey].

Copy the generated public key for future use.

8. Now we are ready to make a deposit. Run `deposit` with the following parameters:
- n: amount of lamports (1 lamport = 1·10<sup>-9</sup> SOL). The user is depositing n/(10^9) SOL and receiving the same amount of tokens.
- user: public key of the user's account ("My Address").
- userTknAcc: public key of the user's token account (initialized in 6.).
- pool: public key of the pool's account.
- mintaccount: public key of the TokenMint account.

9. Now we can make a withdrawal. Run `withdraw` with the following parameters:
- n: amount of tokens to redeem (check for decimal places). The user is redeeming n/(10^9) tokens and receiving the same amount of SOL.
- user: public key of the user's account ("My Address").
- userTknAcc: public key of the user's token account (initialized in 6.).
- pool: public key of the pool's account.
- pooltokenaccount: public key of the pool's token account (created in 7.).

