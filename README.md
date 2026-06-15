# Next-Gen-Explainable-Hybrid-WAF

An enterprise-grade, dual-engine Web Application Firewall (WAF) that fuses deep learning with deterministic validation loops. This system combines a multi-modal neural network with a real-time regex circuit breaker ring, preventing sophisticated mutation exploits while eliminating edge-case false positives.

---

## 🏗️ System Architecture

This gateway operates on a **Late-Fusion Hybrid Model Architecture**, processing incoming payloads simultaneously through two distinct data streams:

1. **Linguistic Sequence Stream:** A Character-level Tokenizer feeds a **Bidirectional LSTM (128 units)** to capture sequential context, semantic structures, and mutation-based syntax bypasses.
2. **Structural Feature Stream:** A dedicated vector math engine extracts **42 tabular metadata features** (e.g., Shannon Entropy, uppercase/digit ratios, character densities, obfuscation flags) processed through deep `Dense` and `BatchNormalization` layers.


```

```
              ┌──────────────────────────────────────────────┐
              │            Incoming Web Payload              │
              └──────────────────────┬───────────────────────┘
                                     │
                ┌────────────────────┴────────────────────┐
                ▼                                         ▼
     [ Linguistic Sequence ]                  [ Tabular Meta-Features ]
     Character Tokenization                    42 Structural Metrics
                │                                         │
                ▼                                         ▼
       Bidirectional LSTM                         Dense Neural Net
         (128 Context Units)                      (Feature Extraction)
                │                                         │
                └────────────────────┬────────────────────┘
                                     ▼
                       [ Merged Late-Fusion Layer ]
                                     │
                                     ▼
                        Softmax 4-Class Prediction
                    (Benign, SQLi, XSS, SSRF)
                                     │
                                     ▼
                 ┌───────────────────────────────────────┐
                 │  Dual-Sided Validation Ring Core      │
                 │  (Anti-Evasion & FP/FN Mitigation)    │
                 └───────────────────┬───────────────────┘
                                     │
                  ┌──────────────────┴──────────────────┐
                  ▼                                     ▼
         [ Traffic Allowed ]                   [ Threat Blocked ]
           Route to App                         - STIX 2.0 JSON Report
                                                - Inline SHAP Diagnostics

```

```

### 🛡️ Dual-Sided Validation Ring (Circuit Breaker)
To bridge the gap between AI unpredictability and high-throughput reliability, a deterministic validation ring enforces boundaries on predictions:
* **False Positive (FP) Mitigation:** If the AI flags a payload as malicious, but it completely lacks physical execution tokens (e.g., clean marketing copy containing the word "select"), the ring gracefully down-routes the final verdict to `Benign`.
* **False Negative (FN) Mitigation:** If an attacker crafts a highly obfuscated payload that tricks the neural network into a `Benign` score, the ring catches explicit tokens and triggers an immediate override to drop the traffic.

---

## 🚀 Key Features

* **Recursive Canonicalization:** Decodes up to 3 layers of nested obfuscation (URL encoding, HTML entities, hex variations, and selective inline Base64 fragments) before evaluation.
* **Explainable AI (XAI):** Integrated with **SHAP (SHapley Additive exPlanations)** to break open the AI "black box," printing the top 3 structural features responsible for a specific threat verdict.
* **Automated Threat Intel:** Compiles verified attacks directly into **MITRE ATT&CK** mapped **STIX 2.0** cryptographic JSON bundles for automated SOC orchestration.
* **Layer-Frozen Soft Fine-Tuning:** Features a targeted patch mechanism that locks early representation graphs (`Bi-LSTM` & `Embedding`) to micro-tune final layers using human-analyst logs without catastrophic forgetting.

---

## 📦 Installation & Setup

### Prerequisites
* Python 3.8+
* TensorFlow 2.x

### 1. Clone the Repository
```bash
git clone [https://github.com/YOUR_USERNAME/structural-hybrid-waf.git](https://github.com/YOUR_USERNAME/structural-hybrid-waf.git)
cd structural-hybrid-waf

```

### 2. Install Enterprise Dependencies

```bash
pip install numpy pandas tensorflow sklearn shap stix2 urllib3

```

---

## 🛠️ Usage Pipeline

The gateway is built as an interactive master control pipeline managing baseline training, analyst fine-tuning, and live production inspection.

### 1. Baseline Training (Phase A)

Train the core fusion model from scratch on your balanced dataset (`unified_web_vulnerabilities_dataset_fixed.csv`):

```python
from main import run_clean_baseline_training
run_clean_baseline_training()

```

### 2. Analyst-Led Fine-Tuning (Phase B)

Ingest human-corrected edge cases (`manually_corrected_edge_cases.csv`) to patch false positives seamlessly:

```python
from main import run_soft_finetuning_pipeline
run_soft_finetuning_pipeline()

```

### 3. Running the Gateway Runtime

Launch the interactive CLI gateway to simulate real-time request screening with inline SHAP diagnostics enabled:

```
python main.py

```

---

## 🔍 Incident Evaluation Logs (Example)

When clean parameter structures like `search?q=laptop+deals&category=5` are passed to the gateway, the hybrid layer intercepts rule-based false positives:


Payload Evaluated: "search?q=laptop+deals&category=5"
 ├─ ML Prediction Model Alert: Benign (99.84%)
 ├─ [CIRCUIT BREAKER OVERRIDE]: Conflicting structural parameters. Re-routing payload...
 └─ RUNTIME ROUTING DECISION: ──> **Benign** (Final Status Confirmation)

```

If a definitive threat passes through:


Payload Evaluated: "SELECT * FROM users WHERE id = 1 OR 1=1; --"
 └─ RUNTIME ROUTING DECISION: ──> **SQLi** (Final Status Confirmation)
    [!] Threat Confirmed. Auto-generating cryptographic STIX report...
    [+] Forensic asset written to workspace disk as: 'stix_threat_report_SQLi_20260614_000640.json'

    🔬 [INLINE SHAP DEEP ANALYSIS FOR CURRENT PAYLOAD]:
      └─ Top 1: Op_SQLComment (Impact Magnitude: 0.4281)
      └─ Top 2: Flag_SQLTautology (Impact Magnitude: 0.3114)
      └─ Top 3: Shannon_Entropy (Impact Magnitude: 0.0812)

## 📄 File Deliverables & Workspace Assets

* `structural_hybrid_waf_model.keras` — Saved baseline network binary.
* `tokenizer_structural_v3.pkl` — Character indexing configuration file.
* `waf_triage_audit.log` — Raw triage log queue for offline forensics.
* `stix_threat_report_*.json` — Structural threat data for SIEM integration.
* `offline_forensic_report.txt` — Generated root-cause analysis summary.

---

## ⚖️ License

Distributed under the MIT License. See `LICENSE` for more information.

```

```
