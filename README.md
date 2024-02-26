# Install EVO MasterNode
Tutorial - Install masternode on Ubuntu Server 22.04
Install a masternode for your coin on Ubuntu Server 22.04 with the following tutorial.

# Generate a Platform Node ID
the following commands can be used generate P2P key, save it to privkey.pem, and generate platformNodeID in hex format:

openssl genpkey -algorithm ed25519 -out privkey.pem
openssl pkey -in privkey.pem -noout  -text_pub |tail -n +3 | tr -d '[:space:]' | xxd -r -p| sha256sum | head -c 40

1e8e241c05ca350c8fe0b8ba4680e7652673dae2

# Send 5.0 GSS to wallet address 1

Go to Tools -> Debug console.

Type the following RPC command, to create an address for the masternode fee:

getnewaddress

Example output

TRPLV4dXmEFMSgXg2Xu6skN9pmw8TAo4N5

Go back to your wallet overview.

Press on the toolbar button "Send".

Enter the address from the RPC command “getnewaddress” behind the text "Pay To:". (Example: TRPLV4dXmEFMSgXg2Xu6skN9pmw8TAo4N5)

Enter the following amount of coins behind the text "Amount:": 5

# Send 4000.0 GSS to wallet address 2
Go back to the console of your wallet.

Type the following RPC command, to create an address for the masternode collateral:

getnewaddress

Example output

TFi68ygQkc8h27DyeBwPykVHjF6oh8PGkH

Go back to your wallet overview.

Press on the toolbar button "Send".

Enter the address from the RPC command “getnewaddress” behind the text "Pay To:". (Example: TFi68ygQkc8h27DyeBwPykVHjF6oh8PGkH)

Enter the following amount of coins behind the text "Amount:": 4000.0

Press on the button "Send".

Wait until the transaction is confirmed by 15 blocks.

# Get Identify the transaction with the following RPC command

masternode outputs

Example output

{
 fdab9dff1ff9caf5d291905ad43b9f7d69775189d4d22cb085d7fedd94ea1c6a-1
}

# Generate a BLS key pair with the the following RPC command
bls generate

Example output

{
 "secret": "0acbf6f183d0c9b794b9bc0dba25f8a1a1eca21aa4f2e4a86ecd3120a59efb35",
 "public": "064bb1741f4707cfe3629176857c41e0d23cbe751061fe5d0d67b506db10c8f3f6f2b684c3cec8e4a128193a001d12e9"
}

# Get Two New Wallet Address
getnewaddress

Example output

TWXGVYPHGA4gWcZ9Zp2qnubFVNagvorwEN

getnewaddress

Example output

TEXj9AgdCh1giGmr1BXpYsngmkMkthNngD

# Prepare the ProRegTx transaction by modifying the following line.

protx register_evo fdab9dff1ff9caf5d291905ad43b9f7d69775189d4d22cb085d7fedd94ea1c6a 1 1.1.1.18:15906 TWXGVYPHGA4gWcZ9Zp2qnubFVNagvorwEN 064bb1741f4707cfe3629176857c41e0d23cbe751061fe5d0d67b506db10c8f3f6f2b684c3cec8e4a128193a001d12e9 TEXj9AgdCh1giGmr1BXpYsngmkMkthNngD 0 TRPLV4dXmEFMSgXg2Xu6skN9pmw8TAo4N5 1e8e241c05ca350c8fe0b8ba4680e7652673dae2 26656 443


# Note
protx register_evo "collateralHash" collateralIndex "ipAndPort" "ownerAddress" "operatorPubKey" "votingAddress" "operatorReward" "payoutAddress" "platformNodeID" platformP2PPort platformHTTPPort ( "feeSourceAddress" submit )

Same as "protx register_fund_evo", but with an externally referenced collateral.
The collateral is specified through "collateralHash" and "collateralIndex" and must be an unspent
transaction output spendable by this wallet. It must also not be used by any other masternode.

Requires wallet passphrase to be set with walletpassphrase call if wallet is encrypted.

Arguments:
1. collateralHash       (string, required) The collateral transaction hash.
2. collateralIndex      (numeric, required) The collateral transaction output index.
3. ipAndPort            (string, required) IP and port in the form "IP:PORT". Must be unique on the network.
                        Can be set to an empty string, which will require a ProUpServTx afterwards.
4. ownerAddress         (string, required) The gaussai address to use for payee updates and proposal voting.
                        The corresponding private key does not have to be known by your wallet.
                        The address must be unused and must differ from the collateralAddress.
5. operatorPubKey       (string, required) The operator BLS public key. The BLS private key does not have to be known.
                        It has to match the BLS private key which is later used when operating the masternode.
6. votingAddress        (string, required) The voting key address. The private key does not have to be known by your wallet.
                        It has to match the private key which is later used when voting on proposals.
                        If set to an empty string, ownerAddress will be used.
7. operatorReward       (string, required) The fraction in %% to share with the operator.
                        The value must be between 0 and 10000.
8. payoutAddress        (string, required) The gaussai address to use for masternode reward payments.
9. platformNodeID       (string, required) Platform P2P node ID, derived from P2P public key.
10. platformP2PPort     (numeric, required) TCP port of Gaussai Platform peer-to-peer communication between nodes (network byte order).
11. platformHTTPPort    (numeric, required) TCP port of Platform HTTP/API interface (network byte order).
12. feeSourceAddress    (string, optional, default=) If specified wallet will only use coins from this address to fund ProTx.
                        If not specified, payoutAddress is the one that is going to be used.
                        The private key belonging to this address must be known in your wallet.
13. submit              (boolean, optional, default=true) If true, the resulting transaction is sent to the network.

Result (if "submit" is not set or set to true):
"hex"    (string) The transaction id

Result (if "submit" is set to false):
"hex"    (string) The serialized signed ProTx in hex format

Examples:
> gaussai-cli protx register_evo "0123456701234567012345670123456701234567012345670123456701234567" 0 "1.2.3.4:1234" "XwQQkwA4FYkq2XERzMY2CiAZhJTEDAbtc0" "93746e8731c57f87f79b3620a7982924e2931717d49540a85864bd543de11c43fb868fd63e501a1db37e19ed59ae6db4" "XwQQkwA4FYkq2XERzMY2CiAZhJTEDAbtc0" 0 "XunLY9Tf7Zsef8gMGL2fhWA9ZmMjt4KPw0" "f2dbd9b0a1f541a7c44d34a58674d0262f5feca5" 22821 22822
 (code -1)

