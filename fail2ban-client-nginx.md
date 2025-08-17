# Fail2Ban Configuration: nginx-json-400

## Purpose

This configuration automatically bans IP addresses that generate **HTTP 4xx errors** in Nginx access logs formatted as JSON, preventing abuse like repeated bad requests or scanning attempts.

---

## 1. Filter: `nginx-json-400`

**File:** `/etc/fail2ban/filter.d/nginx-json-400.conf`

```ini
[Definition]
datepattern = {^.*"time":"%%Y-%%m-%%dT%%H:%%M:%%S.*}
failregex = ^\{.*"remote_addr":"<HOST>".*"status":4\d\d.*\}$
ignoreregex =
```

### Explanation

* **`datepattern`**: Matches the timestamp in the JSON log, e.g., `"time":"2025-08-14T00:05:01.123Z"`
* **`failregex`**: Matches a JSON log line containing a `remote_addr` (IP) and a HTTP 4xx status code.

  * `<HOST>` is automatically replaced by Fail2Ban with the offending IP.
  * `4\d\d` matches any 4xx HTTP status (e.g., 400, 403, 404).
* **`ignoreregex`**: Empty; no logs are ignored.

---

## 2. Jail: `nginx-json-400`

**File:** `/etc/fail2ban/jail.d/nginx-json-400.conf`

```ini
[nginx-json-400]
enabled  = true
filter   = nginx-json-400
port     = http,https
logpath  = /root/project/doneit-telemetry/nginx-logs/access.log
backend  = polling
maxretry = 1
findtime = 600
bantime  = 3600
action   = ufw
```

### Explanation

* **`enabled`**: Enables this jail.
* **`filter`**: References the filter `nginx-json-400`.
* **`port`**: Applies to HTTP (80) and HTTPS (443).
* **`logpath`**: Path to your Nginx JSON access log.
* **`backend`**: `polling` monitors logs in real-time.
* **`maxretry`**: Ban after 1 failed request.
* **`findtime`**: 600 seconds (10 minutes) window to count failures.
* **`bantime`**: 3600 seconds (1 hour) ban duration.
* **`action`**: Uses UFW to block the offending IP.

---

## Example Log Matched

```json
{"time":"2025-08-14T00:05:01.123Z","remote_addr":"203.0.113.45","status":404,"request":"GET /admin HTTP/1.1"}
```

* IP `203.0.113.45` would be banned for 1 hour.

---

## On update always test

```bash
fail2ban-regex /srv/volume/nginx_data/access.log /etc/fail2ban/filter.d/nginx-json-400.conf
```
then 
```bash
sudo fail2ban-client reload nginx-json-400
```

---


## Notes

* Ensure Nginx log format matches the JSON pattern.
* Adjust `maxretry`, `findtime`, or `bantime` to suit your needs.
* Test the regex before enabling the jail:
* Do insertion test:  echo '{"time":"2025-08-17T10:00:00.000Z","remote_addr":"1.2.3.4","status":404,"request":"GET /test HTTP/1.1"}' >> /srv/volume/nginx_data/access.log

```bash
fail2ban-regex /root/project/doneit-telemetry/nginx-logs/access.log /etc/fail2ban/filter.d/nginx-json-400.conf
```
