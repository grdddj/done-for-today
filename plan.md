# Today

## Education

Lightning Network - Platby budoucnosti - https://uploads-ssl.webflow.com/5e5fcd39a7ed2643c8f70a6a/63680ec74eedb9a69fb09d49_lightning-network-bitcoin-kniha-braiins.pdf

## Deep work

Manufacturing tests:
- final station-type configuration and fixes
- new popups for final
- frontend improvements

## Other work

## Ideas

## Notes

# Tomorrow

Manufacturing tests:
- new inputs
- SQL injection defense
- review what we log
- log structured JSON, not strings
- shutting down the Toradex by a button
- profile the whole test suite ... how long does it take to run
- clean up algocraft scripts
- make the close button always visible in logs popup
- fix "Step XX" text in logs
- find out how to read deditec PIN values ... from manual

# Later

Trezor-user-env:
- allow for frontend-button update of all latest emulators
- GithubActions emulators downloading --- but missing for ARM
- running multiple emulators at once
- update trezorlib after new release

Firmware:
- TS5 UI - keyboard new design (recovery, passphrase)
- remove the check for odd pixels in toiftool

Firmware:
- GH issues

Translations:
- zbyva 2 sekund ... solve the issue with plurals

On-demand update of the UI screens

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
