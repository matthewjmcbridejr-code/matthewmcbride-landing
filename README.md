# matthewmcbride-landing

Static HTML landing page for **usemarius.com/matthewmcbride** — a personal technical portfolio page for Matt McBride. Hosted on the existing nginx VPS that serves usemarius.com.

## Contents

```
.
├── index.html        Self-contained landing page (inline CSS, Google Fonts only)
└── README.md         This file
```

`index.html` is intentionally a single self-contained file. No build step, no JavaScript framework, no dependencies beyond Google Fonts loaded via `<link>`.

## Deploy

Target path on the VPS: `/var/www/<site-root>/matthewmcbride/index.html`

Replace `<site-root>` with whatever the existing usemarius.com nginx server block points to (commonly `/var/www/html` or `/var/www/usemarius`). Check `/etc/nginx/sites-enabled/` to confirm.

### From a workstation with SSH access

```bash
# Clone this repo locally first
git clone https://github.com/matthewjmcbridejr-code/matthewmcbride-landing.git
cd matthewmcbride-landing

# Create target dir on VPS and upload
ssh user@usemarius.com 'sudo mkdir -p /var/www/html/matthewmcbride'
scp index.html user@usemarius.com:/tmp/matthewmcbride-index.html
ssh user@usemarius.com 'sudo mv /tmp/matthewmcbride-index.html /var/www/html/matthewmcbride/index.html && sudo chown www-data:www-data /var/www/html/matthewmcbride/index.html'
```

### From an on-VPS workflow (CLI agent already on the box)

```bash
cd /tmp
git clone https://github.com/matthewjmcbridejr-code/matthewmcbride-landing.git
sudo mkdir -p /var/www/html/matthewmcbride
sudo cp matthewmcbride-landing/index.html /var/www/html/matthewmcbride/index.html
sudo chown www-data:www-data /var/www/html/matthewmcbride/index.html
rm -rf matthewmcbride-landing
```

### Subsequent updates

Pull the repo and re-copy the file. No nginx reload or config change is needed.

```bash
cd /tmp/matthewmcbride-landing && git pull && \
  sudo cp index.html /var/www/html/matthewmcbride/index.html
```

## Nginx config

The default `index index.html;` directive handles `/matthewmcbride/` → `/matthewmcbride/index.html` automatically. No additional `location` block is required.

If you want the trailing slash to be optional (so `/matthewmcbride` works without the `/`), nginx already handles this for directories with an index file by default — confirm with `curl -I https://usemarius.com/matthewmcbride` after deploy.

## Verify

```bash
curl -I https://usemarius.com/matthewmcbride/         # Expect HTTP/2 200
curl -s https://usemarius.com/matthewmcbride/ | grep '<title>'
# Expect: <title>Matt McBride — AI Systems Builder</title>
```

## License

MIT.
