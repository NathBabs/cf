# `cf` – Copy Files to Clipboard as Markdown

A lightweight command-line utility that copies the contents of one or more files to your system clipboard, each clearly separated by its filename and wrapped in a Markdown code block. Perfect for sharing code snippets, documenting projects, or feeding context to LLMs.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## Features

- **Multi‑file support** – Specify any number of files, in any directories.
- **Markdown‑friendly output** – Each file is prefixed by its name (with extension) and enclosed in triple backticks, preserving original formatting and indentation.
- **Cross‑platform** – Works on Linux, macOS, and Windows (via Git Bash / WSL).
- **Robust error handling** – Unreadable files are skipped with a warning; successful files are still copied.
- **No temporary files** – Output streams directly to the clipboard, handling large files gracefully.
- **Zero external dependencies** – Uses only standard Unix tools and a common clipboard utility (which you may already have).

---

## Installation

### 1. Save the script

Create a file named `cf` (no extension) and copy [the script](cf) into it.  
You can do this from the terminal:

```bash
curl -o cf https://raw.githubusercontent.com/yourusername/yourrepo/main/cf
# or manually with nano/vim
```

### 2. Make it executable

```bash
chmod +x cf
```

### 3. Move it to a directory in your `PATH`

Choose one of these:

```bash
# For personal use (recommended)
mkdir -p ~/bin
mv cf ~/bin/

# For system-wide use
sudo mv cf /usr/local/bin/
```

If you used `~/bin`, ensure it’s in your `PATH` (usually it is). If not, add `export PATH="$HOME/bin:$PATH"` to your `~/.bashrc` or `~/.zshrc`.

### 4. Install clipboard dependencies (if needed)

- **macOS**: Clipboard tool `pbcopy` is built‑in – nothing to install.
- **Linux**: Install `xclip` or `xsel`.  
  Debian/Ubuntu: `sudo apt install xclip`  
  Fedora: `sudo dnf install xclip`  
  Arch: `sudo pacman -S xclip`
- **Windows (Git Bash / WSL)**: The `clip` command is usually available. If not, you can install `win32yank` (often included with Git for Windows) and ensure it’s in your `PATH`.

### 5. Test it

```bash
cf file1.txt file2.js
```

Now paste somewhere – you should see the formatted output.

---

## Usage

```bash
cf [FILE]...
```

Copy one or more files to the clipboard.

| Option      | Description                     |
|-------------|---------------------------------|
| `-h, --help`| Show this help message and exit |

### Examples

```bash
# Copy two source files
cf src/index.js src/utils.ts

# Use absolute paths
cf /etc/nginx/nginx.conf ~/.bashrc

# Combine files from different locations
cf ./README.md ../project/package.json

# Use shell globbing
cf lib/**/*.ts
```

After running, your clipboard will contain something like:


index.js

```javascript
console.log('Hello world');
```
<br>
<br>

utils.ts
```
export const add = (a: number, b: number) => a + b;
```


---

## How It Works

1. The script accepts any number of file paths.
2. For each readable file, it outputs:
   - The filename (using `basename`; modify the script if you want the full path).
   - A triple backtick line.
   - The file’s contents (preserved exactly).
   - A closing triple backtick.
   - Two blank lines before the next file.
3. All output is piped directly to the system clipboard command (`pbcopy`, `xclip`, or `clip`).
4. Errors (missing/unreadable files) are reported to stderr but do not interrupt the process.

---

## Customisation

The script is intentionally minimal. You can easily tweak it:

- **Include full paths** – Replace `basename "$file"` with `"$file"`.
- **Append to clipboard** – Replace the pipe `|` with `>>` if your clipboard tool supports appending (not common).
- **Recursive directory processing** – Add a `-r` flag and use `find` to include files inside directories.
- **Output to stdout** – Add an option like `-p` to print instead of copying.

---

## Requirements

- Bash 3.2+
- One of the following clipboard utilities:
  - **macOS**: `pbcopy` (built‑in)
  - **Linux**: `xclip` or `xsel`
  - **Windows (Git Bash / WSL)**: `clip` (built‑in) or `win32yank`

The script will warn you if a required tool is missing.

---

## Contributing

Contributions are welcome! Feel free to open an issue or pull request for:

- Bug fixes
- New features (e.g., recursive mode, file filtering)
- Improved cross‑platform support

Please ensure your changes are backwards‑compatible and well‑documented.

---

## License

This project is licensed under the MIT License – see the [LICENSE](LICENSE) file for details.

---

## Acknowledgements

Inspired by the need to quickly share code with colleagues and LLMs without manual formatting.

---

Enjoy effortless code bundling! If you find this tool useful, consider starring the repository ⭐