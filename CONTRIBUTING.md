# Contributing to StereoCrafter WEBUI

Contributions are welcome — bug fixes, optimizations, and new features. This is a community-driven passion project, so every bit helps.

## Project Structure

Only modify files in these locations:

| Path | Purpose |
| :--- | :--- |
| `webui.py` | Main application entry point |
| `gui/` | GUI components (legacy, being phased out) |
| `stereocrafter_ui/` | New WEBUI components (Gradio-based) |

**Do NOT modify:**
- `pipelines/` - Core processing pipelines
- `core/` - Core functionality
- `dependency/` - External dependencies
- Other original enoky/StereoCrafter files

If you need to modify files outside these paths, document the change in `dependencies_update.md` with what changed and why.

---

## Development Workflow

1. Fork the repo and create a branch:
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. Make your changes in the allowed folders.

3. Test:
   ```bash
   python webui.py
   ```

4. Check for errors — test affected features, verify no console errors, test different video resolutions if applicable.

---

## Submitting a Pull Request

```bash
git status
git add webui.py stereocrafter_ui/ gui/
git commit -m "Add feature: brief description"
git push origin feature/your-feature-name
```

**Commit message guidelines:**
- Present tense: "Add feature" not "Added feature"
- Be descriptive but concise
- Reference issue numbers if applicable

Examples:
```
Add manual preview selector to splatting UI
Fix inpainting pipeline loading for local weights
Update file manager with pipeline folder shortcuts
```

Then open a Pull Request at https://github.com/keemzin/StereoCrafter with this template:

```markdown
## Description
Brief description of what this PR does

## Changes Made
- List of specific changes

## Testing
How you tested these changes

## Screenshots (if applicable)

## Checklist
- [ ] Only modified webui.py, gui/, stereocrafter_ui/
- [ ] Tested locally and works as expected
- [ ] No console errors
- [ ] Updated documentation if needed
```

---

## Reporting Issues

1. Go to the Issues page → **New Issue**
2. Include:
   - Clear description of the problem
   - Steps to reproduce
   - Expected vs actual behavior
   - Screenshots/logs if applicable
   - System info (OS, Python version, GPU)

---

## Tips

**Code style:**
- Follow existing patterns in the codebase
- Use meaningful variable names
- Add comments for complex logic
- Keep functions focused

**Testing:**
- Test with different video resolutions
- Test with different parameter combinations
- Check memory usage for large videos

**Common issues:**

PyTorch DLL errors on Windows (`WinError 1114`):
```
OSError: [WinError 1114] A dynamic link library (DLL) initialization routine failed.
```
Use Docker. If you must use venv: install [VC++ Redistributables](https://aka.ms/vs/17/release/vc_redist.x64.exe), verify CUDA 12.8, reinstall PyTorch with `--index-url https://download.pytorch.org/whl/cu128`.

Import errors:
```bash
venv\Scripts\activate   # Windows
source venv/bin/activate  # Linux/Mac
pip install -r requirements.txt
```

CUDA errors:
- Verify CUDA 12.8 is installed
- Check GPU drivers are up to date
- `python -c "import torch; print(torch.cuda.is_available())"`

Model download fails:
- Check your HuggingFace token is valid
- Ensure you've accepted the model licenses on HuggingFace

---

## Getting Help

- **Questions:** Open a discussion
- **Bugs:** Open an issue with details
- **Feature requests:** Open an issue with the "enhancement" label

Thank you for contributing!
