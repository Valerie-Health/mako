# Interview Candidate Setup (macOS + Linux)

This guide gets you from a fresh clone to running and debugging tests quickly.

The core setup is editor-agnostic and works from any terminal. A separate optional section is included for VS Code/Cursor users.

## 1) Prerequisites

- Git
- Python 3.8+ (`python3 --version`)

Linux note: if `venv` is missing, install it first:

```bash
# Debian/Ubuntu
sudo apt-get update
sudo apt-get install -y python3-venv
```

## 2) Clone the project

```bash
git clone <repo-url>
cd mako
```

## 3) Create a virtual environment and install dependencies

From the project root:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -U pip setuptools wheel
python -m pip install -e ".[testing]"
```

Why `".[testing]"`? It installs this package plus the optional test dependencies defined in `setup.cfg`.

## 4) Run tests from terminal (works in any editor)

Run all tests in this file:

```bash
python -m pytest test/test_lookup.py -vv
```

Run one specific test:

```bash
python -m pytest test/test_lookup.py::LookupTest::test_directory_lookup -vv
```

## 5) Debugging options (editor-agnostic)

You can always use `pytest` output and add temporary print/logging statements while investigating failures.

If you prefer step-through debugging with breakpoints, use your editor's Python debugger and run the same test commands above.

## 6) Optional: VS Code/Cursor setup

If you are using VS Code or Cursor, this repo already includes `.vscode/settings.json` and `.vscode/launch.json`.

1. Install extensions:
   - `Python` (`ms-python.python`)
   - `Python Debugger` (`ms-python.debugpy`)
2. Select interpreter:
   - `Cmd/Ctrl+Shift+P` -> `Python: Select Interpreter`
   - Choose `.venv/bin/python`
3. Refresh tests:
   - `Cmd/Ctrl+Shift+P` -> `Testing: Refresh Tests`

To debug in VS Code/Cursor:

- Inline CodeLens in `test/test_lookup.py`: click `Debug Test` above a test.
- Or use Run and Debug configs:
  - `Pytest: test_directory_lookup` (single test)
  - `Pytest: test_lookup.py` (whole file)

## Troubleshooting

- `pip: command not found`
  - Use `python -m pip ...` (or `python3 -m pip ...`) instead of plain `pip`.
- `No module named pytest`
  - Your virtualenv is not active, or dependencies were not installed.
  - Re-run:
    ```bash
    source .venv/bin/activate
    python -m pip install -e ".[testing]"
    ```
- VS Code/Cursor: `launch.json` shows "property not allowed"
  - Install/enable both Python extensions above, then reload window.
- VS Code/Cursor: tests not appearing in Testing panel
  - Confirm interpreter is `.venv/bin/python`, then run `Testing: Refresh Tests`.
