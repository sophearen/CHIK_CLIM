# CHIK_CLIM research progress summary

**Date:** 2026-11-05
**Basis:** `03_Output/*/reports/*.md`, `03_Output/16_research_restart_assessment/RESEARCH_RESTART_ASSESSMENT.md` (2026-10-18, the most recent output), the pipeline READMEs, and `git status`. This document summarises existing outputs; nothing was refitted or rerun.

> **One-line status:** Absolute future-epidemic forecasting with the Ceará renewal model stopped because the introduction process is unresolved. The SSP track is complete up to the continuous thermal suitability index K, but stops just before future FOI prediction because the K-to-FOI coefficient β is not identifiable (decision C). The shared conclusion of both tracks is that conditional, scenario-labelled analyses remain defensible.

---

## 1. Overall structure: three tracks plus two cross-cutting assessments

| Track | Modules | Core question | Final decision |
|---|---|---|---|
| **A. Historical reconstruction (renewal)** | `02_ceara_pipeline`, `03`–`06` state pipelines, `07_national_pipeline` | Can weekly R0, susceptibility S/N and reporting q be reconstructed for 2015–2025? | Ceará accepted without caveat. BA/RJ/MT carry q-identifiability caveats; PE not accepted |
| **B. Future generator (renewal → 2026–2045)** | `02_ceara_pipeline/10`–`18`, `08_vaccination_policy_burden_pipeline` | Can the reconstructed model generate defensible future epidemics? | **Absolute forecasting not defensible (Step 7.5).** Only labelled conditional contrasts and vaccination-strategy ranges |
| **C. SSP climate track (suitability → FOI → burden)** | `11`–`15` | Can SSP-specific future FOI, infections and severe burden be projected? | Thermal index K complete (decision A). **K→FOI elasticity β not identifiable (C)**; initial immunity unstable (C) |
| **Cross-cutting** | `00_integrated_architecture_assessment`, `16_research_restart_assessment` | What is reusable; where to stop | Architecture A recommended. Gate G1 met; G4–G6 not met |

## 2. Chronological progress with figure matching

### Stage 1. Data pipeline and the Ceará reference model (early–mid Sept 2026, committed)

- SINAN 2015–2025 individual records → municipality-week panels; IBGE 2024-revision single-age population 2000–2070; births; ERA5-Land climate.
- Ceará renewal v1 → v4.9 (hierarchical seasonality, serology anchor, fixed q = 0.05) → **climate-forced v4.9 canary** (adds β_T, β_P) is the accepted fit. Clean HMC (0 divergences, Rhat ≤ 1.003).
- Age extension: single-year S/U reconstruction; the cohort simulator reproduces the accepted fit exactly (closed-loop replay, error ~1e-8).

| Figure | Content |
|---|---|
| `renewal_v4_9_hierarchical_seasonality/renewal_v4_9_trajectories_ceara_q0.05.png` | v4.9 baseline reconstruction |
| `climate_forced_v4_9/CE_climate_canary_six_panel.png` | Accepted climate-forced fit, six panels |
| `CE_canary_beta_posteriors.png`, `CE_canary_climate_multiplier_posterior.png` | β_T, β_P posteriors and climate multiplier |
| `QC_A`–`QC_E` | Climate-anomaly covariate QC |
| `AGE_DIAG_FigA1`–`FigD` | Age-specific susceptibility/immunity heatmaps, group trajectories, infection curves, cross-sections |
| `PHASE1_2_closed_loop_replay_validation.png`, `PHASE4_*`, `PHASE5_cohort_qa_summary.png` | Cohort-engine validation |

### Stage 2. Historical age-12 vaccination counterfactual, Ceará 2015–2021 (committed)

- Mechanism stress test (100% coverage, 100% VE, lifelong protection). Infections averted 538,773 (21.8%); NNV 1.69 doses per infection and 33.9 per reported case. Rebound audit: ~35 percentage points of benefit displaced into later quiet years, cumulative effect remains strongly positive.

| Figure | Content |
|---|---|
| `main/fig1_historical_age12_vaccine_impact.png` | Infections with vs without vaccine |
| `main/fig2_transmission_feedback_mechanism.png` | Direct/indirect decomposition, R_eff |
| `main/fig3_age12_cohort_vaccination.png` | Cohort vaccination schedule |
| `nnv/fig_nnv1`–`nnv5` | Primary NNV, year-end cumulative NNV, susceptibility vs NNV, effect decomposition, age-specific benefit |
| `rebound_audit/FIG_R1`–`R5` | Rebound timing, mechanism, import sensitivity, coverage/VE sensitivity, low-incidence context |
| `supplementary/S1`–`S8` | QA: replay, conservation, direct-only diagnostic, age-specific weekly curves, posteriors, climate multiplier, R_eff threshold |

### Stage 3. Future-projection pilot and the retrospective validation chain (12–13 Oct 2026; Steps 1–8b)

This chain is **where the renewal track stopped**.

| Step | Module | Conclusion | Figures |
|---|---|---|---|
| Step 1 climate-inference audit | `10_future_projection_pilot` | **B.** Climate explains ~1% of log R0 variance (seasonality 80%, year effects 19%). cor(β_T, year_effect[2022]) = 0.80 → not a portable climate–transmission relationship | FIG01 coefficient inference, FIG02 historical climate contribution, FIG03 baseline vs climate model, FIG04 transmission decomposition |
| Step 2 future generator | same | **B.** Engine exact (replay, mass conservation). Scenario layer (year-effect magnitude, importation, outbreak rule) unvalidated | FIG05 future transmission potential, FIG06 simulated trajectories, FIG07 epidemic mechanism, FIG08 scenario sensitivity, FIG09 5- vs 10-year outcomes |
| Year-effect diagnostic | `11_year_effect_diagnostic` | Future year effects under fixed SD 0.40 vs history | FIG10 historical vs future distributions, FIG11 annual peak R0, FIG12 paired cumulative infections |
| Climate robustness | `11_climate_robustness` | **No fit improvement** in any of 5 specifications (PPC 96.7–96.9%, RMSE 452–458). 2022 confound robust (cor 0.77–0.83) | FIG13 coefficient robustness, FIG14 multiplier by model, FIG15 climate–year-effect confounding, FIG16 reconstruction sensitivity |
| Future year-effect validation | `12_future_year_effect_validation` | Estimated τ_year median ~0.14, far below the fixed 0.40. H1/H2 training-only splits | FIG17 τ_year prior/posterior, FIG18 historical vs future year effects, FIG19 retrospective holdout trajectories, FIG20 generator comparison |
| HMC reliability audit | `14_hmc_reliability_audit` | Mild divergences at large τ_year in all five fits; upper tail provisional | `HMC_AUDIT_*`, `P2_mixing_*` |
| Step 6 retrospective failure | `15_step6_*` | **B.** 2022 under-prediction is due to **introduction pressure** (near-fadeout 2020–21), not susceptibility or transmission potential | FIG21 susceptibility/transmission diagnostics, FIG22 introduction/establishment, FIG23 mechanistic explanation of 2022 failure |
| Step 7 importation/persistence | `16_step7_*` | Even 4× λ_import under-predicts 2022 by up to **369×**; timing permutation does not help → introduction **magnitude** | FIG24 importation vs entry infectiousness, FIG25 pre/post-2022 importation, FIG26 fade-out timing, FIG27 predictive-performance sensitivity |
| Step 7.5 projection readiness | `17_step7_5_*` | A. Absolute 2026–2045 incidence: **not defensible**. B. Conditional climate contrasts: defensible if labelled. C. Vaccination comparisons: only across a disclosed scenario range | FIG_S75_1 H1 2020–21 persistence, FIG_S75_2 constant vs episodic introduction, FIG_S75_3 amplification and outcomes |
| Step 8b conditional climate renewal | `18_step8b_*` (uncommitted) | No local SSP/ISIMIP data → **SSP component STOPPED**. C1 vs C0 differ ~1%. Single-week pulse is a transient perturbation reconverging within 1–1.5 years | FIG_8B_1 climate and R0 comparison, FIG_8B_2 conditional trajectories |

### Stage 4. National pipeline (27 UFs)

- Wave census v2, early-wave Re, **annual FOI-shape model v2** (25/27 UFs pass HMC; RO, RR excluded; through 2025), 5-km grid susceptibility reconstruction (`allfoi_s1_spatial_scale_v2`).
- Climate × susceptibility phase planes for recurrent BA/RJ/MT waves: descriptive evidence, not a regression test.
- **Outbreak-occurrence pilot** (uncommitted): state-year Bernoulli model. No identifiable climate effect; recurrence dependence encoded only through observed-history variables; only CE has a latent-infection magnitude anchor.

| Figure | Content |
|---|---|
| `national_wave_analysis/brazil_chik_episode_figure1`–`6` | Episode Re vs S, Re vs climate, partial effects, observed vs predicted, mechanistic gate, FOI-scale comparison |
| `FIRST_EPISODE_RE_*`, `FIRST_EPISODE_LOO_UF_SENSITIVITY` | First-epidemic climate–transmission pilot (18 eligible UFs) |
| `CLIMATE_R0_TEMPERATURE/PRECIPITATION_RESPONSE`, `CLIMATE_TRANSMISSION_SURFACE` | First-epidemic GAM climate responses |
| `RECURRENT_*_PHASE_PLANE*`, `RECURRENT_CLIMATE_SUSCEPTIBILITY_TRAJECTORIES` | Recurrent R_climate × S/N phase planes |
| `annual_early_re_maps/..._2015_2025.png` | Annual early-Re maps |
| `national_grid_susceptibility/.../*_2025.png`, `_change_2015_2025.png`, `_selected_years.png` | 5-km susceptibility reconstruction |
| `outbreak_occurrence_pilot/FIGURE1`–`4`, `HOLDOUT_calibration_check`, `dynamic_audit/AUDIT_FIGURE1`–`4` | CE timeline, annual outbreak probability, stochastic calendar, model assessment, holdout calibration, marginal vs path-specific, counterfactual history, recurrence intervals, definition sensitivity |

### Stage 5. Integrated architecture decision (13 Oct 2026)

- **Architecture A** (FOI-driven state × age cohort immunity, direct vaccine effects) recommended, national scope with evidence tiers (Tier 1 CE; Tier 2 23 UFs; Tier 3 RO, RR, PE). Architecture B (renewal feedback) only as a gated CE-only exploratory arm.
- Two double-counting risks: (i) the FOI ensemble's upstream RF already used temperature suitability; (ii) the same Ceará serosurvey (Juazeiro/Quixadá, n = 404) feeds both the FOI ensemble and the renewal likelihood.
- Outputs: `INTEGRATED_MODEL_ARCHITECTURE_DECISION.md`, `ARCHITECTURE_DIAGRAM.md`, `REUSABLE_COMPONENTS.csv`, `STATE_DATA_READINESS.csv`, `HISTORICAL_FOI_IMMUNITY_COMPATIBILITY.csv`.

### Stage 6. Vaccination policy burden pipeline (`08_vaccination_policy_burden_pipeline`, 13–14 Oct 2026, uncommitted)

Ceará only; renewal posterior + age-cohort engine + CHIK_MORBID severity profiles.

| Sub-module | Conclusion | Figures |
|---|---|---|
| Step 1 demography/multimorbidity | Population with 2+ core conditions 10.1% → 14.1% (2026 → 2045). Standardised death severity +33%, hospitalisation +0.1% | FIGURE1 ageing and profile composition, FIGURE2 age-specific severity contribution |
| Integrated 2035 outbreak pilot | Conditional mid-2035 100-case pulse. Background λ = 1/week barely moves population immunity over 9 years | FIGURE1 weekly trajectories, FIGURE2 age-specific susceptibility, FIGURE3 severity by age profile, FIGURE4 scenario comparison |
| Dynamic annual cohort pilot | S0/S1/S2; under S2 (historical-analogue block bootstrap) S/N 0.63 → 0.31 by 2045. Sawtooth artefact discovered | FIGURE1 historical-to-future trajectory, FIGURE2 age-structured susceptibility, FIGURE3 infection burden, FIGURE4 severity burden, FIGURE5 age/morbidity contribution, FIGURE6 demography vs exposure |
| Exposure-risk sensitivity framework | Replaces S0/S1/S2 with a historically benchmarked continuous risk5 grid; sawtooth root cause identified (fixed block-to-calendar phase) | FIGURE1 historical risk benchmarks, FIGURE2 susceptibility response, FIGURE3 cumulative risk response, FIGURE4 age structure, FIGURE5 severity response, FIGURE6 timing sensitivity, FIGURE7 selected paths |
| Reference baseline stabilisation | Library of reference hazard paths (`HIST_DIFFUSE`, `BG_DIFFUSE`, `HIST_CONC2_*`). **"Ready for paired SSP perturbation"**, waiting | FIGURE1 anchors and reference paths, FIGURE2 exposure comparison, FIGURE3 dynamic susceptibility, FIGURE4 infection burden, FIGURE5 severity composition, FIGURE6 assumption importance |

### Stage 7. Climate–incidence association tests (three scales, all decision C)

| Module | Scale | Decision | Figures |
|---|---|---|---|
| `08_lac_surveillance_feasibility` | 50 LAC countries, 2014–2025 | Country-year panel exists (19 usable countries); weekly only 2018–2020; no admin-1. **Option C** (Brazil-anchored multi-country) | `country_year_availability_heatmap`, `lac_historical_annual_cases`, `brazil_paho_sinan_weekly_2018_2020` |
| `09_lac_historical_nb_gamm` | 18 countries × 2018–2025 | **C, not supported.** No temporal hold-out gain (temperature RR 95% CI 0.44–2.16) | `historical_observed_vs_fitted`, `climate_response_functions`, `out_of_sample_validation` |
| `10_brazil_municipality_climate_pilot` | 5 UFs, 1,479 municipalities × 132 months | **C, not supported.** In-sample lagged-precipitation association, but no hold-out gain; sign reversal across UFs | FIGURE1 observed vs fitted, FIGURE2 climate response functions, FIGURE3 out-of-sample performance, FIGURE4 climate variability and incidence |

### Stage 8. SSP track (`11` → `15`, 14–15 Oct 2026, uncommitted) — **the current blocker**

| Module | Content | Decision | Figures |
|---|---|---|---|
| `11_fixed_foi_demography_projection` | Published FOI surface (100 members) held fixed; only IBGE demography changes. Annual latent infections 1.63 M (2026) → 1.54 M (2045); 60+ share 10% → 17%; deaths 1,025 → 1,209. 65+ band error corrected 14 Oct | Reproduced (conditional projection) | FIGURE1 fixed FOI and future population, FIGURE2 future annual infections, FIGURE3 future age-specific susceptibility, FIGURE4 future multimorbidity burden, SUPPLEMENT S1–S5 |
| `12_ssp_thermal_suitability` | WorldClim CMIP6 (3 GCMs × SSP2-4.5/5-8.5 × P1/P2), binary suitable-months M. *Ae. aegypti* almost never decreases, gains in South/highlands; *Ae. albopictus* loses in the hot North | Complete; 11/11 checks | FIGURE1 suitability maps, FIGURE2 change in suitable months, FIGURE3 potential expansion beyond focal, FIGURE4 GCM uncertainty, FIGURE5 suitability × vulnerable population, SUPPLEMENT albopictus |
| `13_continuous_thermal_suitability` | Mordecai 2017 R0(T) reproduced (Topt 29.05/26.43 °C) → continuous K (0–1). Where M saturates at 12 (~70% of land), only K captures ΔK +0.15 to +0.25 | **A**: carry K as a temperature covariate candidate; no SSP-specific FOI | FIGURE1 relative R0 curves, FIGURE2 continuous K maps, FIGURE3 change in K, FIGURE4 binary M vs continuous K, FIGURE5 cold/hot-month mechanisms, FIGURE6 K change × vulnerable population 2045, SUPPLEMENT corrected classification |
| `14_foi_cohort_bridge_pilot` | CE/BA/SP. Annual S/R cohort engine (11/11 checks; matches catalytic solution to 3e-16). Initial immunity A (lifetime) vs B (introduction-aware): +32% infections in 2035, +23% in 2045. K→FOI interface λ = λ_base·exp(β(K − K_ref)) built but **disabled** | **C: β not identifiable; K_ref undefined** (ERA5 2014–2025 K already equals WorldClim P1 K; product offset ~1 °C is as large as the P1 signal; RF already used temperature → double counting) | FIGURE1 initial susceptibility 2025 (A vs B), FIGURE2 annual susceptible/infected, FIGURE3 cross-sectional vs dynamic, FIGURE4 spatial susceptibility/infections, FIGURE5 K-to-FOI bridge concept |
| `15_serology_immunity_reconstruction` | BA (8 calibration sites) and CE. One cumulative-exposure scale H_total per state. Pearson dispersion 118; holdouts off 2–10×; BA S/N at 2026 ranges 0.02–0.76 | **C: insufficient evidence**; conditional bracket only | FIGURE1 observed vs modelled seroprevalence, FIGURE2 reported timing and pressure weights, FIGURE3 age-specific susceptibility 2026, FIGURE4 initial-susceptibility sensitivity |

### Stage 9. Research restart assessment (`16_research_restart_assessment`, 18 Oct 2026, most recent)

- Verified: population layer (exact IBGE match), cohort engine, thermal curves, same-product ERA5 warming +0.19 to +0.34 °C.
- Failed validations: introduction-aware constant FOI (≤ 6.5% vs observed 4–57%), single-scale serology reconstruction, renewal absolute resurgence (369×), climate predictive gain (modules 09, 10).
- **Gates:** G1 engine integrity met. G4 (K_ref), G5 (β), G6 (initial-immunity holdout) not met. G2 (immunity priority) and G3 (climate relevance) to be determined.
- **The one proposed next pilot: the "uncertainty-dominance pilot"** (CE/BA/SP, no new models): variance shares for FOI ensemble vs initial immunity (A/R/B) vs non-climate FOI multiplier (×0.5/1/2), plus **break-even β\*** = log(m\*)/ΔK_P2. Reports how large β would need to be for climate to matter; not an estimate of β.

## 3. Where exactly the work is blocked

**Renewal track (Ceará)**
1. Not a mathematical or coding problem but an **identification and data problem**: generating a 2022-type resurgence after a 2020–21 near-fadeout requires the magnitude of external introduction pressure, which SINAN alone does not identify (Steps 6, 7, 7.5).
2. β_T is confounded with the 2022 year effect (cor 0.8) and climate forcing does not improve fit, so "climate → future R0" is not a validated causal function either.
3. No local SSP climate inputs (Step 8b); ISIMIP3b availability was confirmed.

**SSP track (national)**
1. **β not identified (G5):** within-period K change 2014–2025 (+0.02 to 0.03) is swamped by introduction, depletion and reporting; cross-sectional regression gives a spatial gradient, not a temporal elasticity; the RF FOI already includes temperature.
2. **K_ref undefined (G4):** no same-product-family thermal reference for the period the published FOI represents (~2010–2019).
3. **Initial immunity unstable (G6):** lifetime catalytic (A) is the default convention but partly circular; serology anchoring spans S/N 0.02–0.76 depending on site subset.
4. FOI-surface provenance (citation, methodology) absent from the repository → STOP memo in `docs/dynamic_foi_provenance.md`; currently used only as a prior/scale index.

## 4. What is publishable now

- Ceará historical reconstruction + no climate fit gain + robust 2022 confound: a methodological result that weekly-renewal climate signals are second-order and confounded with year effects (FIG13–16, performance tables).
- Historical age-12 counterfactual and rebound mechanism (fig1–3, nnv1–5, R1–R5).
- A consistent null across three scales (LAC country-year, Brazil municipality-month, Ceará weekly): **no out-of-sample predictive gain from climate** (not evidence of no effect).
- National SSP thermal suitability M/K maps with 65+/multimorbidity overlap (modules 12, 13, FIGURE1–6).
- Fixed FOI + ageing alone: deaths +18% by 2045 and 60+ share of infections 10% → 17% (module 11).
- Quantified structural uncertainty: the initial-immunity assumption changes 2035 infections by 32% (module 14, FIGURE1–3).

## 5. Proposed next directions

**Recommended order: Track 1 → Track 2 (primary deliverable) → Track 3 (gated) → Track 4 (parallel evidence acquisition).**

**Track 1 (2–4 weeks, no new models): run the uncertainty-dominance pilot → decide G2/G3.**
- Exactly as designed in restart assessment §8: reuse `run_cohort()`, CE/BA/SP, initial immunity {A, R (renewal benchmark), B (labelled rejected)} × FOI multiplier {0.5, 1, 2} × 100 members.
- Outputs: variance-share table for 2035/2045 infections and 65+ infections; cell- and state-level break-even β\*.
- Why first: if β\* is large (> 3), "SSP-specific FOI adds little relative to immunity and demography" **becomes the paper's conclusion**, and climate is reported descriptively as K. If β\* is small, acquiring β-identifying data (Track 4) becomes the top priority.

**Track 2 (primary deliverable, 2–3 months): Architecture A national conditional burden and vaccination projection, 2026–2045.**
- Phase 1: pre-register a forward rule for the static FOI ensemble (e.g. hold the 2015–2025 national-pipeline annual mean, ± multipliers). SSP enters only as a **K-multiplier sensitivity** bounded by Track 1's β\* range, labelled non-climate-attributed.
- Phase 2: extend the cohort engine to 25 UFs × single age (module 14's grid-level engine is closer to this than `02_age_cohort_simulator.R`). Keep initial immunity A and R as **separate scenario layers**, never pooled into FOI intervals.
- Phase 3: pre-specify a literature-based waning form (never fit to local data).
- Phase 4: direct-effect vaccination strategies (target age, coverage, catch-up) + CHIK_MORBID severity → hospitalisations, deaths, NNV. Module 08's reference paths are already "ready for paired perturbation" in CE, so run CE first as the Tier 1 validation case.
- This track needs neither β, K_ref nor a resolved introduction process, and it is the deliverable the architecture decision already recommended.

**Track 3 (gated, CE only): indirect effects via the renewal conditional event engine.**
- Combine the Step 8b engine with the national occurrence model's Bernoulli decision: occurrence from the national model, magnitude and depletion from the CE renewal (structure proposed in OCCURRENCE_DYNAMIC_AUDIT §8).
- Report only "direct vs total (with indirect) vaccine effect ratio given a conditional outbreak"; never absolute incidence.
- Role: supply an indirect-effect multiplier range to Track 2.

**Track 4 (parallel, external collaboration): data that identify β and initial immunity.**
- Repeat age-stratified serosurveys at the same sites (e.g. Juazeiro 2018 → 2026, Salvador 2016 → 2026) or state-representative probability-sampled serology → G6, and within-place temporal contrasts across a K gradient → G5.
- Reintroduction-event data (phylogenomics, imported-case notifications) → Step 7.5 introduction gate.
- Original methodology document for the published FOI surface → lift the provenance STOP.
- Download ISIMIP3b (e.g. GFDL-ESM4) daily tas/pr → define K_ref in the same product family (G4). This is data work and can start immediately.

**Do not (reaffirming existing decisions):** set β = 1, fit β to the FOI surface, run cross-sectional K–FOI regressions, generalise single-scale serology to UF level, or report absolute 2026–2045 renewal forecasts.

## 6. Proposed titles

The study's actual content has shifted from a causal "climate → incidence" model to a conditional burden-projection framework under demographic, immunological and climatic uncertainty; the title should follow.

1. **Primary (recommended):** *Projecting chikungunya burden in Brazil to 2045 under demographic, immunity and climate uncertainty: a gated, scenario-conditional modelling framework*
2. **Immunity/ageing emphasis:** *Who will be susceptible? Age-cohort immunity, population ageing and multimorbidity in future chikungunya burden in Brazil, 2026–2045*
3. **Climate-null emphasis (if Track 1 finds G3 not met):** *Beyond thermal suitability: why immunity and demography, not climate, dominate uncertainty in projected chikungunya burden in Brazil*
4. **Vaccination-policy emphasis (after Track 2):** *Scenario-conditional impact of routine chikungunya vaccination in Brazil, 2026–2045: integrating reconstructed immunity, population ageing and climate-driven transmission suitability*

---

## Appendix. Repository state notes (2026-11-05)

- Last commit: `e9a34af` (Ceará year-effect validation, climate robustness, retrospective resurgence chain, Tasks B–7.5).
- **Uncommitted user work:** `02_Script/02_ceara_pipeline/18_step8b_*`, `07_national_pipeline/12_outbreak_occurrence_pilot`, modules `08`–`15`, `03_Output/00_integrated_architecture_assessment`, `03_Output/16_research_restart_assessment`, raw inputs `01_Data/lac_*`, `worldclim_v21_raw/`, IBGE 2024-revision age files.
- `CHIK_CLIM/README.md` and `02_Script/README.md` still describe only pipelines 00–07 and 90; modules 08–16 are not yet documented there.
- Module 11–13 outputs were corrected on 2026-10-14 (65+ age band); superseded versions are archived under `_superseded_60plus/`.
