# Designer Portfolio (self-hosted)

Everyone who opens your website sees your works. Only you (with the PIN) can edit.
No `npm install` needed. Only Node.js 18 or newer.

## Run on your computer
    node server.js
Open http://localhost:3000 , click **Admin**, enter your PIN.

## PIN
Your PIN is already set (stored as a hash in `config.json`, not as plain text).
Change it any time:
    node set-pin.js 482916
then restart the server. On hosts where you cannot run commands, set the `ADMIN_PIN`
environment variable instead (it overrides config.json).
If there is no PIN at all the server refuses to start.

## Where is my data?
Folder `storage/` : `data.json` (profile, works, skills...) and `uploads/` (photos, videos).
BACK THIS FOLDER UP. Set a different folder with `DATA_DIR=/path`.
Max upload size: 300 MB per file (`MAX_UPLOAD_MB=500` to change).
Allowed files: JPG, PNG, WebP, GIF, MP4, WebM, MOV.

## Put it online (pick one)
1. VPS (DigitalOcean, Hetzner, etc.) - easiest and cheapest long term
   - Install Node 18+, upload this folder, run: `PORT=3000 node server.js`
   - Keep it running: `npm i -g pm2 && pm2 start server.js --name portfolio && pm2 save`
   - Put Caddy or nginx in front for HTTPS + your domain (Caddy: `yourdomain.com { reverse_proxy localhost:3000 }`)
2. Render / Railway / Fly.io
   - Start command: `node server.js`
   - Set env var `DATA_DIR` pointing to a **persistent disk/volume**
     (without a persistent disk your uploads disappear on every deploy!)
Use HTTPS always, so the PIN is not sent in plain text.

## Features
- Admin login with server-side PIN (cookie, 7 days, 5 wrong tries = 1 minute lock)
- Works / AI videos / skills (with logos) / experience / education / cover / profile picture
- Live album slideshow, mobile friendly
- CV PDF export (button in admin bar)
- Unused uploads are deleted automatically (after 1 hour)

## Files
- server.js        backend (API + file storage)
- set-pin.js       change your PIN
- config.json      your PIN (hashed)
- public/index.html  the website
- package.json
