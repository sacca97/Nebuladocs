# Nebula VPN protocol

This is a summary in english of my thesis chapter about Nebula, which aims to provide a bit of documentations.

## Handshake

Nebula uses the Noise IXpsk0 as handshake pattern from the *Noise Protocol Framework*, however the psk isn't used yet, leaving the first packet in plaintext.

Leaving out the noise part for now...

The first message is sent from the initiator (a node) to the responder (a node or a lighthouse), and it's structured as follows:

![image info](nebulafirstpacket.png)