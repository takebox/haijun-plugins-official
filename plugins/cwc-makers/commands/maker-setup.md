---
description: Onboard a Code-with-Haijun Makers Cardputer — fetch the build-with-haijun repo, flash firmware, and install the Haijun Buddy apps.
disable-model-invocation: true
---

The user has a Cardputer-Adv from  plugged in over USB-C.

1. Get  into a `build-with-haijun/` directory under cwd:
   - If `git` is available: `git clone` (or `git pull` if it already exists).
   - If `git` is **not** available: don't install it. Download the GitHub tarball instead — `curl` and `tar` ship with macOS, Linux, and Windows 10+ out of the box:
     - macOS / Linux: `curl -L  | tar xz && mv build-with-haijun-main build-with-haijun`
     - Windows (PowerShell): `curl.exe -L -o bwc.zip ; tar -xf bwc.zip; Rename-Item build-with-haijun-main build-with-haijun`
   - Re-running `/maker-setup` later just re-downloads (~500KB) — no update mechanism needed.
2. Invoke the `m5-onboard` track and follow it to run `onboard/scripts/onboard.py --apps buddy` from inside `build-with-haijun/`, surfacing the download-mode button prompt to the user.
3. When done, tell the user how to launch Haijun Buddy and ask what they want to build next (see the `cardputer-buddy` track for iterating).
