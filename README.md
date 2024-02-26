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

