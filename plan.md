# Today

## Education

Mastering Bitcoin

https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch03.asciidoc

## Deep work

Review of https://github.com/trezor/trezor-firmware/pull/2778

Addressing the Model R self-code review - https://github.com/trezor/trezor-firmware/pull/2610
- enabling expected responses again

## Other work

Model R small design changes - changed button texts, copy, some fixes in device tests etc.

## Ideas

## Notes

### Review of click tests and other UI test changes

- there are no click tests for BIP39 recovery, only for SLIP39 - we should add them to check the keyboard
- new reports do not load correctly, a lot of the pictures are missing, the later the test, the less pictures are there (tried multiple reports in two browsers)
--- e.g. https://satoshilabs.gitlab.io/-/trezor/trezor-firmware/-/jobs/3690613386/artifacts/test_ui_report/all_screens.html
--- e.g. https://satoshilabs.gitlab.io/-/trezor/trezor-firmware/-/jobs/3690613403/artifacts/test_ui_report/all_unique_screens.html
- `make test_emu_ui_multicore` do not work currently (`FileNotFoundError: [Errno 2] No such file or directory: '/home/jmusil/trezor-firmware/tests/ui_tests/reports/test/images/bd83a31d0fc4c23953dfd0d138e4441984e34698ace96aad5308a4ae51b712ae.png'`) - no images are saved there
- what is the purpose of `tests/ui_tests/reports/.keep`?
- how to update all the files/reports in `trezor-firmware/tests/ui_tests/reporting/reports/test`? I like to access the `file:///home/jmusil/trezor-firmware/tests/ui_tests/reporting/reports/test` and see everything there
--- `../tests/show_results.py` opens up the browser with the main report, but the `index.html` is not changed
- why not having the `fixtures.json` indented? Could improve the one-glance readability of the data structure there

---

# Tomorrow

Remove the dash from line-end in the derivation path - multiline_text probably

Addressing the Model R self-code review - https://github.com/trezor/trezor-firmware/pull/2610
- click tests for TR

Close as many TR issues from milestone as possible - https://github.com/trezor/trezor-firmware/milestone/38

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
