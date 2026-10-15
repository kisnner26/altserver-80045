# hilo para x (inglés, 6 mensajes, cada uno de 280 caracteres o menos)

Listo para copiar. Adjunta la imagen indicada en cada mensaje (están en `assets/`). El recuento cuenta cada enlace como 23 caracteres, como hace X.

1/
AltServer on macOS 27 stopped signing in to Apple on Oct 12. A fix worked for one day. Then Apple blocked that too.

I traced what changed and a fix is now proposed to AltStore. Thread.
[adjuntar: assets/error-503-html.jpg]

2/
Two blocks, not one.

Oct 12: -80045 on the very first sign-in request. AltServer generates Android anisette data but described a Mac. Apple started checking that they agree.

Oct 14: any Android client info gets an HTTP 503 HTML page that AltServer misreads as a plist error.

3/
I probed the edge without credentials, changing only the client info:

Pixel 7 / 5 / 8: 503, 6 of 6
Mac: 404 / 429
Windows (iTunes): 404

Windows is the identity that still reaches the service. That is what iloader 2.3.8 uses too.

4/
Porting it meant building AltServer with Xcode 27, and it crashed on its own: a call through a null pointer in the SRP handshake.

Cause: Xcode 27 generates the AltSign-Dynamic framework empty, so a symbol stored in a static library was linked into nothing.

5/
The fix: on macOS 27 AltServer gets anisette data from a Windows anisette v3 server and signs in with the identity that server reports. Your password and 2FA code still go straight to Apple.

Tested: sign-in, 2FA, installs on an iPad and an iPhone, and a refresh.
[adjuntar: assets/altstore-iphone.jpg]

6/
Not universal: some accounts still get -80045. It depends on a third-party anisette server, and refreshing other apps is untested.

PR: https://github.com/altstoreio/AltStore/pull/1834
Write-up: https://kisnner26.github.io/altserver-80045/
Credits: whydr4, mattaltgear, nab138, jkcoxson.
