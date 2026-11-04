# Release checklist (iOS and Android)

## Before the build

- [ ] Version and build numbers bumped on both platforms
- [ ] Changelog written: what players will notice
- [ ] Tests and CI green on the release commit
- [ ] Save migration tested: install the live store version, play, update to this build, check the save

## On devices

- [ ] A low-end Android phone and an older iPhone: first session, a full core loop, background and resume
- [ ] Purchases in the sandbox: buy, restore, and what happens after a refund
- [ ] Ads: consent and ATT prompts, rewarded and interstitial flows, no ads where the player paid to remove them
- [ ] Every language: the longest strings fit, and right-to-left and CJK text renders
- [ ] No errors in the Unity console or the device logs

## Store

- [ ] Store text and screenshots up to date for every locale
- [ ] Release notes for every locale
- [ ] Google Play: staged rollout, starting at 10–20%
- [ ] App Store: phased release switched on

## 48 hours after release

- [ ] Crash-free rate and ANRs checked
- [ ] New reviews read and answered
- [ ] Revenue and retention compared with the last version
- [ ] Journal entry: what shipped, what broke, what to change next time
