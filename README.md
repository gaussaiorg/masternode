# masternode
Tutorial - Install masternode on Ubuntu Server 22.04
Install a masternode for your coin on Ubuntu Server 22.04 with the following tutorial.

# Generate a Platform Node ID
the following commands can be used generate P2P key, save it to privkey.pem, and generate platformNodeID in hex format:

openssl genpkey -algorithm ed25519 -out privkey.pem
openssl pkey -in privkey.pem -noout  -text_pub |tail -n +3 | tr -d '[:space:]' | xxd -r -p| sha256sum | head -c 40

1e8e241c05ca350c8fe0b8ba4680e7652673dae2
