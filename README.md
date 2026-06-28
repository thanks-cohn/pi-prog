# evacuate

`evacuate` is a portable Bash utility for moving the largest files from a selected tree to removable storage with recovery-first metadata.

## Example

```bash
./evacuate --non-interactive --root "$HOME/Videos" --dest /media/usb --target-size 50000000000 --min-size 104857600 --mode transactional
```

Use `--dry-run` to create and validate manifests without copying or deleting data. Use `--resume SESSION_ID --dest /media/usb` to continue a local session from `$HOME/Pi-Prog`. Use `--verify PATH` against either a USB root containing `.evacuate` or a metadata directory.

The local authoritative cache is `$HOME/Pi-Prog/<session-id>/`; the USB receives `.evacuate/` metadata and `files/` data.
