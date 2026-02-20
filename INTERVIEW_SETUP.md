# Interview Candidate Setup (macOS + Linux)

This guide gets you from a fresh clone to debugging tests in VS Code (or Cursor) quickly.

## 1) Prerequisites

- Git
- Python 3.8+ (`python3 --version`)
- VS Code

Linux note: if `venv` is missing, install it first:

```bash
# Debian/Ubuntu
sudo apt-get update
sudo apt-get install -y python3-venv
```

## 2) Clone and open the project

- clone down or copy the repository
- navigate to and open the project

## 3) Create a virtual environment and install dependencies

From the project root:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -U pip setuptools wheel
python -m pip install -e ".[testing]"
```

Why `".[testing]"`? It installs this package plus the optional test dependencies defined in `setup.cfg`.

## 4) Configure VS Code once

1. Install extensions:
   - `Python` (ms-python.python)
   - `Python Debugger` (ms-python.debugpy)
2. Select interpreter:
   - `Cmd/Ctrl+Shift+P` -> `Python: Select Interpreter`
   - Choose `.venv/bin/python`
3. Refresh tests:
   - `Cmd/Ctrl+Shift+P` -> `Testing: Refresh Tests`

This repo already includes `.vscode/settings.json` and `.vscode/launch.json` for pytest + debugging.

## 5) Run tests

Run all tests in this file:

```bash
python -m pytest test/test_lookup.py -vv
```

Run one specific test:

```bash
python -m pytest test/test_lookup.py::LookupTest::test_directory_lookup -vv
```

## 6) Debug a failing test in VS Code

Two easy options:

- Inline CodeLens in `test/test_lookup.py`:
  - Click `Debug Test` above the test function.
- Run and Debug panel:
  - Use `Pytest: test_directory_lookup` (single test), or
  - `Pytest: test_lookup.py` (whole file)

Set breakpoints in either test code or library code (for example `mako/lookup.py`) and press `F5`.

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
- `launch.json` shows "property not allowed"
  - Install/enable both VS Code extensions above, then reload window.
- Tests not appearing in Testing panel
  - Confirm interpreter is `.venv/bin/python`, then run `Testing: Refresh Tests`.
