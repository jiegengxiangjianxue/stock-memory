---
name: ownbox_license_evidence_rule
description: Do not treat OwnBoxForAndroid code as reusable until license provenance is verified per-file or per-sub-project
---
- OwnBoxForAndroid mixes application-level code with an upstream native core (likely from a VPN/TUN SDK).
- Before proposing any code borrow into sb-lxf, verify:
  1. Top-level `LICENSE` file and SPDX headers.
  2. Sub-directory / submodule licenses (the native core may have a different or more restrictive license than the Android app layer).
  3. Whether the core is published under an open-source license at all.
- If license is ambiguous or missing, flag as **incompatible / unverified** in the report rather than assuming reuse is permitted.