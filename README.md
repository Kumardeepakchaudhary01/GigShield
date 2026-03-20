# 🛵 GigShield — AI-Powered Parametric Income Insurance for Q-Commerce Delivery Partners

> **Guidewire DEVTrails 2026** | Persona: Q-Commerce Delivery (Zepto / Blinkit) | Platform: Web + Mobile (Flutter)

---

## 1. 📋 Requirement, Persona Scenarios & Application Workflow

### Who We're Protecting
Zepto/Blinkit delivery partners in Indian metros (Mumbai, Delhi, Bengaluru, Hyderabad) who earn ₹2,500–₹5,000/week and have zero income protection against uncontrollable external disruptions.

---

### Persona-Based Scenarios

**Scenario 1 — Heavy Rain (Mumbai)**
Ravi earns ₹800/day on Blinkit doing hyperlocal grocery deliveries. An IMD Red Alert halts outdoor movement for 6 hours.
→ GigShield detects rainfall > 50mm/3hr via API, auto-triggers a claim for all active policyholders in the affected zone, and credits ₹480 (6 hrs × hourly rate) to Ravi's UPI. Zero manual steps.

**Scenario 2 — Bandh / Curfew (Bengaluru)**
Priya loses a full day (₹700) delivering essentials on Zepto when an unplanned bandh shuts her dark store zone.
→ GigShield's traffic anomaly monitor detects a 70%+ drop in zone movement, cross-references with a mock civic alert feed, and auto-initiates her payout before the bandh ends.

**Scenario 3 — Severe Pollution (Delhi)**
Suresh loses 3 days of Zepto deliveries when GRAP Stage 4 restrictions halt vehicle movement (AQI > 400).
→ GigShield polls CPCB AQI API, triggers the pollution clause for his zone, fraud-checks the claim, and processes ₹2,100 instantly.

---

### Application Workflow

```
[Register & KYC] → [AI Risk Profiling] → [Weekly Policy Activation]
        ↓
[Real-time API Monitoring — Weather / AQI / Traffic]
        ↓
[Parametric Threshold Crossed?]
   YES → [Auto Claim Created] → [Fraud Check] → [Instant UPI Payout]
   NO  → [Continue Monitoring]
        ↓
[Worker & Admin Dashboard Updated]
```

**Key Screens:**
- Worker: Register → View Policy → Claim History → Payout Status
- Admin: Active Policies → Disruption Alerts → Loss Ratio → Fraud Flags

---

## 2. 💰 Weekly Premium Model, Parametric Triggers & Platform Choice

### Weekly Premium Model

Premiums are charged **every Monday** for that week's coverage, matching the gig worker's weekly earning cycle.

**Formula:**
```
Weekly Premium = Base Rate × Zone Risk Multiplier × Season Factor × Earnings Factor × Loyalty Discount
```

| Variable | Range | Logic |
|---|---|---|
| Base Rate | ₹30 – ₹80 | Depends on coverage tier |
| Zone Risk | 0.8 – 1.4 | Flood-prone / high-AQI zones cost more |
| Season Factor | 1.0 – 1.3 | Monsoon & Delhi winter = higher risk |
| Earnings Factor | 0.9 – 1.1 | Higher earners pay slightly more |
| Loyalty Discount | 0.95 | After 4 continuous weeks |

**Coverage Tiers:**

| Tier | Weekly Premium | Max Payout/Week |
|---|---|---|
| Basic | ₹29 – ₹49 | ₹800 |
| Standard | ₹59 – ₹89 | ₹1,500 |
| Premium | ₹99 – ₹149 | ₹2,500 |

**Payout Formula:**
```
Payout = (Weekly Earnings / 40 hrs) × Disrupted Hours
         capped at Tier Max Payout
```

> ❌ Hard Exclusions: vehicle repair, health, accident, life insurance

---

### Parametric Triggers

| # | Trigger | API Source | Threshold |
|---|---|---|---|
| 1 | Heavy Rain | OpenWeatherMap | Rainfall > 50mm/3hr OR IMD Red Alert |
| 2 | Severe Pollution | CPCB / OpenAQ | AQI > 400 (GRAP Stage 4) |
| 3 | Extreme Heat | OpenWeatherMap | Feels-like temp > 46°C + Heat Wave advisory |
| 4 | Bandh / Curfew | Traffic API (mock) | Zone traffic drop > 70% for 2+ hrs |
| 5 | Flash Flood | IMD API | Flood alert active in worker's zone |

All triggers are **fully automated** — no manual claim filing by the worker.

---

### Platform Choice — Why Flutter?

- Single codebase targets both **Android** and **Web** — delivery partners can **buy and manage their weekly premium on the go** directly from their phone, while the insurer admin panel runs seamlessly on desktop browsers
- Delivery partners primarily use Android smartphones — Flutter gives a native-feel mobile experience
- Flutter Web serves the Admin/Insurer dashboard on desktop browsers
- Dart + Flutter has fast UI iteration speed
- **FastAPI's REST endpoints** integrate seamlessly with Flutter via the Dio HTTP package

---

## 3. 🤖 AI/ML Integration Plan

### A. Dynamic Premium Calculation
- **Model:** Rule-based weighted scoring in Phase 1 → XGBoost regression in Phase 2+
- **Inputs:** Worker's zone, city, historical disruption frequency, season, declared earnings, claim history
- **Output:** Personalized weekly premium (₹)
- **Where:** Directly served via FastAPI endpoint — no separate microservice needed

### B. Fraud Detection
- **Model:** Isolation Forest (anomaly detection)
- **Checks performed on every claim:**
  - GPS location at time of disruption vs. declared zone
  - Duplicate claims for same event in same zone
  - Claim velocity — unusually high frequency flagged
  - Platform activity check — orders completed during alleged disruption = fraud signal
- **Output:** Fraud Risk Score 0–100. Score > 70 → manual review queue

### C. Risk Profiling (Onboarding)
- On registration, AI assigns a Risk Tier (Low / Medium / High)
- Based on: operating zone's historical disruption rate + city + declared working hours
- Risk Tier feeds directly into the premium multiplier

### D. Predictive Analytics (Admin Dashboard — Phase 3)
- Uses 7-day weather forecast + historical seasonal data
- Predicts expected claim volume and payout liability for next week
- Helps insurer manage reserves and loss ratios proactively

---

## 4. 🛠️ Tech Stack & Development Plan

### Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Flutter (Dart) — Web + Android from single codebase |
| Backend | Python 3.11 + FastAPI + JWT Auth |
| Database | MySQL 8 + SQLAlchemy ORM |
| ML Service | XGBoost + Scikit-learn (built into FastAPI) |
| External APIs | OpenWeatherMap, CPCB AQI, Razorpay Test Mode |
| Infra | Docker + Docker Compose + Kubernetes (K8s) + GitHub Actions |

### Development Plan

| Phase | Dates | Deliverables |
|---|---|---|
| **Phase 1** | Mar 4–20 | README, architecture, premium model design ✅ |
| **Phase 2** | Mar 21–Apr 4 | Registration, policy management, premium calculator, 5 triggers, basic fraud detection |
| **Phase 3** | Apr 5–17 | Advanced fraud detection, Razorpay payout, worker + admin dashboards, final pitch deck |

---

## 5. 🌍 Why This Solution Will Work in the Real World

### Why Parametric Insurance Works Here
Traditional insurance requires manual claim filing, document submission, and long verification windows — completely unsuitable for daily-wage gig workers. Parametric insurance uses **objective external data** (weather APIs, AQI feeds) as the trigger, making payouts instant, transparent, and tamper-proof. This is the only model that realistically works for the gig economy.

### Worker Trust & Adoption Strategy
- Zero paperwork onboarding (Aadhaar + Platform ID only)
- First week free trial to demonstrate instant payout credibility
- WhatsApp notification for every trigger event — full transparency
- Hindi/regional language UI support (Phase 3)

---

## 🛡️ Adversarial Defense & Anti-Spoofing Strategy

> **Threat:** A coordinated syndicate of 500+ workers using GPS-spoofing apps to fake locations inside active weather alert zones, triggering mass false payouts and draining the liquidity pool.

> **Q-Commerce Advantage:** Due to our Q-Commerce persona, GigShield has a **structural advantage** against GPS spoofing — Zepto/Blinkit workers operate from fixed dark stores within a 2–5 km hyperlocal radius. Dark store proximity validation makes location fraud significantly harder to execute compared to food delivery workers who roam freely across a city. Our defense strategy builds on this inherent advantage.

---

### 1. Differentiating a Genuine Stranded Worker vs. a Bad Actor

GPS coordinates alone are **never trusted**. GigShield uses a **multi-signal corroboration engine** — a claim is only considered genuine when multiple independent signals agree.

| Signal | Genuine Worker | Spoofer |
|---|---|---|
| **Platform activity** | Order attempts made, then failed/cancelled during disruption | Zero order attempts — no engagement with the platform at all |
| **Device sensor data** | Accelerometer shows stationary/low-movement pattern consistent with being stuck | Spoofing apps run on idle phones — no realistic motion signature |
| **Network triangulation** | Cell tower + WiFi signals place device in the claimed zone | GPS says Zone A, but cell towers resolve to a different location |
| **Battery & signal behavior** | Fluctuating signal strength consistent with bad weather | Clean, stable signal — inconsistent with a Red Alert zone |
| **Historical zone presence** | Worker has previously operated in this zone on normal days | Worker has never historically operated in the claimed zone |

The AI model computes a **Trust Score (0–100)** by combining all signals. A claim only auto-approves at Trust Score ≥ 75.

---

### 2. Data Points Used to Detect a Coordinated Fraud Ring

Beyond GPS, the system analyzes:

- **Claim burst detection:** If 20+ claims originate from the same zone within a 10-minute window, the system flags it as a potential coordinated event and pauses auto-approval for that batch
- **Telegram/social signal correlation:** Sudden claim spikes that correlate with no verified API trigger (weather, AQI) are flagged — genuine disruptions always have an API footprint
- **Device fingerprinting:** Multiple claims from the same physical device (shared phones within a syndicate) are flagged as duplicates regardless of different worker IDs
- **Velocity analysis:** Workers who file claims in zones they have never delivered in before score low on the historical zone presence check
- **Network graph analysis:** If a cluster of workers all registered within the same 48-hour window AND all file claims simultaneously, the system treats them as a correlated group and escalates to manual review
- **Platform cross-check:** Zepto/Blinkit platform API (simulated) is queried — if the platform shows the worker accepted or completed orders during the alleged disruption window, the claim is auto-rejected

---

### 3. UX Balance — Handling Flagged Claims Without Penalizing Honest Workers

A genuine worker in a Red Alert zone may have poor network, low battery, and no platform activity — which could look suspicious. The system handles this through a **3-tier response model:**

**Tier 1 — Auto-Approved (Trust Score ≥ 75)**
All signals align. Payout processed instantly. No friction for the worker.

**Tier 2 — Soft Flag (Trust Score 50–74)**
Worker receives an in-app notification:
> *"Your claim is under a quick review. This usually resolves in under 2 hours. No action needed from you."*
- A lightweight secondary check runs: the system waits for the disruption event to end, then cross-checks platform re-engagement (did the worker resume deliveries after the disruption lifted?)
- If re-engagement is confirmed → claim approved, payout released
- Worker is never asked to "prove" they were stuck — the system finds proof passively

**Tier 3 — Hard Flag (Trust Score < 50)**
Claim goes to manual review queue with full signal report for the admin.
- Worker notified with transparency:
> *"We need a little more time to verify your claim. You will hear back within 24 hours."*
- If the worker is genuine and frustrated, they can raise a dispute via a one-tap in-app button — no long forms
- Repeated false claims from the same worker ID permanently lower their Trust Score baseline

**The core principle:** Honest workers are never asked to do more work. The burden of proof is on the system, not the worker. Fraud is caught passively through data, not through interrogating every claimant.

---

### Anti-Spoofing Architecture Summary

```
Claim Initiated (auto or manual)
         ↓
Multi-Signal Trust Engine
  ├── Platform activity check     (Zepto/Blinkit API)
  ├── Device sensor corroboration (accelerometer + network)
  ├── Cell tower triangulation    (vs. GPS claim)
  ├── Historical zone presence    (worker's past delivery zones)
  └── Syndicate graph analysis    (burst detection + device fingerprint)
         ↓
Trust Score computed (0–100)
  ├── ≥ 75 → Auto-approve → Instant UPI payout
  ├── 50–74 → Soft flag → Passive re-engagement check → Approve/Escalate
  └── < 50 → Hard flag → Manual review queue → 24hr resolution
```

---

## 📎 Submission Links
- **GitHub Repo:** *[Link]*
- **Demo Video:** *[Link — to be added]*

---

*Built for Guidewire DEVTrails 2026 — Seed. Scale. Soar. 🚀*
