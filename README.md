# Copilot Detox 🧠

Pure bash CLI that helps developers maintain coding skills by:
1. Using **Copilot CLI to generate** random Python questions
2. Opening your terminal editor for manual coding
3. Using **Copilot CLI to review** your solution

## Why Copilot Detox?

In the age of AI assistants, it's easy to lose touch with fundamental coding skills. Copilot Detox helps you:
- ✅ Practice coding **without AI assistance**
- ✅ Get **instant AI feedback** after you're done
- ✅ Track your progress over time
- ✅ Stay sharp on algorithms, data structures, and Python fundamentals

## Installation

### Quick Install

```bash
# Download the script
curl -o detox https://raw.githubusercontent.com/sanjumsanthosh/Copilot-Detox/main/detox
chmod +x detox
sudo mv detox /usr/local/bin/

# Or install to user bin
mkdir -p ~/bin
mv detox ~/bin/
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### From Source

```bash
git clone https://github.com/sanjumsanthosh/Copilot-Detox.git
cd Copilot-Detox
chmod +x detox
sudo cp detox /usr/local/bin/
```

## Prerequisites

- **Bash 4.0+** (comes with most Linux/macOS systems)
- **GitHub CLI**: `brew install gh` or see [cli.github.com](https://cli.github.com/)
- **Copilot access**: 
  ```bash
  gh auth login
  gh copilot
  ```
- **Editor**: vim/nvim/nano (auto-detected, or set `$EDITOR`)

## Usage

### Start a Practice Session

```bash
detox start
```

This will:
1. Ask Copilot to generate a random Python coding question
2. Create a session in `~/.copilot-detox/sessions/`
3. Open your editor with starter code
4. Wait for you to solve it manually (no AI assistance!)

### Submit for Review

```bash
detox submit
```

After saving your solution:
1. Copilot reviews your code
2. Gives you a score (1-10)
3. Points out edge cases, style issues, and improvements
4. Saves the review for later reference

### View Your Work

```bash
detox last              # Show last session summary
detox last --open       # Re-open solution in editor
detox last --review     # View full Copilot review
detox stats             # See overall progress and scores
```

### Cleanup

```bash
detox clean             # Delete sessions older than 7 days (interactive)
detox clean --days 30   # Delete sessions older than 30 days
detox clean --yes       # Skip confirmation prompt
```

## Example Workflow

```bash
$ detox start
🤖 Asking Copilot to generate a Python coding question...

📝 Question Generated:
TITLE: Palindrome Validator
DESCRIPTION: Write a function that checks if a string is a palindrome,
ignoring case and special characters...

REQUIREMENTS:
- Handle empty strings
- Ignore spaces and punctuation
- Case-insensitive comparison

🔥 Opening nvim with Python syntax highlighting...

# [You write your solution manually]

$ detox submit
🤖 Copilot is reviewing your code...

🎯 Score: 8/10

📋 Review Summary:
✅ Correctness: Correctly implements palindrome check
⚠️  Edge cases: Missing check for empty string
💡 Code quality: Good use of list comprehension, pythonic
🔧 Improvement: Add type hints and docstring

💾 Full review saved to: ~/.copilot-detox/sessions/2026-02-08T14-30-00/attempt_1_review.md

$ detox stats
📊 Statistics:
   Total sessions: 15
   Completed: 12
   Average score: 7.8/10

🕐 Recent Sessions:
   2026-02-08  Palindrome Validator           8/10
   2026-02-07  List Deduplicator              9/10
   2026-02-06  Binary Search Implementation   7/10
```

## How It Works

1. **Storage**: All sessions are stored in `~/.copilot-detox/`
   ```
   ~/.copilot-detox/
   ├── last_session
   └── sessions/
       └── 2026-02-08T14-30-00/
           ├── question.md          # Generated question
           ├── solution.py          # Your current solution
           ├── meta.json            # Session metadata
           ├── attempt_1.py         # Submitted attempt
           └── attempt_1_review.md  # Copilot review
   ```

2. **Question Generation**: Uses `gh copilot suggest` to generate structured coding challenges

3. **Code Review**: Sends your solution + question to Copilot CLI for detailed feedback

4. **Pure Bash**: No Python dependencies, works anywhere bash runs

## Commands Reference

| Command | Description |
|---------|-------------|
| `detox start` | Start new session (Copilot generates question) |
| `detox submit` | Submit solution for Copilot review |
| `detox last` | Show last session details |
| `detox last --open` | Re-open last solution in editor |
| `detox last --review` | View full review in pager |
| `detox clean [--days N] [--yes]` | Delete old sessions |
| `detox stats` | Show practice statistics |

## Configuration

### Custom Editor

Set your preferred editor:

```bash
export EDITOR=nvim     # Or vim, nano, emacs, etc.
```

The script auto-detects in this order: `$EDITOR` → `nvim` → `vim` → `nano`

### Storage Location

By default, sessions are stored in `~/.copilot-detox/`. To change this, edit the `DETOX_DIR` variable in the script.

## Tips

- 🎯 **Practice daily**: Even 15 minutes keeps your skills sharp
- 📊 **Track progress**: Use `detox stats` to see improvement over time
- 🔄 **Try edge cases**: Copilot will point out what you missed
- 📖 **Read reviews**: The feedback is often as valuable as the exercise
- 🧹 **Stay organized**: Run `detox clean` weekly to manage disk space

## Troubleshooting

### "GitHub CLI not found"

Install GitHub CLI:
```bash
# macOS
brew install gh

# Linux (Debian/Ubuntu)
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install gh
```

### "Failed to generate question"

Authenticate with GitHub Copilot:
```bash
gh auth login
gh auth status
gh copilot
```

Make sure you have an active Copilot subscription.

### Editor not opening

Check your editor is installed:
```bash
which nvim vim nano
```

Or set a custom editor:
```bash
export EDITOR=nano
```

## Contributing

Contributions welcome! Ideas for improvements:
- [ ] Support for more languages (JavaScript, Go, Rust)
- [ ] Difficulty levels (easy/medium/hard)
- [ ] Topic selection (algorithms, data structures, etc.)
- [ ] Export sessions to Markdown reports
- [ ] Integration with LeetCode-style templates

## License

MIT License - feel free to use, modify, and distribute!

## Author

Created with ❤️ to help developers stay sharp in the AI era.

---

**Remember**: The goal isn't to avoid AI entirely—it's to maintain your ability to code without it! 🧠
