# Compatibility scorecard

Last regenerated **2026-10-07T12:26:21Z** from `https://github.com/rrnewton/hermit_test_ledger.git` commit `dc9470751e4c6c96855b586fda47e1e85ba614fd`, reading 470534 series row(s). Validate run published in this series snapshot, with its cell comparisons: `validate-hermit-lander-61b0870469e2-1791375810996253133-1330801-c37f2635` (910). Earlier validate runs still supplying comparisons: `validate-claude-coord-d1744e9fc07e-1791194186871628473-2231806-38cbc82c` current (1057), `validate-netreplay-rework-8b7b189183d5-1791196936091505003-1516693-ebe3ca6a` current (1056), `validate-buck-re-3-b84faa26c355-1791103535341810231-4116164-a06293f4` current (1050), `validate-buck-re-3-66db685c5279-1791099824835735230-1153730-3be6996b` current (1046), `validate-coord2-a3d7e201e091-1791197685492926552-2311392-b1cd4017` current (1046), `validate-coord2-d1744e9fc07e-1791193403382220055-1138567-499f31af` current (1045), `validate-claude-coord-mega-lander-4c0b7daa8ae6-1791155208411569193-894674-eab4fa86` current (1042), `validate-gate-select-f85de5d891ab-1791150197069282693-1112723-7a425489` current (1042), and 99 more.

This table is derived from the manifest, not from a separately maintained parent-workspace CSV. `./ci/compat-envelope/scorecard.rs check` verifies it.

The count table includes all **12592** cells in the manifest; no row is omitted. A cell is **Selected by full** exactly when it appears in `ci/expected-e2e-plan.json`. A cell is **Not selected by full** when it is in the manifest but absent from that plan. Selection is not a test result: a cell not selected by full may have passed, failed, produced no verdict, or never run. Of these cells, **1099** are selected by full, **698** are not selected by full, and **10795** are **Not applicable**.

Every selected `verify` cell that does not declare the stripped comparator, and every seed in a selected `chaos` cell, runs the same backend twice. The manifest runner adds `--verify-strict` when the selected Hermit binary supports it, and accepts a result only when the typed report says `verified=true`, `verdict=matched`, `bitwise_parity=true`, `strictness=canonical`, `compare_logs=true`, a named canonical `record_envelope`, and both INFO-message counts are nonzero. Bare `--verify` remains a Stripped comparison when invoked directly and does not satisfy this regression plan. **189** of the **1089** selected `verify` cells declare `comparator: stripped` (`compat` on `ptrace`: 189). They run Hermit's default `--verify` and pass only on a verified, matched report of a non-empty stripped comparison; they are below L2, never `bitwise_parity`, and are counted in these tables as selected, not as canonical. These same-backend results do not establish cross-backend parity.

| Backend | Selected by full | Not selected by full | Not applicable | In the manifest |
| --- | ---: | ---: | ---: | ---: |
| `ptrace` | 562 | 372 | 1427 | 2361 |
| `dbt` | 38 | 47 | 2276 | 2361 |
| `kvm` | 259 | 7 | 2095 | 2361 |
| `sabre` | 240 | 239 | 1882 | 2361 |
| `liteinst` | 0 | 0 | 2361 | 2361 |
| `native` | 0 | 33 | 754 | 787 |
| **Total** | **1099** | **698** | **10795** | **12592** |

## Denominator, and why the percentage is not comparable across changes to it

Selected by full is **1099 of 12592**, which is **8.73%** — over THIS population and no other. The population is every combination the manifest declares, and it is composed of:

- backends: `ptrace`, `dbt`, `kvm`, `sabre`, `liteinst`, `native`
- modes: `chaos`, `naked`, `replay`, `verify`

⚠️ **10795 of those 12592 cells are NOT APPLICABLE** — their backend is not applicable for their mode, so they were never asked to run and cannot pass or fail. Over the 1797 cells that CAN run, selected by full is **61.16%**.

⚠️ **DO NOT QUOTE THAT SECOND FIGURE AS PROGRESS.** It is the same 1099 cells selected by full measured against a smaller denominator. Nothing was fixed to produce it; it is what the first figure always meant once the cells that cannot run are excluded. Quote both or neither, and never compare one against the other as though something moved.

⚠️ **Adding or removing a backend or mode changes this denominator and therefore the percentage, without anything about the product changing.** Removing a backend whose cells are mostly not selected RAISES the reported figure; adding manifest cells that are not selected LOWERS it. Neither is progress. Before comparing this percentage against an earlier one, diff the two lists above: if they differ, the numbers are not comparable and the difference is not a result.

The mode view makes the current order of work explicit: expand `verify` first, then `replay`, then `chaos`. Each backend cell is `selected by full / in the manifest`; an em dash means that mode does not exist for that backend. The summary columns use the same selection and applicability facts as the table above.

| Mode | `ptrace` | `dbt` | `kvm` | `sabre` | `liteinst` | `native` | Selected by full | Not selected by full | Not applicable | In the manifest |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| `verify` | 552 / 787 | 38 / 787 | 259 / 787 | 240 / 787 | 0 / 787 | — | 1089 | 525 | 2321 | 3935 |
| `replay` | 4 / 787 | 0 / 787 | 0 / 787 | 0 / 787 | 0 / 787 | — | 4 | 139 | 3792 | 3935 |
| `chaos` | 6 / 787 | 0 / 787 | 0 / 787 | 0 / 787 | 0 / 787 | — | 6 | 1 | 3928 | 3935 |
| `naked` | — | — | — | — | — | 0 / 787 | 0 | 33 | 754 | 787 |
| **Total** | | | | | | | **1099** | **698** | **10795** | **12592** |

## Ptrace by manifest category

This view uses the same Basic Sanity Milestone 1 contracts as the tables above, but makes the ptrace workload mix visible. Each entry is `selected by full / in the manifest`; `custom` commands are not part of this denominator.

| Manifest category | Verify | Replay | Chaos | Selected by full | In the manifest |
| --- | ---: | ---: | ---: | ---: | ---: |
| `applications` | 3 / 6 | 0 / 6 | 0 / 6 | 3 | 18 |
| `bin-c` | 2 / 2 | 0 / 2 | 0 / 2 | 2 | 6 |
| `c-programs` | 274 / 279 | 3 / 279 | 3 / 279 | 280 | 837 |
| `chaos-c` | 1 / 1 | 0 / 1 | 1 / 1 | 2 | 3 |
| `compat` | 189 / 412 | 0 / 412 | 0 / 412 | 189 | 1236 |
| `data-handling` | 6 / 6 | 0 / 6 | 0 / 6 | 6 | 18 |
| `debugger-c` | 1 / 1 | 0 / 1 | 0 / 1 | 1 | 3 |
| `determinism-stress` | 5 / 6 | 0 / 6 | 1 / 6 | 6 | 18 |
| `determinism-stress-c` | 11 / 11 | 0 / 11 | 1 / 11 | 12 | 33 |
| `language-runtimes` | 19 / 19 | 0 / 19 | 0 / 19 | 19 | 57 |
| `shared-futex-c` | 1 / 4 | 0 / 4 | 0 / 4 | 1 | 12 |
| `system-utils` | 39 / 39 | 1 / 39 | 0 / 39 | 40 | 117 |
| `util-c` | 1 / 1 | 0 / 1 | 0 / 1 | 1 | 3 |

Ordinary full validation executes 1103 cells: the 1099 comparable compatibility cells selected by full above (including 6 chaos-mode race-exposure checks), and 4 explicit custom commands outside the comparable denominator. A passing validate must produce a fresh result for all of them; a failing selected cell is a regression, not permission to remove it from the plan.

### Selected custom commands outside the comparable denominator

These rows are part of the selected regression denominator even though they are not rows in `ci/compat-envelope/cells.json`. Their exact identities come from `ci/expected-e2e-plan.json`; `scorecard.rs check` refuses any selected row that is not accounted for by either this table or the comparable green cells above.

| Lane | Category | Test | Mode | Backend |
| --- | --- | --- | --- | --- |
| `portable` | `c-programs` | `c-programs/environment-and-workdir` | `custom` | `ptrace` |
| `portable` | `c-programs` | `c-programs/io-uring-fallback` | `custom` | `dbt` |
| `portable` | `c-programs` | `c-programs/io-uring-fallback` | `custom` | `ptrace` |
| `portable` | `system-utils` | `system-utils/clock-determinism` | `custom` | `ptrace` |

## Selection and measurement

Selection and observation answer different questions. The first column says whether full validation selects a cell. The per-cell `measurement` value says what retained evidence observed: `never-measured`, `measured-and-passed`, `measured-no-verdict`, `diverged-unlocated`, or `diverged`. Of the cells selected by full, **0** have `never-measured`; of the cells not selected by full, **338** have `measured-and-passed`.

Retained history that has not been imported is not counted here. A stored measurement does not establish that it describes current code; `show` reports whether the recorded last test still matches `HEAD:detcore`.

The count table includes all **12592** cells in the manifest; no row is omitted. These claims use the same counts printed in the table below.

| Selection by full | `never-measured` | `measured-and-passed` | `measured-no-verdict` | `diverged-unlocated` | `diverged` | In the manifest |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Selected by full | 0 | 1011 | 0 | 2 | 86 | 1099 |
| Not selected by full | 325 | 338 | 10 | 0 | 25 | 698 |
| Not applicable | 10314 | 242 | 119 | 0 | 120 | 10795 |
| **Total** | **10639** | **1591** | **129** | **2** | **231** | **12592** |

Cells whose stored `measurement` is not `never-measured` are shown individually so selection and measurement remain visible together.

| Test | Mode | Backend | Selection by full | Measurement |
| --- | --- | --- | --- | --- |
| `applications/c-toolchain-workflow` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `applications/c-toolchain-workflow` | `verify` | `ptrace` | `Selected by full` | `diverged` |
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
| `bin-c/posix-timer-test` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `bin-c/posix-timer-test` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `bin-c/posix-timer-test` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `bin-c/robust-futex-test` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `bin-c/robust-futex-test` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `bin-c/robust-futex-test` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/acct-refusal-probe` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/acct-refusal-probe` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/acct-refusal-probe` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/acct-refusal-probe` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/acct-refusal-probe` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/add-key-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/add-key-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/add-key-enosys` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/add-key-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/add-key-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/adjtimex-deterministic` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/adjtimex-deterministic` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/adjtimex-deterministic` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/adjtimex-deterministic` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/adjtimex-deterministic` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/aio-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/aio-refusal` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/aio-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/aio-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/append-pwrite` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/append-pwrite` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/append-pwrite` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/append-pwrite` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/arch-prctl-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/arch-prctl-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/arch-prctl-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/arch-prctl-determinism` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/bind-getsockname` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/bind-getsockname` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/bind-getsockname` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/bind-getsockname` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/bpf-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/bpf-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/bpf-enosys` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/bpf-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/bpf-enosys` | `verify` | `sabre` | `Selected by full` | `diverged` |
| `c-programs/cachestat-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/cachestat-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/cachestat-enosys` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/cachestat-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/cachestat-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/cachestat-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/cachestat-refusal` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/cachestat-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/cachestat-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/child-subreaper-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/child-subreaper-refusal` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/child-subreaper-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/child-subreaper-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/clock-adjtime-deterministic` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/clock-adjtime-deterministic` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/clock-adjtime-deterministic` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/clock-adjtime-deterministic` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/clock-adjtime-deterministic` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/clone` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/clone` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/clone` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/close-range-fds` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/close-range-fds` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/close-range-fds` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/close-range-fds` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/copy-file-range-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal-probe` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal-probe` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal-probe` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/copy-file-range-refusal-probe` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal-probe` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpu-virtualization` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpu-virtualization` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpu-virtualization` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/cpu-virtualization` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpu-virtualization` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/cwd-roundtrip` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/cwd-roundtrip` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/cwd-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/cwd-roundtrip` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-copied-tiocgpgrp` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `c-programs/dbt-copied-tiocgpgrp` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/dbt-copied-tiocgpgrp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-copied-tiocgpgrp` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-exec-failure` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-exec-failure` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/dbt-exec-failure` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-exec-failure` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-execveat-unsupported` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/dbt-execveat-unsupported` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-execveat-unsupported` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/dbt-execveat-unsupported` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-execveat-unsupported` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `c-programs/dbt-mmap-exec` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-mmap-exec` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-mmap-exec` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/dbt-mmap-exec` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-mmap-exec` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-pid-virtualization` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/dbt-pid-virtualization` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `c-programs/dbt-pid-virtualization` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `c-programs/dbt-prlimit-self` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-prlimit-self` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/dbt-prlimit-self` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-prlimit-self` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-self-sigqueue` | `verify` | `dbt` | `Not selected by full` | `measured-no-verdict` |
| `c-programs/dbt-self-sigqueue` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/dbt-wait-lifecycle` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/dbt-wait-lifecycle` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-wait-lifecycle` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/dup-shared-offset` | `verify` | `dbt` | `Selected by full` | `diverged` |
| `c-programs/dup-shared-offset` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/dup-shared-offset` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/dup-shared-offset` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dup-shared-offset` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/environment-and-workdir` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/environment-and-workdir` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/environment-and-workdir` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/epoll-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-determinism` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/epoll-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-determinism` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-pwait2` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-pwait2` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/epoll-pwait2` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-pwait2` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-readiness` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-readiness` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/epoll-readiness` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-readiness` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/event-delivery-ordering` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/event-delivery-ordering` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/event-delivery-ordering` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/eventfd-semantics` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/eventfd-semantics` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/eventfd-semantics` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/eventfd-semantics` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/faccessat2-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/faccessat2-flags` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/faccessat2-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/faccessat2-flags` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/fadvise-hints` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fadvise-hints` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/fadvise-hints` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fadvise-hints` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/fallocate-extents` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fallocate-extents` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/fallocate-extents` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fallocate-extents` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/fchmod-bits` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fchmod-bits` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/fchmod-bits` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fchmod-bits` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/fchmodat2-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fchmodat2-flags` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/fchmodat2-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fchmodat2-flags` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/fcntl-owner` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fcntl-owner` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/fcntl-owner` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fcntl-owner` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/fd-duplication` | `verify` | `dbt` | `Selected by full` | `diverged` |
| `c-programs/fd-duplication` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fd-duplication` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/fd-duplication` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fd-duplication` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/file-backed-mmap` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/file-backed-mmap` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/file-backed-mmap` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/file-backed-mmap` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/file-io-roundtrip` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/file-io-roundtrip` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/file-io-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/file-io-roundtrip` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/flock-lifecycle` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/flock-lifecycle` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/flock-lifecycle` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/flock-lifecycle` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/fork-exec-pipeline` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `c-programs/fork-exec-pipeline` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/fork-exec-pipeline` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fork-exec-pipeline` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `c-programs/fp-reduction-nondeterminism` | `chaos` | `ptrace` | `Selected by full` | `diverged` |
| `c-programs/fp-reduction-nondeterminism` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/fp-reduction-nondeterminism` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `c-programs/fp-reduction-nondeterminism` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/fsync-durability` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fsync-durability` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/fsync-durability` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fsync-durability` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/ftruncate-sparse` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/ftruncate-sparse` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/ftruncate-sparse` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ftruncate-sparse` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-requeue-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/futex-requeue-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-requeue-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/futex-requeue-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-requeue-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-waitv-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/futex-waitv-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-waitv-enosys` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/futex-waitv-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-waitv-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-wake-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/futex-wake-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-wake-enosys` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/futex-wake-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/futex-wake-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/get-robust-list-child` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `c-programs/get-robust-list-child` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/getcpu-identity` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/getcpu-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/getcpu-identity` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/getcpu-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/getcpu-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/getitimer-determinism-probe` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/getitimer-determinism-probe` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/getitimer-determinism-probe` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/getitimer-determinism-probe` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/getpriority-identity` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/getpriority-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/getpriority-identity` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/getpriority-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/getpriority-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/getrusage-self-accounting` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/getrusage-self-accounting` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/getrusage-self-accounting` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/getsockopt-null` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/getsockopt-null` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/getsockopt-null` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/getsockopt-null` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/hardware-trap-identity` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/hardware-trap-identity` | `verify` | `liteinst` | `Not applicable` | `diverged` |
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
| `c-programs/host-identity` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/host-identity` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/host-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/host-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/inline-syscall-sites` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/inline-syscall-sites` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/inline-syscall-sites` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/inline-syscall-sites` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/inotify-watch` | `verify` | `liteinst` | `Not applicable` | `diverged` |
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
| `c-programs/ioctl-fioclex` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/ioctl-fioclex` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ioctl-fioclex` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/ioctl-fionread` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/ioctl-fionread` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/ioctl-fionread` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ioctl-fionread` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/ioctl-siocethtool` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/ioctl-siocethtool` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/ioctl-siocethtool` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ioctl-siocethtool` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/ipc-determinism` | `chaos` | `ptrace` | `Selected by full` | `diverged` |
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
| `c-programs/kcmp-refusal` | `verify` | `liteinst` | `Not applicable` | `diverged` |
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
| `c-programs/linkat-flags` | `verify` | `liteinst` | `Not applicable` | `diverged` |
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
| `c-programs/lseek-positioning` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/lseek-positioning` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/lseek-positioning` | `verify` | `liteinst` | `Not applicable` | `diverged` |
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
| `c-programs/mce-kill-refusal` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/mce-kill-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mce-kill-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/membarrier-query` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/membarrier-query` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/membarrier-query` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/membarrier-query` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/memfd-create` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/memfd-create` | `verify` | `liteinst` | `Not applicable` | `diverged` |
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
| `c-programs/mempolicy-default` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/mempolicy-default` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mempolicy-default` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mincore-residency` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mincore-residency` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/mincore-residency` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mincore-residency` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mixed-inline-and-libc-syscalls` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mixed-inline-and-libc-syscalls` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/mixed-inline-and-libc-syscalls` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mixed-inline-and-libc-syscalls` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mkdir-rmdir` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mkdir-rmdir` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/mkdir-rmdir` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mkdir-rmdir` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mknod-special` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mknod-special` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/mknod-special` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mknod-special` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/mmap-layout-pointer-order` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/mmap-layout-pointer-order` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-layout-pointer-order` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-stress-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-stress-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/mmap-stress-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-stress-determinism` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/msync-writeback` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/msync-writeback` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/msync-writeback` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/msync-writeback` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-at-eopnotsupp` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-at-eopnotsupp` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/name-to-handle-at-eopnotsupp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-at-eopnotsupp` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-directory-eopnotsupp` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-directory-eopnotsupp` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/name-to-handle-directory-eopnotsupp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-directory-eopnotsupp` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-empty-path-eopnotsupp` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-empty-path-eopnotsupp` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/name-to-handle-empty-path-eopnotsupp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-empty-path-eopnotsupp` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-refusal` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/name-to-handle-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-regular-eopnotsupp` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/name-to-handle-regular-eopnotsupp` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/nanosleep-threads-simple` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/netlink-autobind-generic` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/netlink-autobind-generic` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/netlink-autobind-generic` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/netlink-autobind-generic` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/netlink-autobind-route` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/netlink-autobind-route` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/netlink-autobind-route` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/netlink-autobind-route` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/netlink-autobind-usersock` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/netlink-autobind-usersock` | `verify` | `liteinst` | `Not applicable` | `diverged` |
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
| `c-programs/no-new-privs-refusal` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/no-new-privs-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/no-new-privs-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/numa-node-identity` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/numa-node-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/numa-node-identity` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/numa-node-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/numa-node-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/o-tmpfile-anon` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/o-tmpfile-anon` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/o-tmpfile-anon` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/o-tmpfile-anon` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/openat-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/openat-flags` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/openat-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/openat-flags` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/openat2-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/openat2-refusal` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/openat2-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/openat2-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/path-file-ops` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/path-file-ops` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/path-file-ops` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/path-file-ops` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pause-alarm-interrupt` | `verify` | `liteinst` | `Not applicable` | `diverged` |
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
| `c-programs/personality-domain` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/personality-domain` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/personality-domain` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pid-probe` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/pid-probe` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pid-probe` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/pid-probe` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pid-probe` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-open-self` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-open-self` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/pidfd-open-self` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-open-self` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-open-self-pair` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-open-self-pair` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-open-self-pair` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/pipe-capacity` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/pipe-capacity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-capacity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-capacity-pin` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-capacity-pin` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/pipe-capacity-pin` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-capacity-pin` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-ipc` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `c-programs/pipe-ipc` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/pipe2-flags` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/pipe2-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe2-flags` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/poll-readiness` | `replay` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/poll-readiness` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/poll-readiness` | `verify` | `liteinst` | `Not applicable` | `diverged` |
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
| `c-programs/prctl-identity` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-identity` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/prctl-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-option-policy` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-option-policy` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/prctl-option-policy` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-option-policy` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-pdeathsig` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/prctl-pdeathsig` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-pdeathsig` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pread64-nostdlib` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pread64-nostdlib` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/pread64-nostdlib` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pread64-nostdlib` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/preadv2-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/preadv2-flags` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/preadv2-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/preadv2-flags` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/print-memaddrs` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/print-memaddrs` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/print-memaddrs` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/print-memaddrs` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/printf-with-threads` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/printf-with-threads` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/printf-with-threads` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `c-programs/proc-fd-link-aliases` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/pthread-lifecycle` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `c-programs/pthread-lifecycle` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/random-sources` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/random-sources-root-only` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/random-sources-root-only` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/random-sources-root-only` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/random-sources-root-only` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/rcx-canonicalization` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/rcx-canonicalization` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/rcx-canonicalization` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/rcx-canonicalization` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/readdir-entries` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/readdir-entries` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/readdir-entries` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/readdir-entries` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/readdir-order-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/readdir-order-identity` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/readdir-order-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/readdir-order-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/record-lock` | `verify` | `liteinst` | `Not applicable` | `diverged` |
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
| `c-programs/rename-ops` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/rename-ops` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/rename-ops` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/renameat2-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/renameat2-flags` | `verify` | `liteinst` | `Not applicable` | `diverged` |
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
| `c-programs/rlimit-identity` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/rlimit-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/rlimit-identity` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/rlimit-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/rlimit-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/robust-list` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/robust-list` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/robust-list` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/robust-list` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-getaffinity-identity` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-getaffinity-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-getaffinity-identity` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/seccomp-refusal` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/seccomp-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/seccomp-refusal` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sendfile-copy` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sendfile-copy` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sendfile-copy` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sendfile-copy` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/session-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/session-identity` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/session-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/session-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/set-tid-address` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/set-tid-address` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/set-tid-address` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/set-tid-address` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/setitimer-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/setitimer-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/setitimer-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/setitimer-determinism` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/short-io-split-identity` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `c-programs/short-io-split-identity` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/short-io-split-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/short-io-split-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/shutdown-socketpair` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/shutdown-socketpair` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/signal-delivery-sequence` | `verify` | `liteinst` | `Not applicable` | `diverged` |
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
| `c-programs/signal-waitstatus-identity` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/signal-waitstatus-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/signal-waitstatus-identity` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/signalfd-create` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/signalfd-create` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/sigtimedwait-no-timeout` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/sigtimedwait-timeout-0s` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigtimedwait-timeout-0s` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigtimedwait-timeout-0s` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `c-programs/sigtimedwait-timeout-1s` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigtimedwait-timeout-1s` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigtimedwait-timeout-1s` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/so-incoming-cpu-tcp4` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-tcp4` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/so-incoming-cpu-tcp4` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-tcp4` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-tcp6` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-tcp6` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-tcp6` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-tcp6` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-udp4` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-udp4` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/so-incoming-cpu-udp4` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/so-incoming-cpu-udp4` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-cookie-tcp` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-cookie-tcp` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/socket-cookie-tcp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-cookie-tcp` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-cookie-udp` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-cookie-udp` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/socket-cookie-udp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-cookie-udp` | `verify` | `sabre` | `Selected by full` | `diverged` |
| `c-programs/socket-cookie-unix` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-cookie-unix` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/socket-cookie-unix` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-cookie-unix` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-epoll-ordering` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-epoll-ordering` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/socket-epoll-ordering` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-epoll-ordering` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-ioctl-timestamp` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/socket-ioctl-timestamp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-ioctl-timestamp` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/socket-options` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-options` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/socket-options` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-options` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-timestamp-edge-cases` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/socket-timestamp-edge-cases` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-timestamp-edge-cases` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/socket-timestamp-timespec` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-timestamp-timespec` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/socket-timestamp-timespec` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-timestamp-timespec` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-timestamp-timeval` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-timestamp-timeval` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/socket-timestamp-timeval` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-timestamp-timeval` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/socketpair-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socketpair-flags` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/socketpair-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socketpair-flags` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sockname-unnamed` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sockname-unnamed` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/sockname-unnamed` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sockname-unnamed` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/splice-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/splice-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/splice-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/splice-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/splice-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/stat-metadata-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/stat-metadata-identity` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/stat-metadata-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/stat-metadata-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/statfs-free-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/statfs-free-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/statx-metadata` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/statx-metadata` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/statx-metadata` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/symlink-ops` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/symlink-ops` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/symlink-ops` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/symlink-ops` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sync-file-range` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sync-file-range` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/sysinfo-uptime` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/syslog-deterministic` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/syslog-deterministic` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/syslog-deterministic` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/syslog-deterministic` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/syslog-deterministic` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysv-ipc-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sysv-ipc-refusal` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/thp-disable` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/thp-disable` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/thp-disable` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/thread-self-procfs-handoff` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/thread-self-procfs-handoff` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/thread-self-procfs-handoff` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `c-programs/thread-sync-determinism` | `chaos` | `ptrace` | `Selected by full` | `diverged` |
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
| `c-programs/umask-mode` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/umask-mode` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/umask-mode` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/uname` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/uname` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/uname` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/uname` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/uname-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/uname-identity` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/uname-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/uname-identity` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-dgram` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-dgram` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/unix-autobind-dgram` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-dgram` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-seqpacket` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-seqpacket` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/unix-autobind-seqpacket` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-seqpacket` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-stream` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-stream` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/unix-autobind-stream` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/unix-autobind-stream` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/ustat-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/ustat-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/ustat-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/ustat-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ustat-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/utimensat-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/utimensat-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/utimensat-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/utimensat-determinism` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/vectored-file-io` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/vectored-file-io` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `c-programs/vectored-file-io` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/vectored-file-io` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/vectored-io` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/vectored-io` | `verify` | `liteinst` | `Not applicable` | `diverged` |
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
| `chaos-c/lock-granularity` | `chaos` | `ptrace` | `Selected by full` | `diverged` |
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
| `compat/bash` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/bash` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/bash` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/bc` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/bracket` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/bracket` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/bzip2` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/bzip2-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/bzip2-roundtrip` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
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
| `compat/clang` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `compat/cmake` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cmp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/col` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/colrm` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/column` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
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
| `compat/date` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
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
| `compat/git` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `compat/gprof` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/grep` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/groups` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/groups` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/gxx` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/gzip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/gzip-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/head` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/head` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/hexdump` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
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
| `compat/lsmod` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `compat/lsmod` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/lsof` | `verify` | `ptrace` | `Selected by full` | `diverged` |
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
| `compat/nm` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
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
| `compat/python3` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
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
| `compat/ruby` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/ruby` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `compat/rustc` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/sar-resource-tables` | `verify` | `ptrace` | `Selected by full` | `diverged` |
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
| `compat/sleep` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
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
| `compat/tcl` | `replay` | `ptrace` | `Not selected by full` | `measured-and-passed` |
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
| `compat/zip-unzip` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `compat/zstd` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/zstd-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `data-handling/archive-roundtrip` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `data-handling/archive-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `data-handling/archive-roundtrip` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `data-handling/dd-partial-transfers` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `data-handling/dd-partial-transfers` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `data-handling/dd-partial-transfers` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `data-handling/jq-json-transform` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `data-handling/jq-json-transform` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `data-handling/jq-json-transform` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `data-handling/jq-json-transform` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `data-handling/shell-pipeline` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `data-handling/shell-pipeline` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `data-handling/shell-pipeline` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `data-handling/sqlite-query-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `data-handling/sqlite-query-determinism` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `data-handling/sqlite-query-determinism` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `data-handling/zstd-multithread` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `data-handling/zstd-multithread` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `data-handling/zstd-multithread` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `debugger-c/debuggee` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `debugger-c/debuggee` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `debugger-c/debuggee` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `debugger-c/debuggee` | `verify` | `sabre` | `Selected by full` | `diverged` |
| `determinism-stress/example-race` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `determinism-stress/example-race` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress/example-race` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress/order-violation` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress/order-violation` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress/order-violation` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `determinism-stress/process-chains` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress/process-chains` | `verify` | `ptrace` | `Selected by full` | `diverged` |
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
| `determinism-stress/thread-output` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `determinism-stress-c/fork-tree` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/fork-tree` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/fork-tree` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `determinism-stress-c/lock-free` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/lock-free` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/lock-free` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `determinism-stress-c/mmap-fork-shared` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/mmap-fork-shared` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `determinism-stress-c/mmap-fork-shared` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `determinism-stress-c/pid-tid` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/pid-tid` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/pid-tid` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `determinism-stress-c/pid-tid-identity` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/pid-tid-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/pid-tid-identity` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
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
| `determinism-stress-c/producer-consumer` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `determinism-stress-c/signal-order` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/signal-order` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/signal-order` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
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
| `language-runtimes/bash-random` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/cpp-stl-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/cpp-stl-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/cpp-stl-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/cpp-stl-determinism` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/example-python-random` | `verify` | `dbt` | `Not selected by full` | `diverged` |
| `language-runtimes/example-python-random` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `language-runtimes/example-python-random` | `verify` | `liteinst` | `Not applicable` | `diverged` |
| `language-runtimes/example-python-random` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `language-runtimes/example-python-random` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `language-runtimes/gawk-random` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/gawk-random` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/gawk-random` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/gawk-random` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/lua-random` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/lua-random` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/lua-random` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `language-runtimes/m4-macro-mkstemp` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `language-runtimes/m4-macro-mkstemp` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/m4-macro-mkstemp` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `language-runtimes/m4-macro-mkstemp` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/node-v8-jit` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/node-v8-jit` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `language-runtimes/node-v8-jit` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/perl-hash-order` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/perl-hash-order` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/perl-hash-order` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `language-runtimes/perl-hash-order` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/perl-io-subprocess-time` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `language-runtimes/perl-io-subprocess-time` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/perl-io-subprocess-time` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `language-runtimes/perl-io-subprocess-time` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `language-runtimes/perl-random` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/perl-random` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/perl-random` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/perl-random` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/python-dict-hash-iteration` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `language-runtimes/python-dict-hash-iteration` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/python-dict-hash-iteration` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `language-runtimes/python-dict-hash-iteration` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/python-hash-determinism` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `language-runtimes/python-hash-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/python-hash-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/python-hash-determinism` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
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
| `language-runtimes/python-random` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/ruby-random` | `verify` | `kvm` | `Not selected by full` | `measured-and-passed` |
| `language-runtimes/ruby-random` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/ruby-random` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/ruby-random` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `language-runtimes/rust-hashmap-iteration` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/rust-hashmap-iteration` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/rust-hashmap-iteration` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/rust-hashmap-iteration` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/tcl-rand-clock` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `language-runtimes/tcl-rand-clock` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/tcl-rand-clock` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `language-runtimes/tcl-rand-clock` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
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
| `system-utils/clock-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
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
| `system-utils/example-date` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
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
| `system-utils/harness-width-contract` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/mcookie-random` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/mcookie-random` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/mcookie-random` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/mcookie-random` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/mktemp-name` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/mktemp-name` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/mktemp-name` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/mktemp-name` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/nscd-neutralised` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/nscd-neutralised` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/nscd-neutralised` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `system-utils/openssl-enc` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `system-utils/openssl-enc` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/openssl-enc` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `system-utils/openssl-enc` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/openssl-genpkey` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/openssl-genpkey` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/openssl-genpkey` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `system-utils/openssl-genpkey` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/openssl-passwd` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/openssl-passwd` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/openssl-passwd` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `system-utils/openssl-passwd` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/openssl-rand` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/openssl-rand` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/openssl-rand` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `system-utils/openssl-rand` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/openssl-x509` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `system-utils/openssl-x509` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/openssl-x509` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `system-utils/openssl-x509` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/overflow-gid-resolves` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `system-utils/overflow-gid-resolves` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/overflow-gid-resolves` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `system-utils/printf-argument-forwarding` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `system-utils/printf-argument-forwarding` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/printf-argument-forwarding` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/printf-argument-forwarding` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `system-utils/proc-random-uuid` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/proc-random-uuid` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/proc-random-uuid` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/proc-uptime` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/proc-uptime` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/proc-uptime` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/proc-uptime` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/procfs-sanitized-paths` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `system-utils/ps-proc-table` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `system-utils/ps-proc-table` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `system-utils/ps-proc-table` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/random-device` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/random-device` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/random-device` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/random-device` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/record-getpid` | `replay` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/record-getpid` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/record-getpid` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
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
| `system-utils/shuf-permutation` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
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
| `system-utils/uuidgen-random` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `util-c/pmu-skid` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `util-c/pmu-skid` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `util-c/pmu-skid` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `applications/kvm-python-examples` | `verify` | `kvm` | `Not selected by full` | `diverged` |
| `applications/kvm-python-examples` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `applications/kvm-python-examples` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `applications/kvm-shell-environment` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `applications/kvm-shell-environment` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `applications/kvm-shell-environment` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/cpuid-probe` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpuid-probe` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpuid-probe` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/cpuid-probe` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpuid-probe` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/sysfs-sanitized-prefixes` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/sysfs-sanitized-prefixes` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/sysfs-sanitized-prefixes` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/sysfs-sanitized-prefixes` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |

## Parity (measured after determinism)

Cross-backend parity compares a candidate backend's retained `verify` log with the ptrace reference's. It is measured after determinism, from the retained logs, by the parity post-pass, and it is reported here beside determinism, never folded into it: no count, measurement or colour above depends on it. The `c-programs` nodes, which perform ordinary same-backend verification, supply those logs; no validation runs a ptrace rerun any more (https://github.com/rrnewton/hermit/issues/3301).

A measured cell earns credit in [0, 1]: its matched prefix of compared records over the longer log, and 1 only for a full match. **Mean credit** divides the credit sum by the measured (matched plus diverged) cells, so a measured cell without credit counts as 0. **Floor credit** divides it by every selected cell except two kinds that could not be compared: a **no golden** cell, where an operand's own outcome (a determinism mismatch, a timeout, a crash and the like) left no deterministic golden log, and a **not compared** cell, whose backend cannot be given the reference's inputs. So an **unmeasured** cell (a golden log could exist, but the harness or the parity tool made no comparison), a record-missing cell and a refused cell each count as 0. No cell of a backend whose inputs cannot be equalized enters any mean or floor, and a mean or floor over no cells reads n/a, never 0.000. **Selected** reads `W of C` when the run's own Hermit commit's `ci/compat-envelope/parity-cells.json` is known: the run reported W of the C cells that selection owes, and a run that reported fewer is marked partial. Mean credit pools clean credit (inputs equalized) with unequalized credit only under a marker that says so; **Credit inputs** shows which it is. The `legacy-rerun` history at the end is the retired ptrace rerun's last verdicts; it is not current parity and enters no count here.


### validate run `validate-claude-coord-03bcace44e35-1791375448255992477-1931528-221b8d12` at `03bcace44e35`

`parity: 90/198 matched; selected 198 of 198 committed; mean 0.472 over 195 measured; floor 0.465 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 193 of 195 credited: mean 0.477 over 193 with equal inputs; mean 0.028 over 2 with unequal inputs]`

| Candidate backend | Selected | Measured | Matched | Diverged | No golden | Not compared | Unmeasured | Record-missing | Refused | Mean credit (measured) | Floor credit | Credit inputs |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | --- |
| `dbt` | 16 of 16 | 14 | 0 | 14 | 0 | 0 | 2 | 0 | 0 | 0.030 | 0.027 | equalized for 13 of 14 (mean 0.030 equal; 0.040 unequal) |
| `kvm` | 91 of 91 | 91 | 90 | 1 | 0 | 0 | 0 | 0 | 0 | 0.994 | 0.994 | equalized |
| `sabre` | 91 of 91 | 90 | 0 | 90 | 0 | 0 | 1 | 0 | 0 | 0.014 | 0.014 | equalized for 89 of 90 (mean 0.014 equal; 0.016 unequal) |
| **TOTAL** | 198 of 198 | 195 | 90 | 105 | 0 | 0 | 3 | 0 | 0 | 0.472 | 0.465 | equalized for 193 of 195 (mean 0.477 equal; 0.028 unequal) |

Cells that were not measured, by class: a no-golden cell is outside the mean and the floor, and an unmeasured cell counts 0 in the floor.

| Class | Group | `dbt` | `kvm` | `sabre` | **TOTAL** |
| --- | --- | ---: | ---: | ---: | ---: |
| `no-result-row` | unmeasured | 2 | 0 | 1 | 3 |

Most common first divergence: 90 of 105 diverged cell(s) at record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` (for example `c-programs/aio-refusal@sabre`).

Every cell that did not match, with its first divergence or the reason it was not measured:

| Cell | Verdict | Credit | First divergence or reason |
| --- | --- | ---: | --- |
| `c-programs/aio-refusal@sabre` | diverged | 0.017 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/append-pwrite@sabre` | diverged | 0.013 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/bind-getsockname@sabre` | diverged | 0.016 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/cachestat-refusal@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/child-subreaper-refusal@sabre` | diverged | 0.017 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/copy-file-range-refusal@sabre` | diverged | 0.013 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/cpu-virtualization@dbt` | diverged | 0.034 | record 5, syscall ?: token 2: `detcore::scheduler:` vs `detcore::random:` |
| `c-programs/cpu-virtualization@sabre` | diverged | 0.017 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/cpuid-probe@dbt` | diverged | 0.040 (unequalized) | record 5, syscall ?: token 2: `detcore::scheduler:` vs `detcore::random:` |
| `c-programs/cpuid-probe@kvm` | diverged | 0.416 | record 43, syscall 13: token 19: `Ok(140737351716864)` vs `Ok(140737349943296)` |
| `c-programs/cwd-roundtrip@sabre` | diverged | 0.014 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/dup-shared-offset@dbt` | diverged | 0.025 | record 5, syscall ?: token 2: `detcore::scheduler:` vs `detcore::random:` |
| `c-programs/dup-shared-offset@sabre` | diverged | 0.013 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/epoll-pwait2@sabre` | diverged | 0.014 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/epoll-readiness@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/event-delivery-ordering@sabre` | diverged | 0.010 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/eventfd-semantics@sabre` | diverged | 0.011 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/faccessat2-flags@sabre` | diverged | 0.014 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/fadvise-hints@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/fallocate-extents@sabre` | diverged | 0.014 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/fchmod-bits@sabre` | diverged | 0.014 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/fchmodat2-flags@sabre` | diverged | 0.014 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/fcntl-owner@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/fd-duplication@dbt` | diverged | 0.023 | record 5, syscall ?: token 2: `detcore::scheduler:` vs `detcore::random:` |
| `c-programs/fd-duplication@sabre` | diverged | 0.011 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/file-backed-mmap@sabre` | diverged | 0.014 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/file-io-roundtrip@sabre` | diverged | 0.012 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/flock-lifecycle@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/fsync-durability@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/ftruncate-sparse@sabre` | diverged | 0.014 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/getcpu-identity@dbt` | diverged | 0.029 | record 5, syscall ?: token 2: `detcore::scheduler:` vs `detcore::random:` |
| `c-programs/getcpu-identity@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/getpriority-identity@dbt` | diverged | 0.031 | record 5, syscall ?: token 2: `detcore::scheduler:` vs `detcore::random:` |
| `c-programs/getpriority-identity@sabre` | diverged | 0.016 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/hardware-trap-identity@dbt` | candidate-missing[no-result-row] | — | the dbt candidate verify cell of c-programs/hardware-trap-identity has no result row in this run |
| `c-programs/host-identity@dbt` | diverged | 0.031 | record 6, syscall ?: token 2: `detcore::scheduler:` vs `detcore::random:` |
| `c-programs/host-identity@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/inline-syscall-sites@sabre` | diverged | 0.011 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/inotify-watch@sabre` | diverged | 0.016 (unequalized) | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/ioctl-fionread@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/kcmp-refusal@sabre` | diverged | 0.016 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/linkat-flags@sabre` | diverged | 0.013 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/lseek-positioning@dbt` | diverged | 0.026 | record 5, syscall ?: token 2: `detcore::scheduler:` vs `detcore::random:` |
| `c-programs/lseek-positioning@sabre` | diverged | 0.013 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/mce-kill-refusal@sabre` | diverged | 0.017 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/membarrier-query@sabre` | diverged | 0.017 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/memfd-create@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/mempolicy-default@sabre` | diverged | 0.017 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/mincore-residency@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/mixed-inline-and-libc-syscalls@sabre` | diverged | 0.012 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/mkdir-rmdir@sabre` | diverged | 0.014 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/mknod-special@sabre` | diverged | 0.014 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/mmap-layout-pointer-order@sabre` | diverged | 0.014 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/msync-writeback@sabre` | diverged | 0.014 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/name-to-handle-refusal@sabre` | diverged | 0.017 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/no-new-privs-refusal@sabre` | diverged | 0.017 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/numa-node-identity@dbt` | diverged | 0.032 | record 5, syscall ?: token 2: `detcore::scheduler:` vs `detcore::random:` |
| `c-programs/numa-node-identity@sabre` | diverged | 0.016 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/o-tmpfile-anon@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/openat-flags@sabre` | diverged | 0.012 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/openat2-refusal@sabre` | diverged | 0.017 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/path-file-ops@sabre` | diverged | 0.013 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/personality-domain@sabre` | diverged | 0.016 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/pid-probe@dbt` | diverged | 0.035 | record 5, syscall ?: token 2: `detcore::scheduler:` vs `detcore::random:` |
| `c-programs/pid-probe@sabre` | diverged | 0.017 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/pidfd-open-self-pair@dbt` | diverged | 0.031 | record 5, syscall ?: token 2: `detcore::scheduler:` vs `detcore::random:` |
| `c-programs/pidfd-open-self-pair@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/pipe-capacity@sabre` | diverged | 0.016 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/pipe-capacity-pin@sabre` | diverged | 0.016 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/pipe2-flags@sabre` | diverged | 0.011 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/poll-readiness@sabre` | diverged | 0.013 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/prctl-identity@dbt` | diverged | 0.030 | record 5, syscall ?: token 2: `detcore::scheduler:` vs `detcore::random:` |
| `c-programs/prctl-identity@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/prctl-pdeathsig@sabre` | diverged | 0.016 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/preadv2-flags@sabre` | diverged | 0.014 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/pthread-lifecycle@sabre` | candidate-missing[no-result-row] | — | the sabre candidate verify cell of c-programs/pthread-lifecycle has no result row in this run |
| `c-programs/readdir-entries@sabre` | diverged | 0.012 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/record-lock@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/rename-ops@sabre` | diverged | 0.011 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/renameat2-flags@sabre` | diverged | 0.010 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/rlimit-identity@dbt` | diverged | 0.030 | record 5, syscall ?: token 2: `detcore::scheduler:` vs `detcore::random:` |
| `c-programs/rlimit-identity@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/robust-list@sabre` | diverged | 0.017 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/sched-getaffinity-identity@dbt` | diverged | 0.032 | record 5, syscall ?: token 2: `detcore::scheduler:` vs `detcore::random:` |
| `c-programs/sched-getaffinity-identity@sabre` | diverged | 0.016 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/seccomp-refusal@sabre` | diverged | 0.017 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/sendfile-copy@sabre` | diverged | 0.012 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/set-tid-address@sabre` | diverged | 0.017 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/short-io-split-identity@sabre` | diverged | 0.005 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/shutdown-socketpair@sabre` | diverged | 0.014 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/signal-waitstatus-identity@dbt` | candidate-missing[no-result-row] | — | the dbt candidate verify cell of c-programs/signal-waitstatus-identity has no result row in this run |
| `c-programs/signalfd-create@sabre` | diverged | 0.016 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/socket-epoll-ordering@sabre` | diverged | 0.008 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/socket-options@sabre` | diverged | 0.014 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/socketpair-flags@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/sockname-unnamed@sabre` | diverged | 0.016 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/stat-metadata-identity@sabre` | diverged | 0.008 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/statfs-free-determinism@sabre` | diverged | 0.016 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/statx-metadata@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/symlink-ops@sabre` | diverged | 0.012 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/sync-file-range@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/sysv-ipc-refusal@sabre` | diverged | 0.017 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/thp-disable@sabre` | diverged | 0.017 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/umask-mode@sabre` | diverged | 0.013 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/uname-identity@sabre` | diverged | 0.017 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/utimensat-determinism@sabre` | diverged | 0.015 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/vectored-file-io@sabre` | diverged | 0.013 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |
| `c-programs/vectored-io@sabre` | diverged | 0.014 | record 3, syscall ?: token 2: `detcore::tool_local:` vs `detcore::scheduler:` |

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

Outside the clean headline: 4 parity rows from a dirty source tree.

Outside the clean headline: 0 parity rows that did not report their source tree state.

### 224 other parity run(s) in the store

Only a run from a clean source tree can be its producer's headline: at least one of its rows says `"source_tree_dirty": false`, and none says `true` or leaves the value out. A row refused for its own defect does not count; one refused only because its run's rows name more than one Hermit commit does. Among those runs, the headline is the run that reported every cell its own Hermit commit's selection owes; a partial run headlines only when no complete run exists, the most complete first. Then the deepest Hermit commit this checkout can place, then the latest emission.

- validate run `validate-buck-re-3-1931c6ccd8f7-1791054320749237313-288493-c38549d2` at Hermit `1931c6ccd8f7`: `parity: 0/205 matched; selected 205 of 205 committed; mean n/a over 0 measured; floor 0.000 over 205 of 205 selected (counted as 0: 205 record-missing; excluded: 0 no golden; 0 not compared)`
- validate run `validate-buck-re-3-5301e9a2dbb2-1791051975076983128-689340-69dc34f7` at Hermit `5301e9a2dbb2`: `parity: 0/205 matched; selected 205 of 205 committed; mean n/a over 0 measured; floor 0.000 over 205 of 205 selected (counted as 0: 205 record-missing; excluded: 0 no golden; 0 not compared)`
- validate run `validate-buck-re-3-66db685c5279-1791099824835735230-1153730-3be6996b` at Hermit `66db685c5279`: `parity: 0/297 matched; selected 297 of 297 committed; mean n/a over 0 measured; floor 0.000 over 297 of 297 selected (counted as 0: 297 record-missing; excluded: 0 no golden; 0 not compared)`
- validate run `validate-buck-re-3-75bec89e7db9-1791060150360793710-2018425-d3821d9a` at Hermit `75bec89e7db9`: `parity: 0/206 matched; selected 206 of 206 committed; mean n/a over 0 measured; floor 0.000 over 206 of 206 selected (counted as 0: 206 record-missing; excluded: 0 no golden; 0 not compared)`
- validate run `validate-buck-re-3-7831ad1c8931-1791056799838919343-1002648-6b3c5041` at Hermit `7831ad1c8931`: `parity: 0/205 matched; selected 205 of 205 committed; mean 0.055 over 186 measured; floor 0.055 over 189 of 205 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-buck-re-3-84821e53edb7-1791041721981051099-2446052-bf662192` at Hermit `84821e53edb7`: `parity: 0/204 matched; selected 204 of 204 committed; mean n/a over 0 measured; floor 0.000 over 204 of 204 selected (counted as 0: 204 record-missing; excluded: 0 no golden; 0 not compared)`
- validate run `validate-buck-re-3-b84faa26c355-1791103535341810231-4116164-a06293f4` at Hermit `b84faa26c355`: `parity: 0/297 matched; selected 297 of 297 committed; mean n/a over 0 measured; floor 0.000 over 297 of 297 selected (counted as 0: 297 record-missing; excluded: 0 no golden; 0 not compared)`
- validate run `validate-buck-re-3-ccccde5f65d6-1791044441627223683-4139778-3d0a278f` at Hermit `ccccde5f65d6`: `parity: 0/204 matched; selected 204 of 204 committed; mean n/a over 0 measured; floor 0.000 over 204 of 204 selected (counted as 0: 204 record-missing; excluded: 0 no golden; 0 not compared)`
- validate run `validate-buck-re-3-cf2a94ea38d3-1791063611629487524-833061-4b45d912` at Hermit `cf2a94ea38d3`: `parity: 0/206 matched; selected 206 of 206 committed; mean n/a over 0 measured; floor 0.000 over 206 of 206 selected (counted as 0: 206 record-missing; excluded: 0 no golden; 0 not compared)`
- validate run `validate-buck-re-3-e81ace1c0be7-1791057684904978741-1891131-b31df65f` at Hermit `e81ace1c0be7`: `parity: 0/205 matched; selected 205 of 205 committed; mean n/a over 0 measured; floor 0.000 over 205 of 205 selected (counted as 0: 205 record-missing; excluded: 0 no golden; 0 not compared)`
- validate run `validate-claude-2-118f0bfc4553-1791028466715093346-2549334-529fe13e` at Hermit `118f0bfc4553`: `parity: 0/203 matched; committed selection unknown; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-2-4346217e08d4-1791039676390384316-4107946-e2cf462e` at Hermit `4346217e08d4`: `parity: 0/204 matched; selected 204 of 204 committed; mean 0.055 over 185 measured; floor 0.054 over 188 of 204 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-2-6b5424c1860a-1791037904455844226-3351413-5eebd499` at Hermit `6b5424c1860a`: `parity: 0/204 matched; selected 204 of 204 committed; mean 0.055 over 185 measured; floor 0.054 over 188 of 204 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-2-b6ca81b78be8-1791031592862600986-362222-8863246d` at Hermit `b6ca81b78be8`: `parity: 0/203 matched; selected 203 of 203 committed; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-2-cfb9d0c86427-1791035312332954244-2326456-5ddbba2a` at Hermit `cfb9d0c86427`: `parity: 0/203 matched; committed selection unknown; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-4-ad4f9cf598a3-1791030814588067897-4023945-e9bb1627` at Hermit `ad4f9cf598a3`: `parity: 0/203 matched; committed selection unknown; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-4-d53ea6ca1711-1791032694966120621-1230314-61570ac3` at Hermit `d53ea6ca1711`: `parity: 0/203 matched; committed selection unknown; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-coord-04a34b2d10a7-1791358802793721810-158787-47efca10` at Hermit `04a34b2d10a7`: `parity: 87/198 matched; selected 198 of 198 committed; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-04b389291e0d-1791260959647858637-576130-9fe488be` at Hermit `04b389291e0d`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-0a2e0c248258-1791256374756074866-436968-5ad00e9e` at Hermit `0a2e0c248258`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.057 over 182 measured; floor 0.052 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.057 over 180 with equal inputs; mean 0.037 over 2 with unequal inputs]`
- validate run `validate-claude-coord-0bc1ae1691ad-1791336960266428418-698492-d263bf73` at Hermit `0bc1ae1691ad`: `parity: 85/198 matched; selected 198 of 198 committed; mean 0.462 over 195 measured; floor 0.455 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.464 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-140915362327-1791275358162526279-4176730-e9332c2f` at Hermit `140915362327`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-14b354b3b575-1791286050919798525-3332280-ad53696f` at Hermit `14b354b3b575`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-1597e85bd103-1791287362336447961-2952664-d2442ef5` at Hermit `1597e85bd103`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-16f58782e3ff-1791370762819270901-1228730-52f2dc9c` at Hermit `16f58782e3ff`: `parity: 90/198 matched; selected 198 of 198 committed; mean 0.472 over 195 measured; floor 0.465 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.475 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-172396362f8d-1791280868145651672-3308505-4d10eec9` at Hermit `172396362f8d`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-1f1b6e8b126d-1791231632802965745-1072482-08f0e10e` at Hermit `1f1b6e8b126d`: `parity: 0/198 matched; committed selection unknown; mean 0.054 over 181 measured; floor 0.050 over 197 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 1 no golden: ended 1; 0 not compared) [inputs equalized for 91 of 181 credited: mean 0.016 over 91 with equal inputs; mean 0.094 over 90 with unequal inputs]`
- validate run `validate-claude-coord-1f285b80cbf1-1791308071432928095-1522408-59720006` at Hermit `1f285b80cbf1`: `parity: 1/198 matched; selected 198 of 198 committed; mean 0.065 over 195 measured; floor 0.064 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.065 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-2a43eb81acf6-1791366692674881018-635384-a5df6dd9` at Hermit `2a43eb81acf6`: `parity: 90/198 matched; selected 198 of 198 committed; mean 0.472 over 195 measured; floor 0.465 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.475 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-2bfc43166083-1791242556478430751-3723596-4a48eaa8` at Hermit `2bfc43166083`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.057 over 182 measured; floor 0.052 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.057 over 180 with equal inputs; mean 0.037 over 2 with unequal inputs]`
- validate run `validate-claude-coord-2e6427ad27fa-1791213722643207383-2116557-053fcfe7` at Hermit `2e6427ad27fa`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.056 over 180 measured; floor 0.055 over 182 of 198 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 0 no golden; 16 not compared) [inputs equalized for 91 of 180 credited: mean 0.016 over 91 with equal inputs; mean 0.097 over 89 with unequal inputs]`
- validate run `validate-claude-coord-317a3fd03bb9-1791205881096493745-911901-2bf15639` at Hermit `317a3fd03bb9`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.049 over 180 measured; floor 0.048 over 182 of 198 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 0 no golden; 16 not compared) [inputs equalized for 91 of 180 credited: mean 0.016 over 91 with equal inputs; mean 0.083 over 89 with unequal inputs]`
- validate run `validate-claude-coord-3589b3edff56-1791203838387883418-1747108-ebb12384` at Hermit `3589b3edff56`: `parity: 0/198 matched; selected 198 of 198 committed; mean n/a over 0 measured; floor 0.000 over 182 of 198 selected (counted as 0: 182 unmeasured: no-result-row 2, epoch-not-shared 180; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-coord-3b9776f0af50-1791236416072731756-865179-d9da8f5d` at Hermit `3b9776f0af50`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.056 over 180 measured; floor 0.055 over 182 of 198 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 0 no golden; 16 not compared) [inputs equalized for 91 of 180 credited: mean 0.016 over 91 with equal inputs; mean 0.097 over 89 with unequal inputs]`
- validate run `validate-claude-coord-3db677887c3a-1791239278135395123-1225938-fc27c9d1` at Hermit `3db677887c3a`: `parity: 0/198 matched; committed selection unknown; mean 0.057 over 182 measured; floor 0.052 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.057 over 180 with equal inputs; mean 0.037 over 2 with unequal inputs]`
- validate run `validate-claude-coord-3ed06ee8ad55-1791349543766690289-3334255-c02d47a5` at Hermit `3ed06ee8ad55`: `parity: 86/198 matched; selected 198 of 198 committed; mean 0.466 over 195 measured; floor 0.459 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.468 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-44349791d536-1791279772530548076-3809975-318f8174` at Hermit `44349791d536`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-4a93c7e1a7e8-1791282958274858275-2460570-316f556e` at Hermit `4a93c7e1a7e8`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-4e8b0c8348e8-1791216234656993707-648891-43e3c9ea` at Hermit `4e8b0c8348e8`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 180 measured; floor 0.054 over 182 of 198 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 0 no golden; 16 not compared) [inputs equalized for 91 of 180 credited: mean 0.016 over 91 with equal inputs; mean 0.095 over 89 with unequal inputs]`
- validate run `validate-claude-coord-5027d1ecb833-1791218508494716430-215676-a7ab65d1` at Hermit `5027d1ecb833`: `parity: 0/198 matched; committed selection unknown; mean 0.055 over 180 measured; floor 0.055 over 182 of 198 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 0 no golden; 16 not compared) [inputs equalized for 91 of 180 credited: mean 0.016 over 91 with equal inputs; mean 0.096 over 89 with unequal inputs]`
- validate run `validate-claude-coord-53e6393eb4a6-1791305651727982004-525915-7564ab62` at Hermit `53e6393eb4a6`: `parity: 1/198 matched; selected 198 of 198 committed; mean 0.065 over 195 measured; floor 0.064 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.065 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-53fcf05b3040-1791299910449246459-3421442-82cd8eee` at Hermit `53fcf05b3040`: `parity: 1/198 matched; selected 198 of 198 committed; mean 0.065 over 195 measured; floor 0.064 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.065 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-57b0de7ae313-1791350391044324955-1101029-25674a38` at Hermit `57b0de7ae313`: `parity: 87/198 matched; selected 198 of 198 committed; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-5d6478725518-1791365251404899522-2080941-577645b0` at Hermit `5d6478725518`: `parity: 87/198 matched; selected 198 of 198 committed; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-5f55ab9700df-1791361364791543866-2998697-e2dd8aec` at Hermit `5f55ab9700df`: `parity: 90/198 matched; committed selection unknown; mean 0.472 over 195 measured; floor 0.465 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.475 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-661ab1bf9d7f-1791310178065128260-2040869-7947e11f` at Hermit `661ab1bf9d7f`: `parity: 1/198 matched; selected 198 of 198 committed; mean 0.065 over 195 measured; floor 0.064 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 193 of 195 credited: mean 0.066 over 193 with equal inputs; mean 0.028 over 2 with unequal inputs]`
- validate run `validate-claude-coord-6a029e95326e-1791301842779984343-125632-dfb2fa0e` at Hermit `6a029e95326e`: `parity: 1/198 matched; selected 198 of 198 committed; mean 0.065 over 195 measured; floor 0.064 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.065 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-8026bb4941a2-1791268832723581402-2541104-11de8f68` at Hermit `8026bb4941a2`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-845c3a42681d-1791368413218863571-274685-f49a752d` at Hermit `845c3a42681d`: `parity: 90/198 matched; selected 198 of 198 committed; mean 0.472 over 195 measured; floor 0.465 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.475 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-8c070294ccab-1791340120789881771-968348-64e89602` at Hermit `8c070294ccab`: `parity: 86/198 matched; selected 198 of 198 committed; mean 0.466 over 195 measured; floor 0.459 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.468 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-8db41c6017d5-1791285025149611846-412384-5505f3a7` at Hermit `8db41c6017d5`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-920ac3d15302-1791200968413850598-605244-9149536b` at Hermit `920ac3d15302`: `parity: 0/198 matched; selected 198 of 198 committed; mean n/a over 0 measured; floor 0.000 over 182 of 198 selected (counted as 0: 182 unmeasured: no-result-row 2, epoch-not-shared 180; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-coord-95ab5d77f29f-1791373120701638439-188181-4d7b2071` at Hermit `95ab5d77f29f`: `parity: 90/198 matched; selected 198 of 198 committed; mean 0.472 over 195 measured; floor 0.465 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.475 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-9b495ceb3f9c-1791293903209854871-2908375-6f97e53f` at Hermit `9b495ceb3f9c`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-9ef51175234b-1791267355339590586-3149836-389c0be1` at Hermit `9ef51175234b`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-a25b3fcd3805-1791272687372415424-1525843-8acbb49e` at Hermit `a25b3fcd3805`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-a2ed81aa17e1-1791356102721314066-1019516-ed5bb8a0` at Hermit `a2ed81aa17e1`: `parity: 87/198 matched; committed selection unknown; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-a3862b22d7b7-1791198723653873307-3702195-3af6e824` at Hermit `a3862b22d7b7`: `parity: 0/198 matched; selected 198 of 198 committed; mean n/a over 0 measured; floor 0.000 over 182 of 198 selected (counted as 0: 182 unmeasured: no-result-row 2, log-diff-failed 180; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-coord-a9c72e9995d7-1791248877111356256-3751051-adb4667c` at Hermit `a9c72e9995d7`: `parity: 0/198 matched; committed selection unknown; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-afd1f1d5d227-1791178671086851930-991513-70fd1bb2` at Hermit `afd1f1d5d227`: `parity: 0/198 matched; selected 198 of 198 committed; mean n/a over 0 measured; floor 0.000 over 182 of 198 selected (counted as 0: 182 unmeasured: no-result-row 2, epoch-not-shared 180; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-coord-bf8e8a83eae8-1791353867149233991-602289-12731a57` at Hermit `bf8e8a83eae8`: `parity: 87/198 matched; committed selection unknown; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-c4e3b243d2a0-1791324596813967914-2065498-5626860f` at Hermit `c4e3b243d2a0`: `parity: 85/198 matched; selected 198 of 198 committed; mean 0.459 over 195 measured; floor 0.452 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.461 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-c8168551d2bc-1791262504004550875-1461747-ad6c016a` at Hermit `c8168551d2bc`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-cc4832bd50fb-1791347697660167927-2445609-015087c9` at Hermit `cc4832bd50fb`: `parity: 86/198 matched; committed selection unknown; mean 0.466 over 195 measured; floor 0.459 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.468 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-cf0ed8872283-1791330588782596218-2996488-696d3823` at Hermit `cf0ed8872283`: `parity: 85/198 matched; committed selection unknown; mean 0.462 over 195 measured; floor 0.455 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.464 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-cfc69b1d9287-1791327622056290451-785505-84f850aa` at Hermit `cfc69b1d9287`: `parity: 85/198 matched; committed selection unknown; mean 0.459 over 195 measured; floor 0.452 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.461 over 194 with equal inputs; mean 0.039 over 1 with unequal inputs]`
- validate run `validate-claude-coord-d1241596d685-1791208137540279750-506824-202bc69f` at Hermit `d1241596d685`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.056 over 180 measured; floor 0.055 over 182 of 198 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 0 no golden; 16 not compared) [inputs equalized for 91 of 180 credited: mean 0.016 over 91 with equal inputs; mean 0.097 over 89 with unequal inputs]`
- validate run `validate-claude-coord-d151bde6aeb7-1791311346239994717-101125-64972237` at Hermit `d151bde6aeb7`: `parity: 1/198 matched; selected 198 of 198 committed; mean 0.065 over 195 measured; floor 0.064 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.065 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-d1744e9fc07e-1791194186871628473-2231806-38cbc82c` at Hermit `d1744e9fc07e`: `parity: 0/297 matched; selected 297 of 297 committed; mean n/a over 0 measured; floor 0.000 over 275 of 297 selected (counted as 0: 275 unmeasured: no-result-row 3, epoch-not-shared 272; excluded: 6 no golden: determinism-mismatch 6; 16 not compared)`
- validate run `validate-claude-coord-d5a08a9e6d81-1791315831168565080-2240346-d6038eb0` at Hermit `d5a08a9e6d81`: `parity: 85/198 matched; committed selection unknown; mean 0.459 over 195 measured; floor 0.452 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.461 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-d7441caea790-1791326154273426855-1420975-95e8780a` at Hermit `d7441caea790`: `parity: 85/198 matched; selected 198 of 198 committed; mean 0.459 over 195 measured; floor 0.452 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.461 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-da58e3b4799e-1791342383322224462-3640354-f2f72043` at Hermit `da58e3b4799e`: `parity: 86/198 matched; committed selection unknown; mean 0.466 over 195 measured; floor 0.459 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 193 of 195 credited: mean 0.470 over 193 with equal inputs; mean 0.028 over 2 with unequal inputs]`
- validate run `validate-claude-coord-f06ad1931835-1791313107104242182-521762-5dbaad34` at Hermit `f06ad1931835`: `parity: 1/198 matched; selected 198 of 198 committed; mean 0.065 over 195 measured; floor 0.064 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.065 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-f1d591e88c3a-1791244197906338526-2918610-080dfb0a` at Hermit `f1d591e88c3a`: `parity: 0/198 matched; committed selection unknown; mean 0.057 over 182 measured; floor 0.052 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.057 over 180 with equal inputs; mean 0.037 over 2 with unequal inputs]`
- validate run `validate-claude-coord-f20e4619e1e5-1791233666988779907-370563-a76b8b84` at Hermit `f20e4619e1e5`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.051 over 180 measured; floor 0.051 over 182 of 198 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 0 no golden; 16 not compared) [inputs equalized for 91 of 180 credited: mean 0.016 over 91 with equal inputs; mean 0.088 over 89 with unequal inputs]`
- validate run `validate-claude-coord-f2f7c0af8091-1791296363327547009-2731639-06b15ce8` at Hermit `f2f7c0af8091`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 191 of 195 credited: mean 0.056 over 191 with equal inputs; mean 0.025 over 4 with unequal inputs]`
- validate run `validate-claude-coord-f43c990b3c96-1791237892198194518-3054844-abff85f6` at Hermit `f43c990b3c96`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.049 over 182 measured; floor 0.045 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 91 of 182 credited: mean 0.016 over 91 with equal inputs; mean 0.082 over 91 with unequal inputs]`
- validate run `validate-claude-coord-f7b56bd0d3d0-1791265402661147859-705779-d31eb59f` at Hermit `f7b56bd0d3d0`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-f7efac2e8c6a-1791221322434517129-732102-370d21a3` at Hermit `f7efac2e8c6a`: `parity: 0/198 matched; committed selection unknown; mean 0.056 over 180 measured; floor 0.055 over 182 of 198 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 0 no golden; 16 not compared) [inputs equalized for 91 of 180 credited: mean 0.016 over 91 with equal inputs; mean 0.097 over 89 with unequal inputs]`
- validate run `validate-claude-coord-fa6be3120ef3-1791298381620961842-3364612-79fed6ab` at Hermit `fa6be3120ef3`: `parity: 1/198 matched; selected 198 of 198 committed; mean 0.065 over 195 measured; floor 0.064 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.065 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-fb7cb60f6e48-1791334373340857231-3446052-0800573d` at Hermit `fb7cb60f6e48`: `parity: 85/198 matched; committed selection unknown; mean 0.462 over 195 measured; floor 0.455 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.464 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-fcb8a7feedb7-1791318530484448092-3719851-957e332c` at Hermit `fcb8a7feedb7`: `parity: 85/198 matched; selected 198 of 198 committed; mean 0.459 over 195 measured; floor 0.452 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.461 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-claude-coord-mega-lander-4c0b7daa8ae6-1791155208411569193-894674-eab4fa86` at Hermit `4c0b7daa8ae6`: `parity: 0/297 matched; committed selection unknown; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-coord-itvfix-57883eff8f3d-1791046213891728858-2276425-f9271a9e` at Hermit `57883eff8f3d`: `parity: 0/204 matched; selected 204 of 204 committed; mean 0.055 over 185 measured; floor 0.054 over 188 of 204 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-coord-s17-d3a0a4ae3d16-1790649970094317174-3328074-da07a1da` at Hermit `d3a0a4ae3d16`: `parity: 0/192 matched; selected 192 of 192 committed; mean 0.052 over 175 measured; floor 0.051 over 178 of 192 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 14 not compared) [inputs not equalized]`
- validate run `validate-coord-tickval-4565b01b66c2-1791139406221186959-1755386-8f2aad77` at Hermit `4565b01b66c2`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-coord-tickval-b3037b11fa56-1791136526911996485-754722-df9fcd8a` at Hermit `b3037b11fa56`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-coord-tipval2-c1312a563dc5-20261003T235258Z` at Hermit `c1312a563dc5`: `parity: 0/208 matched; selected 208 of 208 committed; mean 0.056 over 188 measured; floor 0.055 over 191 of 208 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 1 no golden: crash 1; 16 not compared)`
- validate run `validate-coord2-a3d7e201e091-1791197685492926552-2311392-b1cd4017` at Hermit `a3d7e201e091`: `parity: 0/297 matched; selected 297 of 297 committed; mean n/a over 0 measured; floor 0.000 over 278 of 297 selected (counted as 0: 278 unmeasured: no-result-row 3, epoch-not-shared 275; excluded: 3 no golden: determinism-mismatch 3; 16 not compared)`
- validate run `validate-coord2-d1744e9fc07e-1791193403382220055-1138567-499f31af` at Hermit `d1744e9fc07e`: `parity: 0/297 matched; selected 297 of 297 committed; mean n/a over 0 measured; floor 0.000 over 278 of 297 selected (counted as 0: 278 unmeasured: no-result-row 3, epoch-not-shared 275; excluded: 3 no golden: determinism-mismatch 3; 16 not compared)`
- validate run `validate-coord2-f20e4619e1e5-1791229035512983738-2101558-ec550a98` at Hermit `f20e4619e1e5`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.053 over 180 measured; floor 0.053 over 182 of 198 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 0 no golden; 16 not compared) [inputs equalized for 91 of 180 credited: mean 0.016 over 91 with equal inputs; mean 0.092 over 89 with unequal inputs]`
- validate run `validate-coord2-fd56c3239015-1791212209210828476-1422541-116dd636` at Hermit `fd56c3239015`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.057 over 180 measured; floor 0.056 over 182 of 198 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 0 no golden; 16 not compared) [inputs equalized for 91 of 180 credited: mean 0.016 over 91 with equal inputs; mean 0.099 over 89 with unequal inputs]`
- validate run `validate-d14-queue-a2b1deecaa21-1791037234447726839-773804-79469e5b` at Hermit `a2b1deecaa21`: `parity: 0/203 matched; selected 203 of 203 committed; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-gate-select-9e698b862c8e-1791147210324610650-2296438-f7c76258` at Hermit `9e698b862c8e`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 276 measured; floor 0.043 over 279 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 2 no golden: ended 2; 16 not compared)`
- validate run `validate-gate-select-9e7dd6e33cf6-1791027490982151389-186043-c4a5cdf6` at Hermit `9e7dd6e33cf6`: `parity: 0/203 matched; selected 203 of 203 committed; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-gate-select-f85de5d891ab-1791150197069282693-1112723-7a425489` at Hermit `f85de5d891ab`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-hermit-call-dirty-false` at Hermit `d3a0a4ae3d16`: `parity: 0/2 matched; selected 2 of 192 committed (partial); mean 0.064 over 2 measured; floor 0.064 over 2 of 2 selected (excluded: 0 no golden; 0 not compared) [inputs not equalized]`
- validate run `validate-hermit-call-dirty-true` at Hermit `d3a0a4ae3d16` (from a dirty source tree): `parity: 0/2 matched; selected 2 of 192 committed (partial); mean 0.064 over 2 measured; floor 0.064 over 2 of 2 selected (excluded: 0 no golden; 0 not compared) [inputs not equalized]`
- validate run `validate-hermit-lander-0930a4b59958-1791260460602815700-1427097-b02e49a4` at Hermit `0930a4b59958`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.057 over 182 measured; floor 0.052 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.057 over 180 with equal inputs; mean 0.037 over 2 with unequal inputs]`
- validate run `validate-hermit-lander-0e176d54bdea-1791294089396942292-1881962-abbb853d` at Hermit `0e176d54bdea`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-0ee0c2860a47-1791307993251780460-3509627-a6b12439` at Hermit `0ee0c2860a47`: `parity: 1/198 matched; selected 198 of 198 committed; mean 0.065 over 195 measured; floor 0.064 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.066 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-33f1c939a2e4-1791054196460293033-32690-ce1f4789` at Hermit `33f1c939a2e4`: `parity: 0/205 matched; selected 205 of 205 committed; mean 0.055 over 186 measured; floor 0.055 over 189 of 205 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-hermit-lander-375a5facdfeb-1791325905326239962-1685580-4c01e653` at Hermit `375a5facdfeb`: `parity: 85/198 matched; selected 198 of 198 committed; mean 0.459 over 195 measured; floor 0.452 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.461 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-3ad5e9d3f477-1791351126654118591-2419393-7a0144c3` at Hermit `3ad5e9d3f477`: `parity: 86/198 matched; selected 198 of 198 committed; mean 0.466 over 195 measured; floor 0.459 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.468 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-3bab9795de22-1791347888917609521-2010613-56bfd2cb` at Hermit `3bab9795de22`: `parity: 86/198 matched; selected 198 of 198 committed; mean 0.466 over 195 measured; floor 0.459 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.468 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-48ff0d995aab-1791283456200827282-396969-d8cde876` at Hermit `48ff0d995aab`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-48ff0d995aab-1791284440676935904-2462961-036cb8aa` at Hermit `48ff0d995aab`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-4dc328776550-1791268225059657154-1333575-ba294a11` at Hermit `4dc328776550`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-50584c02a165-1791366293110336733-1123074-af54dc21` at Hermit `50584c02a165`: `parity: 87/198 matched; selected 198 of 198 committed; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-5a221d7f3e26-1791306499653857024-1738282-5ec34052` at Hermit `5a221d7f3e26`: `parity: 1/198 matched; selected 198 of 198 committed; mean 0.065 over 195 measured; floor 0.064 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.066 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-5fbb7ddbf1bd-1791328954036719128-761119-b803afe4` at Hermit `5fbb7ddbf1bd`: `parity: 85/198 matched; selected 198 of 198 committed; mean 0.459 over 195 measured; floor 0.452 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.461 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-61b0870469e2-1791375810996253133-1330801-c37f2635` at Hermit `61b0870469e2`: `parity: 90/198 matched; selected 198 of 198 committed; mean 0.472 over 195 measured; floor 0.465 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.475 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-6eba7f9b579d-1791257736460279076-2526806-48058b89` at Hermit `6eba7f9b579d`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.057 over 182 measured; floor 0.052 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.057 over 180 with equal inputs; mean 0.037 over 2 with unequal inputs]`
- validate run `validate-hermit-lander-762df286a08e-1791365179676521613-3029775-731640bd` at Hermit `762df286a08e`: `parity: 87/198 matched; selected 198 of 198 committed; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-77fcf6f85543-1791282153028459842-2065390-7aa78f0e` at Hermit `77fcf6f85543`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-86af7af3219c-1791362933941330570-4192702-103ca787` at Hermit `86af7af3219c`: `parity: 87/198 matched; selected 198 of 198 committed; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-86af7af3219c-1791364019622619452-1274562-5c76de16` at Hermit `86af7af3219c`: `parity: 87/198 matched; selected 198 of 198 committed; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-8b7d8e1f8fc0-1791313332241243436-216306-0cde3c55` at Hermit `8b7d8e1f8fc0`: `parity: 1/198 matched; selected 198 of 198 committed; mean 0.065 over 195 measured; floor 0.064 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.066 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-940f6ac850ba-1791292208237695632-3527024-a8db6d95` at Hermit `940f6ac850ba`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-982d6606910f-1791374089710266358-3064113-f482a925` at Hermit `982d6606910f`: `parity: 90/198 matched; selected 198 of 198 committed; mean 0.472 over 195 measured; floor 0.465 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.475 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-98491556e28f-1791302449794791360-1789165-2b3fd50a` at Hermit `98491556e28f`: `parity: 1/198 matched; selected 198 of 198 committed; mean 0.065 over 195 measured; floor 0.064 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.066 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-9e3f257eedfe-1791318118838179938-488203-1bc9d7d3` at Hermit `9e3f257eedfe`: `parity: 85/198 matched; selected 198 of 198 committed; mean 0.459 over 195 measured; floor 0.452 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.461 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-b74460068d33-1791345554500847183-2986395-a0f4d304` at Hermit `b74460068d33`: `parity: 86/198 matched; selected 198 of 198 committed; mean 0.466 over 195 measured; floor 0.459 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.468 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-bc7b28836f48-1791299757612823799-3815947-6e48849e` at Hermit `bc7b28836f48`: `parity: 1/198 matched; selected 198 of 198 committed; mean 0.065 over 195 measured; floor 0.064 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.066 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-ccea3942d23d-1791367706859707269-3347640-c8767c57` at Hermit `ccea3942d23d`: `parity: 90/198 matched; selected 198 of 198 committed; mean 0.472 over 195 measured; floor 0.465 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.475 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-hermit-lander-dd1ce92e34ac-1791251246926096137-473458-56ff8b39` at Hermit `dd1ce92e34ac`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.057 over 182 measured; floor 0.052 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.057 over 180 with equal inputs; mean 0.037 over 2 with unequal inputs]`
- validate run `validate-hermit-lander-eb77ba119e4b-1791356305977895997-1604278-4b1e695c` at Hermit `eb77ba119e4b`: `parity: 87/198 matched; selected 198 of 198 committed; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-11f590383355-1791340523793846808-1940117-1f642dc3` at Hermit `11f590383355`: `parity: 85/198 matched; selected 198 of 198 committed; mean 0.462 over 195 measured; floor 0.455 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.464 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-16a9b4e7c612-1791332873572053007-498150-9ce8987c` at Hermit `16a9b4e7c612`: `parity: 85/198 matched; selected 198 of 198 committed; mean 0.459 over 195 measured; floor 0.452 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.461 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-21e5d7325b72-1791269200292403729-2507537-ac1ad381` at Hermit `21e5d7325b72`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-27ee682e6475-1791179482454733843-2302177-80f2dba6` at Hermit `27ee682e6475`: `parity: 0/297 matched; selected 297 of 297 committed; mean n/a over 0 measured; floor 0.000 over 280 of 297 selected (counted as 0: 280 unmeasured: no-result-row 3, epoch-not-shared 277; excluded: 1 no golden: timeout 1; 16 not compared)`
- validate run `validate-netreplay-rework-339963a905f2-1791317048068264556-3471224-d0b0f97b` at Hermit `339963a905f2`: `parity: 85/198 matched; selected 198 of 198 committed; mean 0.459 over 195 measured; floor 0.452 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.461 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-3b2ffc104e55-1791349123681448272-3605518-735fcf13` at Hermit `3b2ffc104e55`: `parity: 86/198 matched; selected 198 of 198 committed; mean 0.466 over 195 measured; floor 0.459 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.468 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-402e28335c81-1791203834128814949-2026093-7ac832b4` at Hermit `402e28335c81`: `parity: 0/297 matched; selected 297 of 297 committed; mean n/a over 0 measured; floor 0.000 over 281 of 297 selected (counted as 0: 281 unmeasured: no-result-row 3, epoch-not-shared 278; excluded: 0 no golden; 16 not compared)`
- validate run `validate-netreplay-rework-42a82cb5f6ab-1791254513937136636-4178080-1dac094b` at Hermit `42a82cb5f6ab`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.057 over 182 measured; floor 0.052 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.057 over 180 with equal inputs; mean 0.037 over 2 with unequal inputs]`
- validate run `validate-netreplay-rework-47896b1a15dd-1791275634366046702-2110292-fa710e9c` at Hermit `47896b1a15dd`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-4cc833f073d5-1791326863038223052-2854588-13f4991e` at Hermit `4cc833f073d5`: `parity: 85/198 matched; selected 198 of 198 committed; mean 0.459 over 195 measured; floor 0.452 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.461 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-69284949c87a-1791350204865595101-944103-11de41d8` at Hermit `69284949c87a`: `parity: 86/198 matched; selected 198 of 198 committed; mean 0.466 over 195 measured; floor 0.459 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.468 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-69284949c87a-1791351922715459329-3642990-ea1c25ed` at Hermit `69284949c87a`: `parity: 86/198 matched; selected 198 of 198 committed; mean 0.466 over 195 measured; floor 0.459 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.468 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-7f8e75ff4d3a-1791184155517346601-1394308-0ff4a7b6` at Hermit `7f8e75ff4d3a`: `parity: 0/297 matched; selected 297 of 297 committed; mean n/a over 0 measured; floor 0.000 over 277 of 297 selected (counted as 0: 277 unmeasured: no-result-row 3, epoch-not-shared 274; excluded: 4 no golden: determinism-mismatch 3, timeout 1; 16 not compared)`
- validate run `validate-netreplay-rework-83a6123a24d2-1791324503914634595-415693-6f4f5bc7` at Hermit `83a6123a24d2`: `parity: 85/198 matched; selected 198 of 198 committed; mean 0.459 over 195 measured; floor 0.452 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.461 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-860abc70be07-1791259422918292651-29101-e90937f4` at Hermit `860abc70be07`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.057 over 182 measured; floor 0.052 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.057 over 180 with equal inputs; mean 0.037 over 2 with unequal inputs]`
- validate run `validate-netreplay-rework-860abc70be07-1791260701256957085-1608521-42e7ed95` at Hermit `860abc70be07`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.057 over 182 measured; floor 0.052 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.057 over 180 with equal inputs; mean 0.037 over 2 with unequal inputs]`
- validate run `validate-netreplay-rework-887bfd9efe91-1791274239920017981-3860186-7ef63b7a` at Hermit `887bfd9efe91`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-8b7b189183d5-1791196936091505003-1516693-ebe3ca6a` at Hermit `8b7b189183d5`: `parity: 0/297 matched; selected 297 of 297 committed; mean n/a over 0 measured; floor 0.000 over 271 of 297 selected (counted as 0: 271 unmeasured: no-result-row 3, epoch-not-shared 268; excluded: 10 no golden: determinism-mismatch 10; 16 not compared)`
- validate run `validate-netreplay-rework-9992f2866452-1791318176679847880-539164-e88d2a6f` at Hermit `9992f2866452`: `parity: 85/198 matched; selected 198 of 198 committed; mean 0.459 over 195 measured; floor 0.452 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.461 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-b60d8231715c-1791200749755855046-1436481-d0fbefdf` at Hermit `b60d8231715c`: `parity: 0/297 matched; selected 297 of 297 committed; mean n/a over 0 measured; floor 0.000 over 281 of 297 selected (counted as 0: 281 unmeasured: no-result-row 3, epoch-not-shared 278; excluded: 0 no golden; 16 not compared)`
- validate run `validate-netreplay-rework-b7115dfd55aa-1791367690878871831-3326400-eb437e47` at Hermit `b7115dfd55aa`: `parity: 90/198 matched; selected 198 of 198 committed; mean 0.472 over 195 measured; floor 0.465 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.475 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-ba81b5b2ccd3-1791208385653519318-2518059-e1f4c253` at Hermit `ba81b5b2ccd3`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.057 over 180 measured; floor 0.056 over 182 of 198 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 0 no golden; 16 not compared) [inputs equalized for 91 of 180 credited: mean 0.016 over 91 with equal inputs; mean 0.099 over 89 with unequal inputs]`
- validate run `validate-netreplay-rework-cb5d378a1e34-1791369479025176216-1637576-5cb14c58` at Hermit `cb5d378a1e34`: `parity: 90/198 matched; selected 198 of 198 committed; mean 0.472 over 195 measured; floor 0.465 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.475 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-ccf33f52fd05-1791355451231432411-403769-7e44c6f0` at Hermit `ccf33f52fd05`: `parity: 87/198 matched; selected 198 of 198 committed; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-cd4387482b73-1791271294531497324-896439-9f37329d` at Hermit `cd4387482b73`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-f53e746779f9-1791262904524962924-173631-7084882a` at Hermit `f53e746779f9`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-netreplay-rework-ff21a882389c-1791331946455444321-3607701-faaf2efb` at Hermit `ff21a882389c`: `parity: 85/198 matched; selected 198 of 198 committed; mean 0.459 over 195 measured; floor 0.452 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.461 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-ops-tick-0f028322361f-75248bd48fcd` at Hermit `0f028322361f`: `parity: 0/203 matched; selected 203 of 203 committed; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-0f028322361f-a6fa2a143efd` at Hermit `0f028322361f`: `parity: 0/203 matched; selected 203 of 203 committed; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-20077ee5b19f-63f1efea5c13` at Hermit `20077ee5b19f`: `parity: 0/205 matched; selected 205 of 205 committed; mean 0.055 over 186 measured; floor 0.055 over 189 of 205 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-20d49d84ec23-914f280b5537` at Hermit `20d49d84ec23`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-33770223b5d2-11a841f749a1` at Hermit `33770223b5d2`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 277 measured; floor 0.043 over 280 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 1 no golden: crash 1; 16 not compared)`
- validate run `validate-ops-tick-33770223b5d2-27d0ecdc8ccb` at Hermit `33770223b5d2`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 277 measured; floor 0.043 over 280 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 1 no golden: crash 1; 16 not compared)`
- validate run `validate-ops-tick-47896b1a15dd-568ff7f01824` at Hermit `47896b1a15dd`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-ops-tick-4b1894e81cfb-18e5730802ad` at Hermit `4b1894e81cfb`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 277 measured; floor 0.043 over 280 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 1 no golden: crash 1; 16 not compared)`
- validate run `validate-ops-tick-4b1894e81cfb-19db240fdcc8` at Hermit `4b1894e81cfb`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 277 measured; floor 0.043 over 280 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 1 no golden: crash 1; 16 not compared)`
- validate run `validate-ops-tick-562b7dd7a635-80161ea9acc1` at Hermit `562b7dd7a635`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-562b7dd7a635-dffebdc99927` at Hermit `562b7dd7a635`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-7023ea311aa9-925d8f2468cb` at Hermit `7023ea311aa9`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.057 over 182 measured; floor 0.052 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.057 over 180 with equal inputs; mean 0.037 over 2 with unequal inputs]`
- validate run `validate-ops-tick-8503b4fc2c30-57d1aa9b1fa2` at Hermit `8503b4fc2c30`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-8503b4fc2c30-f0c6c4842871` at Hermit `8503b4fc2c30`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-95ab5d77f29f-bbbc434c56b7` at Hermit `95ab5d77f29f`: `parity: 90/198 matched; selected 198 of 198 committed; mean 0.472 over 195 measured; floor 0.465 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 193 of 195 credited: mean 0.477 over 193 with equal inputs; mean 0.025 over 2 with unequal inputs]`
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
- validate run `validate-ops-tick-cbef0ac85172-79bdbb42d8ba` at Hermit `cbef0ac85172`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.057 over 182 measured; floor 0.052 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.057 over 180 with equal inputs; mean 0.037 over 2 with unequal inputs]`
- validate run `validate-ops-tick-cec1ef5f402d-55392313f8a8` at Hermit `cec1ef5f402d`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-d5d1072a6f82-20a21bc9fe7b` at Hermit `d5d1072a6f82`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-e02d9f8368a3-03a15e241a7e` at Hermit `e02d9f8368a3`: `parity: 0/205 matched; selected 205 of 205 committed; mean 0.055 over 186 measured; floor 0.055 over 188 of 205 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 1 no golden: ended 1; 16 not compared)`
- validate run `validate-ops-tick-ef7b55b19fcb-07464fea2ae1` at Hermit `ef7b55b19fcb`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tick-buck-cargo-repro-0a2e0c248258-20261006T034408Z` at Hermit `0a2e0c248258`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.059 over 182 measured; floor 0.054 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.059 over 180 with equal inputs; mean 0.040 over 2 with unequal inputs]`
- validate run `validate-tick-buck-cargo-repro-66378ba1dd58-20261005T010205Z` at Hermit `66378ba1dd58`: `parity: 0/297 matched; selected 297 of 297 committed; mean 0.043 over 278 measured; floor 0.043 over 281 of 297 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-tick-buck-cargo-repro-d7441caea790-20261007T013140Z` at Hermit `d7441caea790`: `parity: 1/198 matched; selected 198 of 198 committed; mean 0.058 over 195 measured; floor 0.057 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 181 of 195 credited: mean 0.060 over 181 with equal inputs; mean 0.034 over 14 with unequal inputs]`
- validate run `validate-tickhub-ops-2-0c3715b402c0-1791279422071821891-2293826-173ce134` at Hermit `0c3715b402c0`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-115d1fee1cb0-1791376661873162277-2717627-de5f36fb` at Hermit `115d1fee1cb0`: `parity: 90/198 matched; selected 198 of 198 committed; mean 0.472 over 195 measured; floor 0.465 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.475 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-1768f46eb450-1791333630960984320-1729983-b066ec88` at Hermit `1768f46eb450`: `parity: 85/198 matched; selected 198 of 198 committed; mean 0.459 over 195 measured; floor 0.452 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.461 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-23d2d0a45bb8-1791353075327029083-1480349-1c166aac` at Hermit `23d2d0a45bb8`: `parity: 87/198 matched; selected 198 of 198 committed; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-23d2d0a45bb8-1791354294466992322-3181812-28903232` at Hermit `23d2d0a45bb8`: `parity: 87/198 matched; selected 198 of 198 committed; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-3f28e1654b17-1791314259719526747-1279493-e0d7a55b` at Hermit `3f28e1654b17`: `parity: 1/198 matched; selected 198 of 198 committed; mean 0.065 over 195 measured; floor 0.064 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.066 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-44349791d536-1791278119259879656-980668-b3c102b0` at Hermit `44349791d536`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-4a6344d1f44c-1791242634174367856-446832-20cdccfd` at Hermit `4a6344d1f44c`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 182 measured; floor 0.050 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 91 of 182 credited: mean 0.016 over 91 with equal inputs; mean 0.094 over 91 with unequal inputs]`
- validate run `validate-tickhub-ops-2-4ec5b86088ba-1791330353284760243-2250943-1676226b` at Hermit `4ec5b86088ba`: `parity: 85/198 matched; selected 198 of 198 committed; mean 0.459 over 195 measured; floor 0.452 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.461 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-51b39ee37917-1791267231324811216-60308-a974555f` at Hermit `51b39ee37917`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-5cca2a25e893-1791366283185052495-1112468-a8314ad3` at Hermit `5cca2a25e893`: `parity: 87/198 matched; selected 198 of 198 committed; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-5d6478725518-1791361179050529498-2403015-0eff4c15` at Hermit `5d6478725518`: `parity: 87/198 matched; selected 198 of 198 committed; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-5d76c9f5ae6d-1791338605038802979-406065-f823d39f` at Hermit `5d76c9f5ae6d`: `parity: 85/198 matched; selected 198 of 198 committed; mean 0.459 over 195 measured; floor 0.452 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.461 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-7023ea311aa9-1791247705180542564-348674-db107847` at Hermit `7023ea311aa9`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.057 over 182 measured; floor 0.052 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.057 over 180 with equal inputs; mean 0.037 over 2 with unequal inputs]`
- validate run `validate-tickhub-ops-2-7023ea311aa9-1791248928901210934-1906546-528e9564` at Hermit `7023ea311aa9`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.057 over 182 measured; floor 0.052 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.057 over 180 with equal inputs; mean 0.037 over 2 with unequal inputs]`
- validate run `validate-tickhub-ops-2-7023ea311aa9-1791250086335957848-3355824-3f477601` at Hermit `7023ea311aa9`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.057 over 182 measured; floor 0.052 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.057 over 180 with equal inputs; mean 0.037 over 2 with unequal inputs]`
- validate run `validate-tickhub-ops-2-7023ea311aa9-1791252496696252285-1929569-23dcf0b9` at Hermit `7023ea311aa9`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.057 over 182 measured; floor 0.052 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.057 over 180 with equal inputs; mean 0.037 over 2 with unequal inputs]`
- validate run `validate-tickhub-ops-2-7dd533962923-1791228151831749581-1407633-e05600d6` at Hermit `7dd533962923`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 180 measured; floor 0.055 over 182 of 198 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 0 no golden; 16 not compared) [inputs equalized for 91 of 180 credited: mean 0.016 over 91 with equal inputs; mean 0.096 over 89 with unequal inputs]`
- validate run `validate-tickhub-ops-2-807dd2c627ab-1791351911248180428-3623808-b61c5329` at Hermit `807dd2c627ab`: `parity: 86/198 matched; selected 198 of 198 committed; mean 0.466 over 195 measured; floor 0.459 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.468 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-8d85cb2d50e0-1791374991259209356-157714-59e7d85a` at Hermit `8d85cb2d50e0`: `parity: 90/198 matched; selected 198 of 198 committed; mean 0.472 over 195 measured; floor 0.465 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.475 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-a10f2a7b7770-1791270254015742714-3935187-08bf141f` at Hermit `a10f2a7b7770`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-a4d8bccdb528-1791364857401391400-2616439-7441d59b` at Hermit `a4d8bccdb528`: `parity: 87/198 matched; selected 198 of 198 committed; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-b90818ac3b4c-1791359883591600909-915312-2531a236` at Hermit `b90818ac3b4c`: `parity: 87/198 matched; selected 198 of 198 committed; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-c239319d7c1d-1791369533286666625-1686473-f7f91942` at Hermit `c239319d7c1d`: `parity: 90/198 matched; selected 198 of 198 committed; mean 0.472 over 195 measured; floor 0.465 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.475 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-d647d98bd0a6-1791240724863946406-2173492-9504a3c4` at Hermit `d647d98bd0a6`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 182 measured; floor 0.051 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 91 of 182 credited: mean 0.016 over 91 with equal inputs; mean 0.095 over 91 with unequal inputs]`
- validate run `validate-tickhub-ops-2-e1872249962f-1791358874431300145-3973771-578fecfb` at Hermit `e1872249962f`: `parity: 87/198 matched; selected 198 of 198 committed; mean 0.468 over 195 measured; floor 0.461 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.470 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-e6179c00508b-1791347152773694927-881949-bcb4d90b` at Hermit `e6179c00508b`: `parity: 86/198 matched; selected 198 of 198 committed; mean 0.466 over 195 measured; floor 0.459 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.468 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-ea69f2123dbf-1791342232008366316-3871079-bff279f8` at Hermit `ea69f2123dbf`: `parity: 86/198 matched; selected 198 of 198 committed; mean 0.466 over 195 measured; floor 0.459 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.468 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-ea69f2123dbf-1791343458349334797-1007730-fb6aa81e` at Hermit `ea69f2123dbf`: `parity: 86/198 matched; selected 198 of 198 committed; mean 0.466 over 195 measured; floor 0.459 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.468 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- validate run `validate-tickhub-ops-2-f20e4619e1e5-1791231708827814929-164883-96df9a5d` at Hermit `f20e4619e1e5`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.054 over 180 measured; floor 0.053 over 182 of 198 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 0 no golden; 16 not compared) [inputs equalized for 91 of 180 credited: mean 0.016 over 91 with equal inputs; mean 0.093 over 89 with unequal inputs]`
- validate run `validate-tickhub-ops-2-f29c47d2bef4-1791244371926289104-1699228-ca089f63` at Hermit `f29c47d2bef4`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.057 over 182 measured; floor 0.052 over 198 of 198 selected (counted as 0: 16 unmeasured: no-result-row 16; excluded: 0 no golden; 0 not compared) [inputs equalized for 180 of 182 credited: mean 0.057 over 180 with equal inputs; mean 0.037 over 2 with unequal inputs]`
- validate run `validate-tickhub-ops-2-fe87704c025d-1791265742000732494-2716231-2b49f547` at Hermit `fe87704c025d`: `parity: 0/198 matched; selected 198 of 198 committed; mean 0.055 over 195 measured; floor 0.054 over 198 of 198 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 0 not compared) [inputs equalized for 194 of 195 credited: mean 0.055 over 194 with equal inputs; mean 0.040 over 1 with unequal inputs]`
- pressure-test run `p1-green10` at Hermit `7759159896ab`: `parity: not compared: 0 measured of 2 selected (inputs cannot be equalized); selected 2 of 297 committed (partial)`
- pressure-test run `pressure-c470fa213ee8-20261003T180650Z` at Hermit `c470fa213ee8`: `parity: not compared: 0 measured of 1 selected (inputs cannot be equalized); selected 1 of 205 committed (partial)`
- pressure-test run `pressure-d7441caea790-20261006T230247Z` at Hermit `d7441caea790`: `parity: 0/2 matched; selected 0 of 198 committed (partial); 2 outside the committed selection; mean 0.035 over 1 measured; floor 0.017 over 2 of 2 selected (counted as 0: 1 unmeasured: no-result-row 1; excluded: 0 no golden; 0 not compared) [inputs not equalized]`
- pressure-test run `pressure-test-hermit-call-dirty-false` at Hermit `d3a0a4ae3d16`: `parity: 0/2 matched; selected 2 of 192 committed (partial); mean 0.064 over 2 measured; floor 0.064 over 2 of 2 selected (excluded: 0 no golden; 0 not compared) [inputs not equalized]`
- pressure-test run `pressure-test-hermit-call-dirty-true` at Hermit `d3a0a4ae3d16` (from a dirty source tree): `parity: 0/2 matched; selected 2 of 192 committed (partial); mean 0.064 over 2 measured; floor 0.064 over 2 of 2 selected (excluded: 0 no golden; 0 not compared) [inputs not equalized]`

### legacy-rerun history

`legacy-rerun (retired ptrace rerun on 173 cell(s), last at 5ee668223a15): 0 matched / 173 diverged`

These verdicts came from the retired ptrace rerun, which ran each candidate beside a fresh ptrace run. They are kept as labelled history only: they are not current parity, they carry no credit, and they enter none of the counts above. The `Determinism` column is the cell's own measurement, which parity never changes.

1211 older comparison(s) of the same cells were superseded by a later one.

Retired rerun evidence not kept as history, by reason: `no-retained-comparison` 4322 on 173 cell(s).

| Cell | Label | Legacy verdict | Compared records | First divergent record | Hermit commit | Determinism | Current parity |
| --- | --- | --- | ---: | ---: | --- | --- | --- |
| `c-programs/aio-refusal@kvm` | legacy-rerun | diverged | 107 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/aio-refusal@liteinst` | legacy-rerun | diverged | 107 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/append-pwrite@kvm` | legacy-rerun | diverged | 144 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/append-pwrite@liteinst` | legacy-rerun | diverged | 152 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/bind-getsockname@kvm` | legacy-rerun | diverged | 110 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/bind-getsockname@liteinst` | legacy-rerun | diverged | 110 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/cachestat-refusal@kvm` | legacy-rerun | diverged | 124 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/cachestat-refusal@liteinst` | legacy-rerun | diverged | 132 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/child-subreaper-refusal@kvm` | legacy-rerun | diverged | 103 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/child-subreaper-refusal@liteinst` | legacy-rerun | diverged | 103 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/close-range-fds@kvm` | legacy-rerun | diverged | 137 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/close-range-fds@liteinst` | legacy-rerun | diverged | 145 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/copy-file-range-refusal@kvm` | legacy-rerun | diverged | 139 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/copy-file-range-refusal@liteinst` | legacy-rerun | diverged | 143 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/cpu-virtualization@kvm` | legacy-rerun | diverged | 104 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/cpu-virtualization@liteinst` | legacy-rerun | diverged | 104 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/cwd-roundtrip@kvm` | legacy-rerun | diverged | 133 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/cwd-roundtrip@liteinst` | legacy-rerun | diverged | 141 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/dup-shared-offset@kvm` | legacy-rerun | diverged | 148 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/dup-shared-offset@liteinst` | legacy-rerun | diverged | 156 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/epoll-pwait2@liteinst` | legacy-rerun | diverged | 125 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/epoll-readiness@kvm` | legacy-rerun | diverged | 123 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/epoll-readiness@liteinst` | legacy-rerun | diverged | 123 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/event-delivery-ordering@liteinst` | legacy-rerun | diverged | 190 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/eventfd-semantics@kvm` | legacy-rerun | diverged | 169 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/eventfd-semantics@liteinst` | legacy-rerun | diverged | 169 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/faccessat2-flags@kvm` | legacy-rerun | diverged | 132 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/faccessat2-flags@liteinst` | legacy-rerun | diverged | 140 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/fadvise-hints@kvm` | legacy-rerun | diverged | 124 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/fadvise-hints@liteinst` | legacy-rerun | diverged | 132 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/fallocate-extents@kvm` | legacy-rerun | diverged | 130 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/fallocate-extents@liteinst` | legacy-rerun | diverged | 138 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/fchmod-bits@kvm` | legacy-rerun | diverged | 131 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/fchmod-bits@liteinst` | legacy-rerun | diverged | 139 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/fchmodat2-flags@liteinst` | legacy-rerun | diverged | 139 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/fcntl-owner@liteinst` | legacy-rerun | diverged | 121 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/fd-duplication@kvm` | legacy-rerun | diverged | 163 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/fd-duplication@liteinst` | legacy-rerun | diverged | 171 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/file-backed-mmap@kvm` | legacy-rerun | diverged | 133 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/file-backed-mmap@liteinst` | legacy-rerun | diverged | 141 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/file-io-roundtrip@kvm` | legacy-rerun | diverged | 148 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/file-io-roundtrip@liteinst` | legacy-rerun | diverged | 148 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/flock-lifecycle@kvm` | legacy-rerun | diverged | 121 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/flock-lifecycle@liteinst` | legacy-rerun | diverged | 129 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/fork-exec-pipeline@kvm` | legacy-rerun | diverged | 337 | 13 | `5ee668223a15` | `diverged` | matched |
| `c-programs/fsync-durability@liteinst` | legacy-rerun | diverged | 132 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/ftruncate-sparse@kvm` | legacy-rerun | diverged | 135 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/ftruncate-sparse@liteinst` | legacy-rerun | diverged | 143 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/getcpu-identity@kvm` | legacy-rerun | diverged | 123 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/getcpu-identity@liteinst` | legacy-rerun | diverged | 123 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/getpriority-identity@kvm` | legacy-rerun | diverged | 113 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/getpriority-identity@liteinst` | legacy-rerun | diverged | 113 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/hardware-trap-identity@liteinst` | legacy-rerun | diverged | 312 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/host-identity@liteinst` | legacy-rerun | diverged | 117 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/inline-syscall-sites@kvm` | legacy-rerun | diverged | 165 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/inline-syscall-sites@liteinst` | legacy-rerun | diverged | 165 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/inotify-watch@liteinst` | legacy-rerun | diverged | 110 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/ioctl-fionread@liteinst` | legacy-rerun | diverged | 120 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/kcmp-refusal@kvm` | legacy-rerun | diverged | 111 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/kcmp-refusal@liteinst` | legacy-rerun | diverged | 111 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/linkat-flags@kvm` | legacy-rerun | diverged | 146 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/linkat-flags@liteinst` | legacy-rerun | diverged | 154 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/lseek-positioning@kvm` | legacy-rerun | diverged | 143 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/lseek-positioning@liteinst` | legacy-rerun | diverged | 151 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/mce-kill-refusal@kvm` | legacy-rerun | diverged | 103 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/mce-kill-refusal@liteinst` | legacy-rerun | diverged | 103 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/membarrier-query@kvm` | legacy-rerun | diverged | 105 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/membarrier-query@liteinst` | legacy-rerun | diverged | 105 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/memfd-create@kvm` | legacy-rerun | diverged | 121 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/memfd-create@liteinst` | legacy-rerun | diverged | 121 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/mempolicy-default@kvm` | legacy-rerun | diverged | 107 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/mempolicy-default@liteinst` | legacy-rerun | diverged | 107 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/mincore-residency@kvm` | legacy-rerun | diverged | 119 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/mincore-residency@liteinst` | legacy-rerun | diverged | 119 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/mixed-inline-and-libc-syscalls@kvm` | legacy-rerun | diverged | 147 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/mixed-inline-and-libc-syscalls@liteinst` | legacy-rerun | diverged | 147 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/mkdir-rmdir@kvm` | legacy-rerun | diverged | 129 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/mkdir-rmdir@liteinst` | legacy-rerun | diverged | 137 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/mknod-special@kvm` | legacy-rerun | diverged | 133 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/mknod-special@liteinst` | legacy-rerun | diverged | 141 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/mmap-layout-pointer-order@kvm` | legacy-rerun | diverged | 133 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/mmap-layout-pointer-order@liteinst` | legacy-rerun | diverged | 141 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/msync-writeback@liteinst` | legacy-rerun | diverged | 137 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/name-to-handle-refusal@kvm` | legacy-rerun | diverged | 103 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/name-to-handle-refusal@liteinst` | legacy-rerun | diverged | 103 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/no-new-privs-refusal@kvm` | legacy-rerun | diverged | 105 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/no-new-privs-refusal@liteinst` | legacy-rerun | diverged | 105 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/numa-node-identity@kvm` | legacy-rerun | diverged | 111 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/numa-node-identity@liteinst` | legacy-rerun | diverged | 111 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/o-tmpfile-anon@kvm` | legacy-rerun | diverged | 120 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/o-tmpfile-anon@liteinst` | legacy-rerun | diverged | 120 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/openat-flags@kvm` | legacy-rerun | diverged | 160 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/openat-flags@liteinst` | legacy-rerun | diverged | 168 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/openat2-refusal@kvm` | legacy-rerun | diverged | 105 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/openat2-refusal@liteinst` | legacy-rerun | diverged | 105 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/path-file-ops@kvm` | legacy-rerun | diverged | 141 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/path-file-ops@liteinst` | legacy-rerun | diverged | 149 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/personality-domain@liteinst` | legacy-rerun | diverged | 109 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/pid-probe@kvm` | legacy-rerun | diverged | 101 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/pid-probe@liteinst` | legacy-rerun | diverged | 101 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/pid-probe@sabre` | legacy-rerun | diverged | 32 | 3 | `5ee668223a15` | `measured-and-passed` | diverged |
| `c-programs/pidfd-open-self-pair@kvm` | legacy-rerun | diverged | 117 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/pidfd-open-self-pair@liteinst` | legacy-rerun | diverged | 117 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/pipe-capacity-pin@kvm` | legacy-rerun | diverged | 111 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/pipe-capacity-pin@liteinst` | legacy-rerun | diverged | 111 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/pipe-capacity@kvm` | legacy-rerun | diverged | 113 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/pipe-capacity@liteinst` | legacy-rerun | diverged | 113 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/pipe-ipc@kvm` | legacy-rerun | diverged | 158 | 13 | `5ee668223a15` | `diverged` | matched |
| `c-programs/pipe-ipc@liteinst` | legacy-rerun | diverged | 164 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/pipe2-flags@kvm` | legacy-rerun | diverged | 163 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/pipe2-flags@liteinst` | legacy-rerun | diverged | 163 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/poll-readiness@liteinst` | legacy-rerun | diverged | 139 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/prctl-identity@liteinst` | legacy-rerun | diverged | 121 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/prctl-pdeathsig@liteinst` | legacy-rerun | diverged | 111 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/preadv2-flags@liteinst` | legacy-rerun | diverged | 140 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/pthread-lifecycle@liteinst` | legacy-rerun | diverged | 214 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/readdir-entries@kvm` | legacy-rerun | diverged | 159 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/readdir-entries@liteinst` | legacy-rerun | diverged | 167 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/readdir-order-identity@kvm` | legacy-rerun | diverged | 13669 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/readdir-order-identity@liteinst` | legacy-rerun | diverged | 13677 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/record-lock@liteinst` | legacy-rerun | diverged | 131 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/rename-ops@kvm` | legacy-rerun | diverged | 163 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/rename-ops@liteinst` | legacy-rerun | diverged | 171 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/renameat2-flags@kvm` | legacy-rerun | diverged | 184 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/renameat2-flags@liteinst` | legacy-rerun | diverged | 192 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/rlimit-identity@kvm` | legacy-rerun | diverged | 121 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/rlimit-identity@liteinst` | legacy-rerun | diverged | 121 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/robust-list@kvm` | legacy-rerun | diverged | 105 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/robust-list@liteinst` | legacy-rerun | diverged | 105 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/sched-getaffinity-identity@kvm` | legacy-rerun | diverged | 111 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/sched-getaffinity-identity@liteinst` | legacy-rerun | diverged | 111 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/seccomp-refusal@kvm` | legacy-rerun | diverged | 103 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/seccomp-refusal@liteinst` | legacy-rerun | diverged | 103 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/sendfile-copy@kvm` | legacy-rerun | diverged | 155 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/sendfile-copy@liteinst` | legacy-rerun | diverged | 159 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/set-tid-address@kvm` | legacy-rerun | diverged | 107 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/set-tid-address@liteinst` | legacy-rerun | diverged | 107 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/short-io-split-identity@kvm` | legacy-rerun | diverged | 431 | 13 | `5ee668223a15` | `diverged` | matched |
| `c-programs/short-io-split-identity@liteinst` | legacy-rerun | diverged | 435 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/shutdown-socketpair@kvm` | legacy-rerun | diverged | 125 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/shutdown-socketpair@liteinst` | legacy-rerun | diverged | 125 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/signal-delivery-sequence@liteinst` | legacy-rerun | diverged | 997 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/signal-waitstatus-identity@liteinst` | legacy-rerun | diverged | 386 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/signalfd-create@kvm` | legacy-rerun | diverged | 113 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/signalfd-create@liteinst` | legacy-rerun | diverged | 113 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/socket-epoll-ordering@kvm` | legacy-rerun | diverged | 250 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/socket-epoll-ordering@liteinst` | legacy-rerun | diverged | 250 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/socket-options@liteinst` | legacy-rerun | diverged | 131 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/socketpair-flags@liteinst` | legacy-rerun | diverged | 119 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/sockname-unnamed@liteinst` | legacy-rerun | diverged | 113 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/stat-metadata-identity@kvm` | legacy-rerun | diverged | 233 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/stat-metadata-identity@liteinst` | legacy-rerun | diverged | 233 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/statfs-free-determinism@kvm` | legacy-rerun | diverged | 109 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/statfs-free-determinism@liteinst` | legacy-rerun | diverged | 109 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/static-nolibc-syscall-sites@kvm` | legacy-rerun | diverged | 76 | 64 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/statx-metadata@kvm` | legacy-rerun | diverged | 123 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/statx-metadata@liteinst` | legacy-rerun | diverged | 131 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/symlink-ops@kvm` | legacy-rerun | diverged | 152 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/symlink-ops@liteinst` | legacy-rerun | diverged | 160 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/sync-file-range@kvm` | legacy-rerun | diverged | 122 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/sync-file-range@liteinst` | legacy-rerun | diverged | 130 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/sysv-ipc-refusal@kvm` | legacy-rerun | diverged | 105 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/sysv-ipc-refusal@liteinst` | legacy-rerun | diverged | 105 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/thp-disable@liteinst` | legacy-rerun | diverged | 107 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/umask-mode@kvm` | legacy-rerun | diverged | 139 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/umask-mode@liteinst` | legacy-rerun | diverged | 147 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/uname-identity@kvm` | legacy-rerun | diverged | 101 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/uname-identity@liteinst` | legacy-rerun | diverged | 101 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/utimensat-determinism@kvm` | legacy-rerun | diverged | 123 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/utimensat-determinism@liteinst` | legacy-rerun | diverged | 131 | 16 | `5ee668223a15` | `measured-and-passed` | — |
| `c-programs/vectored-file-io@liteinst` | legacy-rerun | diverged | 147 | 16 | `5ee668223a15` | `diverged` | — |
| `c-programs/vectored-io@kvm` | legacy-rerun | diverged | 126 | 13 | `5ee668223a15` | `measured-and-passed` | matched |
| `c-programs/vectored-io@liteinst` | legacy-rerun | diverged | 126 | 16 | `5ee668223a15` | `diverged` | — |
