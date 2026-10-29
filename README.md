# Secret Mission Cards

An offline-friendly, single-file social party game with optional DeepSeek AI mission generation and QR-powered private phone cards.

## Play on GitHub Pages

Host at https://tbenitz.github.io/secret-mission-cards/ once GitHub Pages has been enabled.

1. The host opens the site, adds at least three real names (or lets guests request to be added before a round), and chooses each player's comfort level.
2. Select **Invite → Enable phone joining**, then share the displayed QR code or join link. Other phones need internet access to connect. Keep the host tab open.
3. Each guest scans the QR code, selects their name (or requests a new name before the round), and asks the host for approval. The host opens **Invite** and selects **Approve** only for known players.
4. The host starts a round. Each approved phone sees only its own task and the live countdown, with **Mark done**, **New mission** (if allowed), and **Hide** controls.
5. A new round retains the same roster and comfort levels. The game also works without phones or internet by passing the host device around.

## Architecture and limitations

- Self-contained `index.html` with vanilla HTML, CSS, JS, and an embedded MIT-licensed QR code encoder from Kazuhiko Arase.
- Browser-to-browser data uses WebRTC data channels with PeerJS 1.5.5; PeerJS is loaded dynamically from `unpkg.com` with a `jsdelivr` fallback. PeerJS Cloud provides internet-based signaling. There is no app server and no GitHub backend, but external signaling and the CDN must be reachable.
- Keep the host tab open and online. If the host closes it, players are disconnected until the host comes back. Guests reconnect using their saved seat token on the same browser; approval is required on a different phone.
- Host approval reduces accidental card sharing, but this is a party game, not an identity-proofing service. The host can see all missions; anyone holding the host device can open a player card.
- No participant receives the whole assignment map. The host sends a single-card payload to approved connections, and enforces completion/refresh serverlessly from the host browser.
- The DeepSeek API key is optional and saved in the host browser's `localStorage` (not encrypted or secured from local browser access); it is never committed to GitHub or sent to other phones. DeepSeek requests send only the selected theme and mission examples, not players' names. Direct browser requests may be prevented by CORS or network policy.
- Offline host gameplay requires no PeerJS and no API key. Online QR/phone play requires the site to be hosted over HTTPS and access to PeerJS Cloud.

## Publishing via GitHub Pages

In the repository open **Settings → Pages**, select **Deploy from a branch**, choose **main / (root)**, and **Save**. Then visit the Pages URL above. If the site is already configured for Pages, committing `index.html` redeploys it automatically.

## Developer notes

Modify `MISSION_BANKS` near the top of the main JavaScript to change the three difficulty banks. No hard-coded example players are included.

## License

MIT. The bundled QR encoder was authored by Kazuhiko Arase and is MIT-licensed.