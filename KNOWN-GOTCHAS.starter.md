# KNOWN-GOTCHAS — cross-project starter library

**Maximus copies this into each new project's `Agent Coordination Board/KNOWN-GOTCHAS.md` at standup** (LAW 22 / the Learning Loop), then the team appends project-specific entries on top. These are *cross-project* — toolchain, cloud, and judgment lessons that recur regardless of the stack. Grep this by the **signature** column the moment a build/CI/tool step fails, BEFORE diagnosing from scratch.

> Keep it TRUE: re-verify periodically, dedupe, prune. A gotcha naming a flag/path that no longer exists is worse than none.

## Recurring gotchas (grep the signature first)

| Signature / symptom (what you'd search) | Tag | Root cause | Fix |
|---|---|---|---|
| `maximum number of certificates` · `No profiles for … were found` · `ARCHIVE FAILED` exit 65 (iOS/mac CI) | apple-release | Xcode **automatic** signing mints a new cert per run on ephemeral runners → the account hits its cap. **Separate caps for Apple Development AND Apple Distribution.** Cumulative account state, NOT a code fault. | At the Apple cert portal revoke the oldest of **EACH** type (Dev + Dist), then re-run the job. Durable fix: store a `.p12` in CI instead of per-run minting. |
| `error C2220` + `C4996 'strncpy'` in a Firebase C++ plugin (`variant.h`), Windows build | windows-local | Current Win SDK flags `strncpy` C4996; the plugin promotes warnings→errors. CI image differs, so CI passes. | Define `_CRT_SECURE_NO_WARNINGS`. **Set it from PowerShell** (`$env:CL='/D_CRT_SECURE_NO_WARNINGS'`) — Git Bash MSYS-mangles a leading `/D…` into `C:/Program Files/Git/D…`. (Or use `-D` not `/D` in bash.) |
| `atlstr.h: No such file or directory` (Windows build) | windows-local | `flutter_secure_storage_windows` (and other ATL users) need ATL, which isn't in a default VS install. | Visual Studio Installer → Individual components → **"C++ ATL for v143 build tools (x86 & x64)"**. |
| CMake `<3.5` policy / `CMAKE_POLICY_VERSION_MINIMUM` error on a new runner image | windows-ci | CMake 4.x dropped pre-3.5 policy compat; older C++ SDKs declare a pre-3.5 minimum. | Set env `CMAKE_POLICY_VERSION_MINIMUM=3.5` for the build job. |
| a leading `/Flag` arg becomes `C:/Program Files/Git/Flag…` | git-bash | MSYS path-conversion treats a leading `/` as a Unix path and rewrites it. | Run from PowerShell, OR use the tool's alternate flag form (`-Flag`), OR `MSYS_NO_PATHCONV=1`. |
| `python3.13: command not found` from `bq`/gcloud | gcloud-cli | The CLI hard-looks for a python that isn't on PATH under that exact name. | `export CLOUDSDK_PYTHON="$(command -v python)"` before the command. |
| `Cannot find module 'X'` right after a lockfile change / dependency bump | node-build | The lockfile changed but `node_modules` is stale. | `npm ci` (not just `npm install`) before building; flag "reinstall before build" to whoever deploys. |
| A child PR mysteriously **closed** after you merged its base | forge-stacked-pr | Merging a base branch with `--delete-branch` cascade-closes children (a forge can't reopen a PR whose base is gone). | Rebase the child onto the integration branch and **open a fresh PR**; never assume it survived. |
| Deploy/CI command exits 0 but the change isn't live | deploy-verify | "deploy ran" ≠ "deploy landed" — a stale clone, wrong project, or path filter shipped old code. | Verify the **effect**, not the exit code (e.g. `functions:list` shows the new callables; a probe returns the new behavior). |
| A metric/status someone stated doesn't match the dashboard | judgment-claims | An agent (or human) asserted a number from estimate/memory, not the system of record. | **Pull from the authoritative source** (the counter doc, BQ, the dashboard). Never propagate an unsourced number. (LAW 20.) |
| "Feature X is broken" report | judgment-falseflag | Operator/agent "broken" reports are often **state/config**, not code (signed-out, test-mode key, wrong env). | Confirm live == shipped code, reproduce the exact failing branch, probe external deps BEFORE writing a fix. |
| Re-running a tagged release after a workflow fix doesn't pick up the fix | release-ci | The tag points at the old commit. | Dispatch the workflow **from the fixed branch** with `-f ref=v<version>` (don't just re-run the old tag job). |

## Stack-specific (Flutter + Firebase; delete if N/A)
- **`firebase_analytics` has no Windows/Linux plugin** → desktop emits zero in-app events unless you route them elsewhere (e.g. an authed callable relay). "No data from Windows" is expected, not a bug.
- **macOS folds into the `iOS` platform** in Firebase Analytics when it reuses the iOS plist → "no macOS row" ≠ no macOS data; it's under iOS.
- **MSIX `runFullTrust`** is auto-injected by the `msix` package and can't be removed → Partner Center upload needs a one-time justification.

## Decisions log (the *why* behind non-obvious / irreversible calls — seed examples)
- `<date>` — chose **library-mode over a deployed service** for the telemetry layer — trusted/server consumers don't need the public endpoint's HMAC/provisioning; only untrusted clients relay through an authed callable — revisit if a browser/untrusted surface that can't relay appears.
- `<date>` — **held a launch** to build the gating capability properly rather than ship a half-wired flow — no real traction to protect, and the half-built path would've shipped broken — revisit when the capability + its compliance gate are done.

*(Replace the seed decisions with your project's. Append new gotchas on top; promote anything you hit ~3× into an enforced LAW or a CI guard — Learning Loop step 4.)*
