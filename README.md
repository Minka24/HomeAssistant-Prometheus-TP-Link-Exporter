# TP-Link Prometheus Exporter Home Assistant Add-on

This Home Assistant add-on provides a Prometheus exporter for TP-Link smart switches, allowing you to monitor and collect metrics from your TP-Link devices directly in your Home Assistant environment.

## Features

- Exposes metrics from TP-Link switches in Prometheus format
- Supports multiple switches with individual credentials and settings
- Customizable HTTP port for the exporter
- Simple YAML configuration via the Home Assistant add-on UI

## Configuration

Configure the add-on using the Home Assistant UI. Example configuration:

```yaml
http_port: 8000
switches:
  - ip: 192.168.1.1
    username: admin
    password: admin
    cache_login: true
    http_port: 80
```

### Options

- `http_port`: (integer) The port on which the Prometheus exporter will listen.
- `switches`: (list) A list of TP-Link switches to monitor. Each switch supports:
  - `ip`: (string) The IP address of the switch
  - `username`: (string) Username for the switch
  - `password`: (string) Password for the switch
  - `cache_login`: (boolean) Whether to cache login sessions
  - `http_port`: (integer) The HTTP port of the switch (default: 80)

## Usage

1. Install the add-on from your local add-ons repository.
2. Configure the add-on as shown above.
3. Start the add-on.
4. Access Prometheus metrics at `http://<home_assistant_host>:<http_port>/metrics`.

## Support

For issues or feature requests, please open an issue in your repository or contact the maintainer.

---

**Note:** This add-on is not affiliated with TP-Link or Prometheus. Use at your own risk.
