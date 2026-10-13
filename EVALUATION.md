# layer1984

Delusional level: 10 out of 10. A drafted idea. Nothing more.

Intentions are good. The thought process is questionable. This note is an experiment. It is not advice. Sorry for the time.

Date: 13 Oct 2026.
Author: Fermi. The sources and the arithmetic follow the brief in `GROK-BUILD-PROMPT.md`.
Scope: what Kaspa and OpenMiner can actually do, and what a hardware-gated "extra layer of the internet" would have to be if it is going to be real.

This note does not add a token, a covenant, or a miner image. It does not speak for Kaspa core, for Luke Dunshea, or for Olaf Weller.

The short page is [README.md](README.md).

## What

The draft asks for a Tor-like layer that runs on Kaspa, opens when the user holds a miner, and can host a site such as 1984. The record below is what those pieces already are.

### What is already live

Toccata is on mainnet. The Kaspa docs treat it as active from 30 June 2026 at DAA score 474,165,565. The consensus bundle is four KIPs, all marked Active in [kaspanet/kips](https://github.com/kaspanet/kips):

- [KIP-16](https://github.com/kaspanet/kips/blob/master/kip-0016.md) adds `OpZkPrecompile`. Tag `0x20` verifies Groth16. Tag `0x21` verifies a RISC Zero succinct STARK receipt. Groth16 is compact and is not post-quantum. The STARK path is the post-quantum one and is larger and slower to verify. KIP-16's own timing notes put Groth16 verification at about 134 times an ECDSA check and STARK verification at about 738 times, on the benchmark machine they used.
- [KIP-17](https://github.com/kaspanet/kips/blob/master/kip-0017.md) is the covenant scripting surface: introspection, `OpCat`, `OpBlake3`, and the rest of the byte and arithmetic tools a script needs in order to constrain the transaction that spends it.
- [KIP-20](https://github.com/kaspanet/kips/blob/master/kip-0020.md) gives a covenant instance a consensus-tracked id, so a lookalike output cannot join the family just by wearing the same script.
- [KIP-21](https://github.com/kaspanet/kips/blob/master/kip-0021.md) replaces one global sequencing hash with per-lane commitments, so a proof can follow one application's activity. Authors include Michael Sutton, Maxim Biryukov, and Hans Moog. The companion [proving-spec.md](https://github.com/kaspanet/kips/blob/master/kip-0021/proving-spec.md) is a reserved note. It is not itself a consensus spec.

Crescendo, before Toccata, is what made the rate 10 blocks per second and turned on ordinary transaction payloads (KIP-14) under a tighter byte budget (KIP-13). Michael Sutton's March 2025 account of that upgrade is the clearest core-developer statement of what a payload is for: arbitrary data such as a function call, enough for early based applications, still inside a fixed block mass. He also wrote that unique-transaction efficiency on the 10-block test was about 80 to 90 percent, because parallel blocks collide.

Source: https://x.com/michaelsuttonil/status/1905387292853703157

KIP-13 sets transient storage mass to four times the serialized transaction size so that a 500,000-gram block stays near 125 KB. The kips directory has no later proposal that raises that mainnet budget. Experimental TN12 parameters in older rusty-kaspa trees raise signature-script room for large proofs. That is not a mainnet capacity increase.

The practical ceiling, if every gram in every block were spent on bytes and on nothing else:

`10 blocks/s × 125 KB ≈ 1.25 MB/s` for the whole network.

Payments, compute mass, and colliding blocks take part of that. One person's video call is already larger than the entire chain.

### What high frequency is for

The docs page on covenant state says a single UTXO is a bottleneck only when the design forces every update through that one output. At 10 blocks per second, many independent covenant UTXOs can move at once. The design work is how to split the state.

Source: https://docs.kaspa.org/toccata/covenant-state

The same docs send private transitions, expensive logic, and deeply shared state to inline ZK and to Based Apps, not through one covenant.

Based Apps, in the current docs, start as an ordinary version-1 transaction whose subnetwork id selects a lane and whose payload carries an operation. L1 orders that operation. An off-chain runtime executes it. A settlement covenant later checks a proof that the off-chain state moved correctly. The covenant does not run every user operation.

Sources:

- https://docs.kaspa.org/toccata/based-apps
- https://x.com/kaspaunchained/status/2105295783092486348 (30 Sep 2026: L1 support for this pattern has been available since Toccata)
- https://x.com/michaelsuttonil/status/2108234606738604166 (8 Oct 2026: high frequency as many small responsive programs)
- https://x.com/kaspaunchained/status/2109990960486904255 (13 Oct 2026: blocks carry payments and app updates; covenants can update independently, and a joint step can be one transaction)

Sutton's framing, recorded second-hand in April 2026 discussion notes and consistent with the docs, is the useful design rule: shared mutable state points at a Based App; a person-to-person flow points at a covenant.

SilverScript is the covenant language. Argent is an experimental actor frontend that lowers to SilverScript. Sutton wrote on 2 Oct 2026 that he expects every serious stateful covenant to ship a genesis proof tying the covenant id back to source, parameters, and opening state. That is an audit aid. It is not an anonymity system.

Source: https://x.com/michaelsuttonil/status/2106033659698450492

vProgs remain a research runtime. The Kaspa docs describe the repo as a source of patterns, with APIs still moving, and they keep full vProgs off the Toccata list. KIP-21 says a future vProg can sit in a lane. That sentence is compatibility, not a shipped computer.

KCC-20 is a fungible-token convention on these covenants. This desk's Kaspa master file still pins it at Last Call on kccs `3fbec524`, not Final. A token standard does not move packets and does not hide a balance.

### Kaspa Privacy Initiative

Olaf Weller's project is optional privacy for native KAS. The README says it is community research, not Kaspa core, that no production protocol exists, and that experimental code must not hold real funds. The stated constraints are: no new token, no custodian, no architecture declared final.

Sources:

- https://github.com/olafweller/kaspa-privacy-initiative
- https://x.com/WellerOlaf/status/2106031612945199511 (2 Oct 2026 launch)
- https://research.kas.pa/t/optional-privacy-layer-for-kaspa-similar-to-litecoin-mweb/522 (the question that preceded it)

The candidate shape is a covenant-held reserve of native KAS, public commitments, and a shielded state updated by proofs. Their A0 note, dated 3 Oct 2026, reports a narrow TN10 success: a real Groth16 proof authorized release of a 10 test-KAS reserve, a replay was rejected, and the accounting was 10.2 in, 10 out, 0.2 fee buffer. That is a terminal payout experiment. It is not a pool you can pay a person with.

Source: https://github.com/olafweller/kaspa-privacy-initiative/blob/main/docs/poc-a0.md

Their architecture note says the current per-instance Groth16 setups are not the scalable pool, that a single-party setup stays trusted, and that the construction is not post-quantum. It also says a Based App must still explain what its public payloads and access patterns leak.

On 13 Oct 2026 Weller wrote that a conversation with Sutton pointed at Based Apps plus KIP-21 as the way to stop a batcher from omitting user operations: users post into an L1 lane, and the settlement proof has to cover that lane's committed activity. The named next step is a small Based App trial on Testnet-10 that checks lane ordering, execution, proof settlement, deliberate omission, and independent reconstruction. Shielded logic comes after that trial.

Source: https://x.com/WellerOlaf/status/2109944191547793911

Sutton's same-day reply treats the canonical-bridge idea (a pool whose internal distribution is opaque to L1) as a fit for that privacy shape. He adds two limits that matter here. User proofs do not have to be in the L1 opcode's format. Only the outer app proof does. The based lane gives censorship resistance and confirmation times tied to L1 block frequency, not to how often a proof is published.

Source: https://x.com/michaelsuttonil/status/2110060261126095002

That is payment privacy, ordered and settled on Kaspa. It is not a network path for web traffic.

### Kloak

The Kaspa project discussed under the name Kloak is a PayJoin-style transaction. Weller's 7 Jan 2026 explanation: sender and receiver both contribute an input, several outputs appear, balances stay public, nothing is mixed, and the coordinating node does not hold keys. It needs no fork. The @kloakprotocol account said in January 2026 that the node tree was public and the wallet was still in private development. A fetch of `kloakprotocol/kloak_node` returned 404 on 13 Oct 2026, so this note does not claim a running network. Kloak, as described, changes one payment's shape. It does not hide an IP session.

Whonix's kloak is a different program. It blurs keystroke and mouse timing. It is not a Kaspa protocol.

The 1984 square's OLAF note is a third thing. It is a desk-side sealed dollar note. The square's own description sets `proof` and `zk` to false. It is not Weller's initiative, and it is not a settlement covenant.

A KasRanks post about "PhantomPool" (8 wallets in, 2 out, a STARK, Testnet-10) was not checked against chain data for this note. It is not used as evidence.

### OpenMiner

Luke Dunshea posted OpenMiner on 1 Oct 2026.

Source: https://x.com/elldeeone/status/2105643604446494781

The goal he states has three stages:

1. Understand the chips and write down what was tested and what is unknown.
2. Take control of miners people already own: open software, logs, fans, power schedules, and later limits and clocks.
3. Move chips into new form factors, and only later consider a machine of one's own.

He names Bitaxe and the open Bitcoin-miner scene as the model. He says the platform repository, with executable code, comes later. The public repository today is the reference:

https://github.com/elldeeone/openminer-reference (tree `478de854`, read 13 Oct 2026)

The README is explicit. This is community research, not a manufacturer datasheet. The repo holds documentation and static evidence. Drivers and bench tools belong elsewhere.

Two chips are documented:

| Chip | Machine he tested | What is demonstrated | What is still open |
| --- | --- | --- | --- |
| Bitmain BM2382 | Antminer KS5 Pro, two of three hashboards, because the site could not feed all three | Stock-hashboard UART control: init of two 92-chip chains, Kaspa jobs, accepted shares, stop and recovery | Package, pins, rails, clock, and any new board. A one-chip miner is not demonstrated. |
| Goldshell IEN616 | KA Box, firmware 2.2.2, one stock hashboard, controller `BoxII_CoreBoard V2.0` | After stock software has prepared the board: SPI control, Kaspa work, 121 accepted shares in 120 seconds (run 89), both fans | Cold boot without stock preparation. Electrical map. A new board. The Rust driver stays in a private research repo. Investigation closed 7 Oct 2026 at the measurement limit. |

IceRiver, including the KS0 Ultra, is not in the reference. Neither file describes a Kaspa full node on the controller, a secure element, a traffic path, or an access credential.

### Home-miner scale

Public hashrate readings on 13 Oct 2026 cluster around 360 PH/s. `https://api.kaspa.org/info/hashrate` returned `362979.7`. 2miners showed 368.42 PH/s. Those match if the API field is terahashes per second, which is how that field has been read against pool charts. This note uses 363 PH/s and shows the arithmetic so it can be redone.

Kaspa produces 10 blocks per second, so 864,000 blocks per day.

| Machine | Hashrate | Expected blocks per day at 363 PH/s | Power and noise from vendor and reviewer pages |
| --- | --- | --- | --- |
| IceRiver KS0 Ultra | 400 GH/s = 0.4 TH/s | `864000 × 0.4 / 362980 ≈ 0.95` | 100 W. Noise listed at 35 dB or 40 dB. |
| Goldshell KA Box | 1.18 TH/s | `864000 × 1.18 / 362980 ≈ 2.8` | 400 W. Noise listed at 35 dB by sellers. One test writeup reported about 55 to 60 dB. OpenMiner ran this class of board. |
| Antminer KS5 Pro | 21 TH/s on a vendor comparison table | about 50 | 3150 W, 75 dB on that same table. OpenMiner's test site could not power three hashboards. |

Vendor pages differ by a few percent on hashrate and by a lot on noise. The order of magnitude is the part that matters. A quiet home box finds on the order of one to three blocks a day. A KS5-class machine is a 3 kW, 75 dB miner.

September 2026's scheduled subsidy step was 2.182 KAS per block, with a further monthly cut of about 5.6 percent (2miners, 4 Oct 2026). A KS0 Ultra that finds one block a day collects on the order of 2 KAS of subsidy that day, plus whatever fee happened to be in that block. It does not buy a place in anyone else's block. A market print on 8 Oct 2026 put KAS near $0.040. At that print, the subsidy is cents, and 2.4 kWh for a 100 W box costs more than the coins on any ordinary household tariff. Pool "profit" tables the same week show these home boxes losing money after power. Treat those dollar figures as illustrations from public pages, not as a quote.

Solo coinbase outputs are public. A pool sees the worker's connection and the shares. Either way, mining publishes a relationship that a privacy system would rather not publish.

### Tor, in one paragraph

Tor is a low-latency onion network. The 2004 design paper (Dingledine, Mathewson, Syverson) chooses latency over the delays that would defeat traffic confirmation, and it says a watcher who can see both ends of a circuit can correlate them. Relays are a public directory, so an ISP can block them unless the client uses a bridge. An exit relay sees the destination of clearnet traffic. An onion service hides the server's address and removes the exit. Throughput is whatever the relays have, and it is enough for ordinary web use because the bytes travel on the relays, not inside a 125 KB block.

### Today, versus a later fork

Already possible on mainnet and on Testnet-10:

- A local rusty-kaspa node, a wallet, and ordinary private-key control of KAS.
- Covenants, covenant ids, Groth16 and RISC0-Succinct verification, and lane commitments.
- Small payloads: names, offers, proofs, app operations. Mass is the limit.
- A PayJoin-style payment if both wallets cooperate (Kloak's idea), with amounts still visible.
- Open documentation for talking to a stock KS5 Pro hashboard and a stock-prepared KA Box. No public firmware image that replaces the vendor stack. No new board.

Not available:

- A production shielded KAS pool. KPI is early, and its next stated step is a Based App trial without the privacy circuit.
- vProgs as a stable place to run an application.
- A Kaspa path that can carry web or video bytes for more than a toy.
- An open access appliance built from a Kaspa ASIC. OpenMiner's stage 3 is a hope, and the chips he has opened are a 3 kW KS5 and a 400 W KA Box, not a KS0 Ultra.
- Any opcode, lane, or miner protocol whose job is "this person owns a box, so route their packets."

## Why

The draft fails where it asks Kaspa to carry the session and a miner to open the door.

The extra layer, as a faster and stronger Tor that runs on Kaspa and opens when a miner is in the path, is not feasible.

Kaspa can order and settle small messages, and it can check a proof. It cannot be the pipe. A home ASIC can hash kHeavyHash. It cannot be a credential, a router, or a privacy proof. OpenMiner is a serious project about owning the hasher you already bought. It does not become an anonymity layer by being placed between the wall and the laptop.

The part that is feasible, and already mostly exists, is smaller:

- The site stays a normal service, reached through Tor if the operator wants the server address and the client address off the public web.
- The user runs their own Kaspa node on a computer they control.
- A key that lives on that computer is the access check, if a check is wanted.
- Private KAS, if it arrives, arrives through the Kaspa Privacy Initiative or a design of the same kind. This repo should not start a second one.

These are the limits that bind the idea.

**The device ends the LAN session.** The path in the brief is internet, then router, then the hardware, then the user. A session that terminates on that hardware is visible to it: destination, timing, and size. End-to-end encryption keeps the body confidential. The device still holds the session metadata. A closed controller, or a host the user cannot inspect, records that metadata. The hash chip returns nonces. The host software is what sees the traffic.

**Filtering happens on the link in front of the box.** An ISP can drop the Kaspa peer port, the pool port, and the public Tor directory. Tor bridges and pluggable transports exist because that directory is blockable. A block subsidy is paid on a peer connection that has already succeeded.

**Throughput and confirmation answer different questions.** Tor carries one user's bytes at relay speed. Its published limit is correlation by a watcher who sees both ends of the circuit (Dingledine, Mathewson, Syverson, 2004). Kaspa carries transactions in blocks of about 125 KB, ten times a second: about 1.25 MB/s when the entire mass budget is bytes. A session at one megabit per second is larger than that network-wide ceiling. One small transaction still confirms on the order of a second. Economic finality is a further depth of the DAG. Those times describe a payment or an app operation inside the mass limit. A stream of web bytes placed in payloads is public to every indexer and spends the block budget.

**Mining writes public links.** A solo block records a coinbase address. A pool session records the worker's IP and the shares. A payload is stored in full for every indexer. A privacy design counts those links, or it keeps the hasher off the access path.

**A home miner's output is its own subsidy.** At about 363 PH/s, a 400 GH/s box expects about one block a day out of 864,000. At the September 2026 subsidy of 2.182 KAS per block, that is on the order of 2 KAS before power, plus whatever fee was already inside the block it found. Fee bids in every other block are set by the people who sent those transactions. A login check that accepts hashrate admits anyone who can rent that hashrate. The owner is locked out while the box is off, and in the gap between blocks.

**OpenMiner's evidence ends at the stock board.** The 1 Oct 2026 post sets three stages: document the chip, control miners people already own, and later explore new boards. Reference tree `478de854` shows UART control of a KS5 Pro hashboard and SPI control of a stock-prepared KA Box. Package pins, rails, and a new board stay open on both chips. The IEN616 driver remains in a private repository. The post is the primary source for that measured control and for that roadmap. A board sold as an access appliance is later work.

**Shielded native KAS already has a public research track.** Weller's initiative is native KAS, with no new token, and with no protocol marked final. Sutton wrote on 13 Oct 2026 that the based-bridge shape fits that work. A second shielded pool in this repo repeats an unfinished circuit and splits the reviewers. The 1984 OLAF note reports `proof: false` and `zk: false`. Describing that note as the initiative publishes a false privacy claim.

**Ten blocks a second changes how covenant state is laid out.** The docs use that rate so many independent UTXOs can move together. Sutton describes a run of small responsive programs. The Kaspa account describes app updates carried beside payments. Each of those lives inside the block mass limit. The byte stream of a user session stays on the transport plane.

**Real funds stay out.** KPI gives the same instruction for their own code. The mass measurement and any later trial run on Testnet-10.

## How

Three planes, a Testnet-10 check that stops after two measurements, and the hardware that check actually needs.

### Planes

Do not build a packet overlay on Kaspa. Build three separate planes, and only build the one this project can actually finish.

**Transport.** Tor onion service in front of the application. The client circuit ends at the service. The ISP sees a connection to a Tor guard, or to a bridge, which is the same problem Tor already works on. Kaspa peers are not that circuit. Putting the same bytes into a payload would publish them to every indexer and would fill the block mass limit.

**Access.** A signing key generated on the user's machine. Enrollment is "the owner added this public key," kept by the user, with a second key as a backup. Login is a signature over a fresh challenge. There is no manufacturer list and no serial-number server. Those are a single point of failure and a map of customers.

The machine should be the same one that runs the Kaspa node, because that is the box the user can actually inspect. A Raspberry Pi-class computer or an ordinary PC can do this. The ASIC, if one is plugged in beside it, hashes. It does not hold the access key and it does not see the traffic. OpenMiner's own split, controller versus hashboard, is the right picture: the chip returns nonces, the host decides what they mean.

**Settlement.** Wallets and covenants on Kaspa. Public tKAS when the user accepts a public payment. KPI's shielded pool when, and only when, that project has a reviewed testnet design and the user is still on play money. KCC-20 is relevant only if a later app needs a fungible note that covenants can already express. It is not a privacy tool and it is not final.

**What 1984 would do.** Stay a website. The onion service is the private door. The device key is an optional gate in front of that door. Payments that the square already understands stay on Testnet-10 until someone deliberately moves a conservative design to mainnet. The town's pages, video, and chat do not go into transaction payloads. A content hash or a short offer can. The body stays on the web server or on the user's disk.

**Privacy model, stated narrowly.**

- The ISP sees that a Tor client is running, unless a bridge disguises it. The ISP does not see the site, if the site is an onion service.
- Tor relays and a global watcher remain the Tor threat model. Kaspa does not remove traffic correlation.
- Kaspa peers of a local node see that node's IP. This evaluation did not confirm a supported Tor mode for rusty-kaspa peer links. Assume the node is visible to its peers until that is checked.
- A public node operator sees the user's IP and the user's requests. Use one only for a test.
- Other users of the site see what the application shows them. The chain shows what the transaction shows. A public payment shows amount, inputs, and outputs. A future shielded pool is supposed to hide those, and it does not exist yet. Lane payloads of a Based App are public even when the proof hides the state. Weller's own note says that leak has to be explained.
- The manufacturer of a PC learns whatever the PC already phones home. An open miner controller reduces one vendor. OpenMiner has not shipped that controller.
- Indexers see every payload, every covenant transition, and every coinbase. Design so that the sensitive bytes are not in those fields.

**Mining's role.** Economic and optional. A home box finds about one to three blocks a day and collects that block's subsidy. It does not fund the fee market, does not make the user hard to block, and does not prove that the human at the keyboard holds a particular device. Require it as a gate and the gate fails open for anyone who can rent hashrate, and fails closed for the owner whenever luck or the pool is down.

**Nodes.** Full node on the user's computer for anything that is supposed to be private. Light client plus somebody else's node is acceptable for a Testnet-10 demo and publishes the demo to that somebody.

### Testnet-10 check

One pass, play money, no new token, no packet tunnel, no second privacy protocol, no new kaspad process if this desk already has one.

1. Leave this note in this repository. Leave it off the 1984 square.
2. Offline access check. Generate a key on one machine, sign a random challenge, verify the signature in a second process. Write down what the verifier learns: a public key, and the IP of whatever connection delivered the signature. That IP line is the entire reason the transport plane is Tor.
3. One mass measurement, read-only against the existing Testnet-10 node, a single transaction built and not sprayed. Try payloads of 1 KB, 10 KB, and 50 KB. Record compute mass, transient mass, and the standardness rejection at 100,000 grams. Publish the numbers next to the 125 KB block ceiling so the "we will carry traffic later" sentence has a measurement under it.
4. Watch KPI's Based App trial. The trial they described on 13 Oct 2026 is the right experiment: lane order, execution, settlement, a deliberate omission, reconstruction by someone else. Do not fork it and do not start the shielded circuit in this repo.
5. Stop. A passing signature test plus a mass table is the POC. A miner image, a custom board, and a mainnet gate are out of scope.

### Hardware

Use a small computer the user can open, and run the Testnet-10 node there. That computer holds the access key. Any current PC that already runs rusty-kaspa meets this bar. A quiet single-board computer is enough for a node and a signature check. It is the access device.

Leave the ASIC out of the access path.

If the separate goal is "own the miner," follow OpenMiner in the order Dunshea published:

- Read the reference before buying anything.
- The machine his public notes actually cover, at a size that can sit in a room, is the Goldshell KA Box class: IEN616, 400 W, stock firmware 2.2.2, control only after stock software has prepared the board. The driver is not in the public repo. Independent cold boot and a new board are untested. Noise may be closer to 55 dB than to the 35 dB on seller pages. Budget 400 W of heat.
- Do not buy an Antminer KS5 Pro as an access appliance. OpenMiner's own test left one hashboard off because the site could not power three. Vendor figures for that class are about 3 kW and 75 dB.
- Do not design a KS0 Ultra form factor under the OpenMiner name. The reference has no IceRiver chip. The KS0 Ultra is a reasonable quiet hobby miner on its own terms (400 GH/s, 100 W, about one block a day). It is not evidence for an open access device.
- Wait for the platform repo he said he would publish before expecting software that runs a miner without his private tree.

A secure element would be required before "possession" means more than "a file on a disk." OpenMiner does not provide one. A copied key is a copied user. Say that in the product, or do not say "hardware-gated."

### Open questions

- Confirm the `api.kaspa.org` hashrate unit against one pool's published petahash figure on the day a miner is actually measured. The block-rate table scales with that number. The verdict does not.
- Does any KA Box controller have the RAM and the thermal margin to run rusty-kaspa next to the miner process? The reference does not say. Measure before claiming it.
- Does current rusty-kaspa expose a supported way to dial peers through Tor? If it does not, a "private node" is private from public RPC operators and visible to its peers.
- What does a real KIP-21 lane payload contain for KPI's trial, and which fields stay public? Read their PoC when it lands. Do not guess a circuit.
- What is the Testnet-10 fee, in sompi, for one Groth16 settlement of the size KPI already used? Measure one transaction. Mainnet stays untouched.
- Who would manufacture a secure element, and under what audit? Until that has a name, the access key is a software key.
- Kloak's node tree was not readable at the URL given in January 2026. If the project publishes again, read the code before describing it as available.

## Conclusion

The drafted layer, read as a faster and stronger Tor that runs on Kaspa and opens when a miner is in the path, does not fit the chain or the hardware.

What can be finished is already separate:

- The site stays a normal service. An onion service is the door when the operator wants the server address off the public web.
- The user runs their own Kaspa node on a computer they control.
- A key on that computer is the access check, when a check is wanted.
- Native KAS is the money. Shielded KAS, if it arrives, arrives through the Kaspa Privacy Initiative. This repo starts no second pool.

This repository is a public draft of that reading. The 1984 square stays a square: a raw idea, links to other people's work, and Kaspa core as a separate project. The sentence beside that square, when one is wanted, is an onion service for the door, a local node for the money, and a key on that machine for the gate.

Real funds stay on Testnet-10. The mass measurement and any later trial use play money. The Kaspa Privacy Initiative gives the same instruction for its own code.
