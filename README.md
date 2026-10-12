# ahmedkholaif.github.io

Source for my portfolio at **https://ahmedkholaif.github.io** and the how-to for keeping it, and my GitHub landing page, up to date.

There are three places that show who I am on GitHub. Keep them in sync whenever the CV changes:

| What | Where it lives | How it updates |
|---|---|---|
| Portfolio site | this repo → `index.html`, `Ahmad_Kholaif_CV.pdf`, `assets/` | push to `main` → GitHub Pages redeploys in ~1 min |
| GitHub landing page (profile README) | repo [`Ahmedkholaif/Ahmedkholaif`](https://github.com/Ahmedkholaif/Ahmedkholaif) → `README.md` | push to `main` → shows on github.com/Ahmedkholaif immediately |
| Profile settings (bio, website, pinned repos) | github.com settings | edited in the browser (or `gh api`) |

---

## 0. One-time setup

```bash
cd ~/coding
git clone https://github.com/Ahmedkholaif/Ahmedkholaif.github.io.git   # portfolio
git clone https://github.com/Ahmedkholaif/Ahmedkholaif.git              # landing page
```

Commit as the GitHub identity (not the work email):

```bash
git config user.name  "Ahmed Kholaif"
git config user.email "43828360+Ahmedkholaif@users.noreply.github.com"
```

Run both commands inside **each** repo.

Always pull before editing: `git pull`.

---

## 1. Updating the portfolio site

The whole site is one static file, `index.html`: no build step, no framework. Open it in an editor and search for the section you need:

| Section | Search for | What to edit |
|---|---|---|
| Page title / link previews | `<title>`, `name="description"`, `og:` | title and the description shown when the link is shared |
| Hero (name, role, intro) | `<header class="hero">` | `class="role"` line, `class="lead"` paragraph, "Open to…" status |
| Typing animation | `const words =` | the rotating "I build …" phrases |
| Headline numbers | `class="stat"` | `data-count="7"` is the animated number, `class="lbl"` is the label |
| Logo strip | `class="marquee-track"` | each name appears **twice** (the second copy has `aria-hidden="true"`). Edit both. |
| Experience | `<section id="experience">` | one `<article class="job">` per role: title, dates, bullets (`<li>`), tags |
| Stack-over-time chart | `const companies =` and `const stack =` | dates are `'YYYY-MM'`; use `'now'` for current. Each tech lists its periods. |
| Skill chips | `class="skill-group"` | the `<li>` tags under each group |
| Open-source projects | `<section id="projects">` | `class="feat"` cards (featured systems), `class="project"` cards (earlier projects) |
| Education & languages | `<section id="education">` | `edu-card` blocks, language meters |
| Contact & footer | `<section id="contact">`, `<footer>` | email / LinkedIn / GitHub links |

Rules of thumb:
- Every claim on the site should be backed by the current CV, using the same wording or something softer.
- Keep the phone number out of the HTML. It's only in the PDF.
- Use HTML entities for special characters: `&amp;` for &, `&mdash;` for —.

### 1a. Replacing the CV

```bash
cd ~/coding/Ahmedkholaif.github.io
cp ~/Downloads/"<new cv>.pdf" Ahmad_Kholaif_CV.pdf     # keep this exact filename
```

The filename must stay `Ahmad_Kholaif_CV.pdf`, because the "Download CV" buttons and the profile README link to it.

To see exactly what changed between the old and new CV before editing the site:

```bash
pdftotext -layout Ahmad_Kholaif_CV.pdf /tmp/old.txt
pdftotext -layout ~/Downloads/"<new cv>.pdf" /tmp/new.txt
diff -w /tmp/old.txt /tmp/new.txt
```

Then update the matching sections from the table above.

### 1b. Replacing the photo

Put a square image (≈800×800) at `assets/ahmad.jpg` and also generate `assets/ahmad.webp`:

```bash
cwebp -q 82 assets/ahmad.jpg -o assets/ahmad.webp     # or export WebP from any image editor
```

### 1c. Preview locally

```bash
cd ~/coding/Ahmedkholaif.github.io
python3 -m http.server 8000
```

Open http://localhost:8000. Check light **and** dark mode (moon button) and a phone-width window. Press Ctrl+C to stop the server.

### 1d. Publish

```bash
git add -A
git commit -m "Update content from latest CV"
git push origin main
```

Pages redeploys automatically. Watch it under **Actions → pages-build-deployment** in the repo, or run:

```bash
gh run list -R Ahmedkholaif/Ahmedkholaif.github.io -L 1
```

When the run shows `completed success`, hard-refresh the site with Ctrl+Shift+R. The PDF may be cached for a few minutes.

---

## 2. Updating the GitHub landing page (profile README)

The README in the repo named exactly like the username, [`Ahmedkholaif/Ahmedkholaif`](https://github.com/Ahmedkholaif/Ahmedkholaif), is shown at the top of github.com/Ahmedkholaif.

```bash
cd ~/coding/Ahmedkholaif
git pull
# edit README.md
git add -A && git commit -m "Update profile README" && git push origin main
```

What's in it, top to bottom:

| Block | What to edit |
|---|---|
| Avatar | `avatar-circle.png` in the same repo (square PNG, transparent corners) |
| Typing banner | the `readme-typing-svg` URL: phrases go in `lines=`, separated by `;`, with spaces as `+` |
| Badges (Portfolio / LinkedIn / Email / CV) | shields.io URLs; the CV badge points to the PDF on the site |
| Intro paragraph and "Now / Before" table | keep in sync with the site's hero and experience |
| 🛠️ Stack badges | one `<img>` per tech: `https://img.shields.io/badge/<Name>-<hexcolor>?style=flat-square&logo=<simple-icons-slug>&logoColor=white`. Find slugs at https://simpleicons.org |
| ⭐ Featured systems / 📂 Earlier projects | markdown tables linking to repos |

To preview it, open the file on github.com and use the **Preview** tab while editing in the browser. Or push it and look at your profile.

---

## 3. Profile settings (bio, website, pinned repos)

**In the browser:** github.com → avatar → **Settings → Public profile**:
- **Bio:** `Senior Full-Stack Engineer · Node.js · Go · TypeScript · React · AWS & GCP`
- **URL:** `https://ahmedkholaif.github.io`
- Location, LinkedIn under "Social accounts"

**Or from the terminal.** This needs the `user` scope once: `gh auth refresh -h github.com -s user`.

```bash
gh api -X PATCH user \
  -f bio="Senior Full-Stack Engineer · Node.js · Go · TypeScript · React · AWS & GCP" \
  -f blog="https://ahmedkholaif.github.io"
```

**Pinned repos** can only be changed in the browser. On github.com/Ahmedkholaif, click **Customize your pins** and pick the showcase repos: `ledgerd`, `ledger-nest`, `ledger-console`, `hookline`, `hookscope`. You can pin up to six.

---

## 4. Checklist: "I have a new CV"

1. [ ] Diff the old and new CV (step 1a) and note what was added, removed or reworded.
2. [ ] Copy the new PDF to `Ahmad_Kholaif_CV.pdf`.
3. [ ] Site: update the meta description, hero lead, stats, logo strip, experience bullets and tags, skill chips, and chart dates if a role changed.
4. [ ] Remove anything the new CV dropped. Don't keep claims the CV no longer makes.
5. [ ] Preview locally in light mode, dark mode and phone width.
6. [ ] Commit and push, then wait for the Pages deploy to show `success`.
7. [ ] Profile README: update the intro, the Now/Before rows and the stack badges, then push.
8. [ ] If your title or focus changed, update the profile bio too.
9. [ ] Open https://ahmedkholaif.github.io and github.com/Ahmedkholaif and click **Download CV** to confirm it's the new file.

## 5. Checklist: "I published a new project"

1. [ ] In the project repo, add a description, topics and a README with a CI badge. Make sure CI passes.
2. [ ] Site: add a `class="feat"` card (featured) or a `class="project"` card (smaller) in `<section id="projects">`. Copy an existing card and change it.
3. [ ] Profile README: add a row to the featured or earlier-projects table.
4. [ ] Pin it on your profile if it's one of your best six.
