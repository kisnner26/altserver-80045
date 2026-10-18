# altserver-80045

field notes on why altserver stopped signing in to apple on macos 27 in october 2026, how apple blocked it twice, and the fix proposed to altstore.

![altstore installed and refreshed on an iphone, 7 days remaining](assets/altstore-iphone.jpg)

## what happened

- **oct 12**: every sign-in fails at the first grandslam request with `-80045`. altserver on macos 27 generated android anisette data while reporting a mac client info.
- **oct 14, ~20:50 utc**: apple starts answering any android client info with a `503` html page. altserver shows it as "the data is not in the correct format" (`NSCocoaErrorDomain 3840`).
- **what passes**: windows anisette data from an anisette v3 server, with the client info and user agent that server reports.

![the two errors](assets/error-503-html.jpg)

## the fix

- altstore: [altstoreio/AltStore#1834](https://github.com/altstoreio/AltStore/pull/1834)
- altsign: [rileytestut/AltSign#61](https://github.com/rileytestut/AltSign/pull/61)
- unofficial build, temporary: [release](https://github.com/kisnner26/AltStore/releases/tag/v1.8-windows-anisette-unofficial)
- tracking issue: [altstoreio/AltStore#1828](https://github.com/altstoreio/AltStore/issues/1828)
- related: [altstoreio/AltStore#1835](https://github.com/altstoreio/AltStore/pull/1835), by an altstore collaborator. it covers the altstore app on the iphone and is built on top of rileytestut/AltSign#61. the two prs are complementary

tested on macos 27.2 with a free apple id with 2fa: sign-in, install on an ipad pro and an iphone 15 pro max, and a refresh of altstore (6 requests, all http 200, back to 7 days). not universal: some accounts still get `-80045` with the windows identity.

## what others report

public reports, not verified by me:

- [eutudb23](https://github.com/altstoreio/AltStore/issues/1828#issuecomment-6232315194) built altserver from #1834 and reinstalled altstore on macos 27.0.1 with a 2fa account. on the iphone, with only these changes, installing an app still got http 503 until they added an anisette refresh handler in `AuthenticationOperation.swift`. one device, one account
- [izaankml](https://github.com/altstoreio/AltStore/pull/1835#issuecomment-6247158473) tested altsign#61 (832d9ce) under #1835: on-device refresh works again on an iphone with ios 26.6.2
- [dmuiX](https://github.com/altstoreio/AltStore/issues/1828#issuecomment-6205653919) still gets `-80045` with the mac route, even with another apple id. it isn't clear whether that was the official altserver or the unofficial build

the unofficial build has about 700 downloads. none of this is a merge: #1834, #1835 and altsign#61 are still open.

## the article

the full write-up, with the timeline, the probes against the edge and a crash caused by an empty framework in xcode 27, is at [kisnner26.github.io/altserver-80045](https://kisnner26.github.io/altserver-80045/) (source in [`index.html`](index.html)). a 9-slide carousel with the same story is in [`carrusel/`](carrusel/) ([pdf](carrusel/altserver-80045-carousel.pdf)). a short version is in this [thread on x](https://x.com/kisnnerc/status/2110793388354158813).

not affiliated with altstore or apple.
