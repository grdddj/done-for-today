# Today

## Education

Mastering Bitcoin

Transactions - https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch06.asciidoc

## Deep work

Ethereum definitions - https://github.com/trezor/definitions
- properly understand all issues - https://github.com/trezor/definitions/issues
- make the script runnable - fix all undefined symbols - compare it with the status in `fw` repo
- add `shell.nix` and `poetry.toml` into the repo (`poetry` > 1.2)
- figure out how to install the needed `trezorlib` with updated protobuf and other stuff - it lives in a branch currently - `marnova/ethereum_defs_from_host`
- cut down all the unnecessary parts
- refactor the code into more files
- could add some quick-and-easy github action jobs - style check, static type check etc. - with a Makefile

## Other work

## Ideas

## Notes

---

# Tomorrow


---

# General TODOs

Click tests for TR - https://github.com/trezor/trezor-firmware/pull/2610
- PIN for TT, passphrase for both

Review https://github.com/trezor/trezor-firmware/pull/2410

Get review of input-flows - https://github.com/trezor/trezor-firmware/pull/2749

Increase spaces between NORMAL font in TR in some cases - https://github.com/trezor/trezor-firmware/issues/2397

---

# Older relevant notes

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

### Ethereum definitions
- parts of solution
    - definitions repo
    - `Suite` drawing binary from definitions repo and using it to supply data to firmware
    - same for `trezorctl`
    - `firmware` must understand the definitions
- how does the signing/verifying the signatures work? Suite/firmware must know how to recognize the signature
  - do we sign each `.dat` file?
  - is there a lot of overhead in signing/verifying the signatures?
- why do we need a binary, why isn't the JSON enough? (how does the `Suite` talk to the binary?)

- https://github.com/trezor/definitions
    - what part does the `trezor-common` play in this? and what about `ethereum-lists`?
    - where does it store that commit hash of the submodule?
    - `trezor-common` maybe not necessary

- https://github.com/trezor/trezor-firmware/blob/472a04a67fcf996823770d001cfdffab05c68463/docs/common/ethereum-definitions.md
    - how big of a subset will be built-in to the firmware? And what will be the process of distinguishing between the built-in and the external? Will there be some differences? ... "blobs are generated also for built-in definitions"
        - might be nice to have a checking we have up-to-date TOP100
    - why having both `by_chain_id` and `by_slip44`, cannot there be some mapping between them? (could reuse `network.dat`)
    - what are the plans with `https://data.trezor.io/eth_definitions/LOOKUP_TYPE/ID/NAME` - to serve as public API for all tokens?
    - `trezorctl` will use the above-mentioned API?
    - `model 1` code is also ready?

- https://github.com/trezor/definitions/blob/8783b5db600196f98de3a1716b63783c482f5b1e/ethereum_definitions.py
    - is there a reason the code above - 1300 lines - is only one file? Like for transfer purposes?

- Data sources (Coingecko etc.)
    - do we have some special agreement/API tokens with them?

- core/src/apps/ethereum/networks.py
    - `slip44==1` always means a testnet, regardless of the network (BTC, ETH...)?
    - each network has its own blockchain?
    - a lot of them have `slip44==60`, which is `ETH`
    - who is deciding `chain_id`? (`slip44` seems to be according https://github.com/satoshilabs/slips/blob/master/slip-0044.md)

- core/src/apps/ethereum/tokens.py
    - these are all on `ETH` mainnet? --- no, `if chain_id == XXX` is deciding that
    - 1 - Ethereum, 3 - Ropsten (test), 4 - Rinkeby (test), 8 - Ubiq, 30 - RSK, 42 - Kovan (test), 61 - Ethereum Classic, 31102 - Ethersocial Network, 43114 - Avalanche C-Chain

- `chain_id` is completely unique, more networks can have the same `slip44`

---

Done for today!
