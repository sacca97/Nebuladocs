The Noise handshake starts with initiator (I) and responder (R) computing the hash function of the protocol name:

h<sub>0</sub> = hash("Noise_IXpsk0_25519_ChaChaPoly_SHA256")\
ck<sub>0</sub> = h<sub>0</sub>

Then the tokens of the handshake pattern are processed sequentially:

1. -> psk, e, s\
The initiator computes:\
(ck<sub>1</sub>, t, k<sub>0</sub>) = HKDF<sub>3</sub>(ck<sub>0</sub>, Q)\
h<sub>1</sub> = hash(h<sub>0</sub> || t)\
(E<sub>i</sub><sup>pub</sup>, E<sub>i</sub><sup>priv</sup>) = keygen()\
write E<sub>i</sub><sup>pub</sup>\
h<sub>2</sub> = hash(h<sub>1</sub> || E<sub>i</sub><sup>pub</sup>)\
(ck<sub>2</sub>, k<sub>1</sub>) = HKDF<sub>2</sub>(ck<sub>01</sub>, E<sub>i</sub><sup>pub</sup>)\
encrypted-static = enc(k<sub>1</sub>, 0, S<sub>i</sub><sup>pub</sup>, h<sub>2</sub>)\
write encrypted-static\
h<sub>3</sub> = hash(h<sub>2</sub> || encrypted-static)\
encrypted-data = enc(k<sub>1</sub>, 0, S<sub>i</sub><sup>pub</sup>, h<sub>2</sub>)\

