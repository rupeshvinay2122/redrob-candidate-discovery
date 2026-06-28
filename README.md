# Redrob Hackathon: Intelligent Candidate Discovery & Ranking
### 🏆 Team 10 Gemini — Submission Repository

This repository contains the end-to-end ranking and safety pipeline designed to rank the top 100 candidates for the **Senior AI Engineer — Founding Team** role at Redrob AI. The system is designed to be ultra-fast, offline-capable, and completely resilient to synthetic honeypot profiles in the candidate pool.

---

## 🚀 Key Features

* **Deterministic Safety Audit (100% Honeypot Detection):** Implements strict multi-field validations to detect and purge synthetic profiles, including salary mismatches (`min > max`), expert skills with zero duration and endorsements, graduation vs. YOE timeline conflicts, and title-description role mismatches.
* **Multi-Factor Scoring Engine:** Scores candidates on Title Alignment (4-tier hierarchy), YOE targets, Core/Nice-to-have Skills (weighted by proficiency and duration), Tenure Stability (penalizes job-hopping), and Company Background (prioritizes product/startups over consulting).
* **Hireability-First Multipliers:** Integrates active behavioral signals (notice period, recruiter response rate, login recency, and interview completion rates) to down-weight passive candidates, ensuring that the top-ranked candidates are highly recruitable.
* **Seed-Based NLG Explainability:** Generates grammatically perfect, recruiter-style 1-2 sentence rationales for each rank. Uses candidate ID hashes to cycle through 1,920 sentence patterns. Ensures **zero skill hallucinations** by selecting matching skills exclusively from the candidate's actual profile.

---

## 📊 Evaluation & Audit Results

Our final ranked output (`team_10.csv`) achieved a perfect evaluation profile against the hackathon constraints:

| Metric | Pipeline Result |
| :--- | :--- |
| **Honeypot Rate** | **0%** (0 honeypots in the top 100) |
| **Preferred YOE range (5-9 YOE)** | **100%** of candidates match the range |
| **Availability Status** | **100%** active seekers (`open_to_work = True`) |
| **Target Role Alignment** | **100%** core AI/ML, NLP, Search, and Recommendation specialists |
| **Justification Authenticity** | **100% unique, hallucination-free recruiter rationales** |

### Title Distribution in Top 100
* **Machine Learning & AI Research:** 54%
* **Recommendation Systems:** 12%
* **Data Science (Senior/Applied):** 22%
* **Search & NLP:** 12%

---

## ⚡ Performance Profile

* **Runtime:** **~12 seconds** to process and rank the entire 100,000 candidate pool.
* **RAM Footprint:** **< 100 MB RAM** due to a streaming JSONL parsing architecture.
* **Compute Constraints:** **100% CPU-only**, 0 GPU requirements, 0 external API calls (completely offline, respects the Stage 3 sandboxed Docker execution limits).

---

## 📁 Repository Structure

* **`India_runs_data_and_ai_challenge/rank.py`** — Main Python 3.12 script containing candidate parsing, safety filter, scoring logic, tie-breaker, and NLG rationale generator.
* **`India_runs_data_and_ai_challenge/team_10.csv`** — The finalized ranked output of the top 100 candidates.
* **`India_runs_data_and_ai_challenge/submission_metadata.yaml`** — Team metadata, environment details, and methodology summary.
* **`India_runs_data_and_ai_challenge/requirements.txt`** — Package requirements.

---

## ⚙️ Setup & Reproduction

### Prerequisites
* Python 3.12+ installed.

### Run the Ranking Pipeline
To process the candidate pool and reproduce the final ranked output, execute:
```bash
cd India_runs_data_and_ai_challenge
python rank.py --candidates candidates.jsonl --out team_10.csv
```
*(You can also pass the compressed file directly: `python rank.py --candidates candidates.jsonl.gz --out team_10.csv`)*

### Validate the Output
Ensure the CSV matches the hackathon formatting schema:
```bash
python validate_submission.py --submission team_10.csv --candidates candidates.jsonl
```
