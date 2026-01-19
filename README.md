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
