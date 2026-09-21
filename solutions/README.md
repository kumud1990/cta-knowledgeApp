# Solutions

One folder per mock scenario, slugged from its filename in `mocks/` or `scenarios/`
(lowercased, extension stripped, non-alphanumeric collapsed to `-`).

```
solutions/<slug>/
  solution.md              # your current/latest written solution for this mock
  history/attempt-<N>.md   # auto-saved snapshot of the solution as judged at attempt N
  attempt-<N>-judgment.md  # full cta-mock-judge output for attempt N
```

To start a new mock, copy `_template/solution.md` into `solutions/<slug>/solution.md` and fill
it in. Then ask for it to be judged and logged (see `cta-progress-tracker` skill).

Revising a solution after a judged attempt? Just edit `solution.md` in place — the progress
tracker snapshots each judged version into `history/` before overwriting, so nothing is lost.
