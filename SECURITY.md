# Security Policy

## Reporting a vulnerability

Do not post vulnerabilities, account sessions, tokens, or other sensitive details
in public issues. If GitHub private vulnerability reporting is enabled for
[ToneyStuck/Dishylink](https://github.com/ToneyStuck/Dishylink/security), use its
**Report a vulnerability** option. This policy does not confirm that it is enabled.

No separate private contact is listed for this fork. If GitHub private reporting
is unavailable, do not publish sensitive details while seeking a private channel.
Ordinary bugs belong in [fork Issues](https://github.com/ToneyStuck/Dishylink/issues)
with private data removed.

A private report should include reproduction steps, platform, and app version.
There is no guaranteed response time. Allow time for a fix before public disclosure.

For vulnerabilities in the original upstream project, consult
[upstream's security policy](https://github.com/DaveyHert/Dishylink/blob/master/SECURITY.md).
Its contacts belong to upstream, not this fork.

## Supported versions

This fork currently offers an unsigned Windows x64 preview, not a maintained
stable release line. Fixes land in source first; a new installer requires a
separate release. Preview updates are manual. Older previews are not promised patches.

## Scope

In scope:

- The desktop app (macOS and Windows builds)
- The browser extension
- The history recorder that runs in the background
- How the app handles your Starlink account session, and anything it writes to
  local storage or to disk

Out of scope:

- Vulnerabilities in Starlink hardware, in dish or router firmware, or in
  `starlink.com` itself. Those belong to SpaceX and should go through SpaceX's
  own reporting channels.
- Anything that requires an attacker to already have full access to your machine
  or browser profile. [PRIVACY.md](PRIVACY.md) documents what is stored locally
  and how.

## Testing

Test against hardware you own. Dishylink talks to a dish and router on your own
network, and the router is a small embedded device that has been observed
rebooting under ordinary polling load. Please do not fuzz or stress its
endpoints. A crashed router takes the whole connection down with it, and that on
its own is not a finding.
