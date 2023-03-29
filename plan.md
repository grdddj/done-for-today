# Today

## Education

Mastering Bitcoin

Blockchain Applications - https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch12.asciidoc

## Deep work

Review TT UI2 Coinjoin

TR homescreen - https://github.com/trezor/trezor-firmware/issues/2812
-  locking homescreen via button-hold

TR - couple of UI and test fixes

## Other work

## Ideas

Could have a script to validate Rust API generated from layout.rs
- checking the default values and types

Options how to quickly spawn some UI scenarios, e.g. multisig receive, through pytest
- currently it is possible sometimes with INTERACT=1, but it is not very convenient

## Notes

---

# Tomorrow

Persistence tests - try to record UI screenshots

---

# General TODOs

Ethereum definitions - https://github.com/trezor/definitions
- add `shell.nix` and `poetry.toml` into the repo (`poetry` > 1.2)
- figure out how to install the needed `trezorlib` with updated protobuf and other stuff - it lives in a branch currently - `marnova/ethereum_defs_from_host`

Get review of input-flows - https://github.com/trezor/trezor-firmware/pull/2749

Increase spaces between NORMAL font in TR in some cases - https://github.com/trezor/trezor-firmware/issues/2397

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
- change show_tutorial from default=true to default=false? Create specific message spawning tutorial
- change AsRef<str> to Deref<str> for general T types
- could get rid of both `QRCode` in text/layout.rs and the `multiline_text` in display/mod.rs by calling the QR code as a single layout
- move `GlyphMetrics` to display/font.rs?
- investigate whether `obj_place` - `ui_layout_place` is necessary to get all debugging info
- investigate using synthetic events instead of calling `TR`'s `RustLayout`'s `press_XXX` etc. functions (would be possible to see the button highlights, but was causing freezing in some other tests)
- try to use `page_count` from `inner.event_ctx.page_count`
- sort the functions in `tr/__init__.py` the same way as in `tt_v2/__init__.py`
- have some common code for `RustLayout` for `TT` and `TR`? A lot of code is the same.
- will we have a strategy for sorting the QSTRings in `core/embed/rust/librust_qstr.h`?
- should get rid of `text_r` by deleting all the newlines in strings
- what to do with all the gdb scripts - update them or delete them?

### Click tests
- add PIN cases
- add passphrase cases
- for all the input methods (mnemonic keyboards, PIN, passphrase) do a recording of all the options (letters, numbers, symbols), so we save the complete UI state
- in all cases, test all the options like DELETE or SEE

### Emulator shortcuts
Could we somehow use keyboard shortcuts in the emulator?
- like left-arrow is clicking the left btn, right-arrow is clicking the right btn, etc.
- some things like wiping the device, loading the all all seed, etc.
- could even include some clickable buttons there (but then we are almost reimplementing trezor-user-env)

---

Done for today!
