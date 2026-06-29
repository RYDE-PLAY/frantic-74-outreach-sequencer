# Outreach Sequencer Delivery Report

## Package

- Package: `ryde-play/outreach-sequencer@0.1.0`
- Public URL: `https://runx.ai/x/ryde-play/outreach-sequencer@0.1.0`
- PR: `https://github.com/runxhq/runx/pull/169`
- Source commit: `b315b74f03dc2e8dcfe962db74db7cef5d72a4bc`
- Raw `X.yaml`: `https://raw.githubusercontent.com/RYDE-PLAY/runx/b315b74f03dc2e8dcfe962db74db7cef5d72a4bc/skills/outreach-sequencer/X.yaml`
- Raw `SKILL.md`: `https://raw.githubusercontent.com/RYDE-PLAY/runx/b315b74f03dc2e8dcfe962db74db7cef5d72a4bc/skills/outreach-sequencer/SKILL.md`

## What It Does

`outreach-sequencer` decides whether a contact in an outreach sequence is
eligible for the next touch. It reads the sequence/contact engagement projection,
stops after reply or unsubscribe, enforces `min_days_apart`, records eligible
decisions through a data-store-shaped `append_event`, and emits a typed
`runx.outreach.next_touch.v1` packet.

The skill performs no send and mints no authority. The next-touch packet names a
separate governed `send-as` run that a downstream driver or operator may issue
under its own preflight and approval.

## Verification

- `runx --version`: `runx-cli 0.6.14`
- Publisher: `ryde-play`
- Publish digest: `sha256:0d9095b508a34f10a9c160140c831c5e0696c3e143b6fadcd9de700f13943405`
- Profile digest: `sha256:833d9112570b11d5976c7a16e0f903b70d2a87f8b87f8f44b4f3c1cbab938c9d`
- Hosted registry read: success
- Clean install command: `runx add ryde-play/outreach-sequencer@0.1.0 --registry https://api.runx.ai`
- Hosted-installed harness: passed
- Harness cases:
  - `happy_next_touch`
  - `stop_replied`
  - `stop_missing_sequence_definition`
- Dogfood command: `runx skill ryde-play/outreach-sequencer@0.1.0 --registry https://api.runx.ai`
- Dogfood result: sealed
- Receipt ref: `runx:receipt:sha256:a6344a7b401841bcc54d528ecd14d7e7a58a4346270392a8195cdda3956f62c8`

## Dogfood Observation

The dogfood run selected touch 2 for contact `contact:acme:buyer`, returned
`decision.eligible: true`, read one prior `sent` engagement event from the
projection, appended `outreach.next_touch_selected` with idempotency key
`outreach:seq-demo-001:contact-acme:touch-2`, moved state version 4 to 5, and
emitted a next-touch packet naming `send-as` dispatch by naming.

## Verify Note

`runx verify --receipt <dogfood receipt> --allow-local-development-signatures
--json` reports valid digest and valid content address for the dogfood receipt,
but marks the demo-signer signature invalid with `signature_malformed`. The
exact verdict is published as `verification.json` and `runx-verify.json`. The
registry package itself was published, read back from the hosted registry,
installed cleanly, and harness-tested from the installed copy.

## New User Commands

- Install: `runx add ryde-play/outreach-sequencer@0.1.0 --registry https://api.runx.ai`
- Inspect: `runx registry read ryde-play/outreach-sequencer@0.1.0 --registry https://api.runx.ai --json`
- Run: `runx skill ryde-play/outreach-sequencer@0.1.0 --registry https://api.runx.ai --json`
- Verify: `runx verify --receipt <receipt.json> --json`

## Artifacts

- `evidence.json`: full delivery evidence and observations.
- `verification.json`: compact verification result.
- `registry-read.json`: hosted registry read output.
- `registry-install.json`: hosted registry install output.
- `hosted-installed-harness.json`: harness output from the installed package.
- `registry-dogfood.json`: dogfood run output from the registry ref.
- `runx-add.json`: clean install output.
- `runx-verify.json`: runx verify verdict for the dogfood receipt.
