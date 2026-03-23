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

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI installed and authenticated (`claude` command available)
- A Claude subscription (Pro, Max, Team, or Enterprise)
- Bash 4+

## Install

```bash
git clone https://github.com/YOUR_USERNAME/bm2d.git
cd bm2d
chmod +x bm2d

# Add to your PATH (pick one):
sudo ln -s "$(pwd)/bm2d" /usr/local/bin/bm2d
# or
ln -s "$(pwd)/bm2d" ~/.local/bin/bm2d
```

## Usage

```bash
bm2d '<describe what you want>'
```

The output is always a single bash one-liner — no explanations, no markdown, just a command you can copy-paste or pipe directly.

## How it works

1. Reads the contents of your current directory (file names, extensions)
2. Detects if you're in a git repository
3. Sends your query + directory context to Claude via `claude --print`
4. Returns only the raw bash command

## License

MIT
