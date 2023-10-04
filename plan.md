# Today

## Education

Lightning Network - Platby budoucnosti - https://uploads-ssl.webflow.com/5e5fcd39a7ed2643c8f70a6a/63680ec74eedb9a69fb09d49_lightning-network-bitcoin-kniha-braiins.pdf

## Deep work

Update screenshots on https://figma.grdddj.eu/

Translations:
- reading the boundary word in both flash sections

## Other work

## Ideas

Golomb filters
- decreasing the P for mempool filters - the data seem to be smaller with smaller P - https://stefano.brilli.me/gcs/
- send just part of tx_id in the mempool response (e.g. first half, or even less)
- enforce mempool request to be the same type?

- send filter-type from Suite to Blockbook
- add configuration to zero-bytes*16 to block filters

- send M and P in the response, and ONLY M in the request for block
- when not sending the parameter from request, make it optional
---

## Notes

Golomb filters
- increase the P size of mempool filters to something like 23 (more transactions than in one block)
- think about supporting variable P size in the future
- Berlino project - using binary websocket
- gzip and blob

Translations
- storing mappings in JSON files (in nested structure, so it is easy to navigate and know what is used where)
- autogenerating .py and .rs files from JSON with mako templates
--- check how much space they are taking
--- experiment with micropython::const and without it (also with Rust::trait instead of struct)
--- make sure it never goes to RAM and is always in flash
- needs to add new characters into fonts (czech and french)
- handle plurals
- handle {} formatting
- how to handle Micropython vs Rust division? Translators should not care where it is
- how to handle model-specific texts and those that are different?
- translate C and bootloader as well?
- all the languages should contain the same keys
---

Translations issues/problems:
- bootloader cannot import the translation modules
- however, the translations are included even in parts used by it (like buttons or ChoicePage)
- the translations are quite big in size -- just one language, let alone storing multiple languages at once
- how to create the possibility of changing the language from outside without rebuild?
- with large enough flash storage, we should be able to store all languages at once (maybe if we get bigger storage with the next model)
- french translation brings extra 25kb, czech extra 22 kb ... it would be possible to have these three langs all the time
--- we might also not translate the altcoins and have them only in english!!!
--- (it makes very little sense to waste space on (Cardano, Eos, Nem, Stellar, Tezos), when very few people use it and the strings there are very long)
- we might store the current (non-english) language in storage
--- but we should be able to return it as &'static str, which might be a problem
- it is possible to store the byte-array and encode key-value pairs
--- storing the keys as integers takes much less space - however, then mapping would be necessary between keys and integers
- there should always be english translation baked into the firmware, and only other languages pushed into storage
--- (check if something is in storage, and if not, or malformed, use english)
- we should store the translation blob in three flash sectors
--- change MPU - will be hard, secret.c in stm32f4, mpu_config_firmware comment, SECRET_AREA -> TRANSLATIONS_AREA, after it
-  current state of translations means all the UI will be dependent on micropython (unless using String)
--- can limit the upper size to 100 bytes, and then use String
- how to handle validation that the translations are up-to-date?
--- could have some logic in firmware that would compare the supplied translations with internal strings
--- or firmware could have endpoint to list all the keys it currently uses, in order, and it would be up to the client to make sure it sends it correctly
--- what are the data limitations to be streamed to the hardware device? Can we send 50 kb easily to the device? ?(homescreen has the limit of 16 kb)
--- can it, on the other hand, give us a long response with the keys (like 20 kb?)
--- turns out it cannot, and we will need to stream it in chunks
--- therefore, we cannot easily use the ApplySettings message, even though it already has a language field - as we would need to support offset field there as well
- will the translations be signed? (probably not, because it would be too much work for founders)
--- if we really make sure that no malicious code can be supplied to the device by translations, then we do not need signing and we also offer relatively easy way of updating the translations and creating new languages, even by the community
--- is the flash sector executable? could somebody upload some code/instructions there and then call it?
--- e.g. storing a seed phrase in the translations and forcing the firmware to read it from there?
- official translations for the release could be downloaded from Github - direct link to release version of the JSON translations
- how to handle altcoins - translate them at all, or leave them in english?
- how to handle BTC-only translations - should it use the same file with all the altcoins?
- setup a debug endpoint to see the current translations? (e.g. send key and receive value)?
- can we write into the storage even when the device is not initialized?
- we might include a header with some metadata into the blob - version, language name, etc.
- it seems to be impossible to read a word from both sectors at once - probably fill the end of section with ignored bytes
--- who will do the logic - host or Trezor? (could be hard to enforce the longest possible length, maybe have limit of 256 bytes for the longest word?)
- we want to sign the translations - how to deal with that during development?
- how to deal with the replacement of fonts? with many languages the fonts may also need to be dynamically loaded

Ordinals in Trezor
- find out what exactly is missing
- https://xverseapp.notion.site/xverseapp/Sats-Connect-Inscription-Pre-release-6e13b4313bf0454881431c4916b8327b
- just for signing transactions, or also for creating the transaction?
- recognize ordinals are in the wallet, prevent potential loss of funds
- extension to Trezor Suite, not shown be default

Blockbook filters tests
- increase unittest coverage for golomb filters... add zeroed-key (is hardcoded now), add no-ordinals support
- gf.AddAddrDesc(ad, nil)
- create a whole transaction with witness etc.
- use parser and hexadecimal tx data to convert
- hextoBytes

# Tomorrow

Translations:
- reading the boundary word in both flash sections
- link translations.c from unix to stm32f4
- change delimiter from asterisk to 0
- create mapping/dictionary of all the keys
- tests in czech and french
- data validation

---

# Later

Vertically centering the content - investigate it more deeply

TR - internal error screen

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

### Connecting UI tests with Figma screen designs
- create a mapping of Figma flow title (tutorial, recovery, PIN, etc.) to all the test-cases that have the relevant screens (unique only ofc.)
- create a dashboard for each flow with links to all the relevant tests - or maybe group all these test's results on one page
- make sure each screen in Figma has a unique name (Tutorial 1, Tutorial 2, ...)
- add id tags to each screen in the specific UI test-case, so it can be parsed from the test
--- each screen can then be uniquely identified by the test name and screen index
--- TR-click_tests-test_tutorial.py::test_tutorial_again_and_skip#1 ... this picture can then be fetched
- map the Figma unique name to the screen unique name in the test
- have a dashboard with the same titles as in Figma, and images taken from the last master build
--- need to resolve the latest master artifacts somehow
- uncover which screens are missing either in Figma, or in tests - there should be a 1:1 mapping
- goal is NOT to have everything pixel-perfect, goal is to catch obvious errors and to have a quick overview of the current state of the UI
- is it possible to automatically export all the screens from Figma? We might then do a real-time comparison of the similarity
- adding a search field to find screens with given text
Number the screens on the UI report, so they are easily identifiable. Also, allow for <URL>#<screen_id> to visit the screen directly.
Try to generate a list of all unique screens to a specific design - e.g. tutorial, recovery, etc.

---

Done for today!
