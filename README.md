<div align="center">

# 🧠 Copilot Detox

**Practice coding without AI. Get graded by AI afterwards.**

A single-file Bash CLI that hands you a Python challenge, drops you into your editor in
**Raw Mode** — no autocomplete, no chat, no Copilot — and then has Copilot review your
hand-written solution like a strict-but-fair senior engineer.

<br>

[![Bash](https://img.shields.io/badge/Bash-4.0%2B-4EAA25?logo=gnubash&logoColor=white)](https://www.gnu.org/software/bash/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Dependencies](https://img.shields.io/badge/dependencies-none-brightgreen)](./detox)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-ff69b4)](./detox)
[![Copilot
CLI](https://img.shields.io/badge/powered%20by-GitHub%20Copilot%20CLI-8957e5)](https://cli.github.com/)

</div>

---

## The problem

AI assistants are great at shipping code and terrible for your fundamentals. Reach for
completion every time and the muscle that turns a fuzzy problem into a working function
quietly atrophies — you stop noticing until an interview or an outage hands you a blank file
and no tab key.

**Detox is the deliberate practice loop:** AI sets the exercise and grades the result, but
never touches the keyboard in between.

| | Normal day | A Detox session |
|---|---|---|
| Who writes the code | Copilot | **You** |
| Who checks it | Nobody | **Copilot** |
| Feedback you get | "Accepted ✅" | Score, edge cases, style, one concrete fix |
| What improves | Throughput | **Skill** |

---

## Quick start

```bash
# 1. Install (Linux / WSL / macOS)
curl -fsSLo detox https://raw.githubusercontent.com/sanjumsanthosh/Copilot-Detox/main/detox
chmod +x detox && sudo mv detox /usr/local/bin/

# 2. Authenticate the Copilot CLI
gh auth login && gh extension install github/gh-copilot

# 3. Go
detox start
```

Write your solution in the editor that opens. Save, quit, then:

```bash
detox submit     # Copilot reviews your code and scores it 1–10
detox stats      # your running average, last 5 sessions
```

---

## Commands

| Command | What it does |
|---|---|
| `detox start` | Generates a fresh challenge and opens it in `$EDITOR` (Raw Mode) |
| `detox submit` | Sends your saved solution to Copilot for review + score |
| `detox last` | Summary of the most recent session |
| `detox last --open` | Re-open the last solution in your editor |
| `detox last --review` | Read the full review in `less` |
| `detox stats` | Total sessions, completed count, average score, last 5 sessions |
| `detox clean [days] [--yes]` | Delete sessions older than `days` (default `7`) |
| `detox` | Print the built-in help |

> `detox clean 30 --yes` works today; the `--days 30` spelling shown in older docs is
> not parsed by the current script.

---

## What a session looks like

```console
$ detox start
🚀 Starting new Detox session...

🤖 Asking Copilot to generate a Python coding question...

📝 Question Generated:
TITLE: Palindrome Validator
DESCRIPTION: Write a function that checks whether a string reads the same
forwards and backwards, ignoring case, spaces and punctuation.
REQUIREMENTS:
- Handle empty strings
- Ignore spaces and punctuation
- Case-insensitive comparison

💾 Session: 2026-11-15T14-30-00
📂 Location: ~/.copilot-detox/sessions/2026-11-15T14-30-00

🔥 Opening nvim with Python syntax highlighting...
   Save and exit when done (:wq for vim, Ctrl+X for nano)

✅ Editor closed.
   Run 'detox submit' when ready for review!
```

No Copilot. No completion. Just you and the problem — then:

```console
$ detox submit
📤 Submitting solution to Copilot for review...

🤖 Copilot is reviewing your code...

🎯 Score: 8/10

📋 Review Summary:
1. SCORE: 8
2. Correctness: Correctly handles the palindrome check and strips punctuation.
3. Edge cases: Empty string returns True, which is arguably wrong — clarify the spec.
4. Code quality: Clean, pythonic; the two-pointer version avoids the extra allocation.
5. Improvement: Add type hints and a docstring with the edge-case contract.
```

Try again as many times as you like — every attempt is kept as its own snapshot, so you can
diff attempt 1 against attempt 3 and watch the review get shorter.

---

## How it works

```
detox start                       detox submit
     │                                 │
     ▼                                 ▼
gh copilot suggest ──► question.md    solution.py + question.md
     │                                 │
     ▼                                 ▼
starter code ──► $EDITOR         gh copilot suggest (strict reviewer prompt)
     │                                 │
     ▼                                 ▼
solution.py + meta.json          attempt_N.py + attempt_N_review.md + score
```

Everything is plain files — no daemon, no database, no Python runtime for the tool itself.

```text
~/.copilot-detox/
├── last_session                    # pointer to the newest session
└── sessions/
    └── 2026-11-15T14-30-00/
        ├── question.md               # the generated challenge
        ├── solution.py               # your working file
        ├── meta.json                 # id, title, attempts, status, last_score
        ├── attempt_1.py              # snapshot of each submission
        ├── attempt_1_review.md       # Copilot's full review
        └── attempt_2.py …
```

---

## Requirements

| Need | Why |
|---|---|
| **Bash 4.0+** and GNU coreutils | `grep -oP`, `sed -i`, `date -Iseconds` are used as-is |
| **[GitHub CLI](https://cli.github.com/)** with `gh copilot` | Generates challenges and reviews code |
| **Active Copilot subscription** | `gh copilot` requires it |
| **An editor** | Auto-detected: `$EDITOR` → `nvim` → `vim` → `nano` |

**Platforms:** Linux and WSL are the tested path. macOS ships Bash 3.2 and BSD `grep`/`sed`,
so a few commands (`grep -P`, `sed -i ''`, `date -Iseconds`) need portability fixes — see the
roadmap.

---

## Configuration

```bash
export EDITOR=nvim          # force an editor
export DETOX_DIR=~/practice # move session storage (edit the script variable)
```

Nerd knobs live at the top of `detox`: `DETOX_DIR`, the challenge prompt, and the reviewer
prompt. Swap "Python" for any language you want to drill — the review prompt is just text.

---

## Roadmap

- [x] Challenge generation via Copilot CLI
- [x] Raw Mode practice loop with per-attempt snapshots
- [x] Score tracking and session stats
- [x] Session cleanup with age thresholds
- [ ] macOS support (portable `grep`, `sed`, `date`)
- [ ] `--days` / `--topic` / `--difficulty` flags
- [ ] More languages: JavaScript, Go, Rust
- [ ] Weekly Markdown progress report
- [ ] Optional spaced-repetition queue for missed edge cases

---

## Contributing

Issues and PRs are welcome — small and focused beats large and clever.

```bash
git clone https://github.com/sanjumsanthosh/Copilot-Detox
cd Copilot-Detox
chmod +x detox
./detox            # sanity-check the help output
```

If you touch a `grep`/`sed`/`date` line, please keep it working on both GNU and BSD userland.

---

## License

MIT — see [LICENSE](./LICENSE).

## Author

Built by [@sanjumsanthosh](https://github.com/sanjumsanthosh) — to stay sharp while shipping fast.

<div align="center"><br>

**The goal isn't to avoid AI. It's to stay the kind of engineer who doesn't need it.**

</div>
