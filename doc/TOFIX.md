# TOFIX

Findings from a code scan on 2026-10-04.

## High

- `examples/multi_processing/reverse.tcl:33` - `exec ... << \ "ABLE WAS I ERE I SAW ELBA"` has a stray backslash-space, so the heredoc input becomes a lone space and the quoted text is passed as separate arguments. Running it prints `ELBA"` as "the inversion" instead of the reversed sentence. Change it to `<< "ABLE WAS I ERE I SAW ELBA"`.

## Medium

- `rsconstruct.toml:1` - the ~95 `.tcl`/`.tk` files (the whole subject of the repo) go through no processor; only the two shell files in `solutions_jb/chapter1` are checked. Add a `script` processor over `examples`, `exercises` and `solutions_jb` with extensions `.tcl`/`.tk` that at least checks each file parses (for example a small tclsh helper that reads the file and asserts `info complete`), so a broken edit fails the build.
- `examples/core/tcl_specific_version.tcl:1` - the shebang `#!/usr/bin/tclsh8.4` points at a Tcl release that no current distro ships, so the file cannot run as-is. Use a version that exists (`tclsh8.6` or `tclsh9.0`) so the lesson still runs.
- `README.md:8` - the layout section leaves out `solutions_jb/` (chapter1-6 solutions, including the only shell scripts the build lints), and the `exercises/` description ("exercises (`*.txt`) with matching solutions") does not hold for `exercises/ex_list_in_array.txt` and `exercises/ex_wc.txt`, which have no same-named `.tcl`. Document `solutions_jb/` and either add the missing solution or note it.

## Low

- `examples/multi_processing/run_script.tcl:8` - it finds `seven_script.tcl` via `[pwd]`, so it fails unless run from its own directory. Use `[file dirname [info script]]`.
- `doc/links.txt:2` - all three links are plain `http://`, and the ActiveState and IBM developerWorks pages have been retired. Replace them with current resources (tcl-lang.org, wiki.tcl-lang.org).
- `examples/multi_processing/open2.tcl:3` - the header comment is empty (just `#`), so the example does not say what it demonstrates. Add a one-line description like the other examples have.
