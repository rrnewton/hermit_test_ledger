# Compatibility scorecard

Last regenerated **2026-10-05T09:51:18Z** from `https://github.com/rrnewton/hermit_test_ledger.git` commit `1440bf81cdfb46a8f5e28d629388d1ff9afc6f35`, reading 290898 series row(s). Validate run published in this series snapshot, with its cell comparisons: `validate-coord2-d1744e9fc07e-1791193403382220055-1138567-499f31af` (1045). Earlier validate runs still supplying comparisons: `validate-buck-re-3-b84faa26c355-1791103535341810231-4116164-a06293f4` current (1050), `validate-buck-re-3-66db685c5279-1791099824835735230-1153730-3be6996b` current (1046), `validate-claude-coord-mega-lander-4c0b7daa8ae6-1791155208411569193-894674-eab4fa86` current (1042), `validate-gate-select-f85de5d891ab-1791150197069282693-1112723-7a425489` current (1042), `validate-netreplay-rework-7f8e75ff4d3a-1791184155517346601-1394308-0ff4a7b6` current (1042), `validate-ops-tick-cec1ef5f402d-55392313f8a8` current (1042), `validate-tickhub-ops-3446e8af8cc5-1791159936556159007-429447-5717f395` current (1042), `validate-ops-tick-20d49d84ec23-914f280b5537` current (1039), and 26 more.

This table is derived from the manifest, not from a separately maintained parent-workspace CSV. `./ci/compat-envelope/scorecard.rs check` verifies it.

The count table includes all **14800** cells in the manifest; no row is omitted. A cell is **Selected by full** exactly when it appears in `ci/expected-e2e-plan.json`. A cell is **Not selected by full** when it is in the manifest but absent from that plan. Selection is not a test result: a cell not selected by full may have passed, failed, produced no verdict, or never run. Of these cells, **1231** are selected by full, **714** are not selected by full, and **12855** are **Not applicable**.

Every selected `verify` cell that does not declare the stripped comparator, and every seed in a selected `chaos` cell, runs the same backend twice. The manifest runner adds `--verify-strict` when the selected Hermit binary supports it, and accepts a result only when the typed report says `verified=true`, `verdict=matched`, `bitwise_parity=true`, `strictness=canonical`, `compare_logs=true`, a named canonical `record_envelope`, and both INFO-message counts are nonzero. Bare `--verify` remains a Stripped comparison when invoked directly and does not satisfy this regression plan. **189** of the **1221** selected `verify` cells declare `comparator: stripped` (`compat` on `ptrace`: 189). They run Hermit's default `--verify` and pass only on a verified, matched report of a non-empty stripped comparison; they are below L2, never `bitwise_parity`, and are counted in these tables as selected, not as canonical. These same-backend results do not establish cross-backend parity.

| Backend | Selected by full | Not selected by full | Not applicable | In the manifest |
| --- | ---: | ---: | ---: | ---: |
| `ptrace` | 561 | 372 | 1842 | 2775 |
| `dbt` | 26 | 59 | 2690 | 2775 |
| `kvm` | 258 | 8 | 2509 | 2775 |
| `sabre` | 240 | 239 | 2296 | 2775 |
| `liteinst` | 146 | 3 | 2626 | 2775 |
| `native` | 0 | 33 | 892 | 925 |
| **Total** | **1231** | **714** | **12855** | **14800** |

## Denominator, and why the percentage is not comparable across changes to it

Selected by full is **1231 of 14800**, which is **8.32%** — over THIS population and no other. The population is every combination the manifest declares, and it is composed of:

- backends: `ptrace`, `dbt`, `kvm`, `sabre`, `liteinst`, `native`
- modes: `chaos`, `naked`, `replay`, `verify`

⚠️ **12855 of those 14800 cells are NOT APPLICABLE** — their backend is not applicable for their mode, so they were never asked to run and cannot pass or fail. Over the 1945 cells that CAN run, selected by full is **63.29%**.

⚠️ **DO NOT QUOTE THAT SECOND FIGURE AS PROGRESS.** It is the same 1231 cells selected by full measured against a smaller denominator. Nothing was fixed to produce it; it is what the first figure always meant once the cells that cannot run are excluded. Quote both or neither, and never compare one against the other as though something moved.

⚠️ **Adding or removing a backend or mode changes this denominator and therefore the percentage, without anything about the product changing.** Removing a backend whose cells are mostly not selected RAISES the reported figure; adding manifest cells that are not selected LOWERS it. Neither is progress. Before comparing this percentage against an earlier one, diff the two lists above: if they differ, the numbers are not comparable and the difference is not a result.

The mode view makes the current order of work explicit: expand `verify` first, then `replay`, then `chaos`. Each backend cell is `selected by full / in the manifest`; an em dash means that mode does not exist for that backend. The summary columns use the same selection and applicability facts as the table above.

| Mode | `ptrace` | `dbt` | `kvm` | `sabre` | `liteinst` | `native` | Selected by full | Not selected by full | Not applicable | In the manifest |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| `verify` | 551 / 925 | 26 / 925 | 258 / 925 | 240 / 925 | 146 / 925 | — | 1221 | 541 | 2863 | 4625 |
| `replay` | 4 / 925 | 0 / 925 | 0 / 925 | 0 / 925 | 0 / 925 | — | 4 | 139 | 4482 | 4625 |
| `chaos` | 6 / 925 | 0 / 925 | 0 / 925 | 0 / 925 | 0 / 925 | — | 6 | 1 | 4618 | 4625 |
| `naked` | — | — | — | — | — | 0 / 925 | 0 | 33 | 892 | 925 |
| **Total** | | | | | | | **1231** | **714** | **12855** | **14800** |

## Ptrace by manifest category

This view uses the same Basic Sanity Milestone 1 contracts as the tables above, but makes the ptrace workload mix visible. Each entry is `selected by full / in the manifest`; `custom` commands are not part of this denominator.

| Manifest category | Verify | Replay | Chaos | Selected by full | In the manifest |
| --- | ---: | ---: | ---: | ---: | ---: |
| `applications` | 3 / 6 | 0 / 6 | 0 / 6 | 3 | 18 |
| `bin-c` | 2 / 2 | 0 / 2 | 0 / 2 | 2 | 6 |
| `c-programs` | 273 / 278 | 3 / 278 | 3 / 278 | 279 | 834 |
| `chaos-c` | 1 / 1 | 0 / 1 | 1 / 1 | 2 | 3 |
| `compat` | 189 / 551 | 0 / 551 | 0 / 551 | 189 | 1653 |
| `data-handling` | 6 / 6 | 0 / 6 | 0 / 6 | 6 | 18 |
| `debugger-c` | 1 / 1 | 0 / 1 | 0 / 1 | 1 | 3 |
| `determinism-stress` | 5 / 6 | 0 / 6 | 1 / 6 | 6 | 18 |
| `determinism-stress-c` | 11 / 11 | 0 / 11 | 1 / 11 | 12 | 33 |
| `language-runtimes` | 19 / 19 | 0 / 19 | 0 / 19 | 19 | 57 |
| `shared-futex-c` | 1 / 4 | 0 / 4 | 0 / 4 | 1 | 12 |
| `system-utils` | 39 / 39 | 1 / 39 | 0 / 39 | 40 | 117 |
| `util-c` | 1 / 1 | 0 / 1 | 0 / 1 | 1 | 3 |

Ordinary full validation executes 1236 cells: the 1231 comparable compatibility cells selected by full above (including 6 chaos-mode race-exposure checks), and 5 explicit custom commands outside the comparable denominator. A passing validate must produce a fresh result for all of them; a failing selected cell is a regression, not permission to remove it from the plan.

### Selected custom commands outside the comparable denominator

These rows are part of the selected regression denominator even though they are not rows in `ci/compat-envelope/cells.json`. Their exact identities come from `ci/expected-e2e-plan.json`; `scorecard.rs check` refuses any selected row that is not accounted for by either this table or the comparable green cells above.

| Lane | Category | Test | Mode | Backend |
| --- | --- | --- | --- | --- |
| `portable` | `c-programs` | `c-programs/environment-and-workdir` | `custom` | `ptrace` |
| `portable` | `c-programs` | `c-programs/io-uring-fallback` | `custom` | `dbt` |
| `portable` | `c-programs` | `c-programs/io-uring-fallback` | `custom` | `ptrace` |
| `portable` | `system-utils` | `system-utils/clock-determinism` | `custom` | `liteinst` |
| `portable` | `system-utils` | `system-utils/clock-determinism` | `custom` | `ptrace` |

## Selection and measurement

Selection and observation answer different questions. The first column says whether full validation selects a cell. The per-cell `measurement` value says what retained evidence observed: `never-measured`, `measured-and-passed`, `measured-no-verdict`, `diverged-unlocated`, or `diverged`. Of the cells selected by full, **0** have `never-measured`; of the cells not selected by full, **392** have `measured-and-passed`.

Retained history that has not been imported is not counted here. A stored measurement does not establish that it describes current code; `show` reports whether the recorded last test still matches `HEAD:detcore`.

The count table includes all **14800** cells in the manifest; no row is omitted. These claims use the same counts printed in the table below.

| Selection by full | `never-measured` | `measured-and-passed` | `measured-no-verdict` | `diverged-unlocated` | `diverged` | In the manifest |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Selected by full | 0 | 1071 | 0 | 3 | 157 | 1231 |
| Not selected by full | 283 | 392 | 11 | 0 | 28 | 714 |
| Not applicable | 12559 | 199 | 80 | 0 | 17 | 12855 |
| **Total** | **12842** | **1662** | **91** | **3** | **202** | **14800** |

Cells whose stored `measurement` is not `never-measured` are shown individually so selection and measurement remain visible together.

| Test | Mode | Backend | Selection by full | Measurement |
| --- | --- | --- | --- | --- |
| `applications/c-toolchain-workflow` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `applications/c-toolchain-workflow` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `applications/c-toolchain-workflow` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `applications/example-timed-progress-bar` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `applications/example-timed-progress-bar` | `verify` | `kvm` | `Not selected by full` | `measured-and-passed` |
| `applications/example-timed-progress-bar` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `applications/example-timed-progress-bar` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `applications/git-repository-workflow` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `applications/git-repository-workflow` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `applications/git-repository-workflow` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `applications/timed-progress-bar` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `applications/timed-progress-bar` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `applications/timed-progress-bar` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `applications/timed-progress-bar` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `bin-c/posix-timer-test` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `bin-c/posix-timer-test` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `bin-c/posix-timer-test` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `bin-c/robust-futex-test` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `bin-c/robust-futex-test` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `bin-c/robust-futex-test` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/acct-refusal-probe` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/acct-refusal-probe` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/acct-refusal-probe` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/acct-refusal-probe` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/acct-refusal-probe` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/add-key-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/add-key-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/add-key-enosys` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/add-key-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/add-key-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/adjtimex-deterministic` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/adjtimex-deterministic` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/adjtimex-deterministic` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/adjtimex-deterministic` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/adjtimex-deterministic` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/aio-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/aio-refusal` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/aio-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/aio-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/append-pwrite` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/append-pwrite` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/append-pwrite` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/append-pwrite` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/arch-prctl-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/arch-prctl-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/arch-prctl-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/bind-getsockname` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/bind-getsockname` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/bind-getsockname` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/bind-getsockname` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/bpf-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/bpf-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/bpf-enosys` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/bpf-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/bpf-enosys` | `verify` | `sabre` | `Selected by full` | `diverged` |
| `c-programs/cachestat-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/cachestat-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/cachestat-enosys` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/cachestat-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/cachestat-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/cachestat-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/cachestat-refusal` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/cachestat-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/cachestat-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/child-subreaper-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/child-subreaper-refusal` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/child-subreaper-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/child-subreaper-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/clock-adjtime-deterministic` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/clock-adjtime-deterministic` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/clock-adjtime-deterministic` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/clock-adjtime-deterministic` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/clock-adjtime-deterministic` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/clone` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/clone` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/clone` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/close-range-fds` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/close-range-fds` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/close-range-fds` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/close-range-fds` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal-probe` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal-probe` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal-probe` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/copy-file-range-refusal-probe` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal-probe` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpu-virtualization` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/cpu-virtualization` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpu-virtualization` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpu-virtualization` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpu-virtualization` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/cwd-roundtrip` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/cwd-roundtrip` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/cwd-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/cwd-roundtrip` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-copied-tiocgpgrp` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `c-programs/dbt-copied-tiocgpgrp` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-copied-tiocgpgrp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-copied-tiocgpgrp` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-exec-failure` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-exec-failure` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/dbt-exec-failure` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-exec-failure` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-execveat-unsupported` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/dbt-execveat-unsupported` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-execveat-unsupported` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/dbt-execveat-unsupported` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-execveat-unsupported` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `c-programs/dbt-mmap-exec` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-mmap-exec` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-mmap-exec` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/dbt-mmap-exec` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-mmap-exec` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-pid-virtualization` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/dbt-pid-virtualization` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `c-programs/dbt-pid-virtualization` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `c-programs/dbt-prlimit-self` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-prlimit-self` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/dbt-prlimit-self` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-prlimit-self` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-self-sigqueue` | `verify` | `dbt` | `Not selected by full` | `measured-no-verdict` |
| `c-programs/dbt-self-sigqueue` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-self-sigqueue` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-self-sigqueue` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-unsupported-syscall` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/dbt-unsupported-syscall` | `verify` | `ptrace` | `Not selected by full` | `measured-no-verdict` |
| `c-programs/dbt-unsupported-syscall` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/dbt-wait-accounting` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-wait-accounting` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/dbt-wait-accounting` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-wait-accounting` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-wait-lifecycle` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-wait-lifecycle` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/dbt-wait-lifecycle` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-wait-lifecycle` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/dup-shared-offset` | `verify` | `dbt` | `Not selected by full` | `diverged` |
| `c-programs/dup-shared-offset` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/dup-shared-offset` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/dup-shared-offset` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dup-shared-offset` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/environment-and-workdir` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/environment-and-workdir` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/environment-and-workdir` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/epoll-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-determinism` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/epoll-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-determinism` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-pwait2` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-pwait2` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/epoll-pwait2` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-pwait2` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-readiness` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-readiness` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/epoll-readiness` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-readiness` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/event-delivery-ordering` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/event-delivery-ordering` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/event-delivery-ordering` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/eventfd-semantics` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/eventfd-semantics` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/eventfd-semantics` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/eventfd-semantics` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/faccessat2-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/faccessat2-flags` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/faccessat2-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/faccessat2-flags` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/fadvise-hints` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fadvise-hints` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/fadvise-hints` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fadvise-hints` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/fallocate-extents` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fallocate-extents` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/fallocate-extents` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fallocate-extents` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/fchmod-bits` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fchmod-bits` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/fchmod-bits` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fchmod-bits` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/fchmodat2-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fchmodat2-flags` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/fchmodat2-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fchmodat2-flags` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/fcntl-owner` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fcntl-owner` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/fcntl-owner` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fcntl-owner` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/fd-duplication` | `verify` | `dbt` | `Not selected by full` | `diverged` |
| `c-programs/fd-duplication` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fd-duplication` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/fd-duplication` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fd-duplication` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/file-backed-mmap` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/file-backed-mmap` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/file-backed-mmap` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/file-backed-mmap` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/file-io-roundtrip` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/file-io-roundtrip` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/file-io-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/file-io-roundtrip` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/flock-lifecycle` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/flock-lifecycle` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/flock-lifecycle` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/flock-lifecycle` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/fork-exec-pipeline` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `c-programs/fork-exec-pipeline` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/fork-exec-pipeline` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fork-exec-pipeline` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `c-programs/fp-reduction-nondeterminism` | `chaos` | `ptrace` | `Selected by full` | `diverged-unlocated` |
| `c-programs/fp-reduction-nondeterminism` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/fp-reduction-nondeterminism` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `c-programs/fp-reduction-nondeterminism` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/fsync-durability` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fsync-durability` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/fsync-durability` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fsync-durability` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/ftruncate-sparse` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/ftruncate-sparse` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/ftruncate-sparse` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ftruncate-sparse` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-requeue-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/futex-requeue-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-requeue-enosys` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-requeue-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-requeue-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-waitv-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/futex-waitv-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-waitv-enosys` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/futex-waitv-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-waitv-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-wake-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/futex-wake-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-wake-enosys` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-wake-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-wake-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/get-robust-list-child` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `c-programs/get-robust-list-child` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/get-robust-list-child` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/get-robust-list-child` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/get-robust-list-self` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/get-robust-list-self` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/get-robust-list-self` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/get-robust-list-self` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/get-robust-list-self` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/get-robust-list-thread` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/get-robust-list-thread` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/getcpu` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/getcpu` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/getcpu` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/getcpu` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/getcpu-identity` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/getcpu-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/getcpu-identity` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/getcpu-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/getcpu-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/getitimer-determinism-probe` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/getitimer-determinism-probe` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/getitimer-determinism-probe` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/getitimer-determinism-probe` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/getpriority-identity` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/getpriority-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/getpriority-identity` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/getpriority-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/getpriority-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/getrusage-self-accounting` | `verify` | `liteinst` | `Not selected by full` | `measured-no-verdict` |
| `c-programs/getrusage-self-accounting` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/getrusage-self-accounting` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/getsockopt-null` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/getsockopt-null` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/getsockopt-null` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/getsockopt-null` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/hardware-trap-identity` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/hardware-trap-identity` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/hardware-trap-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/hardware-trap-identity` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/hello-alarm` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/hello-alarm` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/hello-alarm` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/hello-nostdlib` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/hello-nostdlib` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/hello-nostdlib` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/hello-nostdlib` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/hello-signals` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/hello-signals` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/hello-signals` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/host-identity` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/host-identity` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/host-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/host-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/inline-syscall-sites` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/inline-syscall-sites` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/inline-syscall-sites` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/inline-syscall-sites` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/inotify-watch` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/inotify-watch` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/inotify-watch` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/io-uring-fallback` | `verify` | `dbt` | `Not selected by full` | `measured-no-verdict` |
| `c-programs/io-uring-fallback` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/io-uring-fallback` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/io-uring-fallback` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/io-uring-fallback` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/io-uring-ring-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/io-uring-ring-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/io-uring-ring-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/io-uring-ring-determinism` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/ioctl-fioclex` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/ioctl-fioclex` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/ioctl-fioclex` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/ioctl-fioclex` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ioctl-fioclex` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/ioctl-fionread` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/ioctl-fionread` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/ioctl-fionread` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ioctl-fionread` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/ioctl-siocethtool` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/ioctl-siocethtool` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/ioctl-siocethtool` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ioctl-siocethtool` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/ipc-determinism` | `chaos` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ipc-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/ipc-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ipc-determinism` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/just-spin` | `verify` | `kvm` | `Not selected by full` | `diverged` |
| `c-programs/just-spin` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/just-spin` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/kcmp-eperm` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/kcmp-eperm` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/kcmp-eperm` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/kcmp-eperm` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/kcmp-eperm` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/kcmp-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/kcmp-refusal` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/kcmp-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/kcmp-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/keyctl-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/keyctl-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/keyctl-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/keyctl-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/keyctl-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/keyctl-passthrough` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/keyctl-passthrough` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/keyctl-passthrough` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/keyctl-passthrough` | `verify` | `sabre` | `Selected by full` | `diverged` |
| `c-programs/linkat-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/linkat-flags` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/linkat-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/linkat-flags` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/listmount-enosys` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/listmount-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/listmount-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/listmount-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/listmount-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/liteinst-advanced` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/liteinst-advanced` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/liteinst-advanced` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/lseek-positioning` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/lseek-positioning` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/lseek-positioning` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/lseek-positioning` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/lseek-positioning` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/lsm-get-self-attr-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/lsm-get-self-attr-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/lsm-get-self-attr-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/lsm-get-self-attr-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/lsm-get-self-attr-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/lsm-list-modules-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/lsm-list-modules-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/lsm-list-modules-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/lsm-list-modules-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/lsm-list-modules-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/lsm-set-self-attr-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/lsm-set-self-attr-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/lsm-set-self-attr-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/lsm-set-self-attr-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/lsm-set-self-attr-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/madvise-determinism` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/madvise-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/madvise-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/madvise-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/madvise-determinism` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/map-shadow-stack-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/map-shadow-stack-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/map-shadow-stack-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/map-shadow-stack-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/map-shadow-stack-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mce-kill-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mce-kill-refusal` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/mce-kill-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mce-kill-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/membarrier-query` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/membarrier-query` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/membarrier-query` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/membarrier-query` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/memfd-create` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/memfd-create` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/memfd-create` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/memfd-create` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/memfd-secret-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/memfd-secret-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/memfd-secret-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/memfd-secret-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/memfd-secret-enosys` | `verify` | `sabre` | `Selected by full` | `diverged` |
| `c-programs/meminfo-available-deterministic` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/meminfo-available-deterministic` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/meminfo-available-deterministic` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/meminfo-available-deterministic` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/meminfo-cached-deterministic` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/meminfo-cached-deterministic` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/meminfo-cached-deterministic` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/meminfo-cached-deterministic` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/meminfo-free-deterministic` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/meminfo-free-deterministic` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/meminfo-free-deterministic` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/meminfo-free-deterministic` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/memorypress` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/memorypress` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/memorypress` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/memorypress` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mempolicy-default` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mempolicy-default` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/mempolicy-default` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mempolicy-default` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mincore-residency` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mincore-residency` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/mincore-residency` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mincore-residency` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mixed-inline-and-libc-syscalls` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mixed-inline-and-libc-syscalls` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/mixed-inline-and-libc-syscalls` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mixed-inline-and-libc-syscalls` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mkdir-rmdir` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mkdir-rmdir` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/mkdir-rmdir` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mkdir-rmdir` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mknod-special` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mknod-special` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/mknod-special` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mknod-special` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism-heap` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism-heap` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/mmap-determinism-heap` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism-heap` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism-shared` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism-shared` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/mmap-determinism-shared` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism-shared` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-layout-pointer-order` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-layout-pointer-order` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-layout-pointer-order` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-layout-pointer-order` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-stress-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-stress-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/mmap-stress-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-stress-determinism` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/msync-writeback` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/msync-writeback` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/msync-writeback` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/msync-writeback` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-at-eopnotsupp` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-at-eopnotsupp` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/name-to-handle-at-eopnotsupp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-at-eopnotsupp` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-directory-eopnotsupp` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-directory-eopnotsupp` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/name-to-handle-directory-eopnotsupp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-directory-eopnotsupp` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-empty-path-eopnotsupp` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-empty-path-eopnotsupp` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-empty-path-eopnotsupp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-empty-path-eopnotsupp` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-refusal` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/name-to-handle-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-regular-eopnotsupp` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-regular-eopnotsupp` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-regular-eopnotsupp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-regular-eopnotsupp` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/nanosleep-par` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `c-programs/nanosleep-par` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/nanosleep-par` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `c-programs/nanosleep-par` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `c-programs/nanosleep-threads-nocrash` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/nanosleep-threads-nocrash` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/nanosleep-threads-simple` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/nanosleep-threads-simple` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/netlink-autobind-generic` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/netlink-autobind-generic` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/netlink-autobind-generic` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/netlink-autobind-generic` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/netlink-autobind-route` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/netlink-autobind-route` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/netlink-autobind-route` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/netlink-autobind-route` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/netlink-autobind-usersock` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/netlink-autobind-usersock` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/netlink-autobind-usersock` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/netlink-autobind-usersock` | `verify` | `sabre` | `Selected by full` | `diverged` |
| `c-programs/netns-cookie-tcp4` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/netns-cookie-tcp4` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/netns-cookie-tcp4` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/netns-cookie-tcp4` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/netns-cookie-tcp6` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/netns-cookie-tcp6` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/netns-cookie-tcp6` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/netns-cookie-tcp6` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/netns-cookie-udp4` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/netns-cookie-udp4` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/netns-cookie-udp4` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/netns-cookie-udp4` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/no-new-privs-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/no-new-privs-refusal` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/no-new-privs-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/no-new-privs-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/numa-node-identity` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/numa-node-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/numa-node-identity` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/numa-node-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/numa-node-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/o-tmpfile-anon` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/o-tmpfile-anon` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/o-tmpfile-anon` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/o-tmpfile-anon` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/openat-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/openat-flags` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/openat-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/openat-flags` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/openat2-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/openat2-refusal` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/openat2-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/openat2-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/path-file-ops` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/path-file-ops` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/path-file-ops` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/path-file-ops` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pause-alarm-interrupt` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/pause-alarm-interrupt` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pause-alarm-interrupt` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/perf-event-hardware-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/perf-event-hardware-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/perf-event-hardware-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/perf-event-hardware-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/perf-event-hardware-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/perf-event-open-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/perf-event-open-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/perf-event-open-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/perf-event-open-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/perf-event-open-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/perf-event-software-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/perf-event-software-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/perf-event-software-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/perf-event-software-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/perf-event-software-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/perf-event-watchpoint-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/perf-event-watchpoint-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/perf-event-watchpoint-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/perf-event-watchpoint-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/perf-event-watchpoint-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/periodic-setitimer-delivery` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/periodic-setitimer-delivery` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/periodic-setitimer-delivery` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/personality-domain` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/personality-domain` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/personality-domain` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pid-probe` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/pid-probe` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pid-probe` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/pid-probe` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pid-probe` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-open-self` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-open-self` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/pidfd-open-self` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-open-self` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-open-self-pair` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/pidfd-open-self-pair` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-open-self-pair` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-open-self-pair` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-open-self-pair` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-poll-self` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-poll-self` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/pidfd-poll-self` | `verify` | `ptrace` | `Selected by full` | `diverged-unlocated` |
| `c-programs/pidfd-poll-self` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-waitid-child` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/pidfd-waitid-child` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-waitid-child` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-capacity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-capacity` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/pipe-capacity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-capacity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-capacity-pin` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-capacity-pin` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/pipe-capacity-pin` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-capacity-pin` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-ipc` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `c-programs/pipe-ipc` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-ipc` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-ipc` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `c-programs/pipe-multiwriter-ordering` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/pipe-multiwriter-ordering` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-multiwriter-ordering` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `c-programs/pipe2-errno-precedence` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/pipe2-errno-precedence` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe2-errno-precedence` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/pipe2-errno-precedence` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe2-errno-precedence` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe2-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe2-flags` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/pipe2-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe2-flags` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/poll-readiness` | `replay` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/poll-readiness` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/poll-readiness` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/poll-readiness` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/poll-readiness` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/ppoll-readv` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/ppoll-readv` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/ppoll-readv` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ppoll-readv` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/ppoll-simulation` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/ppoll-simulation` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ppoll-simulation` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `c-programs/prctl-dumpable` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-dumpable` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/prctl-dumpable` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-dumpable` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-identity` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/prctl-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-identity` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/prctl-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-option-policy` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-option-policy` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/prctl-option-policy` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-option-policy` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-pdeathsig` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-pdeathsig` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-pdeathsig` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pread64-nostdlib` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pread64-nostdlib` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/pread64-nostdlib` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pread64-nostdlib` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/preadv2-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/preadv2-flags` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/preadv2-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/preadv2-flags` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/print-memaddrs` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/print-memaddrs` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/print-memaddrs` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/print-memaddrs` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/printf-with-threads` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/printf-with-threads` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/printf-with-threads` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `c-programs/proc-fd-link-aliases` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/proc-fd-link-aliases` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/proc-fd-link-aliases` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/proc-fdinfo` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/proc-fdinfo` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/proc-fdinfo` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/proc-locks` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/proc-locks` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/proc-locks` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `c-programs/proc-locks` | `verify` | `sabre` | `Selected by full` | `diverged` |
| `c-programs/process-mrelease-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/process-mrelease-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/process-mrelease-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/process-mrelease-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/process-mrelease-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/process-vm-readv-refusal-probe` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/process-vm-readv-refusal-probe` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/process-vm-readv-refusal-probe` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/process-vm-readv-refusal-probe` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/process-vm-readv-refusal-probe` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/process-vm-writev-refusal-probe` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/process-vm-writev-refusal-probe` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/process-vm-writev-refusal-probe` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/process-vm-writev-refusal-probe` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/process-vm-writev-refusal-probe` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/procfs-identity-agreement` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/procfs-identity-agreement` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/procfs-identity-agreement` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/procfs-positioned-probe` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/procfs-positioned-probe` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/procfs-positioned-probe` | `verify` | `ptrace` | `Selected by full` | `diverged-unlocated` |
| `c-programs/procfs-positioned-probe` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/prodcons-determinism` | `verify` | `kvm` | `Not selected by full` | `diverged` |
| `c-programs/prodcons-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/prodcons-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/prodcons-determinism` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `c-programs/pselect6-simulation` | `replay` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pselect6-simulation` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/pselect6-simulation` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pselect6-simulation` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/pthread-lifecycle` | `verify` | `kvm` | `Not selected by full` | `diverged` |
| `c-programs/pthread-lifecycle` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/pthread-lifecycle` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pthread-lifecycle` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `c-programs/ptrace-attach-eperm` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `c-programs/ptrace-attach-eperm` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/ptrace-attach-eperm` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ptrace-attach-eperm` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/ptrace-eperm` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/ptrace-eperm` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/ptrace-eperm` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/ptrace-eperm` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ptrace-eperm` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/ptrace-seize-eperm` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/ptrace-seize-eperm` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/ptrace-seize-eperm` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ptrace-seize-eperm` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/ptrace-traceme-eperm` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/ptrace-traceme-eperm` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/ptrace-traceme-eperm` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/ptrace-traceme-eperm` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ptrace-traceme-eperm` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pty-nr-count` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/pty-nr-count` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pty-nr-count` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/racewrite-nostdlib` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `c-programs/racewrite-nostdlib` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/racewrite-nostdlib` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/racewrite-nostdlib` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/random-readv-stream` | `replay` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/random-readv-stream` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/random-readv-stream` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/random-readv-stream` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/random-readv-stream` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/random-sources` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/random-sources` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/random-sources-root-only` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/random-sources-root-only` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/random-sources-root-only` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/random-sources-root-only` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/rcx-canonicalization` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/rcx-canonicalization` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/rcx-canonicalization` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/rcx-canonicalization` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/readdir-entries` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/readdir-entries` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/readdir-entries` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/readdir-entries` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/readdir-order-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/readdir-order-identity` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/readdir-order-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/readdir-order-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/record-lock` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/record-lock` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/record-lock` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/record-replay-fd-close` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/record-replay-fd-close` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/record-replay-fd-close` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/record-replay-file-state` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/record-replay-file-state` | `verify` | `ptrace` | `Not selected by full` | `diverged` |
| `c-programs/record-replay-file-state` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/record-replay-file-state-regular-sink` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/record-replay-file-state-regular-sink` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/record-replay-file-state-regular-sink` | `verify` | `sabre` | `Selected by full` | `diverged` |
| `c-programs/record-replay-lseek-seek-cur` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/record-replay-lseek-seek-cur` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/record-replay-lseek-seek-cur` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/record-replay-setsockopt` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/record-replay-setsockopt` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/record-replay-setsockopt` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/record-replay-setsockopt` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/recvmsg-scm-rights-mmap` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/recvmsg-scm-rights-mmap` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/recvmsg-scm-rights-mmap` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/recvmsg-scm-rights-mmap` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/remap-file-pages-anonymous-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/remap-file-pages-anonymous-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/remap-file-pages-anonymous-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/remap-file-pages-anonymous-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/remap-file-pages-anonymous-enosys` | `verify` | `sabre` | `Selected by full` | `diverged` |
| `c-programs/remap-file-pages-memfd-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/remap-file-pages-memfd-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/remap-file-pages-memfd-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/remap-file-pages-memfd-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/remap-file-pages-memfd-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/remap-file-pages-tmpfile-enosys` | `verify` | `dbt` | `Not selected by full` | `diverged` |
| `c-programs/remap-file-pages-tmpfile-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/remap-file-pages-tmpfile-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/remap-file-pages-tmpfile-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/remap-file-pages-tmpfile-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/rename-ops` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/rename-ops` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/rename-ops` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/rename-ops` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/renameat2-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/renameat2-flags` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/renameat2-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/renameat2-flags` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/request-key-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/request-key-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/request-key-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/request-key-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/request-key-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/resource-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/resource-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/resource-determinism` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/rlimit-identity` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/rlimit-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/rlimit-identity` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/rlimit-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/rlimit-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/robust-list` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/robust-list` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/robust-list` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/robust-list` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-getaffinity-identity` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/sched-getaffinity-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-getaffinity-identity` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-getaffinity-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-getaffinity-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-setattr-batch` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-setattr-batch` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sched-setattr-batch` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-setattr-batch` | `verify` | `sabre` | `Selected by full` | `diverged` |
| `c-programs/sched-setattr-idle` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-setattr-idle` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sched-setattr-idle` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-setattr-idle` | `verify` | `sabre` | `Selected by full` | `diverged` |
| `c-programs/sched-setattr-other` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-setattr-other` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sched-setattr-other` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-setattr-other` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-yield-progress` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sched-yield-progress` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-yield-progress` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `c-programs/scheduler-policy-queries` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/scheduler-policy-queries` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/scheduler-policy-queries` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/scheduler-policy-queries` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/scheduler-policy-queries` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/seccomp-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/seccomp-refusal` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/seccomp-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/seccomp-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sendfile-copy` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sendfile-copy` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/sendfile-copy` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sendfile-copy` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/session-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/session-identity` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/session-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/session-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/set-tid-address` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/set-tid-address` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/set-tid-address` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/set-tid-address` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/setitimer-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/setitimer-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/setitimer-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/setitimer-determinism` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/short-io-split-identity` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `c-programs/short-io-split-identity` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/short-io-split-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/short-io-split-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/shutdown-socketpair` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/shutdown-socketpair` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/shutdown-socketpair` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/shutdown-socketpair` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigaction-state` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigaction-state` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigaction-state` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigaction-state` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigaltstack-state` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigaltstack-state` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigaltstack-state` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigaltstack-state` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigmask-preemption` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigmask-preemption` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigmask-preemption` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `c-programs/signal-delivery-sequence` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/signal-delivery-sequence` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/signal-delivery-sequence` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/signal-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/signal-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/signal-determinism` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/signal-disposition` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/signal-disposition` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/signal-disposition` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/signal-disposition` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/signal-waitstatus-identity` | `verify` | `dbt` | `Not selected by full` | `measured-no-verdict` |
| `c-programs/signal-waitstatus-identity` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/signal-waitstatus-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/signal-waitstatus-identity` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/signalfd-create` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/signalfd-create` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/signalfd-create` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/signalfd-create` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigpipe-siginfo` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigpipe-siginfo` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigpipe-siginfo` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigprocmask-state` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigprocmask-state` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigprocmask-state` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigprocmask-state` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigtimedwait-no-timeout` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigtimedwait-no-timeout` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigtimedwait-timeout-0s` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigtimedwait-timeout-0s` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigtimedwait-timeout-0s` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `c-programs/sigtimedwait-timeout-1s` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigtimedwait-timeout-1s` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-tcp4` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-tcp4` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/so-incoming-cpu-tcp4` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-tcp4` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-tcp6` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-tcp6` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-tcp6` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-tcp6` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-udp4` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-udp4` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-udp4` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-udp4` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-cookie-tcp` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-cookie-tcp` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-cookie-tcp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-cookie-tcp` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-cookie-udp` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-cookie-udp` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/socket-cookie-udp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-cookie-udp` | `verify` | `sabre` | `Selected by full` | `diverged` |
| `c-programs/socket-cookie-unix` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-cookie-unix` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/socket-cookie-unix` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-cookie-unix` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-epoll-ordering` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-epoll-ordering` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/socket-epoll-ordering` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-epoll-ordering` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-ioctl-timestamp` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-ioctl-timestamp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-ioctl-timestamp` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/socket-options` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-options` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/socket-options` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-options` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-timestamp-edge-cases` | `verify` | `liteinst` | `Not selected by full` | `measured-no-verdict` |
| `c-programs/socket-timestamp-edge-cases` | `verify` | `ptrace` | `Not selected by full` | `measured-no-verdict` |
| `c-programs/socket-timestamp-edge-cases` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/socket-timestamp-timespec` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-timestamp-timespec` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/socket-timestamp-timespec` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-timestamp-timespec` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-timestamp-timeval` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-timestamp-timeval` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/socket-timestamp-timeval` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-timestamp-timeval` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/socketpair-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socketpair-flags` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/socketpair-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socketpair-flags` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sockname-unnamed` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sockname-unnamed` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/sockname-unnamed` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sockname-unnamed` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/splice-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/splice-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/splice-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/splice-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/splice-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/stat-metadata-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/stat-metadata-identity` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/stat-metadata-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/stat-metadata-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/statfs-free-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/statfs-free-determinism` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/statfs-free-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/statfs-free-determinism` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/static-nolibc-syscall-sites` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/static-nolibc-syscall-sites` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/static-nolibc-syscall-sites` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/static-nolibc-syscall-sites` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/statmount-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/statmount-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/statmount-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/statmount-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/statmount-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/statx-metadata` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/statx-metadata` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/statx-metadata` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/statx-metadata` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/symlink-ops` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/symlink-ops` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/symlink-ops` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/symlink-ops` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sync-file-range` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sync-file-range` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/sync-file-range` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sync-file-range` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/syscall-file-io` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/syscall-file-io` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/syscall-file-io` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/syscall-file-io` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/syscall-file-io` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/syscall-file-metadata` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/syscall-file-metadata` | `verify` | `kvm` | `Not selected by full` | `measured-and-passed` |
| `c-programs/syscall-file-metadata` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/syscall-file-metadata` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/syscall-file-metadata` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/syscall-quick-wins` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/syscall-quick-wins` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/syscall-quick-wins` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/syscall-quick-wins` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysfs-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/sysfs-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysfs-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sysfs-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysfs-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysinfo` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sysinfo` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysinfo` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysinfo-uptime` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sysinfo-uptime` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/syslog-deterministic` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/syslog-deterministic` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/syslog-deterministic` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/syslog-deterministic` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/syslog-deterministic` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysv-ipc-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysv-ipc-refusal` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysv-ipc-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysv-ipc-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysv-sem-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/sysv-sem-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysv-sem-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sysv-sem-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysv-sem-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysv-shm-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/sysv-shm-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysv-shm-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sysv-shm-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysv-shm-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/tcp-info-accept4` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/tcp-info-accept4` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/tcp-info-accept4` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/tcp-info-accept4` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/tcp-info-accept6` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/tcp-info-accept6` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/tcp-info-accept6` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/tcp-info-accept6` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/tcp-info-client4` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/tcp-info-client4` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/tcp-info-client4` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/tcp-info-client4` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/tee-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/tee-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/tee-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/tee-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/tee-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/thp-disable` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/thp-disable` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/thp-disable` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/thp-disable` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/thread-self-procfs-handoff` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/thread-self-procfs-handoff` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/thread-self-procfs-handoff` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `c-programs/thread-sync-determinism` | `chaos` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/thread-sync-determinism` | `verify` | `kvm` | `Not selected by full` | `diverged` |
| `c-programs/thread-sync-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/thread-sync-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/thread-sync-determinism` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/threadexhaustion` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/threadexhaustion` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/threadexhaustion` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `c-programs/timer-create-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/timer-create-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/timer-create-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/timer-create-determinism` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/timer-family-identity` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/timer-family-identity` | `verify` | `ptrace` | `Not selected by full` | `diverged` |
| `c-programs/timer-family-identity` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `c-programs/umask-mode` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/umask-mode` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/umask-mode` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/umask-mode` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/uname` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/uname` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/uname` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/uname` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/uname-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/uname-identity` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/uname-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/uname-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-dgram` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-dgram` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-dgram` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-dgram` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-seqpacket` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-seqpacket` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-seqpacket` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-seqpacket` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-stream` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-stream` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/unix-autobind-stream` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-stream` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/ustat-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/ustat-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/ustat-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/ustat-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ustat-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/utimensat-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/utimensat-determinism` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/utimensat-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/utimensat-determinism` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/vectored-file-io` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/vectored-file-io` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/vectored-file-io` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/vectored-file-io` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/vectored-io` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/vectored-io` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/vectored-io` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/vectored-io` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/vforkexec` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/vforkexec` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/vforkexec` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/vforkexec` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `c-programs/vmsplice-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/vmsplice-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/vmsplice-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/vmsplice-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/vmsplice-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/wait-on-child` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `c-programs/wait-on-child` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/wait-on-child` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/wait-on-child` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/writev-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/writev-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/writev-determinism` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `chaos-c/lock-granularity` | `chaos` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `chaos-c/lock-granularity` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `chaos-c/lock-granularity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `chaos-c/lock-granularity` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `compat/addr2line` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/ar` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/arch` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/arch` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/as` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/awk` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/b2sum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/b2sum` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/base32` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/base32` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/base64` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/base64` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/basename` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/basename` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/basenc` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/bash` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/bash` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/bc` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/bracket` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/bracket` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/bzip2` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/bzip2-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cal` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/cargo` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cat` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cat` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/chmod` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/chown` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/chrt` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cksum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cksum` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/clang` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cmake` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cmp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/col` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/colrm` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/column` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/comm` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cpio-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cpp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/crc32` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/cscope` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/csplit` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/curl` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/curl` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/curl-localhost` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cut` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cxxfilt` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/date` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/date` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/dc` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/dd` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/df` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/df-direct` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/df-direct` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/diff` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/diff3` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/dirname` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/dirname` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/dos2unix` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/du` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/du` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/echo` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/echo` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/egrep` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/elfedit` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/env` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/envsubst` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/expand` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/expr` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/expr` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/factor` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/fallocate` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/fgrep` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/file` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/find` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/findmnt` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/findmnt` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/flex` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/flock` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/fmt` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/fold` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/free` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/free` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/gcc` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/gcov` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/getconf` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/getopt` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/getopt` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/git` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/gprof` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/grep` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/groups` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/groups` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/gxx` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/gzip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/gzip-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/head` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/head` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/hexdump` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/hostname` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/hostname` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/iconv` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/id` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/id` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/install` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/ionice` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/iostat` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/ip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/join` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/jq` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/kill` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/kill` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/ld` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/ln` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/logger` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/logname` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/ls` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/ls` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/lscpu` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/lsirq` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/lsirq` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/lsmod` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/lsmod` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/lsof` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/lua` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/lua-direct` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/lua-direct` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/m4` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/make` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/md5sum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/md5sum` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/mkdir` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/mkfifo` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/mktemp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/mountpoint` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/mpstat-softirqs` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/msgfmt` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/msgunfmt` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/mv` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/namei` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/netlink-route` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/netlink-sock-diag` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/nice` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/nl` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/nm` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/node` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/nohup` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/nproc` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/nproc` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/numactl-hardware` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/numactl-hardware` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/numastat` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/numastat` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/numfmt` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/numfmt` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/objcopy` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `compat/objdump` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `compat/od` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/openssl` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/openssl` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/paste` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/patch` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pathchk` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/perl` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/perl-direct` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/perl-direct` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/pgrep` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pidstat-disk` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pidstat-disk` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/pinky` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pinky` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/pkg-config` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pkill` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pr` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pr` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/printenv` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/printf` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/printf` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/ps` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/ps` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/ptx` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pwd` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pwd` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/python3` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/python3` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/ranlib` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `compat/readelf` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/readlink` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/readlink` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/realpath` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/realpath` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/rev` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/rm` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/rmdir` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/rr-arch` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-b2sum` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-base32` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-base64` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-basename` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-bracket` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-cat` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-cksum` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-df-direct` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-dirname` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-du` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-echo` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-expr` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-free` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-getopt` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-groups` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-head` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-hostname` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-id` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-ionice` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-ls` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-md5sum` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-nice` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-nohup` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-nproc` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-numfmt` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-openssl` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-pinky` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-pr` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-printenv` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-printf` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-pwd` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-readlink` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-realpath` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-sha1sum` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-sha224sum` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-sha256sum` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-sha384sum` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-sha512sum` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-sleep` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-stat` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-stdbuf` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-sum` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-test` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-true` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-uname` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-uptime` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-users` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-wc` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-wc-lines` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/rr-whoami` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/ruby` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/ruby` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/rustc` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/sar-resource-tables` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sed` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/seq` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/setfacl` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/setfattr` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/sha1sum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sha1sum` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/sha224sum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sha224sum` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/sha256sum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sha256sum` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/sha384sum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sha384sum` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/sha512sum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sha512sum` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/shell-build` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `compat/shred` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/shuf` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/size` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sleep` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sleep` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/sort` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/split` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sqlite3` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/ss` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/stat` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/stat` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/stdbuf` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/strict-addr2line` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-ar` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-arch` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-as` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-awk` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-b2sum` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-base32` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-base64` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-basename` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-bash` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-bc` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-bracket` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-bzip2` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-bzip2-roundtrip` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-cal` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-cargo` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-cat` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-chmod` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-chown` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-chrt` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-cksum` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-clang` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-cmake` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-cmp` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-column` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-comm` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-cp` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-cpio-roundtrip` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-cpp` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-csplit` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-curl` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-curl-localhost` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-cut` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-cxxfilt` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-date` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-dc` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-dd` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-df` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-diff` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-dirname` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-du` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-echo` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-egrep` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-elfedit` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-env` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-expand` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-expr` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-factor` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-fgrep` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-file` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-find` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-findmnt` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-flock` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-fmt` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-fold` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-free` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-gcc` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-gcov` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-getopt` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-git` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-gprof` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-grep` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-groups` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-gxx` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-gzip` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-gzip-roundtrip` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-head` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-hexdump` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-hostname` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-iconv` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-id` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-install` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-ionice` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-iostat` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-ip` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-join` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-jq` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-kill` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-ld` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-ln` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-logger` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-logname` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-ls` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-lscpu` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-lsirq` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-lsmod` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-lsof` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-lua` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-m4` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-make` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-md5sum` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-mkdir` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-mkfifo` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-mktemp` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-mpstat-softirqs` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-mv` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-netlink-route` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-netlink-sock-diag` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-nice` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-nl` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-nm` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-node` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-nohup` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-nproc` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-numactl-hardware` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-numastat` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-numfmt` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-objcopy` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-objdump` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-od` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-openssl` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-paste` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-patch` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-perl` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-pgrep` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-pidstat-disk` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-pinky` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-pkg-config` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-pkill` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-pr` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-printenv` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-printf` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-ps` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-ptx` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-pwd` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-python3` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-ranlib` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-readelf` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-readlink` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-realpath` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-rev` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-rm` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-rmdir` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-ruby` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-rustc` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-sar-resource-tables` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-sed` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-seq` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-sha1sum` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-sha224sum` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-sha256sum` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-sha384sum` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-sha512sum` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-shell-build` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-shuf` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-size` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-sleep` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-sort` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-split` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-sqlite3` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-ss` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-stat` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-stdbuf` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-strings` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-strip` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-sum` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-sysctl-random-uuid` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-tac` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-tar` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-tar-roundtrip` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-taskset` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-tcl` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-tee` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-test` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-time` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-top` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-touch` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-tr` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-true` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-tsort` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-tty` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-uname` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-unexpand` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-uniq` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-uptime` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-users` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-vmstat` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-vmstat-disk` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-wc` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-wc-lines` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-wget-localhost` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-whoami` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-xargs` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-xmllint` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-xxd` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-xz` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-xz-roundtrip` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-yes` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-zip-unzip` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-zstd` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strict-zstd-roundtrip` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/strings` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/strip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sum` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/sync` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/sysctl-random-uuid` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sysctl-random-uuid` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/tac` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/tar` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/tar-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/taskset` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/tcl` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/tee` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/test` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/test` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/time` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/timeout` | `verify` | `ptrace` | `Not selected by full` | `measured-no-verdict` |
| `compat/top` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/touch` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/tr` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/true` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/true` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/truncate` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/tsort` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/tty` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/uname` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/uname` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/unexpand` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/uniq` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/uptime` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/uptime` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/users` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/users` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/uuidgen` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/vmstat` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/vmstat` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/vmstat-disk` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/vmstat-disk` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/wc` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/wc` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/wc-lines` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/wc-lines` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/wget-localhost` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/whoami` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/whoami` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/xargs` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/xmllint` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/xxd` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/xz` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/xz-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/yes` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/zip-unzip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/zstd` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/zstd-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `data-handling/archive-roundtrip` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `data-handling/archive-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `data-handling/archive-roundtrip` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `data-handling/dd-partial-transfers` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `data-handling/dd-partial-transfers` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `data-handling/dd-partial-transfers` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `data-handling/jq-json-transform` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `data-handling/jq-json-transform` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `data-handling/jq-json-transform` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `data-handling/jq-json-transform` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `data-handling/shell-pipeline` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `data-handling/shell-pipeline` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `data-handling/shell-pipeline` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `data-handling/sqlite-query-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `data-handling/sqlite-query-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `data-handling/sqlite-query-determinism` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `data-handling/zstd-multithread` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `data-handling/zstd-multithread` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `data-handling/zstd-multithread` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `debugger-c/debuggee` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `debugger-c/debuggee` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `debugger-c/debuggee` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `debugger-c/debuggee` | `verify` | `sabre` | `Selected by full` | `diverged` |
| `determinism-stress/example-race` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `determinism-stress/example-race` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress/example-race` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress/order-violation` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress/order-violation` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress/process-chains` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress/process-chains` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress/process-chains` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `determinism-stress/thread-contention` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress/thread-contention` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `determinism-stress/thread-contention` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `determinism-stress/thread-interleaving` | `chaos` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress/thread-interleaving` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress/thread-interleaving` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `determinism-stress/thread-output` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `determinism-stress/thread-output` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress/thread-output` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/fork-tree` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/fork-tree` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/fork-tree` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `determinism-stress-c/lock-free` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/lock-free` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/mmap-fork-shared` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/mmap-fork-shared` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/mmap-fork-shared` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `determinism-stress-c/pid-tid` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/pid-tid` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/pid-tid-identity` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/pid-tid-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/pipe-chain` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/pipe-chain` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/pipe-chain` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/pipe-chain` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `determinism-stress-c/pipe-prefill` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/pipe-prefill` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/pipe-prefill` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/pipe-prefill` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/producer-consumer` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/producer-consumer` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `determinism-stress-c/signal-order` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/signal-order` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/thread-contention` | `chaos` | `ptrace` | `Selected by full` | `diverged` |
| `determinism-stress-c/thread-contention` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/thread-contention` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/thread-contention` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `determinism-stress-c/thread-stress` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/thread-stress` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/thread-stress` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `language-runtimes/bash-loop-pipe-time` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/bash-loop-pipe-time` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/bash-loop-pipe-time` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/bash-random` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/bash-random` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/bash-random` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/cpp-stl-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/cpp-stl-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/cpp-stl-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/example-python-random` | `verify` | `dbt` | `Not selected by full` | `diverged` |
| `language-runtimes/example-python-random` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `language-runtimes/example-python-random` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `language-runtimes/example-python-random` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `language-runtimes/example-python-random` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `language-runtimes/gawk-random` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/gawk-random` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/gawk-random` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/lua-random` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/lua-random` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/lua-random` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `language-runtimes/m4-macro-mkstemp` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `language-runtimes/m4-macro-mkstemp` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/m4-macro-mkstemp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/node-v8-jit` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/node-v8-jit` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `language-runtimes/node-v8-jit` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/perl-hash-order` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/perl-hash-order` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/perl-hash-order` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/perl-io-subprocess-time` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `language-runtimes/perl-io-subprocess-time` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/perl-io-subprocess-time` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `language-runtimes/perl-io-subprocess-time` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `language-runtimes/perl-random` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/perl-random` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/perl-random` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/python-dict-hash-iteration` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `language-runtimes/python-dict-hash-iteration` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/python-dict-hash-iteration` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `language-runtimes/python-dict-hash-iteration` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/python-hash-determinism` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `language-runtimes/python-hash-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/python-hash-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/python-hashseed` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/python-hashseed` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `language-runtimes/python-hashseed` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/python-io-subprocess-time` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `language-runtimes/python-io-subprocess-time` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/python-io-subprocess-time` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `language-runtimes/python-io-subprocess-time` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/python-random` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `language-runtimes/python-random` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/python-random` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `language-runtimes/ruby-random` | `verify` | `kvm` | `Not selected by full` | `measured-and-passed` |
| `language-runtimes/ruby-random` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/ruby-random` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/rust-hashmap-iteration` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/rust-hashmap-iteration` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/rust-hashmap-iteration` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/rust-hashmap-iteration` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/tcl-rand-clock` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `language-runtimes/tcl-rand-clock` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/tcl-rand-clock` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `shared-futex-c/qemu-exec-init` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `shared-futex-c/qemu-exec-init` | `verify` | `ptrace` | `Not selected by full` | `measured-no-verdict` |
| `shared-futex-c/qemu-exec-init` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `shared-futex-c/qemu-hello` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `shared-futex-c/qemu-hello` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `shared-futex-c/qemu-hello` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `shared-futex-c/qemu-init` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `shared-futex-c/qemu-init` | `verify` | `ptrace` | `Not selected by full` | `measured-no-verdict` |
| `shared-futex-c/qemu-init` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `shared-futex-c/qemu-net-init` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `shared-futex-c/qemu-net-init` | `verify` | `ptrace` | `Not selected by full` | `measured-no-verdict` |
| `shared-futex-c/qemu-net-init` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/auxv-loader-dump` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/auxv-loader-dump` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `system-utils/auxv-loader-dump` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/cat-file-read` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `system-utils/cat-file-read` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/cat-file-read` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/cat-file-read` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `system-utils/clock-determinism` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `system-utils/clock-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/clock-determinism` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `system-utils/clock-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/clock-determinism` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `system-utils/clock-exec-continuity` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/clock-exec-continuity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/clock-exec-continuity` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/date-nanoseconds` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `system-utils/date-nanoseconds` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `system-utils/date-nanoseconds` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/date-nanoseconds` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `system-utils/du-tree-summary` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/du-tree-summary` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/du-tree-summary` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `system-utils/du-tree-summary` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `system-utils/echo-stdout` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `system-utils/echo-stdout` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/echo-stdout` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/echo-stdout` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `system-utils/errno-path-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/errno-path-identity` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/errno-path-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/errno-path-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `system-utils/example-date` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/example-date` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/example-date` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/example-devrand` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/example-devrand` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/example-devrand` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/file-timestamp-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/file-timestamp-identity` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/file-timestamp-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/file-timestamp-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `system-utils/find-tree-metadata` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/find-tree-metadata` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/find-tree-metadata` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/find-tree-metadata` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `system-utils/harness-width-contract` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/harness-width-contract` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/harness-width-contract` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/mcookie-random` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/mcookie-random` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/mcookie-random` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/mktemp-name` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/mktemp-name` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/mktemp-name` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/nscd-neutralised` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/nscd-neutralised` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/nscd-neutralised` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `system-utils/openssl-enc` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `system-utils/openssl-enc` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/openssl-enc` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `system-utils/openssl-enc` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/openssl-genpkey` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/openssl-genpkey` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/openssl-genpkey` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/openssl-passwd` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/openssl-passwd` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/openssl-passwd` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/openssl-rand` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/openssl-rand` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/openssl-rand` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/openssl-x509` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `system-utils/openssl-x509` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/openssl-x509` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `system-utils/overflow-gid-resolves` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `system-utils/overflow-gid-resolves` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/overflow-gid-resolves` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `system-utils/printf-argument-forwarding` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `system-utils/printf-argument-forwarding` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/printf-argument-forwarding` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/printf-argument-forwarding` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `system-utils/proc-random-uuid` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/proc-random-uuid` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/proc-uptime` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/proc-uptime` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/proc-uptime` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/procfs-sanitized-paths` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `system-utils/ps-proc-table` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `system-utils/ps-proc-table` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/ps-proc-table` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/random-device` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/random-device` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/random-device` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/record-getpid` | `replay` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/record-getpid` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/record-getpid` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `system-utils/record-getpid` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/record-getpid` | `verify` | `sabre` | `Selected by full` | `diverged` |
| `system-utils/sh-exit-status` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `system-utils/sh-exit-status` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/sh-exit-status` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/sh-exit-status` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `system-utils/shm-coherency-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/shm-coherency-identity` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/shm-coherency-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/shm-coherency-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `system-utils/shuf-permutation` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/shuf-permutation` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/shuf-permutation` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/sort-random` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/sort-random` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/sort-random` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/ssh-keygen-ed25519` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `system-utils/ssh-keygen-ed25519` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/ssh-keygen-ed25519` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/ssh-keygen-ed25519` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/startup-surface-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/startup-surface-identity` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/startup-surface-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/startup-surface-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `system-utils/startup-tls-guards` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/startup-tls-guards` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/startup-tls-guards` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/startup-tls-guards` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `system-utils/true-exit-zero` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `system-utils/true-exit-zero` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/true-exit-zero` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/true-exit-zero` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `system-utils/uuidgen-random` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/uuidgen-random` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/uuidgen-random` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `util-c/pmu-skid` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `util-c/pmu-skid` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `util-c/pmu-skid` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `applications/kvm-python-examples` | `verify` | `kvm` | `Not selected by full` | `diverged` |
| `applications/kvm-python-examples` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `applications/kvm-python-examples` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `applications/kvm-shell-environment` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `applications/kvm-shell-environment` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/cpuid-probe` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpuid-probe` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpuid-probe` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpuid-probe` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpuid-probe` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/sysfs-sanitized-prefixes` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/sysfs-sanitized-prefixes` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/sysfs-sanitized-prefixes` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/sysfs-sanitized-prefixes` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |

## Parity (measured after determinism)

Cross-backend parity compares a candidate backend's retained `verify` log with the ptrace reference's. It is measured after determinism, from the retained logs, by the parity post-pass, and it is reported here beside determinism, never folded into it: no count, measurement or colour above depends on it. The `c-programs` nodes, which perform ordinary same-backend verification, supply those logs; no validation runs a ptrace rerun any more (https://github.com/rrnewton/hermit/issues/3301).

A measured cell earns credit in [0, 1]: its matched prefix of compared records over the longer log, and 1 only for a full match. **Mean credit** divides the credit sum by the measured (matched plus diverged) cells, so a measured cell without credit counts as 0. **Floor credit** divides it by every selected cell except two kinds that could not be compared: a **no golden** cell, where an operand's own outcome (a determinism mismatch, a timeout, a crash and the like) left no deterministic golden log, and a **not compared** cell, whose backend cannot be given the reference's inputs. So an **unmeasured** cell (a golden log could exist, but the harness or the parity tool made no comparison), a record-missing cell and a refused cell each count as 0. No cell of a backend whose inputs cannot be equalized enters any mean or floor, and a mean or floor over no cells reads n/a, never 0.000. **Selected** reads `W of C` when the run's own Hermit commit's `ci/compat-envelope/parity-cells.json` is known: the run reported W of the C cells that selection owes, and a run that reported fewer is marked partial. Mean credit pools clean credit (inputs equalized) with unequalized credit only under a marker that says so; **Credit inputs** shows which it is. The `legacy-rerun` history at the end is the retired ptrace rerun's last verdicts; it is not current parity and enters no count here.


### validate run `validate-coord2-d1744e9fc07e-1791193403382220055-1138567-499f31af` at `d1744e9fc07e`

`parity: 0/297 matched; selected 297 of 297 committed; mean n/a over 0 measured; floor 0.000 over 278 of 297 selected (counted as 0: 278 unmeasured: no-result-row 3, epoch-not-shared 275; excluded: 3 no golden: determinism-mismatch 3; 16 not compared)`

| Candidate backend | Selected | Measured | Matched | Diverged | No golden | Not compared | Unmeasured | Record-missing | Refused | Mean credit (measured) | Floor credit | Credit inputs |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | --- |
| `dbt` | 16 of 16 | 0 | 0 | 0 | 0 | 16 | 0 | 0 | 0 | n/a | n/a | not compared (inputs cannot be equalized) |
| `kvm` | 91 of 91 | 0 | 0 | 0 | 0 | 0 | 91 | 0 | 0 | n/a | 0.000 | — |
| `liteinst` | 99 of 99 | 0 | 0 | 0 | 3 | 0 | 96 | 0 | 0 | n/a | 0.000 | — |
| `sabre` | 91 of 91 | 0 | 0 | 0 | 0 | 0 | 91 | 0 | 0 | n/a | 0.000 | — |
| **TOTAL** | 297 of 297 | 0 | 0 | 0 | 3 | 16 | 278 | 0 | 0 | n/a | 0.000 | — |

Cells that were not measured, by class: a no-golden cell is outside the mean and the floor, and an unmeasured cell counts 0 in the floor.

| Class | Group | `dbt` | `kvm` | `liteinst` | `sabre` | **TOTAL** |
| --- | --- | ---: | ---: | ---: | ---: | ---: |
| `determinism-mismatch` | no golden | 0 | 0 | 3 | 0 | 3 |
| `no-result-row` | unmeasured | 0 | 1 | 1 | 1 | 3 |
| `epoch-not-shared` | unmeasured | 0 | 90 | 95 | 90 | 275 |

Every cell that did not match, with its first divergence or the reason it was not measured:

| Cell | Verdict | Credit | First divergence or reason |
| --- | --- | ---: | --- |
| `c-programs/aio-refusal@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.655042618+00:00, kvm candidate 2026-10-05T10:01:32.372518583+00:00 |
| `c-programs/aio-refusal@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.655042618+00:00, liteinst candidate 2026-10-05T10:01:53.023715038+00:00 |
| `c-programs/aio-refusal@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.655042618+00:00, sabre candidate 2026-10-05T10:01:54.315151078+00:00 |
| `c-programs/append-pwrite@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.138945750+00:00, kvm candidate 2026-10-05T10:01:28.701963384+00:00 |
| `c-programs/append-pwrite@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.138945750+00:00, liteinst candidate 2026-10-05T10:01:55.000900801+00:00 |
| `c-programs/append-pwrite@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.138945750+00:00, sabre candidate 2026-10-05T10:01:56.899412602+00:00 |
| `c-programs/bind-getsockname@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.065116943+00:00, kvm candidate 2026-10-05T10:01:33.414679002+00:00 |
| `c-programs/bind-getsockname@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.065116943+00:00, liteinst candidate 2026-10-05T10:01:52.153037860+00:00 |
| `c-programs/bind-getsockname@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.065116943+00:00, sabre candidate 2026-10-05T10:01:58.585753251+00:00 |
| `c-programs/cachestat-refusal@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.903592513+00:00, kvm candidate 2026-10-05T10:01:36.851973612+00:00 |
| `c-programs/cachestat-refusal@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.903592513+00:00, liteinst candidate 2026-10-05T10:01:51.768599369+00:00 |
| `c-programs/cachestat-refusal@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.903592513+00:00, sabre candidate 2026-10-05T10:01:51.816908785+00:00 |
| `c-programs/child-subreaper-refusal@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.372975395+00:00, kvm candidate 2026-10-05T10:01:30.633325030+00:00 |
| `c-programs/child-subreaper-refusal@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.372975395+00:00, liteinst candidate 2026-10-05T10:01:54.932750336+00:00 |
| `c-programs/child-subreaper-refusal@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.372975395+00:00, sabre candidate 2026-10-05T10:01:57.048283611+00:00 |
| `c-programs/close-range-fds@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.120542038+00:00, kvm candidate 2026-10-05T10:01:42.571363935+00:00 |
| `c-programs/close-range-fds@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.120542038+00:00, liteinst candidate 2026-10-05T10:01:51.990961056+00:00 |
| `c-programs/copy-file-range-refusal@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.940672187+00:00, kvm candidate 2026-10-05T10:01:42.029045582+00:00 |
| `c-programs/copy-file-range-refusal@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.940672187+00:00, liteinst candidate 2026-10-05T10:01:55.626266725+00:00 |
| `c-programs/copy-file-range-refusal@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.940672187+00:00, sabre candidate 2026-10-05T10:01:52.078006751+00:00 |
| `c-programs/cpu-virtualization@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/cpu-virtualization@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.209740016+00:00, kvm candidate 2026-10-05T10:01:37.157705745+00:00 |
| `c-programs/cpu-virtualization@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.209740016+00:00, liteinst candidate 2026-10-05T10:01:52.583703703+00:00 |
| `c-programs/cpu-virtualization@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.209740016+00:00, sabre candidate 2026-10-05T10:01:53.156122318+00:00 |
| `c-programs/cpuid-probe@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/cpuid-probe@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:41.932166194+00:00, kvm candidate 2026-10-05T10:01:31.928921462+00:00 |
| `c-programs/cpuid-probe@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:41.932166194+00:00, liteinst candidate 2026-10-05T10:01:30.023608445+00:00 |
| `c-programs/cwd-roundtrip@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.133815107+00:00, kvm candidate 2026-10-05T10:01:29.113772021+00:00 |
| `c-programs/cwd-roundtrip@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.133815107+00:00, liteinst candidate 2026-10-05T10:01:51.759991569+00:00 |
| `c-programs/cwd-roundtrip@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.133815107+00:00, sabre candidate 2026-10-05T10:01:52.100570665+00:00 |
| `c-programs/dup-shared-offset@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/dup-shared-offset@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.238341484+00:00, kvm candidate 2026-10-05T10:01:29.183189929+00:00 |
| `c-programs/dup-shared-offset@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.238341484+00:00, liteinst candidate 2026-10-05T10:01:52.298909457+00:00 |
| `c-programs/dup-shared-offset@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.238341484+00:00, sabre candidate 2026-10-05T10:01:52.245360306+00:00 |
| `c-programs/epoll-pwait2@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.213476469+00:00, kvm candidate 2026-10-05T10:01:40.879977106+00:00 |
| `c-programs/epoll-pwait2@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.213476469+00:00, liteinst candidate 2026-10-05T10:01:52.162211961+00:00 |
| `c-programs/epoll-pwait2@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.213476469+00:00, sabre candidate 2026-10-05T10:01:57.638985502+00:00 |
| `c-programs/epoll-readiness@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.054881041+00:00, kvm candidate 2026-10-05T10:01:42.633027938+00:00 |
| `c-programs/epoll-readiness@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.054881041+00:00, liteinst candidate 2026-10-05T10:01:51.828239797+00:00 |
| `c-programs/epoll-readiness@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.054881041+00:00, sabre candidate 2026-10-05T10:01:52.650985342+00:00 |
| `c-programs/event-delivery-ordering@liteinst` | nondeterministic[determinism-mismatch] | — | the liteinst candidate verify cell of c-programs/event-delivery-ordering failed determinism on attempt 1 (its two runs diverged), so it has no deterministic log to compare: canonical verification did not match: verified=false verdict=diverged bitwise_parity=false |
| `c-programs/event-delivery-ordering@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.952806267+00:00, sabre candidate 2026-10-05T10:01:55.241836851+00:00 |
| `c-programs/eventfd-semantics@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.535396128+00:00, kvm candidate 2026-10-05T10:01:35.406114043+00:00 |
| `c-programs/eventfd-semantics@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.535396128+00:00, liteinst candidate 2026-10-05T10:01:51.836226682+00:00 |
| `c-programs/eventfd-semantics@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.535396128+00:00, sabre candidate 2026-10-05T10:01:52.216765931+00:00 |
| `c-programs/faccessat2-flags@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:55.016518938+00:00, kvm candidate 2026-10-05T10:01:31.703375031+00:00 |
| `c-programs/faccessat2-flags@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:55.016518938+00:00, liteinst candidate 2026-10-05T10:01:52.141620778+00:00 |
| `c-programs/faccessat2-flags@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:55.016518938+00:00, sabre candidate 2026-10-05T10:01:52.165141237+00:00 |
| `c-programs/fadvise-hints@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.105397817+00:00, kvm candidate 2026-10-05T10:01:36.046654104+00:00 |
| `c-programs/fadvise-hints@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.105397817+00:00, liteinst candidate 2026-10-05T10:01:52.704232164+00:00 |
| `c-programs/fadvise-hints@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.105397817+00:00, sabre candidate 2026-10-05T10:01:54.953758925+00:00 |
| `c-programs/fallocate-extents@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.969573737+00:00, kvm candidate 2026-10-05T10:01:35.129447610+00:00 |
| `c-programs/fallocate-extents@liteinst` | nondeterministic[determinism-mismatch] | — | the liteinst candidate verify cell of c-programs/fallocate-extents failed determinism on attempt 1 (its two runs diverged), so it has no deterministic log to compare: canonical verification did not match: verified=false verdict=diverged bitwise_parity=false |
| `c-programs/fallocate-extents@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.969573737+00:00, sabre candidate 2026-10-05T10:01:51.458611364+00:00 |
| `c-programs/fchmod-bits@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.746902820+00:00, kvm candidate 2026-10-05T10:01:29.862022585+00:00 |
| `c-programs/fchmod-bits@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.746902820+00:00, liteinst candidate 2026-10-05T10:01:54.523311430+00:00 |
| `c-programs/fchmod-bits@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.746902820+00:00, sabre candidate 2026-10-05T10:01:52.083641616+00:00 |
| `c-programs/fchmodat2-flags@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.473517377+00:00, kvm candidate 2026-10-05T10:01:36.315129948+00:00 |
| `c-programs/fchmodat2-flags@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.473517377+00:00, liteinst candidate 2026-10-05T10:01:52.078176806+00:00 |
| `c-programs/fchmodat2-flags@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.473517377+00:00, sabre candidate 2026-10-05T10:01:58.390046664+00:00 |
| `c-programs/fcntl-owner@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:55.213674777+00:00, kvm candidate 2026-10-05T10:01:28.584905613+00:00 |
| `c-programs/fcntl-owner@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:55.213674777+00:00, liteinst candidate 2026-10-05T10:01:51.781751124+00:00 |
| `c-programs/fcntl-owner@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:55.213674777+00:00, sabre candidate 2026-10-05T10:01:51.990012600+00:00 |
| `c-programs/fd-duplication@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/fd-duplication@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.724974153+00:00, kvm candidate 2026-10-05T10:01:35.956272942+00:00 |
| `c-programs/fd-duplication@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.724974153+00:00, liteinst candidate 2026-10-05T10:01:54.873215522+00:00 |
| `c-programs/fd-duplication@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.724974153+00:00, sabre candidate 2026-10-05T10:01:51.572413509+00:00 |
| `c-programs/file-backed-mmap@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.862087880+00:00, kvm candidate 2026-10-05T10:01:38.817076719+00:00 |
| `c-programs/file-backed-mmap@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.862087880+00:00, liteinst candidate 2026-10-05T10:01:55.316795480+00:00 |
| `c-programs/file-backed-mmap@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.862087880+00:00, sabre candidate 2026-10-05T10:01:58.441423881+00:00 |
| `c-programs/file-io-roundtrip@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.618912700+00:00, kvm candidate 2026-10-05T10:01:32.841697449+00:00 |
| `c-programs/file-io-roundtrip@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.618912700+00:00, liteinst candidate 2026-10-05T10:01:54.510814083+00:00 |
| `c-programs/file-io-roundtrip@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.618912700+00:00, sabre candidate 2026-10-05T10:01:56.436148988+00:00 |
| `c-programs/flock-lifecycle@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.159257340+00:00, kvm candidate 2026-10-05T10:01:36.795643985+00:00 |
| `c-programs/flock-lifecycle@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.159257340+00:00, liteinst candidate 2026-10-05T10:01:52.888736711+00:00 |
| `c-programs/flock-lifecycle@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.159257340+00:00, sabre candidate 2026-10-05T10:01:55.510532099+00:00 |
| `c-programs/fork-exec-pipeline@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.517163871+00:00, kvm candidate 2026-10-05T10:01:32.604317904+00:00 |
| `c-programs/fsync-durability@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.944986482+00:00, kvm candidate 2026-10-05T10:01:39.566719897+00:00 |
| `c-programs/fsync-durability@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.944986482+00:00, liteinst candidate 2026-10-05T10:01:52.195330563+00:00 |
| `c-programs/fsync-durability@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.944986482+00:00, sabre candidate 2026-10-05T10:01:55.386627896+00:00 |
| `c-programs/ftruncate-sparse@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:55.101871896+00:00, kvm candidate 2026-10-05T10:01:36.381188769+00:00 |
| `c-programs/ftruncate-sparse@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:55.101871896+00:00, liteinst candidate 2026-10-05T10:01:52.802233118+00:00 |
| `c-programs/ftruncate-sparse@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:55.101871896+00:00, sabre candidate 2026-10-05T10:01:58.993052699+00:00 |
| `c-programs/getcpu-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/getcpu-identity@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.693051907+00:00, kvm candidate 2026-10-05T10:01:39.170995231+00:00 |
| `c-programs/getcpu-identity@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.693051907+00:00, liteinst candidate 2026-10-05T10:01:54.757697956+00:00 |
| `c-programs/getcpu-identity@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.693051907+00:00, sabre candidate 2026-10-05T10:01:57.045906895+00:00 |
| `c-programs/getpriority-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/getpriority-identity@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.203291585+00:00, kvm candidate 2026-10-05T10:01:39.069611040+00:00 |
| `c-programs/getpriority-identity@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.203291585+00:00, liteinst candidate 2026-10-05T10:01:51.763379095+00:00 |
| `c-programs/getpriority-identity@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.203291585+00:00, sabre candidate 2026-10-05T10:01:55.205573394+00:00 |
| `c-programs/getrusage-self-accounting@liteinst` | candidate-missing[no-result-row] | — | the liteinst candidate verify cell of c-programs/getrusage-self-accounting has no result row in this run |
| `c-programs/hardware-trap-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/hardware-trap-identity@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.202072151+00:00, liteinst candidate 2026-10-05T10:01:52.078921335+00:00 |
| `c-programs/host-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/host-identity@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.217949056+00:00, liteinst candidate 2026-10-05T10:01:52.609314351+00:00 |
| `c-programs/host-identity@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.217949056+00:00, sabre candidate 2026-10-05T10:01:59.026811826+00:00 |
| `c-programs/inline-syscall-sites@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.865229646+00:00, kvm candidate 2026-10-05T10:01:36.288375921+00:00 |
| `c-programs/inline-syscall-sites@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.865229646+00:00, liteinst candidate 2026-10-05T10:01:53.187507825+00:00 |
| `c-programs/inline-syscall-sites@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.865229646+00:00, sabre candidate 2026-10-05T10:01:57.494372889+00:00 |
| `c-programs/inotify-watch@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.988999008+00:00, liteinst candidate 2026-10-05T10:01:55.398795239+00:00 |
| `c-programs/inotify-watch@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.988999008+00:00, sabre candidate 2026-10-05T10:01:52.221155459+00:00 |
| `c-programs/ioctl-fionread@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.242366862+00:00, kvm candidate 2026-10-05T10:01:41.177098285+00:00 |
| `c-programs/ioctl-fionread@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.242366862+00:00, liteinst candidate 2026-10-05T10:01:53.238801720+00:00 |
| `c-programs/ioctl-fionread@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.242366862+00:00, sabre candidate 2026-10-05T10:01:58.351424897+00:00 |
| `c-programs/kcmp-refusal@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.989635215+00:00, kvm candidate 2026-10-05T10:01:30.566657299+00:00 |
| `c-programs/kcmp-refusal@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.989635215+00:00, liteinst candidate 2026-10-05T10:01:52.954531947+00:00 |
| `c-programs/kcmp-refusal@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.989635215+00:00, sabre candidate 2026-10-05T10:01:54.995362326+00:00 |
| `c-programs/linkat-flags@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.084512190+00:00, kvm candidate 2026-10-05T10:01:33.125577655+00:00 |
| `c-programs/linkat-flags@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.084512190+00:00, liteinst candidate 2026-10-05T10:01:54.262557884+00:00 |
| `c-programs/linkat-flags@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.084512190+00:00, sabre candidate 2026-10-05T10:01:51.657662055+00:00 |
| `c-programs/lseek-positioning@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/lseek-positioning@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.112302086+00:00, kvm candidate 2026-10-05T10:01:32.194279924+00:00 |
| `c-programs/lseek-positioning@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.112302086+00:00, liteinst candidate 2026-10-05T10:01:53.997680900+00:00 |
| `c-programs/lseek-positioning@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.112302086+00:00, sabre candidate 2026-10-05T10:01:58.961729250+00:00 |
| `c-programs/mce-kill-refusal@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:58.757578934+00:00, kvm candidate 2026-10-05T10:01:32.738719719+00:00 |
| `c-programs/mce-kill-refusal@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:58.757578934+00:00, liteinst candidate 2026-10-05T10:01:53.231673349+00:00 |
| `c-programs/mce-kill-refusal@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:58.757578934+00:00, sabre candidate 2026-10-05T10:01:55.225474775+00:00 |
| `c-programs/membarrier-query@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.969117592+00:00, kvm candidate 2026-10-05T10:01:35.259866044+00:00 |
| `c-programs/membarrier-query@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.969117592+00:00, liteinst candidate 2026-10-05T10:01:52.160390330+00:00 |
| `c-programs/membarrier-query@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.969117592+00:00, sabre candidate 2026-10-05T10:01:55.637154711+00:00 |
| `c-programs/memfd-create@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:58.392759790+00:00, kvm candidate 2026-10-05T10:01:43.702845414+00:00 |
| `c-programs/memfd-create@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:58.392759790+00:00, liteinst candidate 2026-10-05T10:01:52.585498851+00:00 |
| `c-programs/memfd-create@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:58.392759790+00:00, sabre candidate 2026-10-05T10:01:59.219238928+00:00 |
| `c-programs/mempolicy-default@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.663941389+00:00, kvm candidate 2026-10-05T10:01:31.730461009+00:00 |
| `c-programs/mempolicy-default@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.663941389+00:00, liteinst candidate 2026-10-05T10:01:52.220980606+00:00 |
| `c-programs/mempolicy-default@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.663941389+00:00, sabre candidate 2026-10-05T10:01:56.588552566+00:00 |
| `c-programs/mincore-residency@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.744470160+00:00, kvm candidate 2026-10-05T10:01:37.771633764+00:00 |
| `c-programs/mincore-residency@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.744470160+00:00, liteinst candidate 2026-10-05T10:01:52.673300476+00:00 |
| `c-programs/mincore-residency@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.744470160+00:00, sabre candidate 2026-10-05T10:01:55.823443003+00:00 |
| `c-programs/mixed-inline-and-libc-syscalls@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.518798897+00:00, kvm candidate 2026-10-05T10:01:32.830461227+00:00 |
| `c-programs/mixed-inline-and-libc-syscalls@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.518798897+00:00, liteinst candidate 2026-10-05T10:01:52.079697161+00:00 |
| `c-programs/mixed-inline-and-libc-syscalls@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.518798897+00:00, sabre candidate 2026-10-05T10:01:52.213919919+00:00 |
| `c-programs/mkdir-rmdir@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.913808081+00:00, kvm candidate 2026-10-05T10:01:38.090467899+00:00 |
| `c-programs/mkdir-rmdir@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.913808081+00:00, liteinst candidate 2026-10-05T10:01:54.945547523+00:00 |
| `c-programs/mkdir-rmdir@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.913808081+00:00, sabre candidate 2026-10-05T10:01:52.683822157+00:00 |
| `c-programs/mknod-special@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:59.742926402+00:00, kvm candidate 2026-10-05T10:01:36.982667652+00:00 |
| `c-programs/mknod-special@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:59.742926402+00:00, liteinst candidate 2026-10-05T10:01:52.064779948+00:00 |
| `c-programs/mknod-special@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:59.742926402+00:00, sabre candidate 2026-10-05T10:01:55.257030951+00:00 |
| `c-programs/mmap-layout-pointer-order@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.980384476+00:00, kvm candidate 2026-10-05T10:01:40.989661687+00:00 |
| `c-programs/mmap-layout-pointer-order@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.980384476+00:00, liteinst candidate 2026-10-05T10:01:51.782286113+00:00 |
| `c-programs/mmap-layout-pointer-order@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.980384476+00:00, sabre candidate 2026-10-05T10:01:56.611737901+00:00 |
| `c-programs/msync-writeback@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.677882775+00:00, kvm candidate 2026-10-05T10:01:36.886883635+00:00 |
| `c-programs/msync-writeback@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.677882775+00:00, liteinst candidate 2026-10-05T10:01:54.948434052+00:00 |
| `c-programs/msync-writeback@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.677882775+00:00, sabre candidate 2026-10-05T10:01:59.889253016+00:00 |
| `c-programs/name-to-handle-refusal@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.241707103+00:00, kvm candidate 2026-10-05T10:01:43.096914318+00:00 |
| `c-programs/name-to-handle-refusal@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.241707103+00:00, liteinst candidate 2026-10-05T10:01:51.728239992+00:00 |
| `c-programs/name-to-handle-refusal@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.241707103+00:00, sabre candidate 2026-10-05T10:01:54.516097032+00:00 |
| `c-programs/no-new-privs-refusal@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.763272173+00:00, kvm candidate 2026-10-05T10:01:40.605241100+00:00 |
| `c-programs/no-new-privs-refusal@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.763272173+00:00, liteinst candidate 2026-10-05T10:01:53.222737195+00:00 |
| `c-programs/no-new-privs-refusal@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.763272173+00:00, sabre candidate 2026-10-05T10:01:55.904670185+00:00 |
| `c-programs/numa-node-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/numa-node-identity@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.104504942+00:00, kvm candidate 2026-10-05T10:01:30.504128453+00:00 |
| `c-programs/numa-node-identity@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.104504942+00:00, liteinst candidate 2026-10-05T10:01:51.763576766+00:00 |
| `c-programs/numa-node-identity@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.104504942+00:00, sabre candidate 2026-10-05T10:01:58.187210525+00:00 |
| `c-programs/o-tmpfile-anon@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:02:00.073334211+00:00, kvm candidate 2026-10-05T10:01:40.502652132+00:00 |
| `c-programs/o-tmpfile-anon@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:02:00.073334211+00:00, liteinst candidate 2026-10-05T10:01:52.002707078+00:00 |
| `c-programs/o-tmpfile-anon@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:02:00.073334211+00:00, sabre candidate 2026-10-05T10:01:59.124656542+00:00 |
| `c-programs/openat-flags@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.799217858+00:00, kvm candidate 2026-10-05T10:01:41.710246257+00:00 |
| `c-programs/openat-flags@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.799217858+00:00, liteinst candidate 2026-10-05T10:01:51.799109725+00:00 |
| `c-programs/openat-flags@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.799217858+00:00, sabre candidate 2026-10-05T10:01:56.930384656+00:00 |
| `c-programs/openat2-refusal@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:58.761084244+00:00, kvm candidate 2026-10-05T10:01:33.879311887+00:00 |
| `c-programs/openat2-refusal@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:58.761084244+00:00, liteinst candidate 2026-10-05T10:01:52.505973157+00:00 |
| `c-programs/openat2-refusal@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:58.761084244+00:00, sabre candidate 2026-10-05T10:01:59.584025073+00:00 |
| `c-programs/path-file-ops@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.663685683+00:00, kvm candidate 2026-10-05T10:01:33.351206510+00:00 |
| `c-programs/path-file-ops@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.663685683+00:00, liteinst candidate 2026-10-05T10:01:52.669793164+00:00 |
| `c-programs/path-file-ops@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.663685683+00:00, sabre candidate 2026-10-05T10:01:57.211372176+00:00 |
| `c-programs/personality-domain@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.238633045+00:00, liteinst candidate 2026-10-05T10:01:56.882833328+00:00 |
| `c-programs/personality-domain@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.238633045+00:00, sabre candidate 2026-10-05T10:01:51.722831629+00:00 |
| `c-programs/pid-probe@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/pid-probe@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.324201274+00:00, kvm candidate 2026-10-05T10:01:33.210966782+00:00 |
| `c-programs/pid-probe@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.324201274+00:00, liteinst candidate 2026-10-05T10:01:51.698703577+00:00 |
| `c-programs/pid-probe@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.324201274+00:00, sabre candidate 2026-10-05T10:01:53.236908387+00:00 |
| `c-programs/pidfd-open-self-pair@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/pidfd-open-self-pair@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.079544912+00:00, kvm candidate 2026-10-05T10:01:38.203874833+00:00 |
| `c-programs/pidfd-open-self-pair@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.079544912+00:00, liteinst candidate 2026-10-05T10:01:53.119079525+00:00 |
| `c-programs/pidfd-open-self-pair@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.079544912+00:00, sabre candidate 2026-10-05T10:01:51.798834410+00:00 |
| `c-programs/pipe-capacity@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:55.004134592+00:00, kvm candidate 2026-10-05T10:01:28.636238077+00:00 |
| `c-programs/pipe-capacity@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:55.004134592+00:00, liteinst candidate 2026-10-05T10:01:53.003434366+00:00 |
| `c-programs/pipe-capacity@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:55.004134592+00:00, sabre candidate 2026-10-05T10:01:59.835369871+00:00 |
| `c-programs/pipe-capacity-pin@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:56.601457820+00:00, kvm candidate 2026-10-05T10:01:38.308690874+00:00 |
| `c-programs/pipe-capacity-pin@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:56.601457820+00:00, liteinst candidate 2026-10-05T10:01:51.716899057+00:00 |
| `c-programs/pipe-capacity-pin@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:56.601457820+00:00, sabre candidate 2026-10-05T10:01:57.342921418+00:00 |
| `c-programs/pipe-ipc@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:55.004119680+00:00, kvm candidate 2026-10-05T10:01:43.482275613+00:00 |
| `c-programs/pipe-ipc@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:55.004119680+00:00, liteinst candidate 2026-10-05T10:01:54.477743194+00:00 |
| `c-programs/pipe2-flags@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.051428641+00:00, kvm candidate 2026-10-05T10:01:30.934262286+00:00 |
| `c-programs/pipe2-flags@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.051428641+00:00, liteinst candidate 2026-10-05T10:01:52.229383928+00:00 |
| `c-programs/pipe2-flags@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.051428641+00:00, sabre candidate 2026-10-05T10:01:55.999234373+00:00 |
| `c-programs/poll-readiness@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.025104403+00:00, kvm candidate 2026-10-05T10:01:28.673166040+00:00 |
| `c-programs/poll-readiness@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.025104403+00:00, liteinst candidate 2026-10-05T10:01:52.077056071+00:00 |
| `c-programs/poll-readiness@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.025104403+00:00, sabre candidate 2026-10-05T10:01:52.953088144+00:00 |
| `c-programs/prctl-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/prctl-identity@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.100309928+00:00, kvm candidate 2026-10-05T10:01:31.777344078+00:00 |
| `c-programs/prctl-identity@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.100309928+00:00, liteinst candidate 2026-10-05T10:01:51.756947108+00:00 |
| `c-programs/prctl-identity@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.100309928+00:00, sabre candidate 2026-10-05T10:01:53.711316053+00:00 |
| `c-programs/prctl-pdeathsig@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.110542821+00:00, liteinst candidate 2026-10-05T10:01:52.068420099+00:00 |
| `c-programs/prctl-pdeathsig@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.110542821+00:00, sabre candidate 2026-10-05T10:01:55.264627451+00:00 |
| `c-programs/preadv2-flags@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.688428801+00:00, kvm candidate 2026-10-05T10:01:35.798536730+00:00 |
| `c-programs/preadv2-flags@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.688428801+00:00, liteinst candidate 2026-10-05T10:01:53.243796675+00:00 |
| `c-programs/preadv2-flags@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.688428801+00:00, sabre candidate 2026-10-05T10:01:55.630823504+00:00 |
| `c-programs/pthread-lifecycle@kvm` | candidate-missing[no-result-row] | — | the kvm candidate verify cell of c-programs/pthread-lifecycle has no result row in this run |
| `c-programs/pthread-lifecycle@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.858504789+00:00, liteinst candidate 2026-10-05T10:01:54.377629632+00:00 |
| `c-programs/pthread-lifecycle@sabre` | candidate-missing[no-result-row] | — | the sabre candidate verify cell of c-programs/pthread-lifecycle has no result row in this run |
| `c-programs/readdir-entries@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.099666537+00:00, kvm candidate 2026-10-05T10:01:36.634395365+00:00 |
| `c-programs/readdir-entries@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.099666537+00:00, liteinst candidate 2026-10-05T10:01:52.709669607+00:00 |
| `c-programs/readdir-entries@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.099666537+00:00, sabre candidate 2026-10-05T10:01:58.565276787+00:00 |
| `c-programs/readdir-order-identity@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.185524187+00:00, kvm candidate 2026-10-05T10:01:31.223523374+00:00 |
| `c-programs/readdir-order-identity@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.185524187+00:00, liteinst candidate 2026-10-05T10:01:54.264006683+00:00 |
| `c-programs/record-lock@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.948344518+00:00, liteinst candidate 2026-10-05T10:01:52.106013993+00:00 |
| `c-programs/record-lock@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.948344518+00:00, sabre candidate 2026-10-05T10:01:55.557284499+00:00 |
| `c-programs/rename-ops@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.945345560+00:00, kvm candidate 2026-10-05T10:01:29.768178409+00:00 |
| `c-programs/rename-ops@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.945345560+00:00, liteinst candidate 2026-10-05T10:01:52.690467062+00:00 |
| `c-programs/rename-ops@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.945345560+00:00, sabre candidate 2026-10-05T10:01:56.610638846+00:00 |
| `c-programs/renameat2-flags@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.628267031+00:00, kvm candidate 2026-10-05T10:01:41.323347323+00:00 |
| `c-programs/renameat2-flags@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.628267031+00:00, liteinst candidate 2026-10-05T10:01:52.863401329+00:00 |
| `c-programs/renameat2-flags@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.628267031+00:00, sabre candidate 2026-10-05T10:01:54.880201886+00:00 |
| `c-programs/rlimit-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/rlimit-identity@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.357001031+00:00, kvm candidate 2026-10-05T10:01:33.260246003+00:00 |
| `c-programs/rlimit-identity@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.357001031+00:00, liteinst candidate 2026-10-05T10:01:57.763547387+00:00 |
| `c-programs/rlimit-identity@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.357001031+00:00, sabre candidate 2026-10-05T10:01:57.098927977+00:00 |
| `c-programs/robust-list@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.997749779+00:00, kvm candidate 2026-10-05T10:01:31.285991639+00:00 |
| `c-programs/robust-list@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.997749779+00:00, liteinst candidate 2026-10-05T10:01:52.174373642+00:00 |
| `c-programs/robust-list@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.997749779+00:00, sabre candidate 2026-10-05T10:01:57.605252216+00:00 |
| `c-programs/sched-getaffinity-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/sched-getaffinity-identity@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.926347661+00:00, kvm candidate 2026-10-05T10:01:37.508602830+00:00 |
| `c-programs/sched-getaffinity-identity@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.926347661+00:00, liteinst candidate 2026-10-05T10:01:52.138278396+00:00 |
| `c-programs/sched-getaffinity-identity@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.926347661+00:00, sabre candidate 2026-10-05T10:01:58.377293832+00:00 |
| `c-programs/seccomp-refusal@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.947774090+00:00, kvm candidate 2026-10-05T10:01:40.123800695+00:00 |
| `c-programs/seccomp-refusal@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.947774090+00:00, liteinst candidate 2026-10-05T10:01:51.803587262+00:00 |
| `c-programs/seccomp-refusal@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.947774090+00:00, sabre candidate 2026-10-05T10:01:55.831243890+00:00 |
| `c-programs/sendfile-copy@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.612350625+00:00, kvm candidate 2026-10-05T10:01:30.024692194+00:00 |
| `c-programs/sendfile-copy@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.612350625+00:00, liteinst candidate 2026-10-05T10:01:51.774402675+00:00 |
| `c-programs/sendfile-copy@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.612350625+00:00, sabre candidate 2026-10-05T10:01:58.523127217+00:00 |
| `c-programs/set-tid-address@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.237255121+00:00, kvm candidate 2026-10-05T10:01:28.830479343+00:00 |
| `c-programs/set-tid-address@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.237255121+00:00, liteinst candidate 2026-10-05T10:01:57.908282235+00:00 |
| `c-programs/set-tid-address@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.237255121+00:00, sabre candidate 2026-10-05T10:01:55.481541187+00:00 |
| `c-programs/short-io-split-identity@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.821971218+00:00, kvm candidate 2026-10-05T10:01:40.760559156+00:00 |
| `c-programs/short-io-split-identity@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.821971218+00:00, liteinst candidate 2026-10-05T10:01:52.089772457+00:00 |
| `c-programs/short-io-split-identity@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.821971218+00:00, sabre candidate 2026-10-05T10:01:58.978463829+00:00 |
| `c-programs/shutdown-socketpair@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.731673557+00:00, kvm candidate 2026-10-05T10:01:34.061461788+00:00 |
| `c-programs/shutdown-socketpair@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.731673557+00:00, liteinst candidate 2026-10-05T10:01:51.734272147+00:00 |
| `c-programs/shutdown-socketpair@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.731673557+00:00, sabre candidate 2026-10-05T10:01:57.301388623+00:00 |
| `c-programs/signal-delivery-sequence@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:58.600276605+00:00, liteinst candidate 2026-10-05T10:01:59.013241390+00:00 |
| `c-programs/signal-waitstatus-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/signal-waitstatus-identity@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:59.919500392+00:00, liteinst candidate 2026-10-05T10:01:52.208791292+00:00 |
| `c-programs/signalfd-create@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.781784269+00:00, kvm candidate 2026-10-05T10:01:42.864112398+00:00 |
| `c-programs/signalfd-create@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.781784269+00:00, liteinst candidate 2026-10-05T10:01:51.664684132+00:00 |
| `c-programs/signalfd-create@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.781784269+00:00, sabre candidate 2026-10-05T10:01:51.650133872+00:00 |
| `c-programs/socket-epoll-ordering@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.352606396+00:00, kvm candidate 2026-10-05T10:01:32.811041095+00:00 |
| `c-programs/socket-epoll-ordering@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.352606396+00:00, liteinst candidate 2026-10-05T10:01:53.132306670+00:00 |
| `c-programs/socket-epoll-ordering@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:57.352606396+00:00, sabre candidate 2026-10-05T10:01:52.506876615+00:00 |
| `c-programs/socket-options@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.952875370+00:00, kvm candidate 2026-10-05T10:01:29.283025721+00:00 |
| `c-programs/socket-options@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.952875370+00:00, liteinst candidate 2026-10-05T10:01:57.885637615+00:00 |
| `c-programs/socket-options@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.952875370+00:00, sabre candidate 2026-10-05T10:01:51.801462006+00:00 |
| `c-programs/socketpair-flags@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.893327991+00:00, kvm candidate 2026-10-05T10:01:31.448012477+00:00 |
| `c-programs/socketpair-flags@liteinst` | nondeterministic[determinism-mismatch] | — | the liteinst candidate verify cell of c-programs/socketpair-flags failed determinism on attempt 1 (its two runs diverged), so it has no deterministic log to compare: canonical verification did not match: verified=false verdict=diverged bitwise_parity=false |
| `c-programs/socketpair-flags@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.893327991+00:00, sabre candidate 2026-10-05T10:01:52.203810627+00:00 |
| `c-programs/sockname-unnamed@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.151158588+00:00, kvm candidate 2026-10-05T10:01:32.563285267+00:00 |
| `c-programs/sockname-unnamed@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.151158588+00:00, liteinst candidate 2026-10-05T10:01:54.991033729+00:00 |
| `c-programs/sockname-unnamed@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.151158588+00:00, sabre candidate 2026-10-05T10:01:52.150458535+00:00 |
| `c-programs/stat-metadata-identity@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.741439268+00:00, kvm candidate 2026-10-05T10:01:40.698921843+00:00 |
| `c-programs/stat-metadata-identity@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.741439268+00:00, liteinst candidate 2026-10-05T10:01:52.704745615+00:00 |
| `c-programs/stat-metadata-identity@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.741439268+00:00, sabre candidate 2026-10-05T10:01:51.442667110+00:00 |
| `c-programs/statfs-free-determinism@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.986822774+00:00, kvm candidate 2026-10-05T10:01:39.308380100+00:00 |
| `c-programs/statfs-free-determinism@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.986822774+00:00, liteinst candidate 2026-10-05T10:01:52.581835524+00:00 |
| `c-programs/statfs-free-determinism@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.986822774+00:00, sabre candidate 2026-10-05T10:01:51.698341829+00:00 |
| `c-programs/static-nolibc-syscall-sites@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:59.461603201+00:00, kvm candidate 2026-10-05T10:01:42.914995742+00:00 |
| `c-programs/statx-metadata@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.687931660+00:00, kvm candidate 2026-10-05T10:01:33.505380238+00:00 |
| `c-programs/statx-metadata@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.687931660+00:00, liteinst candidate 2026-10-05T10:01:52.687988476+00:00 |
| `c-programs/statx-metadata@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.687931660+00:00, sabre candidate 2026-10-05T10:01:53.011121141+00:00 |
| `c-programs/symlink-ops@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.934767793+00:00, kvm candidate 2026-10-05T10:01:34.506118051+00:00 |
| `c-programs/symlink-ops@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.934767793+00:00, liteinst candidate 2026-10-05T10:01:57.193786411+00:00 |
| `c-programs/symlink-ops@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:53.934767793+00:00, sabre candidate 2026-10-05T10:01:53.970214079+00:00 |
| `c-programs/sync-file-range@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.759708178+00:00, kvm candidate 2026-10-05T10:01:39.933603573+00:00 |
| `c-programs/sync-file-range@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.759708178+00:00, liteinst candidate 2026-10-05T10:01:53.233246018+00:00 |
| `c-programs/sync-file-range@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.759708178+00:00, sabre candidate 2026-10-05T10:01:52.898488534+00:00 |
| `c-programs/sysv-ipc-refusal@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.087779762+00:00, kvm candidate 2026-10-05T10:01:34.105487879+00:00 |
| `c-programs/sysv-ipc-refusal@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.087779762+00:00, liteinst candidate 2026-10-05T10:01:51.728225282+00:00 |
| `c-programs/sysv-ipc-refusal@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.087779762+00:00, sabre candidate 2026-10-05T10:01:51.665206470+00:00 |
| `c-programs/thp-disable@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.523874915+00:00, kvm candidate 2026-10-05T10:01:39.516902380+00:00 |
| `c-programs/thp-disable@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.523874915+00:00, liteinst candidate 2026-10-05T10:01:57.688852820+00:00 |
| `c-programs/thp-disable@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:54.523874915+00:00, sabre candidate 2026-10-05T10:01:54.593150796+00:00 |
| `c-programs/umask-mode@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:59.500489635+00:00, kvm candidate 2026-10-05T10:01:35.595630047+00:00 |
| `c-programs/umask-mode@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:59.500489635+00:00, liteinst candidate 2026-10-05T10:01:58.819857193+00:00 |
| `c-programs/umask-mode@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:59.500489635+00:00, sabre candidate 2026-10-05T10:01:51.795476557+00:00 |
| `c-programs/uname-identity@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:56.749064489+00:00, kvm candidate 2026-10-05T10:01:33.174885429+00:00 |
| `c-programs/uname-identity@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:56.749064489+00:00, liteinst candidate 2026-10-05T10:01:51.896583017+00:00 |
| `c-programs/uname-identity@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:56.749064489+00:00, sabre candidate 2026-10-05T10:01:52.034562878+00:00 |
| `c-programs/utimensat-determinism@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.135891479+00:00, kvm candidate 2026-10-05T10:01:34.810829892+00:00 |
| `c-programs/utimensat-determinism@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.135891479+00:00, liteinst candidate 2026-10-05T10:01:54.240455480+00:00 |
| `c-programs/utimensat-determinism@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:52.135891479+00:00, sabre candidate 2026-10-05T10:01:54.479492382+00:00 |
| `c-programs/vectored-file-io@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:02:00.351196109+00:00, kvm candidate 2026-10-05T10:01:37.875364022+00:00 |
| `c-programs/vectored-file-io@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:02:00.351196109+00:00, liteinst candidate 2026-10-05T10:01:57.778401335+00:00 |
| `c-programs/vectored-file-io@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:02:00.351196109+00:00, sabre candidate 2026-10-05T10:01:52.590537616+00:00 |
| `c-programs/vectored-io@kvm` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.800671030+00:00, kvm candidate 2026-10-05T10:01:35.202287229+00:00 |
| `c-programs/vectored-io@liteinst` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.800671030+00:00, liteinst candidate 2026-10-05T10:01:57.530935692+00:00 |
| `c-programs/vectored-io@sabre` | unavailable[epoch-not-shared] | — | the operands ran with different HERMIT_EPOCH values: ptrace reference 2026-10-05T10:01:51.800671030+00:00, sabre candidate 2026-10-05T10:01:55.218467432+00:00 |

### pressure-test run `p2-screen1` at `7759159896ab` (partial: selected 14 of 297 committed)

`parity: not compared: 0 measured of 14 selected (inputs cannot be equalized); selected 14 of 297 committed (partial)`

| Candidate backend | Selected | Measured | Matched | Diverged | No golden | Not compared | Unmeasured | Record-missing | Refused | Mean credit (measured) | Floor credit | Credit inputs |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | --- |
| `dbt` | 14 of 16 | 0 | 0 | 0 | 0 | 14 | 0 | 0 | 0 | n/a | n/a | not compared (inputs cannot be equalized) |
| `kvm` | 0 of 91 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | n/a | n/a | — |
| `liteinst` | 0 of 99 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | n/a | n/a | — |
| `sabre` | 0 of 91 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | n/a | n/a | — |
| **TOTAL** | 14 of 297 | 0 | 0 | 0 | 0 | 14 | 0 | 0 | 0 | n/a | n/a | not compared (inputs cannot be equalized) |

283 cell(s) its commit selects have no row in this run: `c-programs/aio-refusal@kvm`, `c-programs/aio-refusal@liteinst`, `c-programs/aio-refusal@sabre`, `c-programs/append-pwrite@kvm`, `c-programs/append-pwrite@liteinst`, `c-programs/append-pwrite@sabre`, `c-programs/bind-getsockname@kvm`, `c-programs/bind-getsockname@liteinst`, `c-programs/bind-getsockname@sabre`, `c-programs/cachestat-refusal@kvm`, `c-programs/cachestat-refusal@liteinst`, `c-programs/cachestat-refusal@sabre`, `c-programs/child-subreaper-refusal@kvm`, `c-programs/child-subreaper-refusal@liteinst`, `c-programs/child-subreaper-refusal@sabre`, `c-programs/close-range-fds@kvm`, `c-programs/close-range-fds@liteinst`, `c-programs/copy-file-range-refusal@kvm`, `c-programs/copy-file-range-refusal@liteinst`, `c-programs/copy-file-range-refusal@sabre` and 263 more (all are in `scorecard/parity.json`).

Every cell that did not match, with its first divergence or the reason it was not measured:

| Cell | Verdict | Credit | First divergence or reason |
| --- | --- | ---: | --- |
| `c-programs/cpu-virtualization@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/dup-shared-offset@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/fd-duplication@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/getcpu-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/getpriority-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/hardware-trap-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/host-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/lseek-positioning@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/numa-node-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/pidfd-open-self-pair@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/prctl-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/rlimit-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/sched-getaffinity-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |
| `c-programs/signal-waitstatus-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |

Outside the clean headline: 0 parity rows from a dirty source tree.

Outside the clean headline: 0 parity rows that did not report their source tree state.

### 58 other parity run(s) in the store

Only a run from a clean source tree can be its producer's headline: at least one of its rows says `"source_tree_dirty": false`, and none says `true` or leaves the value out. A row refused for its own defect does not count; one refused only because its run's rows name more than one Hermit commit does. Among those runs, the headline is the run that reported every cell its own Hermit commit's selection owes; a partial run headlines only when no complete run exists, the most complete first. Then the deepest Hermit commit this checkout can place, then the latest emission.

- validate run `validate-buck-re-3-1931c6ccd8f7-1791054320749237313-288493-c38549d2` at Hermit `1931c6ccd8f7`: `parity: 0/205 matched; selected 205 of 205 committed; mean n/a over 0 measured; floor 0.000 over 189 of 205 selected (counted as 0: 189 record-missing or refused; excluded: 0 no golden; 0 not compared; 16 record-missing or refused whose inputs cannot be equalized)`
- validate run `validate-buck-re-3-5301e9a2dbb2-1791051975076983128-689340-69dc34f7` at Hermit `5301e9a2dbb2`: `parity: 0/205 matched; selected 205 of 205 committed; mean n/a over 0 measured; floor 0.000 over 189 of 205 selected (counted as 0: 189 record-missing or refused; excluded: 0 no golden; 0 not compared; 16 record-missing or refused whose inputs cannot be equalized)`
- validate run `validate-buck-re-3-66db685c5279-1791099824835735230-1153730-3be6996b` at Hermit `66db685c5279`: `parity: 0/297 matched; selected 297 of 297 committed; mean n/a over 0 measured; floor 0.000 over 281 of 297 selected (counted as 0: 281 record-missing or refused; excluded: 0 no golden; 0 not compared; 16 record-missing or refused whose inputs cannot be equalized)`
- validate run `validate-buck-re-3-75bec89e7db9-1791060150360793710-2018425-d3821d9a` at Hermit `75bec89e7db9`: `parity: 0/206 matched; selected 206 of 206 committed; mean n/a over 0 measured; floor 0.000 over 190 of 206 selected (counted as 0: 190 record-missing or refused; excluded: 0 no golden; 0 not compared; 16 record-missing or refused whose inputs cannot be equalized)`
- validate run `validate-buck-re-3-7831ad1c8931-1791056799838919343-1002648-6b3c5041` at Hermit `7831ad1c8931`: `parity: 0/205 matched; selected 205 of 205 committed; mean 0.055 over 186 measured; floor 0.055 over 189 of 205 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-buck-re-3-84821e53edb7-1791041721981051099-2446052-bf662192` at Hermit `84821e53edb7`: `parity: 0/204 matched; selected 204 of 204 committed; mean n/a over 0 measured; floor 0.000 over 188 of 204 selected (counted as 0: 188 record-missing or refused; excluded: 0 no golden; 0 not compared; 16 record-missing or refused whose inputs cannot be equalized)`
- validate run `validate-buck-re-3-b84faa26c355-1791103535341810231-4116164-a06293f4` at Hermit `b84faa26c355`: `parity: 0/297 matched; selected 297 of 297 committed; mean n/a over 0 measured; floor 0.000 over 281 of 297 selected (counted as 0: 281 record-missing or refused; excluded: 0 no golden; 0 not compared; 16 record-missing or refused whose inputs cannot be equalized)`
- validate run `validate-buck-re-3-ccccde5f65d6-1791044441627223683-4139778-3d0a278f` at Hermit `ccccde5f65d6`: `parity: 0/204 matched; selected 204 of 204 committed; mean n/a over 0 measured; floor 0.000 over 188 of 204 selected (counted as 0: 188 record-missing or refused; excluded: 0 no golden; 0 not compared; 16 record-missing or refused whose inputs cannot be equalized)`
- validate run `validate-buck-re-3-cf2a94ea38d3-1791063611629487524-833061-4b45d912` at Hermit `cf2a94ea38d3`: `parity: 0/206 matched; selected 206 of 206 committed; mean n/a over 0 measured; floor 0.000 over 190 of 206 selected (counted as 0: 190 record-missing or refused; excluded: 0 no golden; 0 not compared; 16 record-missing or refused whose inputs cannot be equalized)`
- validate run `validate-buck-re-3-e81ace1c0be7-1791057684904978741-1891131-b31df65f` at Hermit `e81ace1c0be7`: `parity: 0/205 matched; selected 205 of 205 committed; mean n/a over 0 measured; floor 0.000 over 189 of 205 selected (counted as 0: 189 record-missing or refused; excluded: 0 no golden; 0 not compared; 16 record-missing or refused whose inputs cannot be equalized)`
- validate run `validate-claude-2-118f0bfc4553-1791028466715093346-2549334-529fe13e` at Hermit `118f0bfc4553`: `parity: 0/203 matched; committed selection unknown; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-2-4346217e08d4-1791039676390384316-4107946-e2cf462e` at Hermit `4346217e08d4`: `parity: 0/204 matched; selected 204 of 204 committed; mean 0.055 over 185 measured; floor 0.054 over 188 of 204 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-2-6b5424c1860a-1791037904455844226-3351413-5eebd499` at Hermit `6b5424c1860a`: `parity: 0/204 matched; selected 204 of 204 committed; mean 0.055 over 185 measured; floor 0.054 over 188 of 204 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-2-b6ca81b78be8-1791031592862600986-362222-8863246d` at Hermit `b6ca81b78be8`: `parity: 0/203 matched; selected 203 of 203 committed; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-2-cfb9d0c86427-1791035312332954244-2326456-5ddbba2a` at Hermit `cfb9d0c86427`: `parity: 0/203 matched; committed selection unknown; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-4-ad4f9cf598a3-1791030814588067897-4023945-e9bb1627` at Hermit `ad4f9cf598a3`: `parity: 0/203 matched; committed selection unknown; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-4-d53ea6ca1711-1791032694966120621-1230314-61570ac3` at Hermit `d53ea6ca1711`: `parity: 0/203 matched; committed selection unknown; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-coord-afd1f1d5d227-1791178671086851930-991513-70fd1bb2` at Hermit `afd1f1d5d227`: `parity: 0/198 matched; committed selection unknown; mean n/a over 0 measured; floor 0.000 over 182 of 198 selected (counted as 0: 182 unmeasured: no-result-row 2, epoch-not-shared 180; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-coord-mega-lander-4c0b7daa8ae6-1791155208411569193-894674-eab4fa86` at Hermit `4c0b7daa8ae6`: `parity: 0/297 matched; committed selection unknown; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-coord-itvfix-57883eff8f3d-1791046213891728858-2276425-f9271a9e` at Hermit `57883eff8f3d`: `parity: 0/204 matched; selected 204 of 204 committed; mean 0.055 over 185 measured; floor 0.054 over 188 of 204 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-coord-tickval-4565b01b66c2-1791139406221186959-1755386-8f2aad77` at Hermit `4565b01b66c2`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-coord-tickval-b3037b11fa56-1791136526911996485-754722-df9fcd8a` at Hermit `b3037b11fa56`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-coord-tipval2-c1312a563dc5-20261003T235258Z` at Hermit `c1312a563dc5`: `parity: 0/208 matched; selected 208 of 208 committed; mean 0.056 over 188 measured; floor 0.055 over 191 of 208 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 1 no golden: crash 1; 16 not compared)`
- validate run `validate-d14-queue-a2b1deecaa21-1791037234447726839-773804-79469e5b` at Hermit `a2b1deecaa21`: `parity: 0/203 matched; selected 203 of 203 committed; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-gate-select-9e698b862c8e-1791147210324610650-2296438-f7c76258` at Hermit `9e698b862c8e`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 276 measured; floor 0.043 over 279 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 2 no golden: ended 2; 16 not compared)`
- validate run `validate-gate-select-9e7dd6e33cf6-1791027490982151389-186043-c4a5cdf6` at Hermit `9e7dd6e33cf6`: `parity: 0/203 matched; selected 203 of 203 committed; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-gate-select-f85de5d891ab-1791150197069282693-1112723-7a425489` at Hermit `f85de5d891ab`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-hermit-lander-33f1c939a2e4-1791054196460293033-32690-ce1f4789` at Hermit `33f1c939a2e4`: `parity: 0/205 matched; selected 205 of 205 committed; mean 0.055 over 186 measured; floor 0.055 over 189 of 205 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-netreplay-rework-27ee682e6475-1791179482454733843-2302177-80f2dba6` at Hermit `27ee682e6475`: `parity: 0/297 matched; selected 297 of 297 committed; mean n/a over 0 measured; floor 0.000 over 280 of 297 selected (counted as 0: 280 unmeasured: no-result-row 3, epoch-not-shared 277; excluded: 1 no golden: timeout 1; 16 not compared)`
- validate run `validate-netreplay-rework-7f8e75ff4d3a-1791184155517346601-1394308-0ff4a7b6` at Hermit `7f8e75ff4d3a`: `parity: 0/297 matched; selected 297 of 297 committed; mean n/a over 0 measured; floor 0.000 over 277 of 297 selected (counted as 0: 277 unmeasured: no-result-row 3, epoch-not-shared 274; excluded: 4 no golden: determinism-mismatch 3, timeout 1; 16 not compared)`
- validate run `validate-ops-tick-0f028322361f-75248bd48fcd` at Hermit `0f028322361f`: `parity: 0/203 matched; selected 203 of 203 committed; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-0f028322361f-a6fa2a143efd` at Hermit `0f028322361f`: `parity: 0/203 matched; selected 203 of 203 committed; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-20077ee5b19f-63f1efea5c13` at Hermit `20077ee5b19f`: `parity: 0/205 matched; selected 205 of 205 committed; mean 0.055 over 186 measured; floor 0.055 over 189 of 205 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-20d49d84ec23-914f280b5537` at Hermit `20d49d84ec23`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-33770223b5d2-11a841f749a1` at Hermit `33770223b5d2`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 277 measured; floor 0.043 over 280 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 1 no golden: crash 1; 16 not compared)`
- validate run `validate-ops-tick-33770223b5d2-27d0ecdc8ccb` at Hermit `33770223b5d2`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 277 measured; floor 0.043 over 280 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 1 no golden: crash 1; 16 not compared)`
- validate run `validate-ops-tick-4b1894e81cfb-18e5730802ad` at Hermit `4b1894e81cfb`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 277 measured; floor 0.043 over 280 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 1 no golden: crash 1; 16 not compared)`
- validate run `validate-ops-tick-4b1894e81cfb-19db240fdcc8` at Hermit `4b1894e81cfb`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 277 measured; floor 0.043 over 280 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 1 no golden: crash 1; 16 not compared)`
- validate run `validate-ops-tick-562b7dd7a635-80161ea9acc1` at Hermit `562b7dd7a635`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-562b7dd7a635-dffebdc99927` at Hermit `562b7dd7a635`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-8503b4fc2c30-57d1aa9b1fa2` at Hermit `8503b4fc2c30`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-8503b4fc2c30-f0c6c4842871` at Hermit `8503b4fc2c30`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-9af999677494-4ea4d2816eaf` at Hermit `9af999677494`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-9d6c1f214242-545b42c1e906` at Hermit `9d6c1f214242`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 277 measured; floor 0.043 over 279 of 297 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 2 no golden: crash 1, ended 1; 16 not compared)`
- validate run `validate-ops-tick-9d6c1f214242-df156e1dcf8c` at Hermit `9d6c1f214242`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 277 measured; floor 0.043 over 280 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 1 no golden: crash 1; 16 not compared)`
- validate run `validate-ops-tick-9f1d0350ab7a-cadb83b856b7` at Hermit `9f1d0350ab7a`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-a015bbb6029b-c266b0f86c0b` at Hermit `a015bbb6029b`: `parity: 0/206 matched; selected 206 of 206 committed; mean 0.056 over 187 measured; floor 0.055 over 190 of 206 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-a2b1deecaa21-3af8d38d7b78` at Hermit `a2b1deecaa21`: `parity: 0/203 matched; selected 203 of 203 committed; mean 0.055 over 184 measured; floor 0.054 over 186 of 203 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 1 no golden: ended 1; 16 not compared)`
- validate run `validate-ops-tick-a8ee9d5a5733-08a2e5aca2da` at Hermit `a8ee9d5a5733`: `parity: 0/206 matched; selected 206 of 206 committed; mean 0.056 over 187 measured; floor 0.055 over 190 of 206 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-a8ee9d5a5733-a07460befccd` at Hermit `a8ee9d5a5733`: `parity: 0/206 matched; selected 206 of 206 committed; mean 0.056 over 187 measured; floor 0.055 over 189 of 206 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 1 no golden: ended 1; 16 not compared)`
- validate run `validate-ops-tick-b5c378041d84-4990804fc489` at Hermit `b5c378041d84`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 277 measured; floor 0.043 over 280 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 1 no golden: crash 1; 16 not compared)`
- validate run `validate-ops-tick-b5c378041d84-c53822974f8f` at Hermit `b5c378041d84`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 277 measured; floor 0.043 over 280 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 1 no golden: crash 1; 16 not compared)`
- validate run `validate-ops-tick-cec1ef5f402d-55392313f8a8` at Hermit `cec1ef5f402d`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-d5d1072a6f82-20a21bc9fe7b` at Hermit `d5d1072a6f82`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-e02d9f8368a3-03a15e241a7e` at Hermit `e02d9f8368a3`: `parity: 0/205 matched; selected 205 of 205 committed; mean 0.055 over 186 measured; floor 0.055 over 188 of 205 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 1 no golden: ended 1; 16 not compared)`
- validate run `validate-tick-buck-cargo-repro-66378ba1dd58-20261005T010205Z` at Hermit `66378ba1dd58`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- pressure-test run `p1-green10` at Hermit `7759159896ab`: `parity: not compared: 0 measured of 2 selected (inputs cannot be equalized); selected 2 of 297 committed (partial)`
- pressure-test run `pressure-c470fa213ee8-20261003T180650Z` at Hermit `c470fa213ee8`: `parity: not compared: 0 measured of 1 selected (inputs cannot be equalized); selected 1 of 205 committed (partial)`

### legacy-rerun history

`legacy-rerun (retired ptrace rerun on 173 cell(s), last at 5ee668223a15): 0 matched / 173 diverged`

These verdicts came from the retired ptrace rerun, which ran each candidate beside a fresh ptrace run. They are kept as labelled history only: they are not current parity, they carry no credit, and they enter none of the counts above. The `Determinism` column is the cell's own measurement, which parity never changes.

1211 older comparison(s) of the same cells were superseded by a later one.

Retired rerun evidence not kept as history, by reason: `no-retained-comparison` 4322 on 173 cell(s).

| Cell | Label | Legacy verdict | Compared records | First divergent record | Hermit commit | Determinism | Current parity |
| --- | --- | --- | ---: | ---: | --- | --- | --- |
| `c-programs/aio-refusal@kvm` | legacy-rerun | diverged | 107 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/aio-refusal@liteinst` | legacy-rerun | diverged | 107 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/append-pwrite@kvm` | legacy-rerun | diverged | 144 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/append-pwrite@liteinst` | legacy-rerun | diverged | 152 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/bind-getsockname@kvm` | legacy-rerun | diverged | 110 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/bind-getsockname@liteinst` | legacy-rerun | diverged | 110 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/cachestat-refusal@kvm` | legacy-rerun | diverged | 124 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/cachestat-refusal@liteinst` | legacy-rerun | diverged | 132 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/child-subreaper-refusal@kvm` | legacy-rerun | diverged | 103 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/child-subreaper-refusal@liteinst` | legacy-rerun | diverged | 103 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/close-range-fds@kvm` | legacy-rerun | diverged | 137 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/close-range-fds@liteinst` | legacy-rerun | diverged | 145 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/copy-file-range-refusal@kvm` | legacy-rerun | diverged | 139 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/copy-file-range-refusal@liteinst` | legacy-rerun | diverged | 143 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/cpu-virtualization@kvm` | legacy-rerun | diverged | 104 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/cpu-virtualization@liteinst` | legacy-rerun | diverged | 104 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/cwd-roundtrip@kvm` | legacy-rerun | diverged | 133 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/cwd-roundtrip@liteinst` | legacy-rerun | diverged | 141 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/dup-shared-offset@kvm` | legacy-rerun | diverged | 148 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/dup-shared-offset@liteinst` | legacy-rerun | diverged | 156 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/epoll-pwait2@liteinst` | legacy-rerun | diverged | 125 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/epoll-readiness@kvm` | legacy-rerun | diverged | 123 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/epoll-readiness@liteinst` | legacy-rerun | diverged | 123 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/event-delivery-ordering@liteinst` | legacy-rerun | diverged | 190 | 16 | `5ee668223a15` | `diverged` | nondeterministic |
| `c-programs/eventfd-semantics@kvm` | legacy-rerun | diverged | 169 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/eventfd-semantics@liteinst` | legacy-rerun | diverged | 169 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/faccessat2-flags@kvm` | legacy-rerun | diverged | 132 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/faccessat2-flags@liteinst` | legacy-rerun | diverged | 140 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/fadvise-hints@kvm` | legacy-rerun | diverged | 124 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/fadvise-hints@liteinst` | legacy-rerun | diverged | 132 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/fallocate-extents@kvm` | legacy-rerun | diverged | 130 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/fallocate-extents@liteinst` | legacy-rerun | diverged | 138 | 16 | `5ee668223a15` | `diverged` | nondeterministic |
| `c-programs/fchmod-bits@kvm` | legacy-rerun | diverged | 131 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/fchmod-bits@liteinst` | legacy-rerun | diverged | 139 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/fchmodat2-flags@liteinst` | legacy-rerun | diverged | 139 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/fcntl-owner@liteinst` | legacy-rerun | diverged | 121 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/fd-duplication@kvm` | legacy-rerun | diverged | 163 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/fd-duplication@liteinst` | legacy-rerun | diverged | 171 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/file-backed-mmap@kvm` | legacy-rerun | diverged | 133 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/file-backed-mmap@liteinst` | legacy-rerun | diverged | 141 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/file-io-roundtrip@kvm` | legacy-rerun | diverged | 148 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/file-io-roundtrip@liteinst` | legacy-rerun | diverged | 148 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/flock-lifecycle@kvm` | legacy-rerun | diverged | 121 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/flock-lifecycle@liteinst` | legacy-rerun | diverged | 129 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/fork-exec-pipeline@kvm` | legacy-rerun | diverged | 337 | 13 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/fsync-durability@liteinst` | legacy-rerun | diverged | 132 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/ftruncate-sparse@kvm` | legacy-rerun | diverged | 135 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/ftruncate-sparse@liteinst` | legacy-rerun | diverged | 143 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/getcpu-identity@kvm` | legacy-rerun | diverged | 123 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/getcpu-identity@liteinst` | legacy-rerun | diverged | 123 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/getpriority-identity@kvm` | legacy-rerun | diverged | 113 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/getpriority-identity@liteinst` | legacy-rerun | diverged | 113 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/hardware-trap-identity@liteinst` | legacy-rerun | diverged | 312 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/host-identity@liteinst` | legacy-rerun | diverged | 117 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/inline-syscall-sites@kvm` | legacy-rerun | diverged | 165 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/inline-syscall-sites@liteinst` | legacy-rerun | diverged | 165 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/inotify-watch@liteinst` | legacy-rerun | diverged | 110 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/ioctl-fionread@liteinst` | legacy-rerun | diverged | 120 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/kcmp-refusal@kvm` | legacy-rerun | diverged | 111 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/kcmp-refusal@liteinst` | legacy-rerun | diverged | 111 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/linkat-flags@kvm` | legacy-rerun | diverged | 146 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/linkat-flags@liteinst` | legacy-rerun | diverged | 154 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/lseek-positioning@kvm` | legacy-rerun | diverged | 143 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/lseek-positioning@liteinst` | legacy-rerun | diverged | 151 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/mce-kill-refusal@kvm` | legacy-rerun | diverged | 103 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/mce-kill-refusal@liteinst` | legacy-rerun | diverged | 103 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/membarrier-query@kvm` | legacy-rerun | diverged | 105 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/membarrier-query@liteinst` | legacy-rerun | diverged | 105 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/memfd-create@kvm` | legacy-rerun | diverged | 121 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/memfd-create@liteinst` | legacy-rerun | diverged | 121 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/mempolicy-default@kvm` | legacy-rerun | diverged | 107 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/mempolicy-default@liteinst` | legacy-rerun | diverged | 107 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/mincore-residency@kvm` | legacy-rerun | diverged | 119 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/mincore-residency@liteinst` | legacy-rerun | diverged | 119 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/mixed-inline-and-libc-syscalls@kvm` | legacy-rerun | diverged | 147 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/mixed-inline-and-libc-syscalls@liteinst` | legacy-rerun | diverged | 147 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/mkdir-rmdir@kvm` | legacy-rerun | diverged | 129 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/mkdir-rmdir@liteinst` | legacy-rerun | diverged | 137 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/mknod-special@kvm` | legacy-rerun | diverged | 133 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/mknod-special@liteinst` | legacy-rerun | diverged | 141 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/mmap-layout-pointer-order@kvm` | legacy-rerun | diverged | 133 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/mmap-layout-pointer-order@liteinst` | legacy-rerun | diverged | 141 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/msync-writeback@liteinst` | legacy-rerun | diverged | 137 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/name-to-handle-refusal@kvm` | legacy-rerun | diverged | 103 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/name-to-handle-refusal@liteinst` | legacy-rerun | diverged | 103 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/no-new-privs-refusal@kvm` | legacy-rerun | diverged | 105 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/no-new-privs-refusal@liteinst` | legacy-rerun | diverged | 105 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/numa-node-identity@kvm` | legacy-rerun | diverged | 111 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/numa-node-identity@liteinst` | legacy-rerun | diverged | 111 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/o-tmpfile-anon@kvm` | legacy-rerun | diverged | 120 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/o-tmpfile-anon@liteinst` | legacy-rerun | diverged | 120 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/openat-flags@kvm` | legacy-rerun | diverged | 160 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/openat-flags@liteinst` | legacy-rerun | diverged | 168 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/openat2-refusal@kvm` | legacy-rerun | diverged | 105 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/openat2-refusal@liteinst` | legacy-rerun | diverged | 105 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/path-file-ops@kvm` | legacy-rerun | diverged | 141 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/path-file-ops@liteinst` | legacy-rerun | diverged | 149 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/personality-domain@liteinst` | legacy-rerun | diverged | 109 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/pid-probe@kvm` | legacy-rerun | diverged | 101 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/pid-probe@liteinst` | legacy-rerun | diverged | 101 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/pid-probe@sabre` | legacy-rerun | diverged | 32 | 3 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/pidfd-open-self-pair@kvm` | legacy-rerun | diverged | 117 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/pidfd-open-self-pair@liteinst` | legacy-rerun | diverged | 117 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/pipe-capacity-pin@kvm` | legacy-rerun | diverged | 111 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/pipe-capacity-pin@liteinst` | legacy-rerun | diverged | 111 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/pipe-capacity@kvm` | legacy-rerun | diverged | 113 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/pipe-capacity@liteinst` | legacy-rerun | diverged | 113 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/pipe-ipc@kvm` | legacy-rerun | diverged | 158 | 13 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/pipe-ipc@liteinst` | legacy-rerun | diverged | 164 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/pipe2-flags@kvm` | legacy-rerun | diverged | 163 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/pipe2-flags@liteinst` | legacy-rerun | diverged | 163 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/poll-readiness@liteinst` | legacy-rerun | diverged | 139 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/prctl-identity@liteinst` | legacy-rerun | diverged | 121 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/prctl-pdeathsig@liteinst` | legacy-rerun | diverged | 111 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/preadv2-flags@liteinst` | legacy-rerun | diverged | 140 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/pthread-lifecycle@liteinst` | legacy-rerun | diverged | 214 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/readdir-entries@kvm` | legacy-rerun | diverged | 159 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/readdir-entries@liteinst` | legacy-rerun | diverged | 167 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/readdir-order-identity@kvm` | legacy-rerun | diverged | 13669 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/readdir-order-identity@liteinst` | legacy-rerun | diverged | 13677 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/record-lock@liteinst` | legacy-rerun | diverged | 131 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/rename-ops@kvm` | legacy-rerun | diverged | 163 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/rename-ops@liteinst` | legacy-rerun | diverged | 171 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/renameat2-flags@kvm` | legacy-rerun | diverged | 184 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/renameat2-flags@liteinst` | legacy-rerun | diverged | 192 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/rlimit-identity@kvm` | legacy-rerun | diverged | 121 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/rlimit-identity@liteinst` | legacy-rerun | diverged | 121 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/robust-list@kvm` | legacy-rerun | diverged | 105 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/robust-list@liteinst` | legacy-rerun | diverged | 105 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/sched-getaffinity-identity@kvm` | legacy-rerun | diverged | 111 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/sched-getaffinity-identity@liteinst` | legacy-rerun | diverged | 111 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/seccomp-refusal@kvm` | legacy-rerun | diverged | 103 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/seccomp-refusal@liteinst` | legacy-rerun | diverged | 103 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/sendfile-copy@kvm` | legacy-rerun | diverged | 155 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/sendfile-copy@liteinst` | legacy-rerun | diverged | 159 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/set-tid-address@kvm` | legacy-rerun | diverged | 107 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/set-tid-address@liteinst` | legacy-rerun | diverged | 107 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/short-io-split-identity@kvm` | legacy-rerun | diverged | 431 | 13 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/short-io-split-identity@liteinst` | legacy-rerun | diverged | 435 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/shutdown-socketpair@kvm` | legacy-rerun | diverged | 125 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/shutdown-socketpair@liteinst` | legacy-rerun | diverged | 125 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/signal-delivery-sequence@liteinst` | legacy-rerun | diverged | 997 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/signal-waitstatus-identity@liteinst` | legacy-rerun | diverged | 386 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/signalfd-create@kvm` | legacy-rerun | diverged | 113 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/signalfd-create@liteinst` | legacy-rerun | diverged | 113 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/socket-epoll-ordering@kvm` | legacy-rerun | diverged | 250 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/socket-epoll-ordering@liteinst` | legacy-rerun | diverged | 250 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/socket-options@liteinst` | legacy-rerun | diverged | 131 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/socketpair-flags@liteinst` | legacy-rerun | diverged | 119 | 16 | `5ee668223a15` | `diverged` | nondeterministic |
| `c-programs/sockname-unnamed@liteinst` | legacy-rerun | diverged | 113 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/stat-metadata-identity@kvm` | legacy-rerun | diverged | 233 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/stat-metadata-identity@liteinst` | legacy-rerun | diverged | 233 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/statfs-free-determinism@kvm` | legacy-rerun | diverged | 109 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/statfs-free-determinism@liteinst` | legacy-rerun | diverged | 109 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/static-nolibc-syscall-sites@kvm` | legacy-rerun | diverged | 76 | 64 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/statx-metadata@kvm` | legacy-rerun | diverged | 123 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/statx-metadata@liteinst` | legacy-rerun | diverged | 131 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/symlink-ops@kvm` | legacy-rerun | diverged | 152 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/symlink-ops@liteinst` | legacy-rerun | diverged | 160 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/sync-file-range@kvm` | legacy-rerun | diverged | 122 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/sync-file-range@liteinst` | legacy-rerun | diverged | 130 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/sysv-ipc-refusal@kvm` | legacy-rerun | diverged | 105 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/sysv-ipc-refusal@liteinst` | legacy-rerun | diverged | 105 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/thp-disable@liteinst` | legacy-rerun | diverged | 107 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/umask-mode@kvm` | legacy-rerun | diverged | 139 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/umask-mode@liteinst` | legacy-rerun | diverged | 147 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/uname-identity@kvm` | legacy-rerun | diverged | 101 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/uname-identity@liteinst` | legacy-rerun | diverged | 101 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/utimensat-determinism@kvm` | legacy-rerun | diverged | 123 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/utimensat-determinism@liteinst` | legacy-rerun | diverged | 131 | 16 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/vectored-file-io@liteinst` | legacy-rerun | diverged | 147 | 16 | `5ee668223a15` | `diverged` | unavailable |
| `c-programs/vectored-io@kvm` | legacy-rerun | diverged | 126 | 13 | `5ee668223a15` | `measured-and-passed` | unavailable |
| `c-programs/vectored-io@liteinst` | legacy-rerun | diverged | 126 | 16 | `5ee668223a15` | `diverged` | unavailable |
