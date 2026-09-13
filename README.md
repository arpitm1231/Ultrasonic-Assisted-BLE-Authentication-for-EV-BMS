# Ultrasonic-Assisted BLE Authentication for EV BMS

A protocol concept + interactive simulation addressing a real, documented weakness in how low-cost EV Battery Management Systems (BMS) pair with a phone over Bluetooth.

**🔗 Live demo:** `[paste your GitHub Pages link here]`

---

## The problem

Most low-cost EV two-wheeler and three-wheeler BMS units pair with an owner's phone over Bluetooth Low Energy (BLE), often protected by little more than an app password. This relies on an assumption that a strong BLE signal means the phone is physically nearby — an assumption that has already been broken in the field. In 2022, NCC Group researchers demonstrated a BLE relay attack against Tesla's phone-key system, showing that even a well-resourced, RSSI-based proximity check can be spoofed with inexpensive relay hardware. Most e-rickshaw and scooter BMS units have no equivalent proximity check at all.

## The approach

This project adds a short-range, physical-proximity check **before** any BLE authentication begins:

1. **Ultrasonic proximity check** — the BMS emits a short ultrasonic pulse (~20–40 kHz). Only a phone within roughly a metre or two can detect it. No detection, no pairing.
2. **One-time session key exchange** — carried over the ultrasonic channel, not BLE, so it isn't exposed to radio-based sniffing.
3. **Mutual challenge-response** — phone and BMS each verify the other before any command is trusted.
4. **Encrypted BLE session** — all data and commands are AES-encrypted once authenticated.
5. **Single active session** — no second device can pair while a session is live.
6. **Instant owner alert** — every successful pairing triggers a notification, so unauthorised access wouldn't go unnoticed.
7. **Session wipe on power-off** — the key is invalidated the moment the vehicle powers down; nothing persists for an attacker to exploit.

This targets the same class of attack that pushed premium automakers toward UWB-based digital keys — but at a cost point (~₹20–100 in added hardware) that fits the low-cost EV segment, where nothing like UWB is currently viable.

## What's in this repo

An interactive HTML/JS simulation of the full protocol. Open the live demo and try:
- **Genuine owner pairs** — walks through all seven steps succeeding.
- **Simulate relay attack** — shows an attacker with only a relayed BLE signal getting rejected at the proximity check, before any key ever exchanges.
- **Power off / reset** — shows the session key being wiped.

No installation needed — it's a single static HTML file, works offline once loaded.

## What this is, and isn't

**Is:** a working simulation of the protocol logic, built to make the authentication flow concrete and testable rather than just described on paper.

**Isn't:** a validated hardware product. Real ultrasonic transducers, real phone microphones, and real ambient noise (traffic, vibration, weather) all introduce failure modes a software simulation can't capture. The honest next step is a physical prototype with layered noise filtering and a tested fallback authentication path (e.g. OTP) for edge cases — with a measured, published false-rejection rate, not a marketing claim of zero failures.

## Roadmap

- [ ] Hardware prototype (piezo transducer + existing BLE module on an actual BMS board)
- [ ] Noise characterisation across real-world conditions
- [ ] Fallback authentication path for edge cases
- [ ] Small-scale pilot (50–100 vehicles) with a manufacturer
- [ ] Safety/compliance review

## Scope

This addresses BLE relay / proximity-spoofing attacks specifically. It does not address insider threats, physical tampering, supply-chain attacks, or social engineering — it's one layer in a broader security posture, not a complete solution.

## Tech

Plain HTML, CSS, and vanilla JavaScript. No frameworks, no build step, no dependencies.

## Feedback

Open to feedback, collaboration, or being told where this breaks — that's the point of sharing it before it's finished. Reach out via [LinkedIn / email — add your contact here].
