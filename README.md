# com-nougueR
Fixed‑Time Equivalence with Censored Data (Com‑Nougué 1993)

Author: Philip M. Croon
Method basis: Com‑Nougué, Rodary, Patte (1993), Statistics in Medicine 12:1353–1364 — “How to establish equivalence when data are censored: A randomized trial of treatments for B non‑Hodgkin lymphoma.”

What this does

This code estimates risk difference at a fixed time T using Kaplan–Meier (KM) with censoring, and provides a 95% CI for the difference using Greenwood’s variance (Com‑Nougué Eq. 9). It also (optionally) reports the relative log‑survival ratio r at T, which Com‑Nougué uses to map absolute margins to a relative scale.

Primary output
	•	Cumulative incidence at T in Intervention and Control
p_g(T) = 1 - \hat{S}_g(T)
	•	Risk difference (Intervention − Control) at T:
\mathrm{RD} = p_I(T) - p_C(T)
	•	95% CI for RD using Greenwood‑based Wald interval on S(T), then transformed to p(T) and differenced (Com‑Nougué §5.2).

Optional output
	•	Log‑survival ratio at T:
r = \dfrac{\log \hat{S}_N(T)}{\log \hat{S}_S(T)}
(Only needed if you want a relative measure or to map a pre‑specified absolute margin A_0 to a relative margin r_0.)

⸻

Why this method
	•	Handles censoring via KM.
	•	Reports absolute effect at a clinically meaningful time T.
	•	Matches the fixed‑time equivalence framework in Com‑Nougué (1993) without requiring proportional hazards or Cox modeling.

⸻

Inputs

A single Excel file with (at minimum) these columns:
	•	Randomization Group (0 = C, 1 = I) — 0 = control, 1 = intervention
	•	time_to_event_or_end_of_study — analysis time in days
	•	outcome_af — event indicator (1 = event by that time, 0 = censored)

Adjust the column names in the script if your file uses different labels.

⸻

How it’s computed
	1.	KM at T:
Fit KM separately by arm and read off \hat{S}_g(T) and its SE via Greenwood.
	2.	Cumulative incidence:
\hat{p}_g(T) = 1 - \hat{S}_g(T)
	3.	Risk difference:
\widehat{\mathrm{RD}} = \hat{p}_I(T) - \hat{p}_C(T)
	4.	CI for RD (Com‑Nougué Eq. 9):
Since \hat{S}_g(T) is asymptotically normal with var from Greenwood,
\hat{p}_g(T) = 1 - \hat{S}_g(T),\quad
\operatorname{Var}\{\hat{p}_I(T) - \hat{p}_C(T)\}
= \operatorname{Var}\{\hat{S}_I(T)\} + \operatorname{Var}\{\hat{S}_C(T)\}.
Wald 95% CI on RD uses z=1.96.
	5.	(Optional) Log‑survival ratio r:
r = \log \hat{S}_N(T) / \log \hat{S}_S(T)


