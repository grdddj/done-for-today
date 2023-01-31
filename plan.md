# Today

## Education

Mastering Bitcoin

https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch02.asciidoc

## Deep work

Showing account names in send/receive/passphrase dialogs - https://github.com/trezor/trezor-firmware/pull/2736
- study the code, try to implement it for model R

Code review of https://github.com/trezor/trezor-firmware/pull/2748
- with discussion about the FormattedText

Squash and rebase of https://github.com/trezor/trezor-firmware/pull/2610

Addressing the Model R self-code review - https://github.com/trezor/trezor-firmware/pull/2610

## Other work

Got to merge T1 UI removal - https://github.com/trezor/trezor-firmware/pull/2761

## Ideas

## Notes

### Showing account names
- blocker - model R designs require also showing the passphrase identification, which we do not yet have logic for

---

# Tomorrow

Addressing the Model R self-code review - https://github.com/trezor/trezor-firmware/pull/2610

Review of https://github.com/trezor/trezor-firmware/pull/2778

---

# General TODOs

Discuss the usage of Formatted text (reintroduced in new UI2)

Discuss the newlines deletion - https://github.com/trezor/trezor-firmware/pull/2763

---

# Older relevant notes

### Model R self-code review

- implement click tests for TR - after click tests support screen recording
- change show_tutorial from default=true to default=false?
- change AsRef<str> to Deref<str> for general T types
- could get rid of both `QRCode` in text/layout.rs and the `multiline_text` in display/mod.rs by calling the QR code as a single layout
- move `GlyphMetrics` to display/font.rs?
- investigate whether `obj_place` - `ui_layout_place` is necessary to get all debugging info
- try to use `page_count` from `inner.event_ctx.page_count`
- sort the functions in `tr/__init__.py` the same way as in `tt_v2/__init__.py`
- have some common code for `RustLayout` for `TT` and `TR`? A lot of code is the same.
- will we have a strategy for sorting the QSTRings in `core/embed/rust/librust_qstr.h`?
- should get rid of `text_r` by deleting all the newlines in strings
- what to do with all the gdb scripts - update them or delete them?
- expected responses are not being checked - we should reenable them

---

Done for today!
