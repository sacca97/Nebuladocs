# Noise IXpsk0

This section uses the following notation for keys:
- Q is the pre-shared key
- E<sub>i</sub><sup>pub</sup> is the initiator ephemeral public key
- S<sub>r</sub><sup>priv</sup> is the responder static private key
- T<sup>send</sup>, T<sup>recv</sup> are the session keys derived


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
(ck<sub>2</sub>, k<sub>1</sub>) = HKDF<sub>2</sub>(ck<sub>1</sub>, E<sub>i</sub><sup>pub</sup>)\
encrypted-static = enc(k<sub>1</sub>, 0, S<sub>i</sub><sup>pub</sup>, h<sub>2</sub>)\
write encrypted-static\
h<sub>3</sub> = hash(h<sub>2</sub> || encrypted-static)\
encrypted-data = enc(k<sub>1</sub>, 1, S<sub>i</sub><sup>pub</sup>, h<sub>2</sub>)\
h<sub>4</sub> = hash(h<sub>3</sub> || encrypted-data)

2. <- e, ee, se, s, es\
The responder computes:\
(E<sub>r</sub><sup>pub</sup>, E<sub>r</sub><sup>priv</sup>) = keygen()\
write E<sub>r</sub><sup>pub</sup>\
h<sub>5</sub> = hash(h<sub>4</sub> || E<sub>r</sub><sup>pub</sup>)\
(ck<sub>3</sub>, k<sub>2</sub>) = HKDF<sub>2</sub>(ck<sub>2</sub>, E<sub>r</sub><sup>pub</sup>)\
(ck<sub>4</sub>, k<sub>3</sub>) = HKDF<sub>2</sub>(ck<sub>3</sub>, DH(E<sub>i</sub><sup>pub</sup>, E<sub>r</sub><sup>priv</sup>))\
(ck<sub>5</sub>, k<sub>4</sub>) = HKDF<sub>2</sub>(ck<sub>4</sub>, DH(S<sub>i</sub><sup>pub</sup>, E<sub>r</sub><sup>priv</sup>))\
encrypted-static = enc(k<sub>4</sub>, 2, S<sub>i</sub><sup>pub</sup>, h<sub>5</sub>)\
write encrypted-static\
h<sub>6</sub> = hash(h<sub>5</sub> || encrypted-static)\
(ck<sub>6</sub>, k<sub>5</sub>) = HKDF<sub>2</sub>(ck<sub>5</sub>, DH(E<sub>i</sub><sup>pub</sup>, S<sub>r</sub><sup>priv</sup>))\
encrypted-data = enc(k<sub>5</sub>, 4, data, h<sub>6</sub>)\
write encrypted-data\
h<sub>7</sub> = hash(h<sub>6</sub> || encrypted-data)

A the end both initiator and responder will compute the session keys:

(T<sup>send</sup>, T<sup>recv</sup>) = HKDF<sub>2</sub>(ck<sub>7</sub>, empty)
