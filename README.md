RUSTY FABLED CHAT — SETUP
==========================

Two files, same engine. Works anonymously out of the box (read-only),
or log in with your own Twitch account to send messages and get exact badges.

1) twitch-chat-window.html  (standalone window)
   - Double-click to open in your browser.
   - Type the channel name, press Connect.
   - Click "⚙ Settings" for background, font size, colors, badges, fade, etc.
     Everything saves automatically and applies live.
   - "Copy settings for OBS version" copies a ready-made ?query string
     matching your current look.

LOGGING IN (optional)
---------------------
Twitch does not allow username + password login for any app — every chat
tool uses OAuth. This app uses Twitch's device login, which feels like a
normal login: click, approve on Twitch, done. No token copying.

ONE-TIME SETUP (about 2 minutes, only ever done once):
1. Open https://dev.twitch.tv/console/apps/create (log in with your
   Twitch account).
2. Register Your Application:
      Name:               anything (e.g. RustyFabledChat)
      OAuth Redirect URL: http://localhost
      Category:           Chat Bot
      Client Type:        Public        <-- important
3. Copy the Client ID and paste it into Settings -> Account in the
   window version. It saves permanently.

EVERY LOGIN AFTER THAT:
1. Click "Log in with Twitch".
2. A short code appears; click "Open twitch.tv/activate", enter the
   code (often prefilled) and press Activate.
3. The app logs in by itself within a few seconds.

Your session auto-refreshes in the background, so it never expires on
you. Everything is stored only in this file's local browser storage.

Fallback: "Advanced: log in with a token instead" still accepts a
pasted OAuth token (e.g. from twitchtokengenerator.com) if you prefer.
What login adds:
   - A send bar appears at the bottom — type and press Enter to chat
     (Arabic input fully supported, dir=auto).
   - Exact badges from the Twitch API, including this channel's custom
     subscriber badge art (anonymous mode uses the standard set).
   - "Copy settings for OBS" now includes &token= so the OBS source
     shows the exact badges too. Remove that part from the URL if you
     don't want the token in your OBS settings.
If the token expires, the app tells you and falls back to read-only.

2) twitch-chat-obs.html  (OBS Browser Source)
   - In OBS: Sources → + → Browser → check "Local file" OFF, and set URL to:
       file:///C:/path/to/twitch-chat-obs.html?channel=YOURCHANNEL
     (or check "Local file" ON, pick the file, then add the parameters
      in the URL field after selecting it — OBS keeps them.)
   - Background is transparent by default, ready to sit over your scene.
   - Recommended size: 400–500 wide, 600–900 tall.

URL PARAMETERS (OBS version)
----------------------------
channel=name          required
bg=transparent        default | bg=%2318181b (hex, # = %23) | bg=https://...jpg (image)
bgopacity=0.5         background image/color opacity
size=18               font size in px
color=ffffff          text color (hex, no #)
shadow=0              disable text outline shadow
bold=1                bold text
bubbles=1             dark rounded bubble behind each message
bubblecolor=222233    custom bubble color
font=Cairo            any installed/Google font name
align=auto            auto (Arabic+English mixed) | ltr | rtl
badges=0              hide badges
emotes=0              disable BTTV/FFZ/7TV emotes
images=0              show image links as links instead of embedding
events=0              hide sub/raid/announcement lines
ts=1                  show timestamps
limit=50              max messages kept on screen
fade=30               messages fade out after N seconds (0 = never)
imgheight=220         max embedded image height in px
token=...             your OAuth token: connects as your account and
                      loads exact channel badges (optional; falls back
                      to anonymous if invalid)

Example:
  twitch-chat-obs.html?channel=shroud&size=20&fade=45&bubbles=1&limit=40

IMAGE LINKS SUPPORTED IN CHAT
-----------------------------
- Any direct image link ending in .png .jpg .jpeg .gif .webp .avif .bmp
  (works for Imgur, Pinterest i.pinimg.com, Discord CDN, Reddit i.redd.it,
   ibb.co, postimg, imgflip, Wikimedia, etc.)
- imgur.com/abc123 page links  -> converted to the direct image
- giphy.com gif page links     -> converted to the direct gif
- tenor c.tenor.com media links
- Dropbox share links to images
- Pinterest: direct i.pinimg.com links embed; pinterest.com/pin/ page
  links can't be resolved without Pinterest's API, so they show as
  clickable links instead.

NOTES
-----
- Twitch native emotes, BTTV, FFZ and 7TV (global + channel) all render.
- Global badges (mod, VIP, sub, founder, prime, turbo, bits, gifter,
  hype train, partner, staff...) use Twitch's official CDN. Channel-custom
  subscriber badge art needs an authenticated API, so those fall back to
  the standard sub badge with the month count shown on hover.
- Arabic is fully supported: emote positions are computed by Unicode code
  points (so Arabic/emoji never shift them), every message uses dir=auto
  with unicode-bidi plaintext, and the font stack includes Noto Sans Arabic.
- Deleted messages and timeouts/bans are removed live (CLEARMSG/CLEARCHAT).
