---
name: research_task_sb-ai_dns_tun_20261011
description: Research task for sb-AI Android proxy client covering DNS/fakeIP/fallback settings in bettbox and FlClash, and their sing-box equivalents; plus TUN stack comparison between sing-box, Leadaxe fork, and
---
## Research Task: sb-AI DNS & TUN Stack Analysis (2026-10-11)

### Context
- **Project**: sb-AI — Android proxy client built on `Leadaxe/sing-box-lx` fork
- **Kernel Version**: v1.14.2-lx.11 (libbox.aar)
- **Today**: 2026-10-11

---

## PART A — DNS / FakeIP / Fallback Settings

### 1. Client Models to Analyze

#### bettbox
- **Likely Repo**: `github.com/appshubcc/Bettbox` (search required to confirm)
- **Base**: mihomo/clash-deepseek-v4.1-flash based
- **Tech Stack**: Dart/Flutter (assumed based on "FlClash" naming pattern)
- **Files to Read**:
  - DNS settings model/file
  - UI components for DNS page
  - Config mapping logic

#### FlClash
- **Repo**: `github.com/chen08209/FlClash`
- **Key Files to Read**:
  - `lib/models/clash_config.dart` — main config model
  - DNS-related UI widgets
  - Config transformation logic

### 2. Mihomo DNS Fields (Reference)
```
dns:
  enhanced-mode: fake-ip | redir-host | normal
  fake-ip-range: 198.18.0.1/16
  fake-ip-filter: [...]
  fake-ip-filter-mode: blackhole | whitehole
  fallback: ["tls://223.5.5.5"]
  fallback-filter:
    geoip: true
    geoip-code: CN
    ipcidr: ["240.0.0.0/4"]
    domain: ["+.com"]
  nameserver-policy:
    "+.cn": ["tls://223.5.5.5"]
  proxy-server-nameserver: ["tls://223.5.5.5"]
  default-nameserver: ["8.8.8.8"]
  direct-nameserver: [...]
  respect-rules: true
  prefer-h3: true
  use-hosts: false
  use-system-hosts: false
  cache-algorithm: lfu |斐波那契堆
```

### 3. Sing-Box DNS Equivalents (v1.12+)
```json
{
  "dns": {
    "servers": [
      {
        "tag": "dns_proxy",
        "type": "fakeip",
        "address": "nameserver",
        "detour": "proxy",
        "address_resolver": "dns_resolver",
        "address_fakeip": {
          "inet4_range": "198.18.0.1/16",
          "inet6_range": "fc00::/16"
        },
        "fakeip_filter_mode": "blackhole|whitehole"
      }
    ],
    "rules": [
      {
        "action": "route|reject|predefined|route-options",
        "rule_set": "geoip-cn",
        "quer