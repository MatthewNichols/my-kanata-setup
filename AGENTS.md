# AGENTS.md

Personal [Kanata](https://github.com/jtroo/kanata) keyboard remapper configs. See [README.md](README.md) for the layout of the repo and the systemd/zsh setup.

## Documentation
- Kanata config reference: https://jtroo.github.io/config.html
- Kanata repo and discussions: https://github.com/jtroo/kanata
- Autoshift approach: https://github.com/jtroo/kanata/discussions/470

## Repo conventions
- `config-sections/autoshift-defalias.kbd` is **generated**. Don't hand-edit it; change `autoshift-generator/generate-autoshift-config.py` and regenerate:
  `python3 ./autoshift-generator/generate-autoshift-config.py > ./config-sections/autoshift-defalias.kbd`
- `machine-configs/*.kbd` are per-machine entry points. Each must start with a `defvar` that sets the autoshift timeout variables used by the included sections (currently `autoshift-tap-repress-timeout` and `autoshift-hold-timeout`; check the generator for the names it expects), then `include` shared files from `config-sections/` via relative paths.
- Put anything reusable across machines in `config-sections/`; keep machine-specific tuning in `machine-configs/`.
- `xps-13-experimental.kbd` is for trying out layers; don't copy experiments into other machine configs unless asked.

## Validating changes
- Check a config parses without grabbing the keyboard: `kanata --cfg machine-configs/<name>.kbd --check` (binary is at `~/.cargo/bin/kanata`).
- Don't start, stop or restart the user's `kanata.service` unprompted; it affects their live keyboard.
