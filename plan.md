# Today

## Education

Lightning Network - Platby budoucnosti - https://uploads-ssl.webflow.com/5e5fcd39a7ed2643c8f70a6a/63680ec74eedb9a69fb09d49_lightning-network-bitcoin-kniha-braiins.pdf

## Deep work

Tests hackathon

Rebase binsize branch on master

TR - address feedback
- making all the components T: AsRef<str>

## Other work

## Ideas

## Notes

---

# Tomorrow

TR - address feedback
- making all the components T: AsRef<str>

TR - common type
pub trait StringType: AsRef<&str> + From<&'static str> + Clone {}
impl<T> StringType for T where T: ... {}

TR - Coinjoin screens

---

# General TODOs

Ethereum definitions - https://github.com/trezor/definitions
- add `shell.nix` and `poetry.toml` into the repo (`poetry` > 1.2)
- figure out how to install the needed `trezorlib` with updated protobuf and other stuff - it lives in a branch currently - `marnova/ethereum_defs_from_host`

TR - new PIN text - https://github.com/trezor/trezor-firmware/issues/2636
- seems like it is not worth creating a special layout just for getting rid of the arrows

TR PR - suggestions and fixes
- Label reusing
- Coinjoin loaders

TR homescreen - https://github.com/trezor/trezor-firmware/issues/2812
- on hardware, the welcome screen is shown for a very short time before homescreen, most of the time there is black screen

Gettext - translations
- look at OneKey
  - report on the status of translations

---

# Older relevant notes

## TT debug trace
Need to investigate why the trace from TT is not being updated (e.g. during PIN entry in click tests), while TR is.
- TT is using synthetic_events in comparison to TR's `RustLayout`'s `press_XXX` etc. functions - and they call `notify_layout_change`

### Model R self-code review
- change AsRef<str> to Deref<str> for general T types
- investigate whether `obj_place` - `ui_layout_place` is necessary to get all debugging info
- investigate using synthetic events instead of calling `TR`'s `RustLayout`'s `press_XXX` etc. functions (would be possible to see the button highlights, but was causing freezing in some other tests)
  - or vice-versa, use the `click_at` debug function in TT
  - the benefit of using synthetic events is that we capture the UI change of the button being pressed, which is good to have
- try to use `page_count` from `inner.event_ctx.page_count`
- have some common code for `RustLayout` for `TT` and `TR`? A lot of code is the same.
- will we have a strategy for sorting the QSTRings in `core/embed/rust/librust_qstr.h`?
- what to do with all the gdb scripts - update them or delete them?

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

---

Done for today!
