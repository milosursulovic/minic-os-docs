# MiniC OS docs site

A static HTML/CSS site (no build step, no JS framework) — install guide,
shell command guide, architecture reference, an annotated real-session
walkthrough, roadmap, and a downloads page for the MiniC OS kernel.
Intended for GitHub Pages or Cloudflare Pages, same as the compiler's
docs site.

This repo is one of two siblings under `d:\Projects\minic` — see
`../CLAUDE.md` for the workspace layout. It documents `../os/` (the
kernel repo) specifically; it has no build dependency on it, but its
`downloads/` folder holds binaries *built from* it (see below). The
MiniC compiler and its own docs site (formerly `../compiler/`/`../docs/`)
were retired and deleted once the kernel was rewritten into hand-written
C - there's no separate compiler docs site anymore for this one to
avoid duplicating.

## Structure

```
index.html, install.html, guide.html, reference.html, examples.html, roadmap.html, download.html
assets/style.css   (shared visual system with ../docs/ - same file, unchanged)
downloads/         pre-built release artifacts, committed directly (not via GitHub Releases)
```

Every page repeats the same topnav (`class="navlink active"` on whichever
page is current) — match that pattern exactly when adding a page or link.
Page roles, matching the compiler docs site's naming convention but
kernel-specific content:

- `install.html` — "Getting Started": QEMU quick start, building from
  source, running outside QEMU (VirtualBox/VMware/real hardware via the
  ISO).
- `guide.html` — "Shell Guide": every shell command, grouped by
  subsystem (heap, frames, paging, scheduler, process model, IPC, disk,
  filesystem, VFS).
- `reference.html` — "Architecture": boot process, project layout,
  memory map, scheduler, process model, the syscall ABI table, the
  native File/Channel/Process API, the POSIX shim, testing methodology,
  known limitations. This is the technical deep-dive page.
- `examples.html` — "Walkthrough": one real, annotated QEMU session
  with captured output and explanations of what each result actually
  proves — mirrors the verification style `../os/README.md` uses.
- `roadmap.html` — every milestone (1 through the current number),
  what's done, what's next.

## The downloads/ folder

Snapshots, not versioned releases — explicitly framed that way on
`download.html`. Refreshing them is a **manual step**, not automated:

```bash
wsl.exe -e bash -c "cd ../os && ./build.sh iso"
cp ../os/kernel.elf downloads/minic-kernel.elf
cp ../os/minic-os.iso downloads/minic-os.iso
```

Refresh these in the same session a kernel milestone ships and this
repo's `roadmap.html`/`reference.html` get updated — a stale binary next
to an updated milestone description is a real inconsistency.

## Docs-sync convention

Every feature-level commit in `../os/` gets a corresponding update here
in the *same working session*: `roadmap.html` for the milestone entry
itself, `reference.html` for anything that changes the architecture
(new syscalls, a new subsystem, a new known limitation), `guide.html`
for a new/changed shell command, `examples.html` if the real captured
session output changes. This repo's pages should never describe a
milestone or command that isn't actually shipped in `../os/`, or omit
one that is. See the `kernel-milestone` skill (defined in the workspace
root) for the exact checklist — treat this repo the same way `../docs/`
was already treated for compiler milestones, just for kernel ones now.
