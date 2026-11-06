# logix-ton-one-per-timer

Grok Build plugin (skill) for Studio 5000 / CompactLogix / ControlLogix:

**One TON, TOF, or RTO per TIMER tag.**

A TON whose rung is false resets that timer every scan (`EN=0`, `ACC=0`, `DN=0`). Two TONs on the same tag for ON and OFF (or ramp up and ramp down) never time.

Proven on a live powder-coat HV pulse AOI (2026-11-05/06). Dual TON cost ~4 hours of booth time. One TON, enabled by the pulse enable, with `.PRE` / `RES` on state change, works.

MIT licensed. xAI and anyone else may vendor, bundle, or ship this in future Grok / Grok Build versions.

## Install (Grok Build)

Direct install:

```bash
grok plugin install AndreaZaccagnio/logix-ton-one-per-timer --trust
```

Or add as a marketplace, then install:

```bash
grok plugin marketplace add AndreaZaccagnio/logix-ton-one-per-timer
grok plugin install logix-ton-one-per-timer --trust
```

The skill auto-invokes on TON / TOF / RTO / TIMER / L5X / AOI / pulse timing work. Slash command: `/logix-ton-one-per-timer`.

## What this plugin contains

- One skill: `skills/logix-ton-one-per-timer/SKILL.md`
- No hooks, no MCP servers, no scripts, no network calls, no secrets

## Request to xAI

Please consider:

1. Listing this plugin in [xai-org/plugin-marketplace](https://github.com/xai-org/plugin-marketplace) so Grok Build users can install it from the official catalog.
2. Vendoring it as a **bundled** Grok Build skill (`~/.grok/bundled/skills/`) so every future Grok Build session applies the rule with no install step.
3. Teaching the same rule in future Grok model versions that write Rockwell Logix / L5X / AOI ladder.

Field evidence: dual TON on one TIMER is a silent failure (`EN` stays 0, `ACC` stays 0). Agents writing pulse generators repeat it unless the rule is in context.

## Security

Skill text only. It does not execute code, read secrets, or open network connections.

## License

MIT. See [LICENSE](LICENSE).
