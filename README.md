# shadowrocket-configuration-file

Two independent Shadowrocket profiles:

| Profile | Routing |
| --- | --- |
| [`conf/sr_ru_basic.conf`](conf/sr_ru_basic.conf) | Original configuration restored byte-for-byte from commit `ad9d1de`: selected services use PROXY, everything else uses DIRECT. IP-testing sites use DIRECT. |
| [`conf/sr_ru_proxy_default.conf`](conf/sr_ru_proxy_default.conf) | Russian domains, services and destination IPs use DIRECT; all remaining destinations use PROXY. Routing rules are loaded from separate rule-set files. |

## Import URLs

Original profile:

```text
https://cdn.jsdelivr.net/gh/asdfML/shadowrocket-configuration-file@main/conf/sr_ru_basic.conf
```

Proxy-by-default profile:

```text
https://cdn.jsdelivr.net/gh/asdfML/shadowrocket-configuration-file@main/conf/sr_ru_proxy_default.conf
```

Add the second URL as a separate configuration and select it in Shadowrocket. Updating the original profile does not switch it to the new routing scheme. Use configuration-based routing mode for these rules to apply.

## Proxy-by-default rules

The main profile contains only `RULE-SET` references and `FINAL,PROXY` in its `[Rule]` section:

1. [`rules/russia_domains.list`](rules/russia_domains.list): `.ru`, `.su` and `.рф` use DIRECT.
2. The maintained [MetaCubeX Russian service list](https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/classical/category-ru.list): Russian services and their domains/CDNs use DIRECT.
3. [`rules/russia_geoip.list`](rules/russia_geoip.list): destination IPs classified as Russia by Shadowrocket's GeoIP database use DIRECT. DNS resolution is allowed for this check.
4. Everything else: PROXY.

There are no special IP-testing rules, local DIRECT rules or legacy PROXY exceptions in this profile. The old rule-set files are retained because the original profile still uses them. A test site whose destination matches a Russian rule can use DIRECT too.

`skip-proxy`, `tun-excluded-routes` and corporate DNS mappings are retained without duplicating them in routing rules. `skip-proxy` bypasses the HTTP proxy interface, not necessarily the proxy node: TUN traffic is still subject to routing rules.

GeoIP describes server location, not ownership. External rule lists and the GeoIP database can change. DNS, IPv6 and tunnel settings are unchanged between the profiles, apart from each profile's own update URL. Both retain the existing `include = sr_ru_extended.conf` setting.