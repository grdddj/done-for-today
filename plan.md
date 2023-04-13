# Today

## Education

Lightning Network - Platby budoucnosti - https://uploads-ssl.webflow.com/5e5fcd39a7ed2643c8f70a6a/63680ec74eedb9a69fb09d49_lightning-network-bitcoin-kniha-braiins.pdf

## Deep work

TR CoinJoin screens

## Other work

## Ideas

## Notes

---

# Tomorrow

TR debugging and other TODOs from self-code review

---

# General TODOs

Ethereum definitions - https://github.com/trezor/definitions
- add `shell.nix` and `poetry.toml` into the repo (`poetry` > 1.2)
- figure out how to install the needed `trezorlib` with updated protobuf and other stuff - it lives in a branch currently - `marnova/ethereum_defs_from_host`

TR - new PIN text - https://github.com/trezor/trezor-firmware/issues/2636
- seems like it is not worth creating a special layout just for getting rid of the arrows

TR homescreen - https://github.com/trezor/trezor-firmware/issues/2812
- on hardware, the welcome screen is shown for a very short time before homescreen, most of the time there is black screen

Study miniscript on the Bitcoin socratic seminar

---

# Older relevant notes

## TT debug trace
Need to investigate why the trace from TT is not being updated (e.g. during PIN entry in click tests), while TR is.
- TT is using synthetic_events in comparison to TR's `RustLayout`'s `press_XXX` etc. functions - and they call `notify_layout_change`

### Model R self-code review
- change AsRef<str> to Deref<str> for general T types
- investigate whether `obj_place` - `ui_layout_place` is necessary to get all debugging info
- investigate using synthetic events instead of calling `TR`'s `RustLayout`'s `press_XXX` etc. functions (would be possible to see the button highlights, but was causing freezing in some other tests)
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

---

Done for today!
