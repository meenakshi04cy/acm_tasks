# GigShield — AI-Powered Parametric Insurance for Gig Delivery Workers

> Guidewire DEVTrails 2026 | Phase 1 Submission

---

## The Problem

India's food delivery partners (Swiggy/Zomato) are the backbone of our digital economy. But when a rainstorm hits, a curfew is declared, or AQI spikes to hazardous levels — they simply cannot work. No orders, no income. No safety net.

A delivery partner earning ₹600/day loses ₹1,800–2,400 in a 3-day disruption event. There is currently no financial product designed to protect this loss. GigShield changes that.

---

## Our Solution

**GigShield** is an AI-enabled parametric income insurance platform exclusively for food delivery partners (Zomato/Swiggy). It automatically detects external disruptions, triggers claims without any action from the worker, and credits lost wages instantly — all on a simple weekly subscription model.

No paperwork. No waiting. No rejection for things outside the worker's control.

---

## Persona

**Target User:** Food delivery partners operating on Zomato and Swiggy in Tier-1 Indian cities (Bengaluru, Mumbai, Chennai, Hyderabad).

**Typical profile:**
- Earns ₹400–800/day depending on zone and hours
- Works 6–10 hours/day, 6 days/week
- Paid weekly by the platform
- Has no access to formal insurance or income protection products
- Highly sensitive to weather, local events, and zone-level disruptions

**Sample scenarios our platform covers:**

| Scenario | Disruption | Income Impact | GigShield Response |
|---|---|---|---|
| Heavy monsoon in Koramangala | Rainfall > 25mm/hr | 0 orders for 4 hours | Auto-payout ₹240 |
| AQI hits 320 in Delhi | Hazardous air quality | Worker stops riding | Auto-payout ₹150/hr |
| Local bandh in HSR Layout | Zone closure | Full day lost | Auto-payout ₹600 |
| Extreme heat > 43°C | Outdoor work halted | Partial day lost | Auto-payout ₹180 |

---

## Coverage Scope

GigShield covers **income loss only** from the following parametric triggers:

- Extreme weather (rainfall, heat, flooding)
- Severe air pollution (AQI > 300)
- Social disruptions (curfews, strikes, zone closures)
- Platform-level outages causing order volume collapse

**Strictly excluded:** health insurance, accident cover, vehicle repair, life insurance.

---

## Weekly Premium Model

Gig workers are paid weekly. Our premium model mirrors this cycle.

**Base weekly premium: ₹49–₹149/week** depending on risk profile.

Premium is dynamically calculated by our ML model using:

```
Weekly Premium = Base Rate
              + Zone Risk Score (historical disruption frequency)
              + Weather Exposure Index (seasonal forecast)
              + Worker Activity Score (hours worked, zone coverage)
              + Platform Tenure Adjustment (loyalty discount)
```

**Example:**
- Worker in low-risk zone, 3+ months on platform → ₹49/week
- Worker in flood-prone zone, new to platform → ₹119/week
- Maximum coverage: ₹3,500/week (~5 days × ₹700/day)

Weekly auto-deduction from platform wallet at the start of each work week.

---

## Parametric Triggers

Claims are initiated **automatically** when a monitored parameter crosses a defined threshold. The worker does not need to file anything.

| Trigger | Threshold | Payout |
|---|---|---|
| Rainfall | > 20mm/hr in worker's zone | ₹150/hr idle |
| Extreme heat | Temperature > 42°C | ₹100/hr idle |
| Air quality | AQI > 300 (Hazardous) | ₹120/hr idle |
| Flood alert | IMD red alert in zone | ₹600/day flat |
| Curfew / bandh | Detected via news signals in zone | ₹600/day flat |
| Platform outage | Order volume drop > 70% in zone | ₹200/hr idle |

---

## Application Workflow

```
Worker Onboards
      ↓
Risk Profile Generated (ML model scores zone + history)
      ↓
Weekly Policy Created (premium deducted from wallet)
      ↓
Disruption Monitor runs every 15 minutes
      ↓
Trigger threshold crossed → Claim auto-initiated
      ↓
Fraud Detection layer validates claim
      ↓
Claim approved → Instant payout via UPI/wallet
      ↓
Worker dashboard updated with earnings protected
```

---

## AI/ML Integration

### 1. Dynamic Premium Calculation
**Model:** XGBoost Regressor
**Features:** zone risk score, historical disruption frequency, weather exposure index, worker activity score, platform tenure
**Training data:** Synthetic dataset of 5,000 worker profiles seeded with real historical weather data from Open-Meteo API (Bengaluru/Mumbai, 12 months)
**Output:** Weekly premium in ₹

### 2. Risk Profiling
**Model:** Logistic Regression / Random Forest classifier
**Purpose:** Assigns each worker a risk tier (Low / Medium / High) at onboarding based on their operating zone, time of day patterns, and historical disruption data for that zone
**Output:** Risk score 0–1, mapped to premium tier

### 3. Fraud Detection (our differentiator)
**Model:** Isolation Forest (unsupervised anomaly detection)

We implement a multi-layer fraud detection system:

**Layer 1 — Behavioral fingerprinting:** Each worker builds a movement and activity profile over time. Claims are cross-checked against this baseline. A worker who is always active 8am–6pm but suddenly claims disruption at 2pm triggers a review flag.

**Layer 2 — Zone-level claim correlation:** If a worker claims disruption but no other worker in the same zone filed a claim for the same event, the claim is flagged for review. Genuine disruptions affect multiple workers simultaneously.

**Layer 3 — GPS idle validation:** Worker's device GPS must show idle or minimal movement during the claimed disruption window. Active movement during a "rainstorm" claim is flagged.

**Layer 4 (architected):** Graph-based collusion detection using NetworkX — identifies clusters of workers filing correlated suspicious claims. Planned for post-hackathon deployment.

---

## Platform Choice

**Web application** (React frontend + FastAPI backend)

Rationale:
- Delivery partners access administrative functions (policy management, claim history) from home on a browser, not mid-delivery
- Admin/insurer dashboard is better suited to a larger screen
- Faster to build and demo within the 8-day hackathon timeline
- Future mobile app (React Native) planned for Phase 2

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React + TailwindCSS |
| Backend | FastAPI (Python) |
| ML Models | scikit-learn, XGBoost |
| Database | PostgreSQL |
| Weather API | OpenWeatherMap (free tier) + Open-Meteo (historical) |
| AQI API | OpenAQ |
| News/social signals | GNews API |
| Mock payout | Razorpay Test Mode |
| Task scheduler | APScheduler (disruption monitor polling) |
| Hosting | Render / Railway (free tier) |

---

## Data Strategy

We use a **hybrid data approach** — a standard practice for new insurance products where historical claim data does not yet exist:

1. **Real weather data** from Open-Meteo API (12 months, Bengaluru + Mumbai) anchors all disruption signals
2. **Synthetic worker profiles** (5,000 rows) generated using domain-seeded rules reflecting real gig economy labor patterns
3. **Fraud labels** injected at 5% rate using known fraud patterns (GPS anomalies, zone mismatches, timing inconsistencies)

This approach is consistent with actuarial modeling practices for new insurance product lines.

---

## Development Plan

| Day | Focus |
|---|---|
| 1–2 | Worker onboarding flow, database schema, basic auth |
| 2–3 | Risk profiling ML model + synthetic data generation |
| 3 | Weekly premium engine + policy creation |
| 3–4 | Disruption monitor (OpenWeatherMap + OpenAQ polling) |
| 4–5 | Claims engine (auto-trigger + approval logic) |
| 5–6 | Fraud detection (Isolation Forest + GPS validation) |
| 6 | Mock payout integration (Razorpay test mode) |
| 7 | Worker dashboard + admin dashboard |
| 8 | Integration testing, demo recording, README polish |

---

## Repository Structure

```
gigshield/
├── frontend/               # React app
│   ├── src/
│   │   ├── pages/          # Onboarding, Dashboard, Claims
│   │   └── components/
├── backend/                # FastAPI app
│   ├── main.py
│   ├── routers/
│   │   ├── auth.py
│   │   ├── policy.py
│   │   ├── claims.py
│   │   └── payout.py
│   ├── ml/
│   │   ├── premium_model.py
│   │   ├── fraud_model.py
│   │   └── data_generation.py
│   └── scheduler/
│       └── disruption_monitor.py
├── data/
│   └── synthetic_training_data.csv
└── README.md
```

---

## Team

> [Add your team member names and roles here]

---

## Demo Video

> [Add your 2-minute Phase 1 video link here]

---

*Built for Guidewire DEVTrails 2026 | Phase 1 Submission — March 20, 2026*
