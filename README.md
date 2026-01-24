# my-kanata-setup
This is my [Kanata](https://github.com/matt-nichols/my-kanata-setup) config and other associated files.

## Sections
- autoshift-generator: This has a script that generates a Kanata config file that can setup "autoshift", i.e. automatically emitting the shift modifier key when the key is long pressed. This is based on [this discussion](https://github.com/jtroo/kanata/discussions/470 
Run the script with 
```
python3 ./autoshift-generator/generate-autoshift-config.py > ./config-sections/autoshift-defalias.kbd
```

- machine-configs: This has machine-specific Kanata config files for various machines. These will include standard Kanata config files using the [`include`](https://jtroo.github.io/config.html#_include_other_files) directive. They need to have a defvar section at the top to define the `autoshift-tap-repress-timeout` and `autoshift-tap-delay-timeout` variables used by the autoshift config section. This allows for machine-specific tuning of the autoshift behavior. 

- config-sections: This has reusable Kanata config sections (including the autoshit-generator result) that can be included in the machine-specific configs. These are intended to be generic and reusable across multiple machines.

## Setup `systemd` Service

### sample `systemd` Service File
Change the path to `kanata` binary and the path to the machine-specific config file as needed.

```
[Unit]
Description=Kanata keyboard remapper
Documentation=https://github.com/jtroo/kanata

[Service]
Type=simple
ExecStart=/home/%u/.cargo/bin/kanata --cfg /home/mnichols/projects/my-kanata-setup/machine-configs/fw-13.kbd
Restart=no

[Install]
WantedBy=default.target
```

### Start and Stop the Service
```
systemctl --user start kanata.service 
```
```
systemctl --user stop kanata.service 
```

### zsh shell function
```
function kanata_reload() {
  # Check if kanata service is active
  if systemctl --user is-active --quiet kanata.service; then
    echo "🔄 Restarting kanata service..."
    systemctl --user restart kanata.service
  else
    echo "▶��  Starting kanata service..."
    systemctl --user start kanata.service
  fi

  # Wait a moment for the service to initialize
  sleep 0.5

  # Check if the service is running successfully
  if systemctl --user is-active --quiet kanata.service; then
    echo "✅ Kanata service is running"
  else
    echo "❌ Kanata service failed to start"
    echo "\nRecent errors:"
    journalctl --user -u kanata.service -n 20 --no-pager | grep -i error
    return 1
  fi

  # Show any recent errors or warnings even if service is running
  local recent_errors=$(journalctl --user -u kanata.service --since "5 seconds ago" --no-pager | grep -iE "error|failed|warning")
  
  if [[ -n "$recent_errors" ]]; then
    echo "\n⚠️  Recent issues detected:"
    echo "$recent_errors"
  fi
}
```
