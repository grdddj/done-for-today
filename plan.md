# Today

## Education

Mastering Bitcoin

https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch01.asciidoc
https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch02.asciidoc

## Deep work

Improving the model R code quality (self-code review) - https://github.com/trezor/trezor-firmware/pull/2610

## Other work

Setting up the firmware development environment with Hans

## Ideas

Is it worth still having so many model R commits, or should we squash them all into one? Would make the merging much easier.

## Notes

### Model R self-code review

- implement click tests for TR - after click tests support screen recording
- change show_tutorial from default=true to default=false?
- ellipsis_icon and prev_page_ellipsis_icon as Option<Icon> instead of Option<[u8]>
- change AsRef<str> to Deref<str> for general T types
- could get rid of both `QRCode` in text/layout.rs and the `multiline_text` in display/mod.rs by calling the QR code as a single layout
- move `GlyphMetrics` to display/font.rs?
- investigate whether `obj_place` - `ui_layout_place` is necessary to get all debugging info
- try to use `page_count` from `inner.event_ctx.page_count`
- will we have a strategy for sorting the QSTRings in `core/embed/rust/librust_qstr.h`?
- should get rid of `text_r` by deleting all the newlines in strings
- create `progress.rs` and put progress objects there
- delete all unused UI stuff from `core/src/trezor/ui/__init__.py` after merge - and run `make unused`
- what to do with all the gdb scripts - update them or delete them?
- expected responses are not being checked - we should reenable them

---

# Tomorrow

Showing account names in send/receive/passphrase dialogs https://github.com/trezor/trezor-firmware/pull/2736
- study the code, try to implement it for model R

---

# General TODOs

Merge T1 UI removal - https://github.com/trezor/trezor-firmware/pull/2761

Discuss the usage of Formatted text (reintroduced in new UI2)

Discuss the newlines deletion - https://github.com/trezor/trezor-firmware/pull/2763

---

Done for today!
