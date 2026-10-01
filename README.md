# Mood Board Demo — Heart–Brain Axis

A single-page demo for "Exploring the Heart–Brain Connection: Emotional
Regulation through Cardiac Neurons and Wearable Technology" (Aarzu, Ghazali,
Khan, Ahmed — NUST School of Health Sciences).

Three-screen flow, built for a phone screen:

1. **Mood select** — six tap targets matching the exact terms from your
   poster's Figure 3 (Calm, Gratitude, Compassion / Frustration, Anxiety,
   Anger). Tap one — no typing, no manual entry.
2. **Loading** — a 3-second "processing" screen with a pulsing heart icon
   and cycling status text (HRV signal → motion filtering → mood-board
   cross-reference), echoing the methodology steps on the poster.
3. **Dashboard** — the simulated cardiac rhythm waveform, coherence index,
   residual HR, the primary biochemical modulator for that mood (pulled
   from Table 1 on the poster), and a one-line interpretation. "Log another
   mood" returns to screen 1.

**Engagement features.**

- **Home:** time-of-day greeting, a rotating prompt, a check-in streak with
  a 7-day dot strip, and a "Did you know?" card citing one of the references.
- **Mood picker:** every mood has its own color. A tap ripples, spotlights
  the choice and vibrates briefly on Android before the reading starts.
- **Loading:** a step-by-step checklist ticks off the three methodology
  steps (HRV signal, motion filtering, cross-reference).
- **Dashboard:**
  - an animated coherence ring and a heart-rhythm trace
  - a "Does this match how you feel?" check-in, saved to history as the
    user's own ground truth
  - a 1-minute guided coherent-breathing exercise (5 s in / 5 s out × 6)
- **History ("Your journey"):** totals, streak, average coherence, a
  coherence trend line and a mood-mix bar. Clearing it takes two taps.
- **Milestones:** first check-in, a new streak day and every 10th check-in
  get a toast and a small confetti burst.

All animation is disabled under `prefers-reduced-motion`.

**References.** A "Science" screen (topbar button, or "Read the science
behind this" on the home screen) presents the paper's four references as
animated cards, each tagged with the paper section it supports:

1. Max Planck Institute (2025): *The heart thinks along with the mind*
   (Villringer, Nikulin & Gaebler, *Trends in Neurosciences*): §1, §2, §5
2. *The Female Heart: Sex Differences in the Dynamics of ECG in Response to
   Stress*, Frontiers in Physiology (2018), PMC6279887: §3, §4a
3. Norwest Chiropractic: *The Heart and Brain Connection*: §3
4. Qur'an 26:192–194 and 53:11–12 (Arabic + English): §6

The same references appear in context: a cited fact cycles on the loading
screen, the dashboard shows an evidence card (ref 2 for incoherent moods,
ref 1 for coherent ones), and the modulator panel carries a [3] citation.
Tapping any of them jumps to that card.

Each mood has a fixed coherence/HR *range* rather than a fixed number, so
tapping the same mood twice gives slightly different but on-theme results —
useful for repeated live demos.

**This is a UI/UX demo, not real data.** The rhythm trace, coherence index,
HR, and modulator are looked up from the selected mood category, not
measured from a wearable device. Say so if you present it: it demonstrates
the *mechanism* described in the methodology section (mood-board entries
validated against a physiological signature), not a live result.

## Instructions for Claude Code

If you (an agent) are working in this folder, deploy it like this:

1. Check `gh` is installed and authenticated:
   ```bash
   gh auth status
   ```
   If this fails, stop and tell the user to run `gh auth login` — that step
   needs an interactive browser and can't be scripted.

2. Init git and commit, if this folder isn't already a repo:
   ```bash
   git init -b main
   git add index.html README.md
   git commit -m "Mood board demo"
   ```

3. Create the GitHub repo and push in one step. Confirm the repo name and
   `--public`/`--private` choice with the user first if it matters to them —
   default to public since the point is a shareable Pages URL:
   ```bash
   gh repo create moodboard-demo --public --source=. --remote=origin --push
   ```

4. Enable GitHub Pages via the API (`gh` has no dedicated `pages enable`
   command):
   ```bash
   REPO=$(gh repo view --json nameWithOwner -q .nameWithOwner)
   gh api -X POST "repos/$REPO/pages" -f "source[branch]=main" -f "source[path]=/"
   ```
   A 409 response means Pages is already enabled — treat that as success,
   not an error.

5. Confirm and report the live URL back to the user (don't just say "done"):
   ```bash
   gh api "repos/$REPO/pages" -q .html_url
   ```
   It can take 1–2 minutes after enabling before the URL resolves.

## Manual deploy (no `gh` CLI)

1. Create a new public repository on GitHub, e.g. `moodboard-demo`.
2. Upload `index.html` via the GitHub web UI ("Add file → Upload files"),
   or push with plain git:
   ```bash
   git init -b main
   git add index.html README.md
   git commit -m "Mood board demo"
   git remote add origin https://github.com/<your-username>/moodboard-demo.git
   git push -u origin main
   ```
3. In the repo: **Settings → Pages → Source → Deploy from a branch**,
   branch `main`, folder `/ (root)`. Save.
4. Live URL within a minute or two, typically:
   `https://<your-username>.github.io/moodboard-demo/`

No build step, no dependencies — it's a single static HTML file either way.
