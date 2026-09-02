# watermark.k13.app
a watermark removal tool powered by llm's

This is a **static, browser-only** demo/fake site — there is no backend, and it's a
single flat `index.html` file (styles and script inlined, Google Material-style
look). Uploaded images are only read locally in the visitor's browser (never sent
anywhere), the "upload" and "removal" steps are simulated with timers, and the
process always ends in a fake "LLM refused for copyright reasons" error. All
timings are controlled by the `CONFIG` object inside the `<script>` tag in
`index.html` (see comments in that file for what each setting does).

Being a single file makes it trivial to host anywhere with zero build step.

## Testing it quickly (before pointing your domain anywhere)

- **GitHub Pages preview**: enable Pages for this repo (*Settings → Pages → Source:
  deploy from a branch*, pick this branch, `/ (root)` folder) and it'll be live at
  `https://<your-username>.github.io/watermark.k13.app/` within a minute or two.
- Or just open `index.html` directly from disk / GitHub's "Raw" view in your
  browser — everything works fully offline since there's no backend.

## Deploying: hosted on GitHub Pages, domain via Squarespace (DNS only)

**Important**: this site is hosted on **GitHub Pages**, not on Squarespace.
Squarespace is only acting as your **domain registrar/DNS provider** for `k13.app`
— you're just pointing the `watermark` subdomain's DNS at GitHub, nothing is
uploaded to or built in Squarespace itself.

### 1. Turn on GitHub Pages for this repo
- *Settings → Pages → Build and deployment → Source*: **"Deploy from a branch"**.
- Pick the branch you want live (e.g. `main`) and folder `/ (root)`, then **Save**.
- GitHub will give you a default URL like `https://kalabaddon.github.io/watermark.k13.app/`
  — confirm it loads and works before moving on.

### 2. Tell GitHub Pages about your custom domain
- Still under *Settings → Pages*, in the **Custom domain** field enter
  `watermark.k13.app` and save.
- This automatically creates/commits a `CNAME` file in the repo root containing
  `watermark.k13.app` — don't remove it, GitHub uses it to know which domain to
  serve and to validate/renew the HTTPS certificate.

### 3. Add the DNS record at Squarespace (your domain provider)
Since `watermark` is a **subdomain** (not the bare `k13.app` apex), you only need
**one CNAME record** — you do **not** need `A` records (those are only for
pointing the root/apex domain, e.g. `k13.app` itself, at GitHub's IPs).

In Squarespace's domain DNS settings for `k13.app`, add:

| Type  | Host        | Data / Value                  |
|-------|-------------|--------------------------------|
| CNAME | `watermark` | `kalabaddon.github.io.`        |

(Replace `kalabaddon` with your actual GitHub username/org if different — it must
point at `<username>.github.io.`, with the trailing dot if Squarespace requires
fully-qualified values; if it errors on the trailing dot, omit it.)

- Remove/skip any conflicting `A`, `ALIAS`, or `URL redirect` records Squarespace
  may have pre-populated for that same `watermark` host.
- Save the DNS changes. Propagation is usually fast (minutes) but can take up to
  24–48 hours.

### 4. Verify and enable HTTPS on GitHub's side
- Back in *Settings → Pages*, GitHub will show a "DNS check in progress" (then
  "✅ DNS check successful") status once the CNAME resolves correctly.
- Once verified, tick **"Enforce HTTPS"** (this checkbox only becomes available
  after the domain check passes) — GitHub then issues a free Let's Encrypt
  certificate for `watermark.k13.app` automatically.
- Visit `https://watermark.k13.app` and confirm the padlock/secure icon shows.

### 5. Verify the fake flow works end-to-end
- Drag/drop or choose any image → confirm it previews after a size-based fake
  upload delay (larger files take longer, per the simulated `128 KB/s` speed).
- Click *Remove Watermark* → confirm the progress bar and its numeric timer move
  together, steady at first then slowing down a lot near the end.
- Move your mouse toward the top of the browser window → confirm the "don't
  leave" banner appears near the top (never covering the progress bar) and the
  bar jumps forward ~25%.
- Click *Stop* → confirm the window greys out with an "are you sure?" dialog
  positioned near the top of the screen, and the bar speeds up in the background
  while it's open.
- Let it finish (or confirm Stop) → confirm you always get: *"LLM refused to
  remove watermark for copyright reasons. Please check settings and try again."*

If you ever want to change timings (upload speed/variance, processing duration
per mode, easing point, jump %, stop-dialog speed-up), edit the `CONFIG` object
inside `index.html`'s `<script>` tag, commit, and push — GitHub Pages will
redeploy automatically.
