# noomoo demo

This repository shows the [noomoo](https://noomoo.dev) gate turn from red to green. It has noomoo 3.0.1 installed and one task, TASK-001. On the `start` branch, the task claims to cover a requirement that does not exist, so the gate fails. You fix one line, and the gate passes.

![The noomoo gate goes red on the start branch, then green after one line is fixed](media/noomoo-red-green.gif)

The same recording as video: [`media/noomoo-red-green.webm`](media/noomoo-red-green.webm).

You need Node.js 20 or newer and git. Nothing else: no account, no token, no Claude Code.

## 1. Clone the `start` branch

```
git clone --branch start https://github.com/TaskForceNow/noomoo-demo.git
cd noomoo-demo
```

## 2. Install

```
npm ci
```

Expected output (the time differs):

```
added 1 package, and audited 2 packages in 1s

found 0 vulnerabilities
```

noomoo is the only dependency, and it has no runtime dependencies of its own.

## 3. Run the gate: red

```
npx noomoo validate
```

Expected output (the run id and counts of skipped checks may differ):

```
▸ links failed
PIPELINE [full] 🔴 fail · phase contract · ...
checks: PASS 38 · FAIL 1 · WARN 0 · ...
  ── Content — requires a decision
  [error] task-001-implement-slugify.md → TASK-001 → unknown-req-id: Requirement ID "REQ-001-3" was not declared in docs/requirements/ (expected: **REQ-001-3.**).
```

The exit code is 1. `docs/requirements/req-001-slug-generation.md` declares only REQ-001-1 and REQ-001-2, so the task points at nothing.

The same gate runs in the `pre-commit` and `pre-push` hooks, so they refuse this broken state. The one commit on `start` was pushed with the hooks turned off, on purpose.

## 4. Fix the task

Open `docs/tasks/task-001-implement-slugify.md` and change the `Covers` line:

```diff
- - Covers: REQ-001-1; REQ-001-3; UC-001
+ - Covers: REQ-001-1; REQ-001-2; UC-001
```

Or run the same edit from the shell:

```
node -e "const fs=require('fs'),f='docs/tasks/task-001-implement-slugify.md';fs.writeFileSync(f,fs.readFileSync(f,'utf8').replace('REQ-001-3','REQ-001-2'))"
```

## 5. Run the gate again: green

```
npx noomoo validate
```

Expected output:

```
PIPELINE [full] 🟢 ok · phase contract · ...
checks: PASS 39 · FAIL 0 · WARN 0 · ...
```

The exit code is 0. `git diff origin/main -- docs` now prints nothing: your edit brought the task back to the state of `main`.

## What else is here

- `docs/` holds the requirement, use case, feature and task that the gate reads.
- `config/pipeline.json` is the noomoo configuration; `npx noomoo setup --yes` wrote it.
- `.githooks/` and `.github/workflows/noomoo.yml` run the same gate before a commit and in CI.
- `media/` holds the recording shown at the top of this page, as GIF and WebM.

The next step after the gate is the task itself. With the noomoo plugin for Claude Code, `/noomoo:task TASK-001` carries it from plan to pull request. See the [quickstart](https://noomoo.dev/docs/quickstart.html) and the [plugin page](https://noomoo.dev/docs/plugin.html).

## License

Apache-2.0. See `LICENSE`.
