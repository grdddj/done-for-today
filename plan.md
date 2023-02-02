# Today

## Education

Mastering Bitcoin

https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch04.asciidoc

## Deep work

Addressing the Model R self-code review - https://github.com/trezor/trezor-firmware/pull/2610
- click tests for TR

## Other work

Retrospective and sprint planning

## Ideas

## Notes

### Click tests
- add BIP39 cases
- add PIN cases
- add passphrase cases
- add case for the tutorial (it has HTC and middle-click)
- for all the input methods (mnemonic keyboards, PIN, passphrase) do a recording of all the options (letters, numbers, symbols), so we save the complete UI state
- in all cases, test all the options like DELETE or SEE
- aren't there always different seed words? os_random is mocked but seems not to have effect 9rebase to master)

---

# Tomorrow

Click tests for TR + new click tests features

Remove the dash from line-end in the derivation path - multiline_text probably

EIP712 flow - paginate EIP712 DOMAIN
- https://satoshilabs.gitlab.io/-/trezor/trezor-firmware/-/jobs/3697879485/artifacts/test_ui_report/all_screens.html
- TR_ethereum-test_sign_typed_data.py::test_ethereum_sign_typed_data_show_more_button

---

# General TODOs

Discuss the newlines deletion - https://github.com/trezor/trezor-firmware/pull/2763

Integrate latest changes into new UI reports - https://github.com/trezor/trezor-firmware/pull/2750
- try to use the new diff algorithm for the differing_screens

Get review of input-flows - https://github.com/trezor/trezor-firmware/pull/2749

Get review of HoldToConfirms - https://github.com/trezor/trezor-firmware/pull/2762

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

---

Done for today!
