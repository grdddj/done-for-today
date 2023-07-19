# Today

## Education

Lightning Network - Platby budoucnosti - https://uploads-ssl.webflow.com/5e5fcd39a7ed2643c8f70a6a/63680ec74eedb9a69fb09d49_lightning-network-bitcoin-kniha-braiins.pdf

## Deep work

Blockbook filters - fix initial sync

TT UI PRs

## Other work

## Ideas

## Notes

---

# Tomorrow

TT UI PRs

---

# Later

Trezorlib documentation PR

---

# General TODOs

Ethereum definitions - https://github.com/trezor/definitions
- add `shell.nix` and `poetry.toml` into the repo (`poetry` > 1.2)
- figure out how to install the needed `trezorlib` with updated protobuf and other stuff - it lives in a branch currently - `marnova/ethereum_defs_from_host`

Gettext - translations
- look at OneKey
  - report on the status of translations

---

# Older relevant notes

### Emulator shortcuts
Could we somehow use keyboard shortcuts in the emulator?
- like left-arrow is clicking the left btn, right-arrow is clicking the right btn, etc.
- some things like wiping the device, loading the all all seed, etc.
- could even include some clickable buttons there (but then we are almost reimplementing trezor-user-env)

### Emu UI testing for people
Options how to quickly spawn some UI scenarios, e.g. multisig receive, through pytest
- currently it is possible sometimes with INTERACT=1, but it is not very convenient

### Hardware profiling
It could be possible to do some hardware profiling/experiments using print traces
- something like getting object sizes, stack sizes, how long some actions take, etc.
- with the use of trace recording via `tio` - https://github.com/tio/tio
- `nix-shell -p tio`
- `sudo tio /dev/ttyACM0`
- killing it with `Ctrl+t` and `q`

### Miniscript
- https://blog.ledger.com/miniscript-is-coming/
- https://bitcoin.sipa.be/miniscript/
- a tool for developers to write better and more secure Bitcoin Scripts
- allows for combining multiple policies into one - allows for creating verified libraries of policies
  - something like ETH has a smart contract library OpenZeppelin
- like a high-level language that compiles into Bitcoin Scripts with some checks and optimizations
- comparison - instead of writing C, you could write python and have everything much more readable
  - but even better, it is doing some checks and optimizations for you, so it is like writing Assembly vs writing Rust
- https://github.com/rust-bitcoin/rust-miniscript
  - it has no-std support, we could use it in the firmware

### Mempool RBF functioning
- is it possible to be gradually increasing the fee - by one satoshi per each increase - and to spam the network like this?
  - (is there some limit of how many times one UTXO could be RBF-eed?)
- could the transaction be retrospectively verified that it was valid? even after the RBF-eed UTXO is spent?
- it could be theoretically used as kind of a chat/communication channel between people with BTC nodes
  - arbitrary messages could be send in the inscription-envelope
- assumption: BTC full-node synced with network is be biggest source of truth you can get online
  - when I check that the message is signed by some public key, I can be sure it really comes from that person
- the transaction does not have to be fully validated in blockchain to have a meaning
- one can send it to the same address it originated from, so even if it mined, nothing so bad happens
  - when fee-rate is 1sat/b, I would pay almost nothing for the transaction (4 cents currently), even if it got mined
- incredible possibilities for censorship resistance
- also some interesting services could be created - like "Only messages coming from addresses with > 0.1 BTC will be shown"

### Emulator more models
Is it possible to have both TR and TT emulator builds at the same time? It would be useful e.g. for testing and comparing.


### Dev server usage
Use the dev/test server for building things mostly for product people.

---

Done for today!
