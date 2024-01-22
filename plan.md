# Today

## Education

Lightning Network - Platby budoucnosti - https://uploads-ssl.webflow.com/5e5fcd39a7ed2643c8f70a6a/63680ec74eedb9a69fb09d49_lightning-network-bitcoin-kniha-braiins.pdf

## Deep work

Manufacturing tests:
- try proof of concept of HTTP communication with HTML frontend

Translations:
- fix all the tests, make sure they are all green
- record tests for all languages

## Other work

## Ideas

## Notes

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

Attribute-based lookup instead of function parameters
TREZOR_MODEL=R PYOPT=0 make build_firmware
Memory region         Used Size  Region Size  %age Used
before
          FLASH2:      750600 B       896 KB     81.81%
after
          FLASH2:      744968 B       896 KB     81.20%

Translations header:
- magic bytes (4 bytes)
- version number (4 bytes)
- language name (8 bytes)
- number of keys (4 bytes) - will compute the offset_table_size from it
- whole data size (4 bytes)
- checksum? (16 bytes)
- signature? (64 bytes)

Offset table:
- is it even necessary to add this complexity?
- looping through at most 32 kb of data should not be that slow even on hardware
- the offset table is adding additional storage - 2 bytes (u16) per key
--- however, when having the offset table, we could get rid of the delimiter completely (so the cost is 1 byte per key)

Translations backwards compatibility issue:
- what happens when the translations are newer/older than the firmware?
- might try to setup some benchmark/tests on the hardware device
- homescreen might have a banner "NEEDS TRANSLATIONS" when the translations are not up-to-date after firmware update
- new translations should be stored after the already existing ones
- centralizing the order of keys and only adding new ones at the end
--- the deleted ones may never be deleted, as they might be used in the old translations
- we can enforce that older firmwares can never use newer translations --- therefore the issue of missing keys should not happen

Font issue:
- how to dynamically supply all the necessary font/glyph characters together with the translation data?
- we would need to have all the special (non-english) characters in all the fonts, which may take a LOT of space
- we have the limitation of 32kb combined with the translations
- TT's fonts in EN+CS+FR have `53 kb`
--- Regular_21 ... 13_564
--- DemiBold_21 ... 14_824
- extra_size 22890
--- {'TTHoves_DemiBold_21': 6592, 'TTHoves_Regular_21': 5998, 'RobotoMono_Medium_20': 5705, 'TTHoves_Bold_17': 4595}
- czech_sum 11754
- we might have just one font for the translations
- on TR, the fonts are MUCH smaller, like 10 times

Transforming into regular PR, so it is ready for review.

There is still quite some work, but I think it makes sense to stop now and analyze the status and create clear requirements for what should be done.

Current high-level status:
- contains translations into czech and french, both done by ChatGPT, so the quality is rather poor
- translations are centralized in JSON files - en.json, cs.json, fr.json - in `core/embed/rust/src/ui/translations`
- english translations are embedded/hardcoded in the code, they will be there all the time, acting as a backup/default
- foreign translations are stored in two `16 kb` sectors, which is enough right now for the translations data
- fonts/glyphs are currently hardcoded into the firmware, so both czech/french fonts are there all the time
- trezorctl is responsible for generating the translation data blob (will be replaced by a custom tool)
- the structure of the blob is a `256-byte` header with metadata and then the translations data delimited by `0x00` byte
- the translations data has around `22kB` for each language, the extra fonts for czech and french have around `12 kB` each (on TT, TR is like `2 kB`)
- on TT hardware, it takes at most `2.5 ms` to load the translation (even with the "inefficient" delimiter storing)
- it increases the firmware size quite a lot, on TT with both the czech/french font, the firmware is `90 kB` bigger - mostly because of storing all the translations keys

Some things/considerations/questions that are missing and should be prioritized:
- we might want to include offset table into the translations blob (it would increase the size by around `1 kB`)
- the translations are not being signed/verified
- we do not yet have logic to append new translations to the existing ones (they should be always at the end)
- firmware update process is not fully accounted for, it should make sure the translations are up-to-date with firmware (tell user that they should update the translations as well)
- it might make sense to not translate altcoins at all, to save space and a lot of translating work (and confusion for users)
- we might want to load fonts dynamically with each language
- with bigger glyph sets, like Azbuka, we might have just one font, instead of four/five
- `TT` has one extra 16kb sector (occupied by secret on `TR`), which might be used specifically for the font data (fonts on TR are much smaller, they may fit together with the translations)
- in TR, how is it with the last `16 kb` sector? could we safely use it? (also on `TT`)
- it would be nice to use some data compression (for both translations and font data), but we have a limitation on the Rust side of no-std libraries only and no dynamic memory allocation ... also, it would mean some runtime CPU and RAM overhead

Language	Standard Letters Shared with English	Unique/Special Letters	Number of Unique/Special Letters
English	26		0
French	26	é, è, ê, ë, à, â, æ, ô, œ, ù, û, ü, ç, ï, î	15
Spanish	26	á, é, í, ó, ú, ü, ñ, ¡, ¿	9
German	26	ä, ö, ü, ß	4
Italian	26	à, è, é, ì, ò, ù	6
Portuguese	26	á, â, ã, à, ç, é, ê, í, ó, ô, õ, ú, ü	13
Dutch	26	é, ë, ï, ö, ù, â, ê, î, ô, û, ÿ	11
Polish	26	ą, ć, ę, ł, ń, ó, ś, ź, ż	9
Russian	Different Script	Whole Cyrillic Alphabet	33
Ukrainian	Different Script	Whole Cyrillic Alphabet	33

Change language flow:
- might also send the question "Do you want to change the language?" in the new language

Hardware translations performance:
- one retrieving of the translation ranges from 1 to 2.5 ms, based on the position in the blob

Translations compression (including font data):
- decompression will decrease the size, but also slow down the process and increase the RAM/CPU usage
--- also, it will not be available as &'static anymore

Trezor Translations guide - maximal length of title/button
TT:
- title ... "CONFIRM TYPED DATA" ... 18 chars + 2 (can fit) = 20 chars
- button ... "HOLD TO CONFIRM" ... 15 chars
- button left to cancel ... "SKIP" ... 4 chars + 1 (can fit) = 5 chars
TR:
- title ... "WIPE CODE SETTINGS" ... 18 chars + 1 (can fit) = 19 chars
- button ... "HOLD TO CONFIRM" ... 15 chars
- middle button single ... "OK, I UNDERSTAND" ... 16 chars + 2 (can fit) = 18 chars
- middle button with others ... "CONFIRM" ... 7 chars + 4 (can fit) = 11 chars

UI design step-by-step guide
- 0) create new sections/columns in `Figma` model-pages:
--- `new design` section - will show proposed design changes - where both the old and new design will be shown side-by-side
--- `history/accepted` section - will show all the changes that were already implemented and accepted, with connections to the `PR`s
----- (unless `Figma` has some concept of commits)
- 1) copy the old screen into `new design` section, so we know its previous state and what changed
- 2) mark the old screens in `main` section as being `in progress` (a hint to look to `new design` section for the proposed changes)
- 3) do some changes in `new design` section - leaving the old screen as it is for comparison
- 4) create `GitHub` issue with screenshots of the proposed changes
- ... developers implement the changes ...
- 5) review and test the UI changes (via UI tests, emulator, or preferably with HW device to show "real" colors/feel, when something new is done)
- 6) move the pages from `new design` section to `history/accepted` section, mark the date and `PR` that did the changes
- 7) replace the old screens in `main` section with the new ones
- 8) remove the `in progress` mark from the replaced screens in `main` section

Crowdin translation strings upload:
- do it manually via the script when some new strings are added
- in CI we should have only a checker that will fail if the translations are not up-to-date (not to give it commit rights)

Translations possible issues:
- need to be signed
- updating process - should be done together with firmware update in Suite?
--- homescreen may show "NEEDS TRANSLATIONS" banner
- new strings creation - put them at the end of the blob
- crowdin translations upload and download
- handling of plurals - not yet done

Translations update scenarios:
- we should only allow to upload translations that have the same version as current firmware

- firmware and translation same version
--- OK
- firmware newer than translations
--- there might be some missing strings in the translations
--- these strings will default to english
--- we will notice user that they should update the translations ... can be done in Suite as well
- translations newer than firmware
--- should only happen when user downgrades the firmware, and that is his problem
--- theoretically there might be missing strings in the translations, as they were removed --- unlikely but possible
--- still, there is a fallback to english

Translations verification process:
- the only 100 percent solution is to run all the UI tests
- we could have some script defining the maximum length (in rows) of each screen
  - however, each model is different, and it uses different fonts

Should we run the device/click tests in non-english only when the translations have changed from master?

After rebasing on Solana, there is only 11 kB left in flash2

# Tomorrow

Translations:
- agree on how to verify/validate the translations with the agency

Manufacturing tests:
- continue with the HTTP communication

---

# Later

On-demand update of the UI screens

Translations:
- consider using data compression

Vertically centering the content - investigate it more deeply

TR - internal error screen

Trezorlib documentation PR

---

# General TODOs

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
