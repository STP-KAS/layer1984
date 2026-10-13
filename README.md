# layer1984

Delusional level: 10 out of 10. A drafted idea. Nothing more.

Intentions are good. The thought process is questionable. This page is an experiment. It is not advice, and it does not speak for Kaspa core, for Luke Dunshea, or for Olaf Weller. Sorry for the time.

The sourced note is [EVALUATION.md](EVALUATION.md). The question that started it is [GROK-BUILD-PROMPT.md](GROK-BUILD-PROMPT.md).

## What

The draft asks for an extra layer of the internet. The picture in the draft is Tor-like and faster, opened by a piece of hardware the user owns, able to carry a site such as 1984, with Kaspa underneath.

What exists today is smaller, and it is already other people's work.

- Kaspa, after Toccata, orders payments and small application updates, checks Groth16 and RISC Zero receipts, and gives a covenant a tracked id and a lane. Under KIP-13 a block stays near 125 KB. At 10 blocks a second, a network that spent every gram on bytes would move about 1.25 MB/s, and real blocks also carry payments, proofs, and collisions.
- Tor already carries web bytes. The 2004 design paper says a watcher who sees both ends of a circuit can correlate them.
- OpenMiner, by Luke Dunshea, documents open control of miners people already own. The public reference covers a Bitmain BM2382 on a KS5 Pro and a Goldshell IEN616 on a KA Box. The reference covers hashboards. A router, a credential, and a traffic path are outside it. IceRiver, including the KS0 Ultra, is absent from that reference.
- The Kaspa Privacy Initiative, by Olaf Weller, is optional privacy research for native KAS. The stated rules are no new token, no custodian, and no architecture marked final. No production protocol exists. The 1984 square's OLAF note is a separate desk-side dollar, and that note sets `proof` and `zk` to false.

Sources for those four lines:

- OpenMiner announcement: https://x.com/elldeeone/status/2105643604446494781
- OpenMiner reference: https://github.com/elldeeone/openminer-reference
- Kaspa Privacy Initiative: https://github.com/olafweller/kaspa-privacy-initiative
- Kaspa KIPs: https://github.com/kaspanet/kips

## Why

A block can carry a small update or a proof. One user's web session is larger than the whole chain's byte budget. A payload is stored in full for every indexer.

A home miner hashes kHeavyHash. A quiet box expects on the order of one to three blocks a day at the public hashrate reading used in the note (about 363 PH/s on 13 Oct 2026). The coinbase and the pool session are public. A login that demands hashrate admits anyone who rents that hashrate, and it locks the owner out between blocks.

OpenMiner's published evidence ends at stock-board control. A new access appliance is later work, and that work is unpublished. The chips in the reference are a 3 kW KS5 class and a 400 W KA Box.

A second shielded pool in this repo would repeat an unfinished circuit. Shielded native KAS already has a public research track.

## How

Keep three planes apart.

**Transport.** Reach the site through a Tor onion service when the operator wants the server address off the public web. The session bytes stay on Tor.

**Access.** Generate a signing key on the same computer that runs the user's Kaspa node. Login is a signature over a fresh challenge, and a second key is the backup. The ASIC, if one sits beside that computer, returns nonces. The host holds the key. A copied key is a copied user. OpenMiner provides no secure element, so this gate is a software key on a machine the user can open.

**Settlement.** Pay with the Kaspa wallet the user already accepts. On Testnet-10 that can be public tKAS. Shielded native KAS, when a reviewed testnet design exists, stays with the Kaspa Privacy Initiative, and it stays on play money. KCC-20 is Last Call on kccs `3fbec524`. It is a fungible-note convention.

1984 stays a website. A content hash or a short offer can sit in a transaction. The page, the video, and the chat stay on the server or on the user's disk.

A later Testnet-10 check, if anyone runs one, is two measurements and then a stop. One is an offline signature over a random challenge. The other is a read-only mass reading of 1 KB, 10 KB, and 50 KB payloads against a node that already exists. No broadcast spray, no second kaspad, no new token, and no miner image.

## Conclusion

The drafted layer, read as a faster Tor that runs inside Kaspa and opens because a miner is in the path, does not fit the chain or the hardware.

The pieces that do fit are already separate. An onion service carries the bytes. A Kaspa node runs on the user's own machine. A key on that machine is the gate, when a gate is wanted. Native KAS is the money. Payment privacy stays with the Kaspa Privacy Initiative.

This repository is a public draft of that reading. Real funds stay on Testnet-10. The 1984 square remains the square.
