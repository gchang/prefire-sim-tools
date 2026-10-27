# Changes from upstream

This copy starts from the file tree of https://github.com/UW-PREFIRE/PREFIRE_sim_tools at commit `9cf1becfbd4ffab850766bcf828c3ec0364d2ef0` (tag `R01`, 2025-09-09, `PREFIRE_sim_tools 1.0.0`), without its git history. Nothing was left out. The code is by the PREFIRE Mission Team under the MIT license (see `LICENSE`).

## 2026-10-27: `utils` no longer imports `hsremis` when it loads

`PREFIRE_sim_tools/utils/__init__.py` imported `hsremis` along with the other utilities. `hsremis.py` opens `UWiremis_labeigvects.nc` at import time (line 20), and that data file is not in this repository: `.gitignore` excludes `*.nc`, and it is in no public archive we know of. So any program that imports `PREFIRE_sim_tools.utils` stopped with `FileNotFoundError` before reading its inputs, although 2B-ATM and AUX-MET never call `hsremis`.

The `from . import hsremis` line is removed from `utils/__init__.py`. Code that needs it can still write `from PREFIRE_sim_tools.utils import hsremis` (and then needs the data file). Nothing else in this repository, PREFIRE_ATM, PREFIRE_AUX_MET, PREFIRE_PRD_GEN or PREFIRE_tools refers to `utils.hsremis`. No results change.

Who asked for it: our own change, made at the lead's request on 2026-10-27. We found the failure in our own tests of 2B-ATM and AUX-MET. The registration wizard parked its 2B-ATM bundle draft over the missing file but proposed no code fix, and its AUX-MET analysis did not report it.
