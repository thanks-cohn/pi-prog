# evacuate

`evacuate` is a portable Bash utility for moving selected data from a source tree to removable storage with recovery-first metadata. It is designed as a manifest-first evacuation tool: decisions are made before scanning or copying, manifests are immutable once written, transfers are journaled, and interrupted sessions can be resumed.

## Interactive workflow

Run `./evacuate` to use the guided workflow:

1. Select the scan root.
2. Optionally add folder exclusions before scanning. When available, `evacuate` uses a graphical folder picker (`zenity`, `kdialog`, or `yad`) and falls back to terminal input when a file explorer is inaccessible.
3. Select a writable storage destination from filtered real storage mounts, or enter a destination path manually.
4. Enter the target evacuation size, such as `40G`, `120GB`, or `0` for everything.
5. Review manifest analysis, including largest files, tiny-file counts, average size, and size buckets.
6. Enable tiny-file optimization when many small files are detected.
7. Review the pre-flight report and confirm before any copy begins.
8. Review the completion report after transfer.

## Example

```bash
./evacuate --non-interactive --root "$HOME/Videos" --dest /media/usb --target-size 50G --min-size 104857600 --mode transactional
```

Use `--dry-run` to create and validate manifests without copying or deleting data. Use `--resume SESSION_ID --dest /media/usb` to continue a local session from `$HOME/Pi-Prog`. Use `--verify PATH` against either a USB root containing `.evacuate` or a metadata directory.

The local authoritative cache is `$HOME/Pi-Prog/<session-id>/`; the USB receives `.evacuate/` metadata and `files/` data. When tiny-file optimization is enabled, tiny files are transferred in deterministic tar transaction containers under `bundles/` while large files continue through the normal copy path.
