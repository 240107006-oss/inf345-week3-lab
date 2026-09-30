# INF 345 — Week 3 Practice

**Your project starts here**

Deadline: **Sunday 4 October, 23:59** Almaty time. Your last commit before then
is what gets marked.

---

## What this week is

Everything you build for the rest of the semester sits on one repository, and
this week you create it. Next week a pipeline runs its tests. In week 5 it
becomes a container image. In week 9 it runs on Kubernetes. Nothing gets thrown
away and nothing gets restarted.

Two things to do:

1. **Register your project** — one pull request to this repository
2. **Build it to the contract** — in your own repository, marked by `mark_m1.py`

---

## Part 1 — Choose a project

It must be:

- **An HTTP service** that starts with one command and answers on a port
- **Small.** A notes API. A URL shortener. A currency converter. A service that
  returns one fact about a city. Smaller than you think is right.
- **In any language you can read errors in.** Python, Go, Node, Java, C#, Rust —
  the marker does not care and never will, because everything goes through the
  contract below.
- **Yours.** Not a tutorial clone you cannot explain. In December you will
  answer questions about this code with nothing able to answer for you.

**It does not need a database.** It does not need a front end. It does not need
to be original. The course is about the path from your laptop to production —
the app is the thing being carried along that path.

<details>
<summary><b>If you have not chosen by the deadline</b></summary>

Take the default: a service with three endpoints — `GET /` returning a greeting,
`GET /healthz`, and `GET /notes` returning a hard-coded list. Three tests. That
is a legitimate choice and about a fifth of the group will make it. It will not
cost you a single mark. Every milestone from here is about the pipeline.
</details>

## Part 2 — The contract

Because everyone is in a different language, every check in this course goes
through one small interface. Fix it now and it never changes:

```
scripts/run.sh      starts the service; uses $PORT, defaults to 8080
scripts/test.sh     runs the tests; exit 0 = pass; prints "TESTS: n/n"
GET /healthz        200, no database, answers in under a second
```

You wrote scripts exactly like these last week. Same shebang, same
`set -euo pipefail`, same `chmod +x`, same quoting around `"$1"`.

**Three details that look pedantic today and are not:**

`$PORT` **from the environment.** Whoever runs your service decides its port,
not the code: you with `docker run -e PORT=…` in week 5, a Kubernetes manifest in
week 9, and platforms such as Heroku or Cloud Run set it for you. A hardcoded
8080 works on your laptop, passes a careless reading of this brief, and fails in
November when the fix means reopening code you have forgotten. The marker sets
`PORT` to a random number, so it is checked now.

`/healthz` **now, not later.** In week 9 your liveness and readiness probes
call it. Adding it in week 9 means editing the app in week 9.

`TESTS: n/n` **on stdout.** Every framework prints its own summary and none of
them agree. One normalised line makes your test count readable by a machine in
any language. That is the whole course in one line — your output is data for the
next program.

## Part 3 — Mark yourself

```bash
python3 scripts/mark_m1.py --path .          # a local checkout
python3 scripts/mark_m1.py --repo <your url> # exactly what I will run
```

Three gates, then 100 points in four blocks of 25.

| Gate — any failure scores 0 | |
|---|---|
| It is a git repository | |
| **No credential anywhere in the history** | Deleting the file does not help. The scan reads every commit. |
| `scripts/test.sh` exists and is executable | `chmod +x`, and commit the bit |

| Block | 25 points for |
|---|---|
| **runs** | `scripts/run.sh` answers `GET /` within 20s on a random `$PORT` |
| **healthz** | `/healthz` returns 200, non-empty, under a second, twice |
| **tests** | `scripts/test.sh` exits 0, prints `TESTS: n/n` with n ≥ 3, passes twice — **and fails when your application is deleted** (the marker checks this in a scratch copy) |
| **repository quality** | ≥ 5 commits · a pull request merged on GitHub (any merge button — merge, squash or rebase) · `.gitignore` with no build artefacts committed · README covering what it does, how to run it, how to test it, and the port |

You may read [`scripts/mark_m1.py`](scripts/mark_m1.py). It is the rubric and
there is nothing hidden in it.

### The gate worth reading twice

A credential in git history is the one mistake in this course that would
genuinely matter at work. It is also the one students assume they can undo:

```bash
git rm src/config.py && git commit -m "remove it"    # does nothing
```

The key is still in the history, still fetchable by anyone who clones you, and
still valid until somebody revokes it. The marker reads `git log -p --all` for
exactly this reason. Placeholders are fine — `your_api_key_here`, `REPLACE_ME`,
and the vendors' own documentation values are all recognised and allowed.

## Part 4 — Register your project

Fork this repository, then add one file:

```yaml
# projects/<your-github-username>.yml
repo: https://github.com/<your-github-username>/<your-project>
language: python
what: A small notes API
```

Open a pull request. CI checks the repository exists, is public, and is not
someone else's. **Without this I cannot mark you** — the same as the week 1
roster.

## Submitting

There is nothing to submit for the milestone itself. Five minutes after the
deadline, a GitHub Actions workflow clones the URL you registered and runs this
same `mark_m1.py` against whatever is on your default branch — one student per
machine, on GitHub's servers. Push as often as you like before then; what is on
your default branch at 23:59 on Sunday is what counts.

**Why "tests that fail when the app is deleted"?** Because a test that passes no
matter what is not a test. `echo "TESTS: 3/3"` prints the right line and checks
nothing. The marker deletes your application's source files in a throwaway copy
and runs your tests again. They must go red. If yours stay green, they are not
calling your service.

## Stuck?

In the session: chat, immediately. Afterwards: email with `INF 345` and your
group in the subject, from your `@sdu.edu.kz` address, with your repository
link and the marker's output. Two working days.

Helping each other is encouraged. Submitting someone else's repository is not,
and git history shows how work was done.
