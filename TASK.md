# TASK.md — Profile README redesign: "Cyber Security × AI" + reliable stats

You are a senior open-source maintainer redesigning your own GitHub profile.
Repo: the profile repo `sohamwagh777/sohamwagh777` (you are in it). Single
deliverable: `README.md` (rewrite) + `.github/workflows/stats.yml` (new).
NO git commands (the orchestrator commits). Only touch these two files.

## Who this profile is (facts — do not invent anything beyond these)
Soham Wagh. Positioning shift: NOT just cybersecurity — he is
**Cyber Security × AI**: security engineer who builds and ships AI systems,
especially **local-first / open-weight** ones (no cloud tax, no data egress),
plus an open-source project builder. Daily-driver vibe: runs LLMs, RAG and TTS
on local hardware (DGX Spark), does bug-bounty/security research, automates
with agents.

Public repos to feature (verify each exists with
`gh repo view sohamwagh777/<name>` before featuring — ONLY feature ones that pass):
- chat-with-your-pdfs — 100% local PDF RAG: Ollama + Chroma, table-safe chunking
- qwen-tts-local-lab — one-command fully-local TTS (Qwen3-TTS), GB10-benchmarked
- Lexicon — 198 cybersecurity/systems terms as interactive glossary (Pages live)
- toolbench — 100 offline-first client-side tools in one HTML suite (Pages live)
- CyberLog-Analyser — AI-powered SOC dashboard (log triage + brute-force detection)
- php2shell-lab — security research lab (offensive web exploit tooling)
- hermes-n8n-lab-skills — agent prompt-skills + 13 practical n8n workflows

STRICT BANS (non-negotiable):
- NEVER mention "shortlistkit" (private product) — any casing.
- NEVER mention "cbse-Paper" or its render.com link (repo is now private).
- No fabricated achievements, stars, follower counts, employer names, or "X+
  contributions" text badges. Numbers only come from the stats cards.
- Do not link demo URLs you have not curl-verified to 200 in this session.
  (Known-good Pages bases: https://sohamwagh777.github.io/toolbench/ and
  https://sohamwagh777.github.io/Lexicon/ . The old sohamindia123.github.io
  links may 404 — curl them; drop any that fail.)

## The typing line (replace the old one)
Keep the readme-typing-svg.demolab.com widget (same amber #D8A25E, center),
but new lines, e.g.:
"Cyber Security × AI"; "Building local-first AI — RAG · TTS · LLM ops";
"Shipping open-source tools with zero cloud tax"; "SOC dashboards → agentic pipelines"
(URL-encode separators with %3B).

## Layout (overall redesign, keep it clean and calm)
Suggested structure (you may refine, keep it single-column, no table-mazes):
1. Typing widget (new lines) + one-line tagline under it.
2. "What I do" — 3 tight bullets: Security (SOC tooling, bounty hunting, web
   exploitation research) / Local-first AI (RAG, TTS, LLM serving on consumer
   & ARM+Blackwell hardware) / Open source (offline-first tools, agent skills).
3. "Featured" — 4-5 repo tiles using github-repo-card style badges or a simple
   markdown list with shields repo badges (stars/forks live from shields.io)
   — pick the cleanest that renders without JS.
4. "Live demos" — only curl-verified URLs.
5. Toolbox shields — update set: Python, TypeScript, Flask, FastAPI, Docker,
   Ollama, LangChain/RAG, n8n, plus keep the two security ones (hackthebox
   offensive-research, security-engineering). Drop Phaser/CSS/HTML badges.
6. Stats section — see below (this is the big fix).
7. Keep the komarev profile-views badge, small, bottom.

## Stats: the real fix — Actions-rendered SVGs, not rate-limited public endpoints
Create `.github/workflows/stats.yml`:
- `name: Stats`, `on: schedule (cron 0 4 * * *) + workflow_dispatch`
- `permissions: contents: write`
- Job using `lowlighter/metrics@latest` with:
  - `config: yes`, user self, `base: header,activity,community`
  - `template: terminal`
  - `commits: yes` (total commit counts), `repositories: 10`
  - `plugin_languages: yes` (indepth off), `plugin_languages_ignored: HTML,CSS` kept small
  - `plugin_isocalendar: yes` + `plugin_isocalendar_duration: half-year` (contribution calendar)
  - `plugin_achievements: yes` with `plugin_achievements_threshold: B` +
    `plugin_achievements_secrets: no` + `plugin_achievements_display: compact`
    (this is the honest "total contributions" tiles: commits, stars, issues, PRs)
  - `compatibility: yes`
  - `config_output: .github/metrics.svg` (commit to repo)
  - Use `${{ secrets.METRICS_TOKEN || secrets.GITHUB_TOKEN }}` as token line
    so a PAT can be added later for private-repo counting.
- Keep ONE remote card as visual fallback for streak: `streak-stats.demolab.com`
  with `&type=weekly` and the existing dark/amber theme, `&hide_border=true`.
  (Demolab instance, longest-streak shows natively.)
- README stats row: `<img src=".github/metrics.svg" alt="metrics" width="100%">`
  plus the streak img. Add alt text everywhere.
- NOTE: svgs won't exist until the workflow's first run — that's OK, the
  orchestrator triggers it right after push. The README must use relative
  paths exactly `./github/metrics.svg` style (` .github/metrics.svg`).

## Verification gates — RUN YOURSELF, paste raw outputs
1. `python3 -c "import yaml,sys; yaml.safe_load(open('.github/workflows/stats.yml')); print('yaml OK')"`
2. Every `https?://` URL in README.md: `curl -s -o /dev/null -w "%{http_code} %{url_effective}\\n" -L <url>`
   — every one must be 200 (badge/services hosts count: api.github.com repo
   endpoints used by shields are fine as the shield URL itself; verify the
   full shields URL returns 200 image).
3. `grep -ri "shortlistkit\|cbse\|sohamindia123" README.md` → no matches (exit 1).
4. `grep -c "<img" README.md` ≥ 5 (stats+badges) and file ≤ 250 lines.
5. Feature check: for each featured repo, paste the `gh repo view --json name,description` one-liner you verified.
6. Final answer format: raw gate outputs + DEFERRED/FAILED list + the exact
   list of URLs you verified with codes.

## Definition of done
Profile reads as "security engineer who builds local AI and ships open
source", stats will render from committed SVGs (no public-renderer single
point of failure), zero dead links, zero banned names.
