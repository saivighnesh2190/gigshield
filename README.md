# 🛡️ GigShield — AI-Powered Parametric Insurance for Gig Workers

> **Guidewire DEVTrails Hackathon 2026** | Team: Syntax Shields | Phase 1 — Seed

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Built With](https://img.shields.io/badge/Built%20With-React%20%7C%20Node.js%20%7C%20Python%20%7C%20MongoDB-blue)](#)

---

## 🌟 Inspiration

India has over 15 million gig economy delivery workers — yet most of them have zero financial protection when disruptions strike. A single day of heavy rain, extreme heat, a city curfew, or a pollution emergency can wipe out an entire week's income for a delivery partner with no savings buffer.

We were inspired to build **GigShield** — a platform that treats every delivery worker as a policyholder, not just a data point. Instead of complex claim forms and weeks of waiting, GigShield uses AI and real-time environmental data to automatically detect disruptions and trigger instant payouts — no paperwork, no delays.

---

## 💡 What It Does

GigShield is an **AI-powered parametric insurance platform** for platform-based gig economy delivery partners in India. It:

- **Assesses risk dynamically** using hyper-local weather, AQI, traffic, and historical disruption data
- **Calculates weekly premiums** based on the worker's delivery zone, historical risk, and disruption frequency
- **Automatically triggers payouts** when verified disruptions (rain, heat, curfews, strikes) exceed defined thresholds
- **Detects fraud** using multi-signal GPS validation, anomaly detection, and syndicate pattern recognition
- **Protects honest workers** with a tiered review system that never auto-denies — it verifies first

### Core Modules
| Module | Description |
|---|---|
| Risk Assessment Engine | ML model predicting disruption probability per pincode |
| Premium Calculator | Dynamic weekly premium based on real-time risk score |
| Claims Automation | Event-driven payout trigger on verified disruption |
| Fraud Detection | Multi-signal fusion: GPS + IMU + IP + Wi-Fi + behavioral graph |
| Worker Dashboard | Mobile-first UI for policy, claims, and payout tracking |
| Insurer Dashboard | Analytics, fraud alerts, and claims management |

---

## 🛠️ How We Built It

**Frontend:** React.js (mobile-first, PWA-ready)
**Backend:** Node.js + Express REST API
**Database:** MongoDB Atlas
**AI/ML:** Python (scikit-learn, Prophet for time-series risk forecasting)
**External APIs:** OpenWeatherMap, AQICN (Air Quality), Google Maps Geocoding
**Auth:** JWT-based authentication
**Deployment:** Vercel (frontend) + Render (backend)

The system follows an event-driven architecture. When a disruption event is detected via APIs, the risk engine evaluates affected pincodes, cross-references active policyholders, validates claims through the fraud detection layer, and releases payouts automatically.

---

## 🚧 Challenges We Ran Into

- **Data availability:** Hyper-local disruption data for Tier-2/3 cities in India is sparse. We used a combination of weather APIs, government AQI feeds, and historical scraping to bootstrap our risk model.
- **GPS fraud complexity:** Simple GPS verification is trivially bypassed. Building a multi-signal fraud confidence score required careful feature engineering.
- **Premium fairness:** Balancing actuarial soundness with affordability for low-income workers required iterative modeling.
- **Edge connectivity:** Delivery workers in disruption zones often have poor network. Designing offline-tolerant claim flows was a key UX challenge.

---

## 🏆 Accomplishments We're Proud Of

- Built a working risk assessment model that ingests real weather + AQI data
- Designed a fraud detection pipeline that flags syndicates without penalizing genuine workers
- Created a fully mobile-first worker dashboard prototype
- Architected a zero-touch claims flow — from disruption event to payout trigger in under 60 seconds

---

## 📚 What We Learned

- Parametric insurance design requires deep domain understanding — trigger design is everything
- Fraud detection in social welfare platforms must be built with fairness as a first-class constraint
- Real-world insurance products balance risk, affordability, and trust — not just accuracy metrics
- Event-driven microservice architecture is a natural fit for insurance triggers

---

## 🚀 What's Next for GigShield

- **Phase 2:** Working registration flow, live policy management, dynamic premium engine, and 3–5 automated disruption triggers using real APIs
- **Phase 3:** Advanced fraud detection, instant payout simulation, full insurer dashboard, and final pitch deck
- Long-term: Expand coverage to auto-rickshaw drivers, street vendors, and other informal economy workers

---

## 🛡️ Adversarial Defense & Anti-Spoofing Strategy

> **Context (Market Crash Challenge):** A coordinated syndicate of 500 delivery partners exploited a parametric insurance platform using GPS-spoofing apps to fake their location inside a red-alert weather zone — triggering mass false payouts and draining the liquidity pool. This section outlines our multi-layered defense architecture.

### 1. Detecting Fake GPS vs. Genuine Location

Simple GPS coordinates are trivially spoofed. GigShield uses **multi-signal fusion** to compute a real-time **Fraud Confidence Score (FCS)** (0–100) for every claim:

| Signal | Genuine Worker | Fraudster at Home |
|---|---|---|
| **Accelerometer / IMU** | High motion, vehicle vibration | Near-zero motion |
| **Cell Tower Triangulation** | Tower matches claimed GPS zone | Tower points to home area |
| **Wi-Fi SSID Scan** | Unknown/open hotspots | Home/office saved network detected |
| **IP Geolocation** | IP matches claimed region | IP resolves to home ISP |
| **Movement Trajectory** | Was actively moving before event | Teleported instantly to zone |
| **Mock Location Flag** | Disabled | Android mock location enabled |

Claims with FCS < 30 are auto-approved. Scores 30–70 enter soft verification. Scores > 70 go to manual review.

### 2. Data Signals for Fraud Ring Detection

Beyond individual GPS, GigShield monitors:

- **Claim Velocity Anomaly:** 500 claims from the same zone in 30 minutes triggers a Z-score anomaly alert — classic syndicate fingerprint
- **Device Clustering:** Workers filing claims with identical device model, OS version, or app version form a suspicious behavioral cluster
- **Social Graph Analysis:** Claims submitted within seconds of each other by workers who have never co-delivered in the same zone are flagged
- **Historical Presence Check:** Was this worker ever active in the claimed zone before? No prior GPS history in that pincode = red flag
- **Photo/Video EXIF Metadata:** Lighting, timestamp, and weather-consistency check via lightweight CV model
- **Claim Timing vs. Disruption Onset:** Claims filed milliseconds after a disruption event is recorded (before public broadcast) suggest API scraping

### 3. Protecting Honest Workers — Tiered Response System

A hard block on suspicious claims risks denying genuine workers in real emergencies. GigShield uses a **three-tier response** that never auto-denies:

**Tier 1 — Auto-Approve (FCS < 30)**
- All signals consistent → instant payout released

**Tier 2 — Assisted Verification (FCS 30–70)**
- Worker receives in-app prompt: *"We're verifying your location. Please share a 10-second video from your current environment."*
- Video analyzed by CV model for weather-consistent conditions (rain, low visibility, wet roads)
- If GPS is lost during confirmed severe weather in the worker's last known area, system uses last valid location — no penalty

**Tier 3 — Manual Review Queue (FCS > 70)**
- Claim is **paused, not denied** — escalated to human reviewer within 2 hours
- Worker notified: *"Your claim is under verification. You'll hear back within 2 hours. This does not affect your account standing."*
- Confirmed legitimate workers are added to a **Trusted Veteran List** — lower FCS threshold for all future claims

**Appeals & Trust Building**
- Every worker can raise an appeal with evidence (photos, delivery receipts, call logs)
- Clean 6-month history upgrades worker to **Verified Tier** — reduced friction permanently
- Syndicate-flagged workers are reported to platform admin with full audit trail — never silently banned

---

## 👥 Team

**Team Name:** Syntax Shields
**Hackathon:** Guidewire DEVTrails 2026 — Seed. Scale. Soar.

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.
