# Compatibility scorecard

Last regenerated **2026-10-03T21:47:59Z** from `https://github.com/rrnewton/hermit_test_ledger.git` commit `7390223b51c6b712c79aea04938dbf0112b1cfcf`, reading 241357 series row(s). Validate run published in this series snapshot, with its cell comparisons: `validate-buck-re-3-cf2a94ea38d3-1791063611629487524-833061-4b45d912` (908). Earlier validate runs still supplying comparisons: `validate-buck-validate-cargo-5ee668223a15-1790586574232510178-2945118-43b1052e` current (1030), `validate-buck-validate-cargo-116306360331-1790568657341247584-953732-59d32212` current (1029), `validate-buck-validate-cargo-5f7fe6f5469e-1790581119742174450-899201-26d8395d` current (1029), `validate-ops-tick-20077ee5b19f-63f1efea5c13` current (908), `validate-coord-84da7b816939-1790723396555428157-708227-fd736ab2` current (893), `validate-coord-s15-fdd8f55a8e7c-1790627089637531101-811416-cdde0755` current (856), `validate-coord-s17-d3a0a4ae3d16-1790649970094317174-3328074-da07a1da` current (856), `validate-liteinst-lane-claude-20260927-inodes-78e69b8de6d1-1790665034398292621-2080784-da1f272e` current (856), and 2 more.

This table is derived from the manifest, not from a separately maintained parent-workspace CSV. `./ci/compat-envelope/scorecard.rs check` verifies it.

The count table includes all **14800** cells in the manifest; no row is omitted. A cell is **Selected by full** exactly when it appears in `ci/expected-e2e-plan.json`. A cell is **Not selected by full** when it is in the manifest but absent from that plan. Selection is not a test result: a cell not selected by full may have passed, failed, produced no verdict, or never run. Of these cells, **1098** are selected by full, **725** are not selected by full, and **12977** are **Not applicable**.

Every selected `verify` cell that does not declare the stripped comparator, and every seed in a selected `chaos` cell, runs the same backend twice. The manifest runner adds `--verify-strict` when the selected Hermit binary supports it, and accepts a result only when the typed report says `verified=true`, `verdict=matched`, `bitwise_parity=true`, `strictness=canonical`, `compare_logs=true`, a named canonical `record_envelope`, and both INFO-message counts are nonzero. Bare `--verify` remains a Stripped comparison when invoked directly and does not satisfy this regression plan. **189** of the **1088** selected `verify` cells declare `comparator: stripped` (`compat` on `ptrace`: 189). They run Hermit's default `--verify` and pass only on a verified, matched report of a non-empty stripped comparison; they are below L2, never `bitwise_parity`, and are counted in these tables as selected, not as canonical. These same-backend results do not establish cross-backend parity.

| Backend | Selected by full | Not selected by full | Not applicable | In the manifest |
| --- | ---: | ---: | ---: | ---: |
| `ptrace` | 558 | 375 | 1842 | 2775 |
| `dbt` | 26 | 59 | 2690 | 2775 |
| `kvm` | 256 | 8 | 2511 | 2775 |
| `sabre` | 112 | 247 | 2416 | 2775 |
| `liteinst` | 146 | 3 | 2626 | 2775 |
| `native` | 0 | 33 | 892 | 925 |
| **Total** | **1098** | **725** | **12977** | **14800** |

## Denominator, and why the percentage is not comparable across changes to it

Selected by full is **1098 of 14800**, which is **7.42%** — over THIS population and no other. The population is every combination the manifest declares, and it is composed of:

- backends: `ptrace`, `dbt`, `kvm`, `sabre`, `liteinst`, `native`
- modes: `chaos`, `naked`, `replay`, `verify`

⚠️ **12977 of those 14800 cells are NOT APPLICABLE** — their backend is not applicable for their mode, so they were never asked to run and cannot pass or fail. Over the 1823 cells that CAN run, selected by full is **60.23%**.

⚠️ **DO NOT QUOTE THAT SECOND FIGURE AS PROGRESS.** It is the same 1098 cells selected by full measured against a smaller denominator. Nothing was fixed to produce it; it is what the first figure always meant once the cells that cannot run are excluded. Quote both or neither, and never compare one against the other as though something moved.

⚠️ **Adding or removing a backend or mode changes this denominator and therefore the percentage, without anything about the product changing.** Removing a backend whose cells are mostly not selected RAISES the reported figure; adding manifest cells that are not selected LOWERS it. Neither is progress. Before comparing this percentage against an earlier one, diff the two lists above: if they differ, the numbers are not comparable and the difference is not a result.

The mode view makes the current order of work explicit: expand `verify` first, then `replay`, then `chaos`. Each backend cell is `selected by full / in the manifest`; an em dash means that mode does not exist for that backend. The summary columns use the same selection and applicability facts as the table above.

| Mode | `ptrace` | `dbt` | `kvm` | `sabre` | `liteinst` | `native` | Selected by full | Not selected by full | Not applicable | In the manifest |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| `verify` | 548 / 925 | 26 / 925 | 256 / 925 | 112 / 925 | 146 / 925 | — | 1088 | 552 | 2985 | 4625 |
| `replay` | 4 / 925 | 0 / 925 | 0 / 925 | 0 / 925 | 0 / 925 | — | 4 | 139 | 4482 | 4625 |
| `chaos` | 6 / 925 | 0 / 925 | 0 / 925 | 0 / 925 | 0 / 925 | — | 6 | 1 | 4618 | 4625 |
| `naked` | — | — | — | — | — | 0 / 925 | 0 | 33 | 892 | 925 |
| **Total** | | | | | | | **1098** | **725** | **12977** | **14800** |

## Ptrace by manifest category

This view uses the same Basic Sanity Milestone 1 contracts as the tables above, but makes the ptrace workload mix visible. Each entry is `selected by full / in the manifest`; `custom` commands are not part of this denominator.

| Manifest category | Verify | Replay | Chaos | Selected by full | In the manifest |
| --- | ---: | ---: | ---: | ---: | ---: |
| `applications` | 3 / 6 | 0 / 6 | 0 / 6 | 3 | 18 |
| `bin-c` | 1 / 2 | 0 / 2 | 0 / 2 | 1 | 6 |
| `c-programs` | 273 / 278 | 3 / 278 | 3 / 278 | 279 | 834 |
| `chaos-c` | 1 / 1 | 0 / 1 | 1 / 1 | 2 | 3 |
| `compat` | 189 / 551 | 0 / 551 | 0 / 551 | 189 | 1653 |
| `data-handling` | 6 / 6 | 0 / 6 | 0 / 6 | 6 | 18 |
| `debugger-c` | 1 / 1 | 0 / 1 | 0 / 1 | 1 | 3 |
| `determinism-stress` | 5 / 6 | 0 / 6 | 1 / 6 | 6 | 18 |
| `determinism-stress-c` | 11 / 11 | 0 / 11 | 1 / 11 | 12 | 33 |
| `language-runtimes` | 18 / 19 | 0 / 19 | 0 / 19 | 18 | 57 |
| `shared-futex-c` | 1 / 4 | 0 / 4 | 0 / 4 | 1 | 12 |
| `system-utils` | 38 / 39 | 1 / 39 | 0 / 39 | 39 | 117 |
| `util-c` | 1 / 1 | 0 / 1 | 0 / 1 | 1 | 3 |

Ordinary full validation executes 1103 cells: the 1098 comparable compatibility cells selected by full above (including 6 chaos-mode race-exposure checks), and 5 explicit custom commands outside the comparable denominator. A passing validate must produce a fresh result for all of them; a failing selected cell is a regression, not permission to remove it from the plan.

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

Selection and observation answer different questions. The first column says whether full validation selects a cell. The per-cell `measurement` value says what retained evidence observed: `never-measured`, `measured-and-passed`, `measured-no-verdict`, `diverged-unlocated`, or `diverged`. Of the cells selected by full, **0** have `never-measured`; of the cells not selected by full, **145** have `measured-and-passed`.

Retained history that has not been imported is not counted here. A stored measurement does not establish that it describes current code; `show` reports whether the recorded last test still matches `HEAD:detcore`.

The count table includes all **14800** cells in the manifest; no row is omitted. These claims use the same counts printed in the table below.

| Selection by full | `never-measured` | `measured-and-passed` | `measured-no-verdict` | `diverged-unlocated` | `diverged` | In the manifest |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Selected by full | 0 | 952 | 0 | 3 | 143 | 1098 |
| Not selected by full | 477 | 145 | 73 | 0 | 30 | 725 |
| Not applicable | 12566 | 314 | 80 | 0 | 17 | 12977 |
| **Total** | **13043** | **1411** | **153** | **3** | **190** | **14800** |

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
| `bin-c/robust-futex-test` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
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
| `c-programs/aio-refusal` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/append-pwrite` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/append-pwrite` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/append-pwrite` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/append-pwrite` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/arch-prctl-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/arch-prctl-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/arch-prctl-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/bind-getsockname` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/bind-getsockname` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/bind-getsockname` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/bind-getsockname` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/cachestat-refusal` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/child-subreaper-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/child-subreaper-refusal` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/child-subreaper-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/child-subreaper-refusal` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/copy-file-range-refusal` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal-probe` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal-probe` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal-probe` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/copy-file-range-refusal-probe` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/copy-file-range-refusal-probe` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpu-virtualization` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/cpu-virtualization` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpu-virtualization` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpu-virtualization` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/cpu-virtualization` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/cwd-roundtrip` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/cwd-roundtrip` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/cwd-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/cwd-roundtrip` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/dbt-copied-tiocgpgrp` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `c-programs/dbt-copied-tiocgpgrp` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-copied-tiocgpgrp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-copied-tiocgpgrp` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
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
| `c-programs/dbt-prlimit-self` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-prlimit-self` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-prlimit-self` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-self-sigqueue` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-self-sigqueue` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-self-sigqueue` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-unsupported-syscall` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/dbt-unsupported-syscall` | `verify` | `ptrace` | `Not selected by full` | `measured-no-verdict` |
| `c-programs/dbt-unsupported-syscall` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/dbt-wait-accounting` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-wait-accounting` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/dbt-wait-accounting` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-wait-accounting` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/dbt-wait-lifecycle` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-wait-lifecycle` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/dbt-wait-lifecycle` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dbt-wait-lifecycle` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `c-programs/dup-shared-offset` | `verify` | `dbt` | `Not selected by full` | `diverged` |
| `c-programs/dup-shared-offset` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/dup-shared-offset` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/dup-shared-offset` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/dup-shared-offset` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/epoll-pwait2` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/epoll-readiness` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-readiness` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/epoll-readiness` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/epoll-readiness` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/event-delivery-ordering` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/event-delivery-ordering` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/event-delivery-ordering` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/eventfd-semantics` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/eventfd-semantics` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/eventfd-semantics` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/eventfd-semantics` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/faccessat2-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/faccessat2-flags` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/faccessat2-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/faccessat2-flags` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/fadvise-hints` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fadvise-hints` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/fadvise-hints` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fadvise-hints` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/fallocate-extents` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fallocate-extents` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/fallocate-extents` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fallocate-extents` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/fchmod-bits` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fchmod-bits` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/fchmod-bits` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fchmod-bits` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/fchmodat2-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fchmodat2-flags` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/fchmodat2-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fchmodat2-flags` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/fcntl-owner` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/fcntl-owner` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fcntl-owner` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/fd-duplication` | `verify` | `dbt` | `Not selected by full` | `diverged` |
| `c-programs/fd-duplication` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/fd-duplication` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/fd-duplication` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/fd-duplication` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/file-backed-mmap` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/file-backed-mmap` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/file-backed-mmap` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/file-backed-mmap` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/file-io-roundtrip` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/file-io-roundtrip` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/file-io-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/file-io-roundtrip` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/flock-lifecycle` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/flock-lifecycle` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/flock-lifecycle` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/flock-lifecycle` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/fsync-durability` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/ftruncate-sparse` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/ftruncate-sparse` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/ftruncate-sparse` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ftruncate-sparse` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/get-robust-list-child` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
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
| `c-programs/getcpu-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/getitimer-determinism-probe` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/getitimer-determinism-probe` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/getitimer-determinism-probe` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/getitimer-determinism-probe` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/getpriority-identity` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/getpriority-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/getpriority-identity` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/getpriority-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/getpriority-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/getrusage-self-accounting` | `verify` | `liteinst` | `Not selected by full` | `measured-no-verdict` |
| `c-programs/getrusage-self-accounting` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/getrusage-self-accounting` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/getsockopt-null` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/getsockopt-null` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/getsockopt-null` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/getsockopt-null` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/hardware-trap-identity` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/hardware-trap-identity` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
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
| `c-programs/host-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/inline-syscall-sites` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/inline-syscall-sites` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/inline-syscall-sites` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/inline-syscall-sites` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/inotify-watch` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/inotify-watch` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/inotify-watch` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/ioctl-fionread` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/kcmp-refusal` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/linkat-flags` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/lseek-positioning` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/lseek-positioning` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/lseek-positioning` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/mce-kill-refusal` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/membarrier-query` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/membarrier-query` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/membarrier-query` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/membarrier-query` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/memfd-create` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/memfd-create` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/memfd-create` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/memfd-create` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/mempolicy-default` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/mincore-residency` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mincore-residency` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/mincore-residency` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mincore-residency` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/mixed-inline-and-libc-syscalls` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mixed-inline-and-libc-syscalls` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/mixed-inline-and-libc-syscalls` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mixed-inline-and-libc-syscalls` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/mkdir-rmdir` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mkdir-rmdir` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/mkdir-rmdir` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mkdir-rmdir` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/mknod-special` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mknod-special` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/mknod-special` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mknod-special` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/mmap-determinism` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism-heap` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism-heap` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/mmap-determinism-heap` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism-heap` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/mmap-determinism-shared` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism-shared` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/mmap-determinism-shared` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-determinism-shared` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/mmap-layout-pointer-order` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-layout-pointer-order` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-layout-pointer-order` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-layout-pointer-order` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/mmap-stress-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-stress-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/mmap-stress-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/mmap-stress-determinism` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/msync-writeback` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/msync-writeback` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/msync-writeback` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/name-to-handle-refusal` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/no-new-privs-refusal` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/numa-node-identity` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/numa-node-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/numa-node-identity` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/numa-node-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/numa-node-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/o-tmpfile-anon` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/o-tmpfile-anon` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/o-tmpfile-anon` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/o-tmpfile-anon` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/openat-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/openat-flags` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/openat-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/openat-flags` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/openat2-refusal` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/openat2-refusal` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/openat2-refusal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/openat2-refusal` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/path-file-ops` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/path-file-ops` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/path-file-ops` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/path-file-ops` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/pause-alarm-interrupt` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/pause-alarm-interrupt` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pause-alarm-interrupt` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/personality-domain` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/pidfd-open-self-pair` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/pidfd-poll-self` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-poll-self` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/pidfd-poll-self` | `verify` | `ptrace` | `Selected by full` | `diverged-unlocated` |
| `c-programs/pidfd-poll-self` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-waitid-child` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/pidfd-waitid-child` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pidfd-waitid-child` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `c-programs/pipe-capacity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-capacity` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/pipe-capacity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-capacity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/pipe-capacity-pin` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-capacity-pin` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/pipe-capacity-pin` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pipe-capacity-pin` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/pipe2-flags` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/poll-readiness` | `replay` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/poll-readiness` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/poll-readiness` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/poll-readiness` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/poll-readiness` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/prctl-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/prctl-option-policy` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-option-policy` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/prctl-option-policy` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-option-policy` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/prctl-pdeathsig` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-pdeathsig` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/prctl-pdeathsig` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/pread64-nostdlib` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/pread64-nostdlib` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `c-programs/pread64-nostdlib` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/pread64-nostdlib` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/preadv2-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/preadv2-flags` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/preadv2-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/preadv2-flags` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/print-memaddrs` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/print-memaddrs` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/print-memaddrs` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/print-memaddrs` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/printf-with-threads` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/printf-with-threads` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/printf-with-threads` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `c-programs/proc-fd-link-aliases` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/proc-fd-link-aliases` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/proc-fd-link-aliases` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/proc-fdinfo` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/proc-fdinfo` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/proc-fdinfo` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/ptrace-attach-eperm` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `c-programs/ptrace-eperm` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/ptrace-eperm` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/ptrace-eperm` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/ptrace-eperm` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ptrace-eperm` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/ptrace-seize-eperm` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/ptrace-seize-eperm` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/ptrace-seize-eperm` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/ptrace-seize-eperm` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
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
| `c-programs/random-sources-root-only` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/rcx-canonicalization` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/rcx-canonicalization` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/rcx-canonicalization` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/rcx-canonicalization` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/readdir-entries` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/readdir-entries` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/readdir-entries` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/readdir-entries` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/readdir-order-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/readdir-order-identity` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/readdir-order-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/readdir-order-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/record-lock` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/record-lock` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/record-lock` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/remap-file-pages-tmpfile-enosys` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
| `c-programs/rename-ops` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/rename-ops` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/rename-ops` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/rename-ops` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/renameat2-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/renameat2-flags` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/renameat2-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/renameat2-flags` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/rlimit-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/robust-list` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/robust-list` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/robust-list` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/robust-list` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/sched-getaffinity-identity` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/sched-getaffinity-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-getaffinity-identity` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-getaffinity-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sched-getaffinity-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/seccomp-refusal` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/sendfile-copy` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sendfile-copy` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/sendfile-copy` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sendfile-copy` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/session-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/session-identity` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/session-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/session-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/set-tid-address` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/set-tid-address` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/set-tid-address` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/set-tid-address` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/setitimer-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/setitimer-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/setitimer-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/setitimer-determinism` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/short-io-split-identity` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `c-programs/short-io-split-identity` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/short-io-split-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/short-io-split-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/shutdown-socketpair` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/shutdown-socketpair` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/shutdown-socketpair` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/shutdown-socketpair` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigaction-state` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigaction-state` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigaction-state` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigaction-state` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigaltstack-state` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigaltstack-state` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigaltstack-state` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigaltstack-state` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigmask-preemption` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigmask-preemption` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigmask-preemption` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `c-programs/signal-delivery-sequence` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/signal-delivery-sequence` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/signal-delivery-sequence` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/signal-determinism` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/signal-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/signal-determinism` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/signal-disposition` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/signal-disposition` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/signal-disposition` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/signal-disposition` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/signal-waitstatus-identity` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/signal-waitstatus-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/signal-waitstatus-identity` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/signalfd-create` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/signalfd-create` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/signalfd-create` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/signalfd-create` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigpipe-siginfo` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigpipe-siginfo` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigpipe-siginfo` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigprocmask-state` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigprocmask-state` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/sigprocmask-state` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sigprocmask-state` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/so-incoming-cpu-tcp4` | `verify` | `sabre` | `Not selected by full` | `measured-and-passed` |
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
| `c-programs/socket-epoll-ordering` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/socket-ioctl-timestamp` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-ioctl-timestamp` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-ioctl-timestamp` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/socket-options` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-options` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/socket-options` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-options` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/socket-timestamp-edge-cases` | `verify` | `liteinst` | `Not selected by full` | `measured-no-verdict` |
| `c-programs/socket-timestamp-edge-cases` | `verify` | `ptrace` | `Not selected by full` | `measured-no-verdict` |
| `c-programs/socket-timestamp-edge-cases` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `c-programs/socket-timestamp-timespec` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-timestamp-timespec` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/socket-timestamp-timespec` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-timestamp-timespec` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/socket-timestamp-timeval` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-timestamp-timeval` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/socket-timestamp-timeval` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socket-timestamp-timeval` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/socketpair-flags` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/socketpair-flags` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/socketpair-flags` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/socketpair-flags` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/sockname-unnamed` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sockname-unnamed` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/sockname-unnamed` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sockname-unnamed` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/splice-enosys` | `verify` | `dbt` | `Not selected by full` | `measured-and-passed` |
| `c-programs/splice-enosys` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/splice-enosys` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/splice-enosys` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/splice-enosys` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/stat-metadata-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/stat-metadata-identity` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/stat-metadata-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/stat-metadata-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/statfs-free-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/statfs-free-determinism` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/statfs-free-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/statfs-free-determinism` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/statx-metadata` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/symlink-ops` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/symlink-ops` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/symlink-ops` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/symlink-ops` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/sync-file-range` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/sync-file-range` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/sync-file-range` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/sync-file-range` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/sysinfo` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/sysv-ipc-refusal` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/thp-disable` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/thp-disable` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/thp-disable` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/umask-mode` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `c-programs/umask-mode` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/umask-mode` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/uname` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/uname` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `c-programs/uname` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/uname` | `verify` | `sabre` | `Selected by full` | `measured-and-passed` |
| `c-programs/uname-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/uname-identity` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/uname-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/uname-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/utimensat-determinism` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/vectored-file-io` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/vectored-file-io` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/vectored-file-io` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/vectored-file-io` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `c-programs/vectored-io` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `c-programs/vectored-io` | `verify` | `liteinst` | `Selected by full` | `diverged` |
| `c-programs/vectored-io` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `c-programs/vectored-io` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `c-programs/wait-on-child` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `compat/arch` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/as` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/awk` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/b2sum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/b2sum` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/base32` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/base32` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/base64` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/base64` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/basename` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/basename` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/basenc` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/bash` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/bash` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/bc` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/bracket` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/bracket` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/bzip2` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/bzip2-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cal` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cal` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/cargo` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cat` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cat` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/chmod` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/chown` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/chrt` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cksum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cksum` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
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
| `compat/curl` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/curl-localhost` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cut` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/cxxfilt` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/date` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/date` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/dc` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/dd` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/df` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/df-direct` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/df-direct` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/diff` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/diff3` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/dirname` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/dirname` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/dos2unix` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/du` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/du` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/echo` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/echo` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/egrep` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/elfedit` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/env` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/envsubst` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/expand` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/expr` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/expr` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/factor` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/fallocate` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/fgrep` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/file` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/find` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/findmnt` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/findmnt` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/flex` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/flock` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/fmt` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/fold` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/free` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/free` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/gcc` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/gcov` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/getconf` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/getopt` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/getopt` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/git` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/gprof` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/grep` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/groups` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/groups` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/gxx` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/gzip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/gzip-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/head` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/head` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/hexdump` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/hostname` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/hostname` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/iconv` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/id` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/id` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/install` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/ionice` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/iostat` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/ip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/join` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/jq` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/kill` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/kill` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/ld` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/ln` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/logger` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/logname` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/ls` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/ls` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/lscpu` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/lsirq` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/lsirq` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/lsmod` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/lsmod` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/lsof` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/lua` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/lua-direct` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/lua-direct` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/m4` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/make` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/md5sum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/md5sum` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
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
| `compat/nproc` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/numactl-hardware` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/numactl-hardware` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/numastat` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/numastat` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/numfmt` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/numfmt` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/objcopy` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/objdump` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/od` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/openssl` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/openssl` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/paste` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/patch` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pathchk` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/perl` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/perl-direct` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/perl-direct` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/pgrep` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pidstat-disk` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pidstat-disk` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/pinky` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pinky` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/pkg-config` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pkill` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pr` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pr` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/printenv` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/printf` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/printf` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/ps` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/ps` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/ptx` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pwd` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/pwd` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/python3` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/python3` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/ranlib` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `compat/readelf` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/readlink` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/readlink` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/realpath` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/realpath` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
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
| `compat/ruby` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/rustc` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/sar-resource-tables` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sed` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/seq` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/setfacl` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/setfattr` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/sha1sum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sha1sum` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/sha224sum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sha224sum` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/sha256sum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sha256sum` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/sha384sum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sha384sum` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/sha512sum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sha512sum` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/shell-build` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `compat/shred` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/shuf` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/size` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sleep` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sleep` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/sort` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/split` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sqlite3` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/ss` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/stat` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/stat` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/stdbuf` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/strings` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/strip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sum` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sum` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/sync` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/sysctl-random-uuid` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/sysctl-random-uuid` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/tac` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/tar` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/tar-roundtrip` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/taskset` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/tcl` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/tee` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/test` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/test` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/time` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/timeout` | `verify` | `ptrace` | `Not selected by full` | `measured-no-verdict` |
| `compat/top` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/touch` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/tr` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/true` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/true` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/truncate` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/tsort` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/tty` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/uname` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/uname` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/unexpand` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/uniq` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/uptime` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/uptime` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/users` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/users` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/uuidgen` | `verify` | `ptrace` | `Not selected by full` | `measured-and-passed` |
| `compat/vmstat` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/vmstat` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/vmstat-disk` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/vmstat-disk` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/wc` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/wc` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/wc-lines` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/wc-lines` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
| `compat/wget-localhost` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/whoami` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `compat/whoami` | `verify` | `sabre` | `Not selected by full` | `measured-no-verdict` |
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
| `determinism-stress-c/pipe-prefill` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/producer-consumer` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/producer-consumer` | `verify` | `ptrace` | `Selected by full` | `diverged` |
| `determinism-stress-c/signal-order` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `determinism-stress-c/signal-order` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `determinism-stress-c/thread-contention` | `chaos` | `ptrace` | `Selected by full` | `measured-and-passed` |
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
| `language-runtimes/python-hashseed` | `verify` | `ptrace` | `Not selected by full` | `diverged` |
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
| `language-runtimes/rust-hashmap-iteration` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/tcl-rand-clock` | `verify` | `kvm` | `Selected by full` | `diverged` |
| `language-runtimes/tcl-rand-clock` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `language-runtimes/tcl-rand-clock` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `shared-futex-c/qemu-exec-init` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `shared-futex-c/qemu-exec-init` | `verify` | `ptrace` | `Not selected by full` | `measured-no-verdict` |
| `shared-futex-c/qemu-exec-init` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `shared-futex-c/qemu-hello` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `shared-futex-c/qemu-hello` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `shared-futex-c/qemu-init` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `shared-futex-c/qemu-init` | `verify` | `ptrace` | `Not selected by full` | `measured-no-verdict` |
| `shared-futex-c/qemu-init` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `shared-futex-c/qemu-net-init` | `verify` | `liteinst` | `Not applicable` | `measured-no-verdict` |
| `shared-futex-c/qemu-net-init` | `verify` | `ptrace` | `Not selected by full` | `measured-no-verdict` |
| `shared-futex-c/qemu-net-init` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/auxv-loader-dump` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/auxv-loader-dump` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/auxv-loader-dump` | `verify` | `sabre` | `Not applicable` | `measured-no-verdict` |
| `system-utils/cat-file-read` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `system-utils/cat-file-read` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/cat-file-read` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/cat-file-read` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `system-utils/clock-determinism` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `system-utils/clock-determinism` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/clock-determinism` | `verify` | `liteinst` | `Selected by full` | `measured-and-passed` |
| `system-utils/clock-determinism` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/clock-determinism` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `system-utils/echo-stdout` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `system-utils/errno-path-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/errno-path-identity` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/errno-path-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/errno-path-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `system-utils/example-date` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/example-date` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/example-date` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/example-devrand` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/example-devrand` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/example-devrand` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/file-timestamp-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/file-timestamp-identity` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/file-timestamp-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/file-timestamp-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `system-utils/find-tree-metadata` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/find-tree-metadata` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/find-tree-metadata` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/find-tree-metadata` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `system-utils/printf-argument-forwarding` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `system-utils/proc-random-uuid` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/proc-random-uuid` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/proc-uptime` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/proc-uptime` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/proc-uptime` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/procfs-sanitized-paths` | `verify` | `ptrace` | `Not selected by full` | `diverged` |
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
| `system-utils/sh-exit-status` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/sh-exit-status` | `verify` | `sabre` | `Not applicable` | `diverged` |
| `system-utils/shm-coherency-identity` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/shm-coherency-identity` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/shm-coherency-identity` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/shm-coherency-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
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
| `system-utils/startup-surface-identity` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `system-utils/startup-tls-guards` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/startup-tls-guards` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/startup-tls-guards` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/startup-tls-guards` | `verify` | `sabre` | `Not selected by full` | `diverged` |
| `system-utils/true-exit-zero` | `verify` | `dbt` | `Selected by full` | `measured-and-passed` |
| `system-utils/true-exit-zero` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/true-exit-zero` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `system-utils/true-exit-zero` | `verify` | `sabre` | `Not applicable` | `measured-and-passed` |
| `system-utils/uuidgen-random` | `verify` | `kvm` | `Selected by full` | `measured-and-passed` |
| `system-utils/uuidgen-random` | `verify` | `liteinst` | `Not applicable` | `measured-and-passed` |
| `system-utils/uuidgen-random` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
| `util-c/pmu-skid` | `verify` | `ptrace` | `Selected by full` | `measured-and-passed` |
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


### validate run `validate-buck-re-3-cf2a94ea38d3-1791063611629487524-833061-4b45d912` at `cf2a94ea38d3`

`parity: 0/206 matched; selected 206 of 206 committed; mean n/a over 0 measured; floor 0.000 over 190 of 206 selected (counted as 0: 190 record-missing or refused; excluded: 0 no golden; 0 not compared; 16 record-missing or refused whose inputs cannot be equalized)`

| Candidate backend | Selected | Measured | Matched | Diverged | No golden | Not compared | Unmeasured | Record-missing | Refused | Mean credit (measured) | Floor credit | Credit inputs |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | --- |
| `dbt` | 16 of 16 | 0 | 0 | 0 | 0 | 0 | 0 | 16 | 0 | n/a | n/a | — |
| `kvm` | 89 of 89 | 0 | 0 | 0 | 0 | 0 | 0 | 89 | 0 | n/a | 0.000 | — |
| `liteinst` | 99 of 99 | 0 | 0 | 0 | 0 | 0 | 0 | 99 | 0 | n/a | 0.000 | — |
| `sabre` | 2 of 2 | 0 | 0 | 0 | 0 | 0 | 0 | 2 | 0 | n/a | 0.000 | — |
| **TOTAL** | 206 of 206 | 0 | 0 | 0 | 0 | 0 | 0 | 206 | 0 | n/a | 0.000 | — |

Every cell that did not match, with its first divergence or the reason it was not measured:

| Cell | Verdict | Credit | First divergence or reason |
| --- | --- | ---: | --- |
| `c-programs/aio-refusal@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/aio-refusal@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/append-pwrite@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/append-pwrite@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/bind-getsockname@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/bind-getsockname@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/cachestat-refusal@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/cachestat-refusal@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/child-subreaper-refusal@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/child-subreaper-refusal@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/close-range-fds@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/close-range-fds@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/copy-file-range-refusal@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/copy-file-range-refusal@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/cpu-virtualization@dbt` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/cpu-virtualization@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/cpu-virtualization@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/cpuid-probe@dbt` | record-missing | — | no parity row: node privileged/manifest_c_programs left no parity.status.json |
| `c-programs/cpuid-probe@kvm` | record-missing | — | no parity row: node privileged/manifest_c_programs left no parity.status.json |
| `c-programs/cpuid-probe@liteinst` | record-missing | — | no parity row: node privileged/manifest_c_programs left no parity.status.json |
| `c-programs/cwd-roundtrip@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/cwd-roundtrip@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/dup-shared-offset@dbt` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/dup-shared-offset@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/dup-shared-offset@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/epoll-pwait2@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/epoll-pwait2@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/epoll-readiness@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/epoll-readiness@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/event-delivery-ordering@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/eventfd-semantics@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/eventfd-semantics@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/faccessat2-flags@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/faccessat2-flags@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/fadvise-hints@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/fadvise-hints@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/fallocate-extents@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/fallocate-extents@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/fchmod-bits@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/fchmod-bits@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/fchmodat2-flags@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/fchmodat2-flags@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/fcntl-owner@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/fd-duplication@dbt` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/fd-duplication@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/fd-duplication@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/file-backed-mmap@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/file-backed-mmap@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/file-io-roundtrip@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/file-io-roundtrip@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/flock-lifecycle@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/flock-lifecycle@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/fork-exec-pipeline@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/fsync-durability@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/fsync-durability@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/ftruncate-sparse@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/ftruncate-sparse@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/getcpu-identity@dbt` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/getcpu-identity@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/getcpu-identity@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/getpriority-identity@dbt` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/getpriority-identity@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/getpriority-identity@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/getrusage-self-accounting@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/hardware-trap-identity@dbt` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/hardware-trap-identity@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/host-identity@dbt` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/host-identity@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/inline-syscall-sites@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/inline-syscall-sites@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/inotify-watch@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/ioctl-fionread@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/ioctl-fionread@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/kcmp-refusal@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/kcmp-refusal@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/linkat-flags@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/linkat-flags@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/lseek-positioning@dbt` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/lseek-positioning@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/lseek-positioning@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/mce-kill-refusal@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/mce-kill-refusal@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/membarrier-query@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/membarrier-query@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/memfd-create@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/memfd-create@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/mempolicy-default@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/mempolicy-default@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/mincore-residency@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/mincore-residency@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/mixed-inline-and-libc-syscalls@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/mixed-inline-and-libc-syscalls@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/mkdir-rmdir@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/mkdir-rmdir@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/mknod-special@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/mknod-special@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/mmap-layout-pointer-order@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/mmap-layout-pointer-order@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/msync-writeback@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/name-to-handle-refusal@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/name-to-handle-refusal@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/no-new-privs-refusal@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/no-new-privs-refusal@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/numa-node-identity@dbt` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/numa-node-identity@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/numa-node-identity@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/o-tmpfile-anon@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/o-tmpfile-anon@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/openat-flags@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/openat-flags@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/openat2-refusal@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/openat2-refusal@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/path-file-ops@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/path-file-ops@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/personality-domain@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pid-probe@dbt` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pid-probe@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pid-probe@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pid-probe@sabre` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pidfd-open-self-pair@dbt` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pidfd-open-self-pair@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pidfd-open-self-pair@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pipe-capacity@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pipe-capacity@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pipe-capacity-pin@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pipe-capacity-pin@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pipe-ipc@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pipe-ipc@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pipe2-flags@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pipe2-flags@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/poll-readiness@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/poll-readiness@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/prctl-identity@dbt` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/prctl-identity@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/prctl-identity@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/prctl-pdeathsig@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/preadv2-flags@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/preadv2-flags@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pthread-lifecycle@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pthread-lifecycle@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/pthread-lifecycle@sabre` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/readdir-entries@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/readdir-entries@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/readdir-order-identity@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/readdir-order-identity@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/record-lock@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/rename-ops@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/rename-ops@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/renameat2-flags@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/renameat2-flags@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/rlimit-identity@dbt` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/rlimit-identity@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/rlimit-identity@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/robust-list@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/robust-list@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/sched-getaffinity-identity@dbt` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/sched-getaffinity-identity@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/sched-getaffinity-identity@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/seccomp-refusal@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/seccomp-refusal@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/sendfile-copy@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/sendfile-copy@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/set-tid-address@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/set-tid-address@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/short-io-split-identity@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/short-io-split-identity@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/shutdown-socketpair@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/shutdown-socketpair@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/signal-delivery-sequence@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/signal-waitstatus-identity@dbt` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/signal-waitstatus-identity@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/signalfd-create@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/signalfd-create@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/socket-epoll-ordering@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/socket-epoll-ordering@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/socket-options@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/socket-options@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/socketpair-flags@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/socketpair-flags@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/sockname-unnamed@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/sockname-unnamed@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/stat-metadata-identity@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/stat-metadata-identity@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/statfs-free-determinism@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/statfs-free-determinism@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/static-nolibc-syscall-sites@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/statx-metadata@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/statx-metadata@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/symlink-ops@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/symlink-ops@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/sync-file-range@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/sync-file-range@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/sysv-ipc-refusal@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/sysv-ipc-refusal@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/thp-disable@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/thp-disable@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/umask-mode@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/umask-mode@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/uname-identity@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/uname-identity@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/utimensat-determinism@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/utimensat-determinism@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/vectored-file-io@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/vectored-file-io@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/vectored-io@kvm` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |
| `c-programs/vectored-io@liteinst` | record-missing | — | no parity row: node portable/manifest_c_programs left no parity.status.json |

### pressure-test run `pressure-c470fa213ee8-20261003T180650Z` at `c470fa213ee8` (partial: selected 1 of 205 committed)

`parity: not compared: 0 measured of 1 selected (inputs cannot be equalized); selected 1 of 205 committed (partial)`

| Candidate backend | Selected | Measured | Matched | Diverged | No golden | Not compared | Unmeasured | Record-missing | Refused | Mean credit (measured) | Floor credit | Credit inputs |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | --- |
| `dbt` | 1 of 16 | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | n/a | n/a | not compared (inputs cannot be equalized) |
| `kvm` | 0 of 88 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | n/a | n/a | — |
| `liteinst` | 0 of 99 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | n/a | n/a | — |
| `sabre` | 0 of 2 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | n/a | n/a | — |
| **TOTAL** | 1 of 205 | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | n/a | n/a | not compared (inputs cannot be equalized) |

204 cell(s) its commit selects have no row in this run: `c-programs/aio-refusal@kvm`, `c-programs/aio-refusal@liteinst`, `c-programs/append-pwrite@kvm`, `c-programs/append-pwrite@liteinst`, `c-programs/bind-getsockname@kvm`, `c-programs/bind-getsockname@liteinst`, `c-programs/cachestat-refusal@kvm`, `c-programs/cachestat-refusal@liteinst`, `c-programs/child-subreaper-refusal@kvm`, `c-programs/child-subreaper-refusal@liteinst`, `c-programs/close-range-fds@kvm`, `c-programs/close-range-fds@liteinst`, `c-programs/copy-file-range-refusal@kvm`, `c-programs/copy-file-range-refusal@liteinst`, `c-programs/cpu-virtualization@dbt`, `c-programs/cpu-virtualization@kvm`, `c-programs/cpu-virtualization@liteinst`, `c-programs/cpuid-probe@dbt`, `c-programs/cpuid-probe@kvm`, `c-programs/cpuid-probe@liteinst` and 184 more (all are in `scorecard/parity.json`).

Every cell that did not match, with its first divergence or the reason it was not measured:

| Cell | Verdict | Credit | First divergence or reason |
| --- | --- | ---: | --- |
| `c-programs/prctl-identity@dbt` | inputs-not-equalized | — | the dbt backend refuses --bind and --mount (hermit-cli/src/bin/hermit/run.rs: "its DynamoRIO adapter does not enter the guest mount namespace"), so its guest cannot be given the ptrace cell's input paths |

Outside the clean headline: 0 parity rows from a dirty source tree.

Outside the clean headline: 0 parity rows that did not report their source tree state.

### 26 other parity run(s) in the store

Only a run from a clean source tree can be its producer's headline: at least one of its rows says `"source_tree_dirty": false`, and none says `true` or leaves the value out. A row refused for its own defect does not count; one refused only because its run's rows name more than one Hermit commit does. Among those runs, the headline is the run that reported every cell its own Hermit commit's selection owes; a partial run headlines only when no complete run exists, the most complete first. Then the deepest Hermit commit this checkout can place, then the latest emission.

- validate run `validate-buck-re-3-1931c6ccd8f7-1791054320749237313-288493-c38549d2` at Hermit `1931c6ccd8f7`: `parity: 0/205 matched; selected 205 of 205 committed; mean n/a over 0 measured; floor 0.000 over 189 of 205 selected (counted as 0: 189 record-missing or refused; excluded: 0 no golden; 0 not compared; 16 record-missing or refused whose inputs cannot be equalized)`
- validate run `validate-buck-re-3-5301e9a2dbb2-1791051975076983128-689340-69dc34f7` at Hermit `5301e9a2dbb2`: `parity: 0/205 matched; selected 205 of 205 committed; mean n/a over 0 measured; floor 0.000 over 189 of 205 selected (counted as 0: 189 record-missing or refused; excluded: 0 no golden; 0 not compared; 16 record-missing or refused whose inputs cannot be equalized)`
- validate run `validate-buck-re-3-75bec89e7db9-1791060150360793710-2018425-d3821d9a` at Hermit `75bec89e7db9`: `parity: 0/206 matched; selected 206 of 206 committed; mean n/a over 0 measured; floor 0.000 over 190 of 206 selected (counted as 0: 190 record-missing or refused; excluded: 0 no golden; 0 not compared; 16 record-missing or refused whose inputs cannot be equalized)`
- validate run `validate-buck-re-3-7831ad1c8931-1791056799838919343-1002648-6b3c5041` at Hermit `7831ad1c8931`: `parity: 0/205 matched; selected 205 of 205 committed; mean 0.055 over 186 measured; floor 0.055 over 189 of 205 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-buck-re-3-84821e53edb7-1791041721981051099-2446052-bf662192` at Hermit `84821e53edb7`: `parity: 0/204 matched; selected 204 of 204 committed; mean n/a over 0 measured; floor 0.000 over 188 of 204 selected (counted as 0: 188 record-missing or refused; excluded: 0 no golden; 0 not compared; 16 record-missing or refused whose inputs cannot be equalized)`
- validate run `validate-buck-re-3-ccccde5f65d6-1791044441627223683-4139778-3d0a278f` at Hermit `ccccde5f65d6`: `parity: 0/204 matched; selected 204 of 204 committed; mean n/a over 0 measured; floor 0.000 over 188 of 204 selected (counted as 0: 188 record-missing or refused; excluded: 0 no golden; 0 not compared; 16 record-missing or refused whose inputs cannot be equalized)`
- validate run `validate-buck-re-3-e81ace1c0be7-1791057684904978741-1891131-b31df65f` at Hermit `e81ace1c0be7`: `parity: 0/205 matched; selected 205 of 205 committed; mean n/a over 0 measured; floor 0.000 over 189 of 205 selected (counted as 0: 189 record-missing or refused; excluded: 0 no golden; 0 not compared; 16 record-missing or refused whose inputs cannot be equalized)`
- validate run `validate-claude-2-118f0bfc4553-1791028466715093346-2549334-529fe13e` at Hermit `118f0bfc4553`: `parity: 0/203 matched; committed selection unknown; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-2-4346217e08d4-1791039676390384316-4107946-e2cf462e` at Hermit `4346217e08d4`: `parity: 0/204 matched; selected 204 of 204 committed; mean 0.055 over 185 measured; floor 0.054 over 188 of 204 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-2-6b5424c1860a-1791037904455844226-3351413-5eebd499` at Hermit `6b5424c1860a`: `parity: 0/204 matched; selected 204 of 204 committed; mean 0.055 over 185 measured; floor 0.054 over 188 of 204 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-2-b6ca81b78be8-1791031592862600986-362222-8863246d` at Hermit `b6ca81b78be8`: `parity: 0/203 matched; selected 203 of 203 committed; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-2-cfb9d0c86427-1791035312332954244-2326456-5ddbba2a` at Hermit `cfb9d0c86427`: `parity: 0/203 matched; committed selection unknown; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-4-ad4f9cf598a3-1791030814588067897-4023945-e9bb1627` at Hermit `ad4f9cf598a3`: `parity: 0/203 matched; committed selection unknown; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-claude-4-d53ea6ca1711-1791032694966120621-1230314-61570ac3` at Hermit `d53ea6ca1711`: `parity: 0/203 matched; committed selection unknown; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-coord-itvfix-57883eff8f3d-1791046213891728858-2276425-f9271a9e` at Hermit `57883eff8f3d`: `parity: 0/204 matched; selected 204 of 204 committed; mean 0.055 over 185 measured; floor 0.054 over 188 of 204 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-d14-queue-a2b1deecaa21-1791037234447726839-773804-79469e5b` at Hermit `a2b1deecaa21`: `parity: 0/203 matched; selected 203 of 203 committed; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-gate-select-9e7dd6e33cf6-1791027490982151389-186043-c4a5cdf6` at Hermit `9e7dd6e33cf6`: `parity: 0/203 matched; selected 203 of 203 committed; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-hermit-lander-33f1c939a2e4-1791054196460293033-32690-ce1f4789` at Hermit `33f1c939a2e4`: `parity: 0/205 matched; selected 205 of 205 committed; mean 0.055 over 186 measured; floor 0.055 over 189 of 205 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-0f028322361f-75248bd48fcd` at Hermit `0f028322361f`: `parity: 0/203 matched; selected 203 of 203 committed; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-0f028322361f-a6fa2a143efd` at Hermit `0f028322361f`: `parity: 0/203 matched; selected 203 of 203 committed; mean 0.055 over 184 measured; floor 0.054 over 187 of 203 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-20077ee5b19f-63f1efea5c13` at Hermit `20077ee5b19f`: `parity: 0/205 matched; selected 205 of 205 committed; mean 0.055 over 186 measured; floor 0.055 over 189 of 205 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-a015bbb6029b-c266b0f86c0b` at Hermit `a015bbb6029b`: `parity: 0/206 matched; selected 206 of 206 committed; mean 0.056 over 187 measured; floor 0.055 over 190 of 206 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-a2b1deecaa21-3af8d38d7b78` at Hermit `a2b1deecaa21`: `parity: 0/203 matched; selected 203 of 203 committed; mean 0.055 over 184 measured; floor 0.054 over 186 of 203 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 1 no golden: ended 1; 16 not compared)`
- validate run `validate-ops-tick-a8ee9d5a5733-08a2e5aca2da` at Hermit `a8ee9d5a5733`: `parity: 0/206 matched; selected 206 of 206 committed; mean 0.056 over 187 measured; floor 0.055 over 190 of 206 selected (counted as 0: 3 unmeasured: no-result-row 3; excluded: 0 no golden; 16 not compared)`
- validate run `validate-ops-tick-a8ee9d5a5733-a07460befccd` at Hermit `a8ee9d5a5733`: `parity: 0/206 matched; selected 206 of 206 committed; mean 0.056 over 187 measured; floor 0.055 over 189 of 206 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 1 no golden: ended 1; 16 not compared)`
- validate run `validate-ops-tick-e02d9f8368a3-03a15e241a7e` at Hermit `e02d9f8368a3`: `parity: 0/205 matched; selected 205 of 205 committed; mean 0.055 over 186 measured; floor 0.055 over 188 of 205 selected (counted as 0: 2 unmeasured: no-result-row 2; excluded: 1 no golden: ended 1; 16 not compared)`

### legacy-rerun history

`legacy-rerun (retired ptrace rerun on 173 cell(s), last at 5ee668223a15): 0 matched / 173 diverged`

These verdicts came from the retired ptrace rerun, which ran each candidate beside a fresh ptrace run. They are kept as labelled history only: they are not current parity, they carry no credit, and they enter none of the counts above. The `Determinism` column is the cell's own measurement, which parity never changes.

1211 older comparison(s) of the same cells were superseded by a later one.

Retired rerun evidence not kept as history, by reason: `no-retained-comparison` 4322 on 173 cell(s).

| Cell | Label | Legacy verdict | Compared records | First divergent record | Hermit commit | Determinism | Current parity |
| --- | --- | --- | ---: | ---: | --- | --- | --- |
| `c-programs/aio-refusal@kvm` | legacy-rerun | diverged | 107 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/aio-refusal@liteinst` | legacy-rerun | diverged | 107 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/append-pwrite@kvm` | legacy-rerun | diverged | 144 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/append-pwrite@liteinst` | legacy-rerun | diverged | 152 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/bind-getsockname@kvm` | legacy-rerun | diverged | 110 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/bind-getsockname@liteinst` | legacy-rerun | diverged | 110 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/cachestat-refusal@kvm` | legacy-rerun | diverged | 124 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/cachestat-refusal@liteinst` | legacy-rerun | diverged | 132 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/child-subreaper-refusal@kvm` | legacy-rerun | diverged | 103 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/child-subreaper-refusal@liteinst` | legacy-rerun | diverged | 103 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/close-range-fds@kvm` | legacy-rerun | diverged | 137 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/close-range-fds@liteinst` | legacy-rerun | diverged | 145 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/copy-file-range-refusal@kvm` | legacy-rerun | diverged | 139 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/copy-file-range-refusal@liteinst` | legacy-rerun | diverged | 143 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/cpu-virtualization@kvm` | legacy-rerun | diverged | 104 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/cpu-virtualization@liteinst` | legacy-rerun | diverged | 104 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/cwd-roundtrip@kvm` | legacy-rerun | diverged | 133 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/cwd-roundtrip@liteinst` | legacy-rerun | diverged | 141 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/dup-shared-offset@kvm` | legacy-rerun | diverged | 148 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/dup-shared-offset@liteinst` | legacy-rerun | diverged | 156 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/epoll-pwait2@liteinst` | legacy-rerun | diverged | 125 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/epoll-readiness@kvm` | legacy-rerun | diverged | 123 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/epoll-readiness@liteinst` | legacy-rerun | diverged | 123 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/event-delivery-ordering@liteinst` | legacy-rerun | diverged | 190 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/eventfd-semantics@kvm` | legacy-rerun | diverged | 169 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/eventfd-semantics@liteinst` | legacy-rerun | diverged | 169 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/faccessat2-flags@kvm` | legacy-rerun | diverged | 132 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/faccessat2-flags@liteinst` | legacy-rerun | diverged | 140 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/fadvise-hints@kvm` | legacy-rerun | diverged | 124 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/fadvise-hints@liteinst` | legacy-rerun | diverged | 132 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/fallocate-extents@kvm` | legacy-rerun | diverged | 130 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/fallocate-extents@liteinst` | legacy-rerun | diverged | 138 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/fchmod-bits@kvm` | legacy-rerun | diverged | 131 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/fchmod-bits@liteinst` | legacy-rerun | diverged | 139 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/fchmodat2-flags@liteinst` | legacy-rerun | diverged | 139 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/fcntl-owner@liteinst` | legacy-rerun | diverged | 121 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/fd-duplication@kvm` | legacy-rerun | diverged | 163 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/fd-duplication@liteinst` | legacy-rerun | diverged | 171 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/file-backed-mmap@kvm` | legacy-rerun | diverged | 133 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/file-backed-mmap@liteinst` | legacy-rerun | diverged | 141 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/file-io-roundtrip@kvm` | legacy-rerun | diverged | 148 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/file-io-roundtrip@liteinst` | legacy-rerun | diverged | 148 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/flock-lifecycle@kvm` | legacy-rerun | diverged | 121 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/flock-lifecycle@liteinst` | legacy-rerun | diverged | 129 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/fork-exec-pipeline@kvm` | legacy-rerun | diverged | 337 | 13 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/fsync-durability@liteinst` | legacy-rerun | diverged | 132 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/ftruncate-sparse@kvm` | legacy-rerun | diverged | 135 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/ftruncate-sparse@liteinst` | legacy-rerun | diverged | 143 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/getcpu-identity@kvm` | legacy-rerun | diverged | 123 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/getcpu-identity@liteinst` | legacy-rerun | diverged | 123 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/getpriority-identity@kvm` | legacy-rerun | diverged | 113 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/getpriority-identity@liteinst` | legacy-rerun | diverged | 113 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/hardware-trap-identity@liteinst` | legacy-rerun | diverged | 312 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/host-identity@liteinst` | legacy-rerun | diverged | 117 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/inline-syscall-sites@kvm` | legacy-rerun | diverged | 165 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/inline-syscall-sites@liteinst` | legacy-rerun | diverged | 165 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/inotify-watch@liteinst` | legacy-rerun | diverged | 110 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/ioctl-fionread@liteinst` | legacy-rerun | diverged | 120 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/kcmp-refusal@kvm` | legacy-rerun | diverged | 111 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/kcmp-refusal@liteinst` | legacy-rerun | diverged | 111 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/linkat-flags@kvm` | legacy-rerun | diverged | 146 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/linkat-flags@liteinst` | legacy-rerun | diverged | 154 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/lseek-positioning@kvm` | legacy-rerun | diverged | 143 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/lseek-positioning@liteinst` | legacy-rerun | diverged | 151 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/mce-kill-refusal@kvm` | legacy-rerun | diverged | 103 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/mce-kill-refusal@liteinst` | legacy-rerun | diverged | 103 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/membarrier-query@kvm` | legacy-rerun | diverged | 105 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/membarrier-query@liteinst` | legacy-rerun | diverged | 105 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/memfd-create@kvm` | legacy-rerun | diverged | 121 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/memfd-create@liteinst` | legacy-rerun | diverged | 121 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/mempolicy-default@kvm` | legacy-rerun | diverged | 107 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/mempolicy-default@liteinst` | legacy-rerun | diverged | 107 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/mincore-residency@kvm` | legacy-rerun | diverged | 119 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/mincore-residency@liteinst` | legacy-rerun | diverged | 119 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/mixed-inline-and-libc-syscalls@kvm` | legacy-rerun | diverged | 147 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/mixed-inline-and-libc-syscalls@liteinst` | legacy-rerun | diverged | 147 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/mkdir-rmdir@kvm` | legacy-rerun | diverged | 129 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/mkdir-rmdir@liteinst` | legacy-rerun | diverged | 137 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/mknod-special@kvm` | legacy-rerun | diverged | 133 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/mknod-special@liteinst` | legacy-rerun | diverged | 141 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/mmap-layout-pointer-order@kvm` | legacy-rerun | diverged | 133 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/mmap-layout-pointer-order@liteinst` | legacy-rerun | diverged | 141 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/msync-writeback@liteinst` | legacy-rerun | diverged | 137 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/name-to-handle-refusal@kvm` | legacy-rerun | diverged | 103 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/name-to-handle-refusal@liteinst` | legacy-rerun | diverged | 103 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/no-new-privs-refusal@kvm` | legacy-rerun | diverged | 105 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/no-new-privs-refusal@liteinst` | legacy-rerun | diverged | 105 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/numa-node-identity@kvm` | legacy-rerun | diverged | 111 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/numa-node-identity@liteinst` | legacy-rerun | diverged | 111 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/o-tmpfile-anon@kvm` | legacy-rerun | diverged | 120 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/o-tmpfile-anon@liteinst` | legacy-rerun | diverged | 120 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/openat-flags@kvm` | legacy-rerun | diverged | 160 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/openat-flags@liteinst` | legacy-rerun | diverged | 168 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/openat2-refusal@kvm` | legacy-rerun | diverged | 105 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/openat2-refusal@liteinst` | legacy-rerun | diverged | 105 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/path-file-ops@kvm` | legacy-rerun | diverged | 141 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/path-file-ops@liteinst` | legacy-rerun | diverged | 149 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/personality-domain@liteinst` | legacy-rerun | diverged | 109 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/pid-probe@kvm` | legacy-rerun | diverged | 101 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/pid-probe@liteinst` | legacy-rerun | diverged | 101 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/pid-probe@sabre` | legacy-rerun | diverged | 32 | 3 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/pidfd-open-self-pair@kvm` | legacy-rerun | diverged | 117 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/pidfd-open-self-pair@liteinst` | legacy-rerun | diverged | 117 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/pipe-capacity-pin@kvm` | legacy-rerun | diverged | 111 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/pipe-capacity-pin@liteinst` | legacy-rerun | diverged | 111 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/pipe-capacity@kvm` | legacy-rerun | diverged | 113 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/pipe-capacity@liteinst` | legacy-rerun | diverged | 113 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/pipe-ipc@kvm` | legacy-rerun | diverged | 158 | 13 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/pipe-ipc@liteinst` | legacy-rerun | diverged | 164 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/pipe2-flags@kvm` | legacy-rerun | diverged | 163 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/pipe2-flags@liteinst` | legacy-rerun | diverged | 163 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/poll-readiness@liteinst` | legacy-rerun | diverged | 139 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/prctl-identity@liteinst` | legacy-rerun | diverged | 121 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/prctl-pdeathsig@liteinst` | legacy-rerun | diverged | 111 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/preadv2-flags@liteinst` | legacy-rerun | diverged | 140 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/pthread-lifecycle@liteinst` | legacy-rerun | diverged | 214 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/readdir-entries@kvm` | legacy-rerun | diverged | 159 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/readdir-entries@liteinst` | legacy-rerun | diverged | 167 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/readdir-order-identity@kvm` | legacy-rerun | diverged | 13669 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/readdir-order-identity@liteinst` | legacy-rerun | diverged | 13677 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/record-lock@liteinst` | legacy-rerun | diverged | 131 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/rename-ops@kvm` | legacy-rerun | diverged | 163 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/rename-ops@liteinst` | legacy-rerun | diverged | 171 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/renameat2-flags@kvm` | legacy-rerun | diverged | 184 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/renameat2-flags@liteinst` | legacy-rerun | diverged | 192 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/rlimit-identity@kvm` | legacy-rerun | diverged | 121 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/rlimit-identity@liteinst` | legacy-rerun | diverged | 121 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/robust-list@kvm` | legacy-rerun | diverged | 105 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/robust-list@liteinst` | legacy-rerun | diverged | 105 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/sched-getaffinity-identity@kvm` | legacy-rerun | diverged | 111 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/sched-getaffinity-identity@liteinst` | legacy-rerun | diverged | 111 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/seccomp-refusal@kvm` | legacy-rerun | diverged | 103 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/seccomp-refusal@liteinst` | legacy-rerun | diverged | 103 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/sendfile-copy@kvm` | legacy-rerun | diverged | 155 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/sendfile-copy@liteinst` | legacy-rerun | diverged | 159 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/set-tid-address@kvm` | legacy-rerun | diverged | 107 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/set-tid-address@liteinst` | legacy-rerun | diverged | 107 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/short-io-split-identity@kvm` | legacy-rerun | diverged | 431 | 13 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/short-io-split-identity@liteinst` | legacy-rerun | diverged | 435 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/shutdown-socketpair@kvm` | legacy-rerun | diverged | 125 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/shutdown-socketpair@liteinst` | legacy-rerun | diverged | 125 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/signal-delivery-sequence@liteinst` | legacy-rerun | diverged | 997 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/signal-waitstatus-identity@liteinst` | legacy-rerun | diverged | 386 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/signalfd-create@kvm` | legacy-rerun | diverged | 113 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/signalfd-create@liteinst` | legacy-rerun | diverged | 113 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/socket-epoll-ordering@kvm` | legacy-rerun | diverged | 250 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/socket-epoll-ordering@liteinst` | legacy-rerun | diverged | 250 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/socket-options@liteinst` | legacy-rerun | diverged | 131 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/socketpair-flags@liteinst` | legacy-rerun | diverged | 119 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/sockname-unnamed@liteinst` | legacy-rerun | diverged | 113 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/stat-metadata-identity@kvm` | legacy-rerun | diverged | 233 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/stat-metadata-identity@liteinst` | legacy-rerun | diverged | 233 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/statfs-free-determinism@kvm` | legacy-rerun | diverged | 109 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/statfs-free-determinism@liteinst` | legacy-rerun | diverged | 109 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/static-nolibc-syscall-sites@kvm` | legacy-rerun | diverged | 76 | 64 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/statx-metadata@kvm` | legacy-rerun | diverged | 123 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/statx-metadata@liteinst` | legacy-rerun | diverged | 131 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/symlink-ops@kvm` | legacy-rerun | diverged | 152 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/symlink-ops@liteinst` | legacy-rerun | diverged | 160 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/sync-file-range@kvm` | legacy-rerun | diverged | 122 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/sync-file-range@liteinst` | legacy-rerun | diverged | 130 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/sysv-ipc-refusal@kvm` | legacy-rerun | diverged | 105 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/sysv-ipc-refusal@liteinst` | legacy-rerun | diverged | 105 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/thp-disable@liteinst` | legacy-rerun | diverged | 107 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/umask-mode@kvm` | legacy-rerun | diverged | 139 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/umask-mode@liteinst` | legacy-rerun | diverged | 147 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/uname-identity@kvm` | legacy-rerun | diverged | 101 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/uname-identity@liteinst` | legacy-rerun | diverged | 101 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/utimensat-determinism@kvm` | legacy-rerun | diverged | 123 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/utimensat-determinism@liteinst` | legacy-rerun | diverged | 131 | 16 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/vectored-file-io@liteinst` | legacy-rerun | diverged | 147 | 16 | `5ee668223a15` | `diverged` | record-missing |
| `c-programs/vectored-io@kvm` | legacy-rerun | diverged | 126 | 13 | `5ee668223a15` | `measured-and-passed` | record-missing |
| `c-programs/vectored-io@liteinst` | legacy-rerun | diverged | 126 | 16 | `5ee668223a15` | `diverged` | record-missing |
