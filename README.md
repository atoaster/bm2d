# bm2d

AI-powered bash one-liner lookup. Describe what you want to do in plain English, get a ready-to-run bash command back.

bm2d is context-aware — it reads the files in your current directory to generate accurate commands that match your actual file names and extensions.

## Examples

```bash
# In a folder of mixed video files:
$ bm2d 'convert all these videos to h265'
for f in *.{mp4,mkv,avi}; do [ -f "$f" ] && ffmpeg -i "$f" -c:v libx265 -preset medium -c:a aac "${f%.*}_h265.mp4"; done

# In a project directory:
$ bm2d 'find all TODO comments in python files'
grep -rn 'TODO' --include='*.py' .

# Anywhere:
$ bm2d 'show disk usage of subdirectories sorted by size'
du -sh */ | sort -rh
```

## Install

```bash
git clone https://github.com/atoaster/bm2d.git
cd bm2d
chmod +x bm2d

# Add to your PATH (pick one):
sudo ln -s "$(pwd)/bm2d" /usr/local/bin/bm2d
# or
ln -s "$(pwd)/bm2d" ~/.local/bin/bm2d
```

For quick access, add an alias to your `~/.bashrc` or `~/.zshrc`:

```bash
alias bm='bm2d'
```

Then just use `bm 'your query'` instead of `bm2d`.

## Backend Setup

bm2d supports three AI backends. Set the `BM2D_BACKEND` environment variable to choose one (defaults to `claude`).

### Claude (default)

Uses the [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI with your existing Claude subscription. No API key needed.

**Requirements:**
- Claude Code CLI installed and authenticated (`claude` command available)
- A Claude subscription (Pro, Max, Team, or Enterprise)

```bash
# No extra config needed — just use it:
bm2d 'list all files larger than 100MB'

# Or be explicit:
BM2D_BACKEND=claude bm2d 'list all files larger than 100MB'
```

### OpenAI

Uses the OpenAI API with GPT-4o.

**Requirements:**
- An OpenAI API key ([get one here](https://platform.openai.com/api-keys))
- `curl` and `jq` installed

```bash
# Set your API key:
export OPENAI_API_KEY="your-key-here"

# Use the OpenAI backend:
BM2D_BACKEND=openai bm2d 'list all files larger than 100MB'
```

To make it permanent, add to your `~/.bashrc` or `~/.zshrc`:

```bash
export BM2D_BACKEND=openai
export OPENAI_API_KEY="your-key-here"
```

### Gemini

Uses the Google Gemini API with Gemini 2.5 Flash.

**Requirements:**
- A Gemini API key ([get one here](https://aistudio.google.com/apikey))
- `curl` and `jq` installed

```bash
# Set your API key:
export GEMINI_API_KEY="your-key-here"

# Use the Gemini backend:
BM2D_BACKEND=gemini bm2d 'list all files larger than 100MB'
```

To make it permanent, add to your `~/.bashrc` or `~/.zshrc`:

```bash
export BM2D_BACKEND=gemini
export GEMINI_API_KEY="your-key-here"
```

## Usage

```bash
bm2d '<describe what you want>'
```

The output is always a single bash one-liner — no explanations, no markdown, just a command you can copy-paste or pipe directly.

## How it works

1. Reads the contents of your current directory (file names, extensions)
2. Detects if you're in a git repository
3. Sends your query + directory context to your chosen AI backend
4. Returns only the raw bash command

## Dependencies

| Backend | Requirements |
|---------|-------------|
| Claude  | `claude` CLI |
| OpenAI  | `curl`, `jq`, `OPENAI_API_KEY` |
| Gemini  | `curl`, `jq`, `GEMINI_API_KEY` |

All backends require **Bash 4+**.

## License

MIT
