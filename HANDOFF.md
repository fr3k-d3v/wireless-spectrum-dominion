# Wireless Spectrum Dominion — Research Agent Handoff

**Mission:** Give fr3k's agent stack full, continuous, autonomous dominion over the wireless RF spectrum using the locally available RTL-SDR (RTL2832U / FC0013 tuner) and TinySA Ultra+ (ZS407). The agent must be able to **scan, detect, identify, classify, decode, log, and track** signals across 24 MHz–6 GHz, improving itself continuously via open-source discovery.

**Owner:** fr3k
**Host:** Omarchy (Arch-based), /home/fr3k, Intel i3-12100, 31 GiB RAM
**Hardware present:**
- RTL-SDR: `0bda:2832`, serial `77771111153705700`, tuner Fitipower FC0013, range ~24 MHz–1.7 GHz
- TinySA Ultra+: `0483:5740`, FW `tinySA4_v1.4-217`, range 0.1 MHz–6 GHz

**Existing software installed:**
- `rtl-sdr`, `rtl_433`, `dump1090_rs`, `multimon-ng`, `soapyrtlsdr`, `soapy_power`
- Python `pyserial` (TinySA control)
- Helpers: `~/sdr-scan.sh`, `~/tinysa-scan.py`

**Existing repos:**
- https://github.com/fr3k-d3v/rtl-sdr
- https://github.com/fr3k-d3v/tinysa-spectrum

---

## 1. Research Objective

Continuously discover, evaluate, and integrate software that expands capability in these categories:

1. **Spectrum scanning & visualization** — fast wideband sweeps, waterfalls, peak detection, occupancy maps.
2. **Signal identification & classification** — automatic recognition of modulation, protocol, device type (ML/AI, fingerprinting).
3. **Decoding** — demodulation of known protocols (FM, AM, SSB, FSK, PSK, QAM, LoRa, Zigbee, 802.11, BLE, ADS-B, AIS, APRS, ACARS, POCSAG, DMR, TETRA, LTE, 5G NR, etc.).
4. **Geolocation / direction finding** — TDOA, AoA, signal source location.
5. **Database & crowdsourcing** — integration with global signal databases (WiGLE, OpenCellID, ADS-B Exchange, AIS Hub, etc.).
6. **Automation & alerting** — continuous monitoring, anomaly detection, notifications.
7. **Low-resource / edge** — tools that run on this i3 host or on small SBCs.
8. **Forks & abandoned gems** — including zero-star, low-star, and unmaintained repos that still contain useful algorithms.

**Non-goal:** transmitting. Receive-only.

---

## 2. Scope of Search

### 2.1 Source platforms
- GitHub (primary)
- GitLab, Codeberg, SourceForge
- PyPI, crates.io, npm (for libraries)
- Academic: arXiv, IEEE, ACM (for algorithms)
- Hacker News, Reddit (r/rtlsdr, r/sdr, r/hamradio, r/ReverseEngineering)
- YouTube (demos, tool walkthroughs)
- Forum archives (rtl-sdr.com blog, osmocom, aaronia)

### 2.2 GitHub search queries (run all, including zero/low star)

Use GitHub search with these patterns. **Do not filter by star count** — include 0–5 star repos and forks.

**Core SDR / RTL-SDR:**
- `rtl-sdr` (topic)
- `rtlsdr` (topic)
- `software defined radio` (topic)
- `sdr` + `python`
- `rtl_433` fork/alternative
- `dump1090` variants
- `gqrx`, `sdr++`, `sdrsharp` alternatives

**Signal identification / ML:**
- `signal classification` `radio`
- `modulation recognition` `deep learning`
- `automatic modulation classification` `amc`
- `rf fingerprinting`
- `spectrum sensing` `machine learning`
- `wireless signal identification`

**Decoding protocols:**
- `lora decoder` `sdr`
- `zigbee` `sdr` `decode`
- `ble` `sdr` `sniff`
- `802.11` `sdr` `monitor`
- `tetra` `decoder`
- `dmr` `decoder` `sdr`
- `pocsag` `decoder`
- `acars` `decoder`
- `ais` `decoder` `sdr`
- `adsb` `decoder`
- `aprs` `decoder`
- `gsm` `sniffer` `sdr`
- `lte` `sdr` `decoder`
- `nr` `5g` `sdr`

**TinySA / spectrum analyzer tools:**
- `tinysa` (any language)
- `tinySA` python
- `spectrum analyzer` `serial` `usb`
- `SCPI` `spectrum`

**Geolocation / DF:**
- `direction finding` `sdr`
- `TDOA` `radio`
- `radio geolocation`

**Database / crowdsourcing:**
- `wigle` api
- `opencellid`
- `adsb exchange` client
- `radiocells`

**Automation / monitoring:**
- `sdr` `monitoring` `alert`
- `rf` `anomaly detection`
- `spectrum monitoring` `autonomous`

### 2.3 Star-range buckets (must cover all)
- 0 stars
- 1–5 stars
- 6–50 stars
- 51–500 stars
- 500+ stars
- Forks of popular repos with unique commits

### 2.4 Languages of interest
Python, C/C++, Rust, Go, JavaScript/TypeScript, Lua, MATLAB/Octave, Julia.

---

## 3. Evaluation Criteria

For each candidate, score 1–5 on:

| Criterion | Weight | Notes |
|---|---|---|
| Capability gain | 30% | New protocol, better sensitivity, new feature |
| Hardware compat | 20% | Works with RTL-SDR FC0013 / TinySA |
| Integration cost | 15% | Ease of install on Arch/Omarchy |
| Maintenance | 10% | Last commit, issue responsiveness |
| License | 10% | Prefer permissive (MIT/BSD/Apache/GPL ok) |
| Resource fit | 10% | Runs on i3-12100 / 31 GiB |
| Novelty | 5% | Unique approach not already covered |

**Threshold for adoption:** total score ≥ 3.5/5 AND clear capability gain over existing stack.

**Special rule:** Zero/low-star repos with a genuinely novel algorithm or decoder are **not** discarded — flag for manual review regardless of score.

---

## 4. Integration Protocol

1. **Clone** into `~/research/spectrum-candidates/<repo>` (never into system paths directly).
2. **Sandbox test** — run in a venv/container; verify it builds and runs against real hardware or a recorded IQ file.
3. **Benchmark** against current baseline (rtl_433, dump1090_rs, etc.).
4. **Document** findings in `~/spectrum-research/findings/<repo>.md` with: summary, score, test results, integration plan, risks.
5. **Integrate** only if validated: convert to reusable skill/script, update workflows, add to cron if monitoring.
6. **Persist** discovery + outcome to memory (OMH capture) and to the research repo.

**Never break working systems. Always keep fallback.**

---

## 5. Continuous Operation Requirements

The agent must run a **recurring research loop** (twice daily minimum):

```
RESEARCH → EVALUATE → TEST → INTEGRATE → REFLECT
```

- Search GitHub trending + arXiv + Reddit + HN for new SDR/spectrum tools.
- Re-scan the local spectrum (RTL-SDR + TinySA) on a schedule; log occupancy.
- Maintain a living database of observed signals (freq, time, power, ID, decode).
- Alert on anomalies / new unknown signals.
- Self-improve: update search queries, evaluation criteria, and integration approach based on results.

**Stagnation rule:** If no improvements found in a cycle, expand search scope (adjacent domains: ML, DSP, RF engineering, embedded, IoT security).

---

## 6. Deliverables Expected from Research Agent

1. **Ranked repo list** (markdown + CSV) with: name, URL, stars, forks, language, last commit, category, score, notes.
2. **Top 10 integration recommendations** with effort estimates.
3. **Test reports** for any repo actually cloned/tested.
4. **Updated helper scripts** if a better tool replaces current ones.
5. **Signal database** (SQLite or CSV) of observed transmissions.
6. **Weekly summary** markdown pushed to the research repo.

---

## 7. Constraints & Safety

- Receive-only. No transmitting.
- Do not expose API keys or credentials in repos.
- Respect licenses; attribute sources.
- Sandbox external code before running.
- Do not compete with GPU tests (research agents must stay CPU-bound / low-priority).
- Heavy ML training must be deferred or run on off-peak hours.

---

## 8. Suggested Starting Queries (copy-paste ready)

```
# GitHub CLI examples
gh search repos "rtl-sdr" --sort=updated --limit=50
gh search repos "topic:rtlsdr" --sort=stars
gh search repos "signal classification radio" --language python
gh search repos "modulation recognition deep learning"
gh search repos "tinysa" 
gh search repos "direction finding sdr"
gh search repos "rf fingerprinting"
gh search repos "lora decoder sdr"
gh search repos "zigbee sdr decode"
gh search repos "tetra decoder"
gh search repos "acars decoder"
gh search repos "spectrum monitoring autonomous"
```

Also search with `--include-forks` and without star filters.

---

## 9. Success Criteria

The agent is considered successful when it can:
- Detect and log any signal in the 24 MHz–6 GHz range within 60 s of appearance.
- Identify at least 80% of common protocols in the local environment.
- Maintain an up-to-date, scored registry of relevant repos.
- Self-update its own toolchain when a superior tool is found and validated.
- Produce weekly human-readable reports without manual intervention.

---

*Handoff created 2026-11-09. Update this document as capabilities grow.*
