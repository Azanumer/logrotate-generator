# Logrotate Generator

One command to generate a safe logrotate config for a site's logs — no more hand-writing `/etc/logrotate.d` entries and guessing the options.

```bash
# Custom path
./generate-logrotate.sh --name mysite --path '/var/www/mysite/logs/*.log' \
    --rotate 14 --size 100M --postrotate 'systemctl reload nginx'

# Presets (one command per stack)
./generate-logrotate.sh --preset wordpress --domain example.com
./generate-logrotate.sh --preset nginx --domain example.com
./generate-logrotate.sh --preset php-fpm --domain example.com
```

Defaults are the safe ones: `daily`, `missingok`, `notifempty`, `delaycompress`, `compress`, `sharedscripts`, and `create 0640 www-data adm` so rotated logs keep working permissions.

- Without `--write` it prints the config for review (preview mode).
- With `--write` (as root) it installs to `/etc/logrotate.d/<name>` and runs a `logrotate --debug` parse check.
- `--size` adds `maxsize` so a log that explodes mid-day rotates early; `--postrotate` reloads the service so it reopens the new file.
- `--name` is validated to a safe filename — it becomes the file in `/etc/logrotate.d`, never a path.

## License

MIT
