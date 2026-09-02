# watermark.k13.app
a watermark removal tool powered by llm's

This is a **static, browser-only** demo/fake site — there is no backend, and it's a
single flat `index.html` file (styles and script inlined, Google Material-style
look). Uploaded images are only read locally in the visitor's browser (never sent
anywhere), the "upload" and "removal" steps are simulated with timers, and the
process always ends in a fake "LLM refused for copyright reasons" error. All
timings are controlled by the `CONFIG` object inside the `<script>` tag in
`index.html`.

Being a single file makes it trivial to host anywhere, including pasting straight
into a Squarespace Code Block (see below) — no build step, no separate `.css`/`.js`
files to link up.

## Deploying to `watermark.k13.app` on Squarespace

You already own `k13.app`. Here's the full checklist to get this fake site live at
`https://watermark.k13.app` using your existing Squarespace account:

1. **Create a blank page in Squarespace**
   - In the Squarespace editor, go to *Pages* → add a new page (a blank/regular page
     works fine — you don't need a specific template).
   - Give it any title (e.g. "Watermark Tool"); this page's content will be replaced
     entirely by the embed below, so its title/URL slug don't matter much.

2. **Add a Code Block with the site's content**
   - Edit that page and add a **Code Block** (not a "Markdown" block — it must be
     the Code Block so `<style>`/`<script>` tags aren't stripped).
   - Open `index.html` from this repo, copy its **entire contents**, and paste them
     into the Code Block.
   - Save the page.

3. **Connect the `watermark` subdomain to Squarespace**
   - In Squarespace: *Settings* → *Domains* → *Add Domain* → choose **"Use a domain
     I own"** and enter `watermark.k13.app`.
   - Squarespace will show you a DNS record to add (typically a `CNAME` record for
     host `watermark` pointing to something like `ext-cust.squarespace.com`, or an
     `A`/`ALIAS` record — Squarespace will give you the exact values).
   - Go to wherever `k13.app`'s DNS is managed (your domain registrar or DNS
     provider) and add that exact record for the `watermark` subdomain.
   - Back in Squarespace, click verify/connect. DNS changes can take anywhere from a
     few minutes up to ~24-48 hours to propagate.

4. **Point the subdomain at the page you built**
   - Still under *Settings* → *Domains*, select `watermark.k13.app` and set it to
     point at the page you created in step 1 (some Squarespace plans map a
     connected subdomain straight to a single page; others require it to be your
     site's homepage — if so, temporarily set that page as the homepage, or move
     its content to the homepage instead).

5. **Enable HTTPS**
   - Once the domain shows as "Connected"/verified in Squarespace, HTTPS is
     automatic — Squarespace issues a free SSL certificate (via Let's Encrypt) for
     every connected domain, usually within a few hours of DNS verifying.
   - Confirm the *Secure connection* / *HTTPS* toggle is on under the domain's
     settings, then visit `https://watermark.k13.app` and check for the padlock
     icon in your browser.

6. **Verify the fake flow works end-to-end**
   - Drag/drop or choose any image → confirm it previews after ~2s.
   - Click *Remove Watermark* → confirm the progress bar crawls slowly.
   - Move your mouse toward the top of the browser window → confirm the "don't
     leave" banner appears and the bar jumps ~25%.
   - Click *Stop* → confirm the window greys out with an "are you sure?" dialog and
     the bar speeds up in the background.
   - Let it finish → confirm you always get: *"LLM refused to remove watermark for
     copyright reasons. Please check settings and try again."*

If you ever want to change timings (upload delay, processing duration, jump %,
stop-dialog speed-up), edit the `CONFIG` object inside `index.html`'s `<script>`
tag — then just re-copy/paste the updated file into the Code Block.
