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

## 5. 💡 Additional Relevant Information

### Why Parametric Insurance Works Here
Traditional insurance requires manual claim filing, document submission, and long verification windows — completely unsuitable for daily-wage gig workers. Parametric insurance uses **objective external data** (weather APIs, AQI feeds) as the trigger, making payouts instant, transparent, and tamper-proof. This is the only model that realistically works for the gig economy.

### Worker Trust & Adoption Strategy
- Zero paperwork onboarding (Aadhaar + Platform ID only)
- First week free trial to demonstrate instant payout credibility
- WhatsApp notification for every trigger event — full transparency
- Hindi/regional language UI support (Phase 3)

---

## 📎 Submission Links
- **Demo Video:** *[Link — to be added]*

---

*Built for Guidewire DEVTrails 2026 — Seed. Scale. Soar. 🚀*
