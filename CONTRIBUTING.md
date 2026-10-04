# Contributing to Dishylink

Thanks for taking an interest. Dishylink talks to real Starlink hardware, so a
couple of the rules below are about not breaking someone's internet rather than
about code style — please read the hardware section before touching anything
that polls the dish or router.

## Before you start

Most of the app can be worked on from anywhere, but anything that reads live
telemetry needs you to be **on the Starlink network itself**. The dish answers on
`192.168.100.1` and the router on `192.168.1.1`; neither is reachable from
outside the LAN, and there is no public test fixture that behaves like real
hardware under load.

If you can't get on a Starlink network, good areas to help with are the charts,
the recorded-history views, tests, and documentation — all of which run against
recorded or synthetic data.

## Hardware safety

The router is a small embedded device and has rebooted under ordinary polling
load. Two rules follow from that:

- **Never call the router's `get_ping` (field 1009), at any cadence.** It was
  trialled at 2s, 5s and 30s; each trial was followed within ~15 minutes by a
  router watchdog reboot that took the network down. Router ping success is
  already available from `get_status`'s `popPingDropRate5m`, which rides a reply
  the app is fetching anyway.
- **Don't add a new poll against the dish or router.** Reuse a reply that is
  already being fetched — `routerStatusFeed` in the browser, or the existing
  status poll in the recorder. If you genuinely need a new one, raise an issue
  first so it can be discussed before anyone's link goes down.

Custom DNS, bypass mode and content filtering are deliberately not exposed: a
bad write there can take the WiFi down until a physical reset.

## Running it

These are source development paths, not published fork downloads. Clone the fork:

```bash
git clone https://github.com/ToneyStuck/Dishylink.git
cd Dishylink
npm install

npm run dev              # web dev harness, requires the Starlink LAN
npm run dev:electron     # desktop app (macOS, Linux)
npm run dev:electron:win # desktop app (Windows)
npm run dev:extension    # browser extension (Chrome, Edge, Firefox)
```

The three products are independent: they don't share a runtime, and each polls
and records on its own. A change to shared code under `src/` affects all three,
so check the one you didn't intend to touch.

## Checks

CI runs on every push and pull request, and must be green before a PR is merged:

```bash
npm run typecheck            # tsc -b
npm run lint                 # eslint, warnings fail the build
npm test                     # vitest
npm run format               # prettier, fixes the tree in place
npm run typecheck:extension  # extension-specific types
```

Formatting is only enforced on files a change touches, so you won't be asked to
reformat code you didn't write.

Tests run in Node except for a few extension files that need real IndexedDB;
those run in headless Chromium via Playwright. `npx playwright install chromium`
once if you haven't got it.

## Pull requests

- Branch off `master`, one topic per PR.
- **Label the PR** `enhancement`, `bug` or `documentation`. Release notes are
  generated from these labels, so an unlabelled PR lands under "Other Changes".
- Describe what you changed and, for anything touching the dish or router, how
  you verified it against real hardware.

## Releasing

For maintainers:

```bash
npm version minor        # bumps package.json, commits, and tags
git push --follow-tags
```

The commands above are the original upstream release path, not instructions to
publish this fork's preview. Fork maintainers must choose a new prerelease version
and tag deliberately, keeping `package.json` and lockfile metadata in sync.

In ToneyStuck/Dishylink, the release workflow packages only an unsigned Windows
x64 installer into a draft prerelease. It does not upload updater metadata or
replace assets on a published release. Preview users update manually. Other
repositories cannot run its release jobs; the original DaveyHert/Dishylink path
still builds macOS, Windows, and extension archives.

The tag must match `package.json`'s version. Fork publishing must also target
ToneyStuck/Dishylink in `electron-builder.yml`. Publishing a draft is a separate
maintainer action. No workflow deploys this fork to an STB, and landing deployment
is restricted to the original upstream repository.

## Reporting problems

Open an issue in [fork Issues](https://github.com/ToneyStuck/Dishylink/issues)
with firmware versions, platform, and relevant diagnostics. **Copy debug data**
includes status and config as JSON; review it and remove account sessions, tokens,
device identifiers, and other private data before sharing.

For vulnerabilities, follow [SECURITY.md](SECURITY.md). Use GitHub private
reporting only if enabled; if unavailable, do not post sensitive details publicly.
Upstream contacts and store listings are not fork support channels.

## Thank you

However you're helping — a new feature, a bug fix, better docs, or just a typo
in this file — it's appreciated. Dishylink is better for having more eyes on it,
and every improvement lands with someone squinting at a bad link at 2am.

Happy contributing.
