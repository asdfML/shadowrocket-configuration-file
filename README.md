# shadowrocket-configuration-file

Two independent Shadowrocket profiles:

| Profile | Routing |
| --- | --- |
| [`conf/sr_ru_basic.conf`](conf/sr_ru_basic.conf) | Original configuration restored byte-for-byte from commit `ad9d1de`: selected services use PROXY, everything else uses DIRECT. IP-testing sites use DIRECT. |
| [`conf/sr_ru_proxy_default.conf`](conf/sr_ru_proxy_default.conf) | Local/corporate destinations use DIRECT; existing explicit PROXY exceptions take priority over Russian DIRECT rules; all remaining destinations use PROXY. |

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

1. Local networks, `localhost`, `.local`, `captive.apple.com`, `avito.com` and `avito.ru`: DIRECT. Corporate DNS mappings are retained.
2. IP-testing sites and existing PROXY lists: PROXY, even when a Russian rule would otherwise match.
3. `.ru`, `.su`, `.рф` and the maintained [MetaCubeX Russian service list](https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/classical/category-ru.list): DIRECT.
4. Other destination IPs classified as Russia by Shadowrocket's GeoIP database: DIRECT.
5. Everything else: PROXY.

GeoIP describes server location, not ownership. External rule lists and the GeoIP database can change. DNS, IPv6 and tunnel settings are unchanged between the profiles, apart from each profile's own update URL. Both retain the existing `include = sr_ru_extended.conf` setting.