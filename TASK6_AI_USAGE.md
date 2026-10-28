# Task 6 — How I used AI

## Tools used
- **Grok (this session)** for end-to-end analysis: data profiling, anomaly detection, metric design (lag-aware conversion), ranking logic, budget allocation with diminishing returns, chart generation, and PDF report writing.
- **pandas / numpy / matplotlib / reportlab** (executed via Python in the sandbox) for all numerical work and deliverables. No external AI coding assistants beyond the conversation itself.

## Single best prompt (pasted exactly)
```
There is one real problem in this file. Find it, say what you think happened, 
say which rows it touches, and say exactly how you handled it — dropped, flagged, 
interpolated, left alone. how you handled it matters more than what you called it.
```
(Used while probing the Google mid-July collapse. The model correctly surfaced the 12-row anomaly after I asked it to look for days where trials collapsed while installs stayed normal.)

## One thing the model got wrong
Early in exploration the model suggested that same-day `renewals / trials` was “fine for a 90-day window” and that the lag warning was mostly theoretical. When I forced a lag-matched calculation (renewals[d] / trials[d-1]), the rates moved by 0.5–1 pp on some campaigns — small but real, and exactly the direction the brief warned about when spend is trending. I caught it by implementing both methods side-by-side and comparing. Thereafter every conversion rate in the deliverable is lag-aware.

(Also: an early ROAS formula momentarily double-counted the ₹1 trial revenue; corrected by verifying `revenue == trials*1 + (renewals-annual)*199 + annual*1199` before any ratios were used.)
