---
title: Wearable EEG operating points
---



_Generated 2026-11-20 05:25 UTC from aggregated results. Interactive version: open `index.html` in this folder (or the GitHub Pages site). This README is the static, GitHub-rendered summary._

## What this shows

For each wearable EEG task we ask how far the **number of channels × sampling rate × ADC bit width** can be reduced before the task metric drops, and whether a **deployed model can switch operating points without retraining**. Grid per task: channels {2, 4, 6, 8, 16} × fs {16, 32, 64, 128, 256} Hz × bits {4, 8, 12, 16, 24} = 125 points (some tasks have fewer channels or a lower native rate). Data rate = channels · fs · bits (bit/s).

| Protocol | What happens at a new operating point | Labels / retraining at switch time |
|---|---|---|
| **A** | a new EEGNet is trained for every (channels, fs, bits) point — upper bound | labels + retraining |
| **B** | one model per (channels, fs) trained at 24 bit, tested at every bit width | retraining per (channels, fs) |
| **B_ls** | one full model; closed-form least-squares channel folding + temporal-kernel resampling + test-time quantization | none |
| **B_ls_bn** | B_ls + re-estimating BatchNorm statistics on unlabeled data | unlabeled data only |
| **E** | one elastic model trained once over random channels / rates / bit widths | none |
| **S** | one slimmable model (width 0.25–1) with the same input randomisation | none |
| **X** | intra-model early exit (confidence threshold τ) | none |
| **ESC** | uncertainty-triggered escalation of the sensing configuration along a ladder (2 ch/32 Hz/8 b → … → 16 ch/256 Hz/24 b) | none |

## Headline (tasks near chance excluded)

- Task instances with results: **25**; usable (full-config A clearly above chance): **21**.
- Median data-rate reduction while staying within 0.02 of the full-config A metric, retraining per point (A): **6×**.
- Same for the best label-free protocol (no retraining; 3 task(s) with label-free runs): **24×**.

## Tasks

| # | Type | Dataset | Task | Electrodes | Subjects | Licence | Status |
|---|---|---|---|---|---|---|---|
| 1 | Auditory attention (AAD) | Ear-SAAD | Selective AAD, stimulus reconstruction (5 / 30 / 60 s windows) | real wearable (in-ear 12 + around-ear 19 + scalp) | 15 | CC-BY-4.0 | results: A, B |
| 2 | Auditory attention (AAD) | ICL ear-EEG AAD | AAD, stimulus reconstruction (5 / 30 / 60 s windows) | real wearable (2 in-ear dry electrodes) | 19 | CC-BY-4.0 | results: A, B |
| 3 | Auditory attention (AAD) | DTU hearing-impaired AAD | Attend left vs right, 5 s | real wearable (in-ear 6 + scalp 64) | 19 | CC-BY-4.0 | results: A, B |
| 4 | Auditory attention (AAD) | Oldenburg cEEGrid AAD (OpenNeuro ds004015) | Attended talker | real wearable (around-ear cEEGrid) | 36 | CC0 | not cached: attended side is constant per subject (envelope-reconstruction only) |
| 5 | Auditory attention (AAD) | 4-speaker ear-EEG AAD | Attention direction | real wearable (around-ear) | – | CC-BY-4.0 | downloaded, not yet cached |
| 6 | Auditory attention (AAD) | KUL AAD | Attend left vs right, 5 s | simulated (scalp 64 -> ear/forehead positions) | 16 | CC-BY-NC-SA-4.0 (non-commercial) | results: A, B |
| 7 | Auditory attention (AAD) | AV-GC AAD | Attend left vs right (gaze-controlled), 5 s | simulated (scalp 64 -> ear/forehead positions) | 13 | CC-BY-SA-4.0 | results: A, B |
| 8 | Target detection | Aarhus event-based ear-EEG AAD | Attended-target word detection | real wearable (in-ear 12 + scalp 32) | 24 | CC-BY-4.0 | results: A, B |
| 9 | Target detection | DAAMEE | Pitch oddball in attended vs unattended instrument | real wearable (around-ear cEEGrid 20 + scalp 32) | 27 | CC-BY-4.0 | results: A, B |
| 10 | P300 | Lee2021 mobile BCI | P300 target vs non-target (standing) | real wearable (around-ear cEEGrid 14 + scalp 32) | 18 | CC-BY-4.0 | results: A, B |
| 11 | P300 | Lee2021 mobile BCI | P300 while walking | real wearable (around-ear cEEGrid 14 + scalp 32) | 18 | CC-BY-4.0 | cached, not yet in the factorial grid |
| 12 | SSVEP | Lee2021 mobile BCI | SSVEP 3-class (standing) | real wearable (around-ear cEEGrid 14 + scalp 32) | 17 | CC-BY-4.0 | results: A, B |
| 13 | SSVEP | Lee2021 mobile BCI | SSVEP 3-class, standing <-> walking (cross-state) | real wearable (around-ear cEEGrid 14 + scalp 32) | 17 | CC-BY-4.0 | results: A, B |
| 14 | Drowsiness / fatigue | Cao2019 driving (lane-keeping) | Alert vs drowsy (leave-subjects-out) | simulated (32-ch cap -> ear/forehead positions) | 22 | CC-BY-4.0 | results: A, B |
| 15 | Drowsiness / fatigue | MPD-DF | Driving fatigue, 3 levels (leave-subjects-out) | simulated (32-ch cap -> forehead/temporal positions) | 50 | CC-BY-4.0 | cached, queued |
| 16 | Drowsiness / fatigue | DD-Database | Driving drowsiness | real wearable (4 EEG + 2 EOG) | – | CC0 | not usable: download requires login |
| 17 | Vigilance | COG-BCI | PVT: fast vs slow response | simulated (64-ch cap -> 32 forehead/ear/scalp) | 29 | CC-BY-4.0 | results: A, B |
| 18 | Vigilance | 1-h PVT (OpenNeuro ds007509) | PVT reaction speed | simulated (scalp cap) | 69 | CC0 | not usable: no reaction times in events |
| 19 | Vigilance | Sleep deprivation rest (OpenNeuro ds004902) | Rested vs sleep-deprived (leave-subjects-out) | simulated (61-ch cap -> ear/forehead positions) | 71 | CC0 | cached, queued |
| 20 | Mental workload | COG-BCI | MATB workload, 3 levels | simulated (64-ch cap -> 32 forehead/ear/scalp) | 29 | CC-BY-4.0 | results: A, B |
| 21 | Mental workload | COG-BCI | N-back 0 / 1 / 2 | simulated (64-ch cap -> 32 forehead/ear/scalp) | 29 | CC-BY-4.0 | results: A, B |
| 22 | Mental workload | Muse wearable n-back (PhysioNet) | 1-back vs 3-back | real wearable (Muse headband TP9/AF7/AF8/TP10) | 20 | ODC-By-1.0 | results: A, B, B_ls, B_ls_bn, E, S, X, ESC_E, ESC_B_ls |
| 23 | Mental workload | HPI Muse cognitive load | High vs low load | real wearable (Muse S headband) | – | CC-BY-4.0 | not cached: task timing needs manual alignment |
| 24 | Mental workload | ImBrAid | Arithmetic load while driving, easy vs hard | simulated (24-ch mobile cap -> forehead/mastoid) | 43 | CC-BY-4.0 | cached, queued |
| 25 | Mental workload | ImBrAid | Same task, in-ear stream subset | real wearable (1 in-ear channel + 24-ch cap) | 27 | CC-BY-4.0 | cached, queued |
| 26 | Mental workload | Ear-EmoLoad | Workload / emotion | real wearable (in-ear) | – | CC-BY-4.0 | not usable: no stimulus timing in raw files |
| 27 | Sleep staging | EESM23 (OpenNeuro ds005178) | Sleep staging, 5 classes | real wearable (in-ear 4 (+ PSG)) | 10 | CC0 | cached, queued |
| 28 | Sleep staging | Forehead-patch sleep (OpenNeuro ds006695) | Sleep staging, 5 classes | real wearable (forehead patch 3) | 19 | CC0 | cached, queued |
| 29 | Seizure detection | SeizeIT2 (OpenNeuro ds005873) | Seizure vs non-seizure 10 s windows | real wearable (behind-ear 2) | 109 | CC0 | cached, queued |
| 30 | Emotion | DAAMEE | Music valence high vs low | real wearable (around-ear cEEGrid 20 + scalp 32) | 29 | CC-BY-4.0 | cached, queued |
| 31 | Music attention | DAAMEE | Which instrument is attended (3 classes) | real wearable (around-ear cEEGrid 20 + scalp 32) | 28 | CC-BY-4.0 | cached, queued |
| 32 | Artifact detection | Consumer 5-device artifact set (Muse 2) | Artifact type, 5 classes (leave-subjects-out) | real wearable (Muse 2 AF7/AF8/TP9/TP10) | 30 | CC-BY-4.0 | results: A, B, B_ls, B_ls_bn, E, S, X, ESC_E, ESC_B_ls |
| 33 | Artifact detection | Consumer 5-device artifact set (DSI-24) | Artifact type, 5 classes (leave-subjects-out) | simulated (DSI-24 dry cap -> forehead/ear positions) | 30 | CC-BY-4.0 | results: A, B |
| 34 | Motor execution | Wu2020 in-ear motor | Left vs right hand grasp | real wearable (in-ear 8 + scalp) | 6 | CC-BY-4.0 | cached, queued |
| 35 | Eyes open / closed | COG-BCI | Eyes open vs closed | simulated (64-ch cap -> 32 forehead/ear/scalp) | 29 | CC-BY-4.0 | results: A, B |
| 36 | Eyes open / closed | DTU hearing-impaired AAD | Eyes open vs closed (leave-subjects-out) | real wearable (in-ear 6 + scalp 64) | 19 | CC-BY-4.0 | results: A, B, B_ls, B_ls_bn, E, S, X, ESC_E, ESC_B_ls |
| 37 | Auditory steady-state response | DTU hearing-impaired AAD | 40 Hz ASSR: tone on vs off (leave-subjects-out) | real wearable (in-ear 6 + scalp 64) | 19 | CC-BY-4.0 | results: A, B |
| 38 | Hearing-loss screening | DTU hearing-impaired AAD | Hearing-impaired vs normal hearing (subject-level label) | simulated (scalp 64 -> ear/forehead positions) | 44 | CC-BY-4.0 | results: A, B |
| 39 | Memory / music / arithmetic | OpenNeuro ds004148 | Memory / music / arithmetic vs rest | simulated (scalp cap -> TP9/TP10) | – | CC0 | not downloaded (lower priority) |

## Data-rate savings

![savings](figs/savings_eps0.02.png)

### Minimum data rate within 0.01 of the full-config A metric

Cells: minimum data rate (reduction × vs full config). Near-chance tasks are marked ⚠ and excluded from the headline.

| Task | metric | full-config A | A | B | B_ls | B_ls_bn | E | S | X |
|---|---|---|---|---|---|---|---|---|---|
| Ear-SAAD — AAD-SR 30s | acc 0.717 (chance 0.50) | scalp 8 ch · 16 Hz · 24 b = 3.1 kbit/s | 1.2 kbit/s (2.67×) | 1.2 kbit/s (2.67×) |  |  |  |  |  |
| Ear-SAAD — AAD-SR 5s | acc 0.592 (chance 0.50) | scalp 8 ch · 16 Hz · 24 b = 3.1 kbit/s | 1.2 kbit/s (2.67×) | 1.2 kbit/s (2.67×) |  |  |  |  |  |
| Ear-SAAD — AAD-SR 60s | acc 0.775 (chance 0.50) | scalp 8 ch · 16 Hz · 24 b = 3.1 kbit/s | 1.5 kbit/s (2×) | 1.5 kbit/s (2×) |  |  |  |  |  |
| ICL-ear-AAD — AAD-SR 30s | acc 0.589 (chance 0.50) | in-ear 2 ch · 128 Hz · 24 b = 6.1 kbit/s | 320 bit/s (19.2×) | 320 bit/s (19.2×) |  |  |  |  |  |
| ICL-ear-AAD — AAD-SR 5s | acc 0.547 (chance 0.50) | in-ear 2 ch · 128 Hz · 24 b = 6.1 kbit/s | 160 bit/s (38.4×) | 160 bit/s (38.4×) |  |  |  |  |  |
| ICL-ear-AAD — AAD-SR 60s | acc 0.628 (chance 0.50) | in-ear 2 ch · 128 Hz · 24 b = 6.1 kbit/s | 320 bit/s (19.2×) | 320 bit/s (19.2×) |  |  |  |  |  |
| DTU-HI-AAD — AAD direction L/R | bacc 0.668 (chance 0.50) | forehead 2 ch · 16 Hz · 24 b = 768 bit/s | 256 bit/s (3×) | 256 bit/s (3×) |  |  |  |  |  |
| KUL-AAD — AAD direction L/R (sim. ear, NC licence) | bacc 0.615 (chance 0.50) | scalp 16 ch · 128 Hz · 24 b = 49.2 kbit/s | 16.4 kbit/s (3×) | 16.4 kbit/s (3×) |  |  |  |  |  |
| Aarhus-event-AAD — attended-target word | auroc 0.694 (chance 0.50) | scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 32.8 kbit/s (3×) | 32.8 kbit/s (3×) |  |  |  |  |  |
| DAAMEE — music oddball P300 | auroc 0.557 (chance 0.50) | scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 16.4 kbit/s (6×) | 16.4 kbit/s (6×) |  |  |  |  |  |
| Lee2021-mobile — P300 target (standing) | auroc 0.778 (chance 0.50) | scalp 32 ch · 32 Hz · 24 b = 24.6 kbit/s | 512 bit/s (48×) | 256 bit/s (96×) |  |  |  |  |  |
| Cao2019-driving — alert vs drowsy (LSO) | bacc 0.833 (chance 0.50) | all 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 49.2 kbit/s (2×) | 49.2 kbit/s (2×) |  |  |  |  |  |
| COG-BCI — PVT fast vs slow | bacc 0.610 (chance 0.50) | ear 2 ch · 16 Hz · 24 b = 768 bit/s | 128 bit/s (6×) | 128 bit/s (6×) |  |  |  |  |  |
| COG-BCI — MATB workload 3-level | bacc 0.482 (chance 0.33) | scalp 2 ch · 16 Hz · 24 b = 768 bit/s | 128 bit/s (6×) | 128 bit/s (6×) |  |  |  |  |  |
| Muse-PhysioNet — 1-back vs 3-back | bacc 0.616 (chance 0.50) | all 4 ch · 256 Hz · 24 b = 24.6 kbit/s | 512 bit/s (48×) | 512 bit/s (48×) | 4.1 kbit/s (6×) | 2.0 kbit/s (12×) | not reached | not reached | not reached |
| Consumer-Muse2 — artifact type 5-class (LSO) | bacc 0.749 (chance 0.20) | all 4 ch · 256 Hz · 24 b = 24.6 kbit/s | 4.1 kbit/s (6×) | 4.1 kbit/s (6×) | 4.1 kbit/s (6×) | 4.1 kbit/s (6×) | not reached | not reached | not reached |
| Consumer-DSI24 — artifact type 5-class (LSO) | bacc 0.833 (chance 0.20) | all 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 32.8 kbit/s (3×) | 32.8 kbit/s (3×) |  |  |  |  |  |
| COG-BCI — eyes open vs closed | bacc 0.777 (chance 0.50) | all 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 32.8 kbit/s (3×) | 32.8 kbit/s (3×) |  |  |  |  |  |
| DTU-HI-AAD — eyes open vs closed (LSO) | bacc 0.733 (chance 0.50) | ear+scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 32.8 kbit/s (3×) | 32.8 kbit/s (3×) | 3.1 kbit/s (32×) | 2.0 kbit/s (48×) | 2.0 kbit/s (48×) | 2.0 kbit/s (48×) | 2.0 kbit/s (48×) |
| DTU-HI-AAD — 40 Hz ASSR detection (LSO) | bacc 0.652 (chance 0.50) | ear+scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 32.8 kbit/s (3×) | 32.8 kbit/s (3×) |  |  |  |  |  |
| DTU-HI-AAD — hearing-impaired screening (LSO) | bacc 0.612 (chance 0.50) | scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 32.8 kbit/s (3×) | 32.8 kbit/s (3×) |  |  |  |  |  |
| ⚠ AV-GC-AAD — AAD direction L/R (sim. ear) | bacc 0.458 (chance 0.50) | scalp 16 ch · 128 Hz · 24 b = 49.2 kbit/s | 8.2 kbit/s (6×) | 8.2 kbit/s (6×) |  |  |  |  |  |
| ⚠ Lee2021-mobile — SSVEP 3-class (standing) | acc 0.344 (chance 0.33) | scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 128 bit/s (768×) | 128 bit/s (768×) |  |  |  |  |  |
| ⚠ Lee2021-mobile — SSVEP stand<->walk | acc 0.349 (chance 0.33) | ear 14 ch · 256 Hz · 24 b = 86.0 kbit/s | 128 bit/s (672×) | 128 bit/s (672×) |  |  |  |  |  |
| ⚠ COG-BCI — N-back 0/1/2 | bacc 0.358 (chance 0.33) | ear 2 ch · 16 Hz · 24 b = 768 bit/s | 128 bit/s (6×) | 128 bit/s (6×) |  |  |  |  |  |

### Minimum data rate within 0.02 of the full-config A metric

Cells: minimum data rate (reduction × vs full config). Near-chance tasks are marked ⚠ and excluded from the headline.

| Task | metric | full-config A | A | B | B_ls | B_ls_bn | E | S | X |
|---|---|---|---|---|---|---|---|---|---|
| Ear-SAAD — AAD-SR 30s | acc 0.717 (chance 0.50) | scalp 8 ch · 16 Hz · 24 b = 3.1 kbit/s | 1.2 kbit/s (2.67×) | 1.2 kbit/s (2.67×) |  |  |  |  |  |
| Ear-SAAD — AAD-SR 5s | acc 0.592 (chance 0.50) | scalp 8 ch · 16 Hz · 24 b = 3.1 kbit/s | 1.2 kbit/s (2.67×) | 768 bit/s (4×) |  |  |  |  |  |
| Ear-SAAD — AAD-SR 60s | acc 0.775 (chance 0.50) | scalp 8 ch · 16 Hz · 24 b = 3.1 kbit/s | 1.2 kbit/s (2.67×) | 1.2 kbit/s (2.67×) |  |  |  |  |  |
| ICL-ear-AAD — AAD-SR 30s | acc 0.589 (chance 0.50) | in-ear 2 ch · 128 Hz · 24 b = 6.1 kbit/s | 160 bit/s (38.4×) | 320 bit/s (19.2×) |  |  |  |  |  |
| ICL-ear-AAD — AAD-SR 5s | acc 0.547 (chance 0.50) | in-ear 2 ch · 128 Hz · 24 b = 6.1 kbit/s | 160 bit/s (38.4×) | 160 bit/s (38.4×) |  |  |  |  |  |
| ICL-ear-AAD — AAD-SR 60s | acc 0.628 (chance 0.50) | in-ear 2 ch · 128 Hz · 24 b = 6.1 kbit/s | 320 bit/s (19.2×) | 320 bit/s (19.2×) |  |  |  |  |  |
| DTU-HI-AAD — AAD direction L/R | bacc 0.668 (chance 0.50) | forehead 2 ch · 16 Hz · 24 b = 768 bit/s | 256 bit/s (3×) | 256 bit/s (3×) |  |  |  |  |  |
| KUL-AAD — AAD direction L/R (sim. ear, NC licence) | bacc 0.615 (chance 0.50) | scalp 16 ch · 128 Hz · 24 b = 49.2 kbit/s | 16.4 kbit/s (3×) | 16.4 kbit/s (3×) |  |  |  |  |  |
| Aarhus-event-AAD — attended-target word | auroc 0.694 (chance 0.50) | scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 256 bit/s (384×) | 256 bit/s (384×) |  |  |  |  |  |
| DAAMEE — music oddball P300 | auroc 0.557 (chance 0.50) | scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 16.4 kbit/s (6×) | 16.4 kbit/s (6×) |  |  |  |  |  |
| Lee2021-mobile — P300 target (standing) | auroc 0.778 (chance 0.50) | scalp 32 ch · 32 Hz · 24 b = 24.6 kbit/s | 256 bit/s (96×) | 256 bit/s (96×) |  |  |  |  |  |
| Cao2019-driving — alert vs drowsy (LSO) | bacc 0.833 (chance 0.50) | all 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 1.5 kbit/s (64×) | 1.5 kbit/s (64×) |  |  |  |  |  |
| COG-BCI — PVT fast vs slow | bacc 0.610 (chance 0.50) | ear 2 ch · 16 Hz · 24 b = 768 bit/s | 128 bit/s (6×) | 128 bit/s (6×) |  |  |  |  |  |
| COG-BCI — MATB workload 3-level | bacc 0.482 (chance 0.33) | scalp 2 ch · 16 Hz · 24 b = 768 bit/s | 128 bit/s (6×) | 128 bit/s (6×) |  |  |  |  |  |
| Muse-PhysioNet — 1-back vs 3-back | bacc 0.616 (chance 0.50) | all 4 ch · 256 Hz · 24 b = 24.6 kbit/s | 384 bit/s (64×) | 256 bit/s (96×) | 2.0 kbit/s (12×) | 1.0 kbit/s (24×) | 3.1 kbit/s (8×) | not reached | 2.0 kbit/s (12×) |
| Consumer-Muse2 — artifact type 5-class (LSO) | bacc 0.749 (chance 0.20) | all 4 ch · 256 Hz · 24 b = 24.6 kbit/s | 3.1 kbit/s (8×) | 2.0 kbit/s (12×) | 4.1 kbit/s (6×) | 4.1 kbit/s (6×) | not reached | not reached | not reached |
| Consumer-DSI24 — artifact type 5-class (LSO) | bacc 0.833 (chance 0.20) | all 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 32.8 kbit/s (3×) | 32.8 kbit/s (3×) |  |  |  |  |  |
| COG-BCI — eyes open vs closed | bacc 0.777 (chance 0.50) | all 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 32.8 kbit/s (3×) | 32.8 kbit/s (3×) |  |  |  |  |  |
| DTU-HI-AAD — eyes open vs closed (LSO) | bacc 0.733 (chance 0.50) | ear+scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 16.4 kbit/s (6×) | 32.8 kbit/s (3×) | 3.1 kbit/s (32×) | 1.5 kbit/s (64×) | 2.0 kbit/s (48×) | 2.0 kbit/s (48×) | 2.0 kbit/s (48×) |
| DTU-HI-AAD — 40 Hz ASSR detection (LSO) | bacc 0.652 (chance 0.50) | ear+scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 32.8 kbit/s (3×) | 32.8 kbit/s (3×) |  |  |  |  |  |
| DTU-HI-AAD — hearing-impaired screening (LSO) | bacc 0.612 (chance 0.50) | scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 32.8 kbit/s (3×) | 16.4 kbit/s (6×) |  |  |  |  |  |
| ⚠ AV-GC-AAD — AAD direction L/R (sim. ear) | bacc 0.458 (chance 0.50) | scalp 16 ch · 128 Hz · 24 b = 49.2 kbit/s | 8.2 kbit/s (6×) | 8.2 kbit/s (6×) |  |  |  |  |  |
| ⚠ Lee2021-mobile — SSVEP 3-class (standing) | acc 0.344 (chance 0.33) | scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 128 bit/s (768×) | 128 bit/s (768×) |  |  |  |  |  |
| ⚠ Lee2021-mobile — SSVEP stand<->walk | acc 0.349 (chance 0.33) | ear 14 ch · 256 Hz · 24 b = 86.0 kbit/s | 128 bit/s (672×) | 128 bit/s (672×) |  |  |  |  |  |
| ⚠ COG-BCI — N-back 0/1/2 | bacc 0.358 (chance 0.33) | ear 2 ch · 16 Hz · 24 b = 768 bit/s | 128 bit/s (6×) | 128 bit/s (6×) |  |  |  |  |  |

### Minimum data rate within 0.05 of the full-config A metric

Cells: minimum data rate (reduction × vs full config). Near-chance tasks are marked ⚠ and excluded from the headline.

| Task | metric | full-config A | A | B | B_ls | B_ls_bn | E | S | X |
|---|---|---|---|---|---|---|---|---|---|
| Ear-SAAD — AAD-SR 30s | acc 0.717 (chance 0.50) | scalp 8 ch · 16 Hz · 24 b = 3.1 kbit/s | 1.2 kbit/s (2.67×) | 768 bit/s (4×) |  |  |  |  |  |
| Ear-SAAD — AAD-SR 5s | acc 0.592 (chance 0.50) | scalp 8 ch · 16 Hz · 24 b = 3.1 kbit/s | 512 bit/s (6×) | 512 bit/s (6×) |  |  |  |  |  |
| Ear-SAAD — AAD-SR 60s | acc 0.775 (chance 0.50) | scalp 8 ch · 16 Hz · 24 b = 3.1 kbit/s | 1.2 kbit/s (2.67×) | 1.2 kbit/s (2.67×) |  |  |  |  |  |
| ICL-ear-AAD — AAD-SR 30s | acc 0.589 (chance 0.50) | in-ear 2 ch · 128 Hz · 24 b = 6.1 kbit/s | 128 bit/s (48×) | 80 bit/s (76.8×) |  |  |  |  |  |
| ICL-ear-AAD — AAD-SR 5s | acc 0.547 (chance 0.50) | in-ear 2 ch · 128 Hz · 24 b = 6.1 kbit/s | 80 bit/s (76.8×) | 80 bit/s (76.8×) |  |  |  |  |  |
| ICL-ear-AAD — AAD-SR 60s | acc 0.628 (chance 0.50) | in-ear 2 ch · 128 Hz · 24 b = 6.1 kbit/s | 160 bit/s (38.4×) | 160 bit/s (38.4×) |  |  |  |  |  |
| DTU-HI-AAD — AAD direction L/R | bacc 0.668 (chance 0.50) | forehead 2 ch · 16 Hz · 24 b = 768 bit/s | 256 bit/s (3×) | 256 bit/s (3×) |  |  |  |  |  |
| KUL-AAD — AAD direction L/R (sim. ear, NC licence) | bacc 0.615 (chance 0.50) | scalp 16 ch · 128 Hz · 24 b = 49.2 kbit/s | 8.2 kbit/s (6×) | 16.4 kbit/s (3×) |  |  |  |  |  |
| Aarhus-event-AAD — attended-target word | auroc 0.694 (chance 0.50) | scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 256 bit/s (384×) | 256 bit/s (384×) |  |  |  |  |  |
| DAAMEE — music oddball P300 | auroc 0.557 (chance 0.50) | scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 16.4 kbit/s (6×) | 16.4 kbit/s (6×) |  |  |  |  |  |
| Lee2021-mobile — P300 target (standing) | auroc 0.778 (chance 0.50) | scalp 32 ch · 32 Hz · 24 b = 24.6 kbit/s | 128 bit/s (192×) | 128 bit/s (192×) |  |  |  |  |  |
| Cao2019-driving — alert vs drowsy (LSO) | bacc 0.833 (chance 0.50) | all 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 1.0 kbit/s (96×) | 1.5 kbit/s (64×) |  |  |  |  |  |
| COG-BCI — PVT fast vs slow | bacc 0.610 (chance 0.50) | ear 2 ch · 16 Hz · 24 b = 768 bit/s | 128 bit/s (6×) | 128 bit/s (6×) |  |  |  |  |  |
| COG-BCI — MATB workload 3-level | bacc 0.482 (chance 0.33) | scalp 2 ch · 16 Hz · 24 b = 768 bit/s | 128 bit/s (6×) | 128 bit/s (6×) |  |  |  |  |  |
| Muse-PhysioNet — 1-back vs 3-back | bacc 0.616 (chance 0.50) | all 4 ch · 256 Hz · 24 b = 24.6 kbit/s | 64 bit/s (384×) | 64 bit/s (384×) | 1.0 kbit/s (24×) | 512 bit/s (48×) | 128 bit/s (192×) | 256 bit/s (96×) | 128 bit/s (192×) |
| Consumer-Muse2 — artifact type 5-class (LSO) | bacc 0.749 (chance 0.20) | all 4 ch · 256 Hz · 24 b = 24.6 kbit/s | 2.0 kbit/s (12×) | 1.0 kbit/s (24×) | 4.1 kbit/s (6×) | 2.0 kbit/s (12×) | not reached | not reached | 4.1 kbit/s (6×) |
| Consumer-DSI24 — artifact type 5-class (LSO) | bacc 0.833 (chance 0.20) | all 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 32.8 kbit/s (3×) | 32.8 kbit/s (3×) |  |  |  |  |  |
| COG-BCI — eyes open vs closed | bacc 0.777 (chance 0.50) | all 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 32.8 kbit/s (3×) | 32.8 kbit/s (3×) |  |  |  |  |  |
| DTU-HI-AAD — eyes open vs closed (LSO) | bacc 0.733 (chance 0.50) | ear+scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 16.4 kbit/s (6×) | 32.8 kbit/s (3×) | 2.0 kbit/s (48×) | 768 bit/s (128×) | 512 bit/s (192×) | 512 bit/s (192×) | 768 bit/s (128×) |
| DTU-HI-AAD — 40 Hz ASSR detection (LSO) | bacc 0.652 (chance 0.50) | ear+scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 16.4 kbit/s (6×) | 16.4 kbit/s (6×) |  |  |  |  |  |
| DTU-HI-AAD — hearing-impaired screening (LSO) | bacc 0.612 (chance 0.50) | scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 128 bit/s (768×) | 128 bit/s (768×) |  |  |  |  |  |
| ⚠ AV-GC-AAD — AAD direction L/R (sim. ear) | bacc 0.458 (chance 0.50) | scalp 16 ch · 128 Hz · 24 b = 49.2 kbit/s | 8.2 kbit/s (6×) | 8.2 kbit/s (6×) |  |  |  |  |  |
| ⚠ Lee2021-mobile — SSVEP 3-class (standing) | acc 0.344 (chance 0.33) | scalp 16 ch · 256 Hz · 24 b = 98.3 kbit/s | 128 bit/s (768×) | 128 bit/s (768×) |  |  |  |  |  |
| ⚠ Lee2021-mobile — SSVEP stand<->walk | acc 0.349 (chance 0.33) | ear 14 ch · 256 Hz · 24 b = 86.0 kbit/s | 128 bit/s (672×) | 128 bit/s (672×) |  |  |  |  |  |
| ⚠ COG-BCI — N-back 0/1/2 | bacc 0.358 (chance 0.33) | ear 2 ch · 16 Hz · 24 b = 768 bit/s | 128 bit/s (6×) | 128 bit/s (6×) |  |  |  |  |  |

### Compute (MACs) savings of S and X within 0.02 of the full-config A metric

| Task | full-config A MACs | S: min MACs (×) | X: min avg MACs (×) |
|---|---|---|---|
| Muse-PhysioNet — 1-back vs 3-back | 4,330,560 | – | 4,264,128 (1.02×; all 4 ch, 64 Hz, 12 b, tau 0.5) |
| DTU-HI-AAD — eyes open vs closed (LSO) | 8,569,856 | 16,818,488 (0.51×; earscalp 16 ch, 64 Hz, 12 b, width 0.5) | 33,611,888 (0.25×; earscalp 16 ch, 64 Hz, 12 b, tau 0.5) |

### Escalation trade-off (accuracy vs average data rate)

![escalation muse_raw](figs/esc_muse_raw.png)
![escalation consumer5_muse](figs/esc_consumer5_muse.png)
![escalation dtu_rest](figs/esc_dtu_rest.png)

## Per-task heatmaps

Rows: A (retrained per point), the best available label-free protocol, and the gap between them, for each bit width. Each cell shows the best montage at that (channels, fs, bits); `·` = not run.

<details><summary>Ear-SAAD — AAD-SR 30s (label-free row: B)</summary>

![ear_saad_30s](figs/task_ear_saad_30s.png)

</details>
<details><summary>Ear-SAAD — AAD-SR 5s (label-free row: B)</summary>

![ear_saad_5s](figs/task_ear_saad_5s.png)

</details>
<details><summary>Ear-SAAD — AAD-SR 60s (label-free row: B)</summary>

![ear_saad_60s](figs/task_ear_saad_60s.png)

</details>
<details><summary>ICL-ear-AAD — AAD-SR 30s (label-free row: B)</summary>

![icl_30s](figs/task_icl_30s.png)

</details>
<details><summary>ICL-ear-AAD — AAD-SR 5s (label-free row: B)</summary>

![icl_5s](figs/task_icl_5s.png)

</details>
<details><summary>ICL-ear-AAD — AAD-SR 60s (label-free row: B)</summary>

![icl_60s](figs/task_icl_60s.png)

</details>
<details><summary>DTU-HI-AAD — AAD direction L/R (label-free row: B)</summary>

![dtu_aad](figs/task_dtu_aad.png)

</details>
<details><summary>KUL-AAD — AAD direction L/R (sim. ear, NC licence) (label-free row: B)</summary>

![kul_aad_lr](figs/task_kul_aad_lr.png)

</details>
<details><summary>Aarhus-event-AAD — attended-target word (label-free row: B)</summary>

![aarhus_raw](figs/task_aarhus_raw.png)

</details>
<details><summary>DAAMEE — music oddball P300 (label-free row: B)</summary>

![daamee_oddball](figs/task_daamee_oddball.png)

</details>
<details><summary>Lee2021-mobile — P300 target (standing) (label-free row: B)</summary>

![lee2021_raw](figs/task_lee2021_raw.png)

</details>
<details><summary>Cao2019-driving — alert vs drowsy (LSO) (label-free row: B)</summary>

![cao2019_raw](figs/task_cao2019_raw.png)

</details>
<details><summary>COG-BCI — PVT fast vs slow (label-free row: B)</summary>

![cogbci_pvt](figs/task_cogbci_pvt.png)

</details>
<details><summary>COG-BCI — MATB workload 3-level (label-free row: B)</summary>

![cogbci](figs/task_cogbci.png)

</details>
<details><summary>Muse-PhysioNet — 1-back vs 3-back (label-free row: B_ls_bn)</summary>

![muse_raw](figs/task_muse_raw.png)

</details>
<details><summary>Consumer-Muse2 — artifact type 5-class (LSO) (label-free row: E)</summary>

![consumer5_muse](figs/task_consumer5_muse.png)

</details>
<details><summary>Consumer-DSI24 — artifact type 5-class (LSO) (label-free row: B)</summary>

![consumer5_dsi](figs/task_consumer5_dsi.png)

</details>
<details><summary>COG-BCI — eyes open vs closed (label-free row: B)</summary>

![cogbci_rest](figs/task_cogbci_rest.png)

</details>
<details><summary>DTU-HI-AAD — eyes open vs closed (LSO) (label-free row: B_ls_bn)</summary>

![dtu_rest](figs/task_dtu_rest.png)

</details>
<details><summary>DTU-HI-AAD — 40 Hz ASSR detection (LSO) (label-free row: B)</summary>

![dtu_assr](figs/task_dtu_assr.png)

</details>
<details><summary>DTU-HI-AAD — hearing-impaired screening (LSO) (label-free row: B)</summary>

![dtu_hi_screen](figs/task_dtu_hi_screen.png)

</details>
<details><summary>⚠ near chance — AV-GC-AAD — AAD direction L/R (sim. ear) (label-free row: B)</summary>

![avgc_aad_lr](figs/task_avgc_aad_lr.png)

</details>
<details><summary>⚠ near chance — Lee2021-mobile — SSVEP 3-class (standing) (label-free row: B)</summary>

![lee2021_ssvep](figs/task_lee2021_ssvep.png)

</details>
<details><summary>⚠ near chance — Lee2021-mobile — SSVEP stand&lt;-&gt;walk (label-free row: B)</summary>

![lee2021_ssvep_walk](figs/task_lee2021_ssvep_walk.png)

</details>
<details><summary>⚠ near chance — COG-BCI — N-back 0/1/2 (label-free row: B)</summary>

![cogbci_nback](figs/task_cogbci_nback.png)

</details>

### Background: lab-cap datasets (retrained per point)

![grid2d](assets/grid2d_retrained.png)

## Related work

Knobs: ✓ studied for accuracy · ◐ design-time / offline only · F feasibility (power/area) only · – not studied.

| Paper | Venue | Setting | ch | fs | bits | capacity | Tasks | Retraining needed? | Runtime switching? | Notes |
|---|---|---|---|---|---|---|---|---|---|---|
| This work | 2026 | wearable EEG (ear / forehead / headband) | ✓ | ✓ | ✓ | ✓ | ~25 task instances | A: yes; B_ls / E / S / X: no | yes | Full channels x fs x bits grid per task; label-free switching vs per-point retraining. |
| [Power-saving design opportunities for wireless intracortical BCIs](https://doi.org/10.1038/s41551-020-0595-9) | Nat. Biomed. Eng. 2020 | implant (96-ch array) | – | ✓ | ✓ | – | 1 (velocity decoding) | offline spec | no | Bandwidth, bits, noise, bit-error sweeps -> decoding accuracy -> power; channels fixed. |
| [Low-power band of spiking activity (SBP)](https://doi.org/10.1038/s41551-020-0591-0) | Nat. Biomed. Eng. 2020 | implant | – | ◐ | – | – | finger / cursor decoding | n/a | no | 2 kSps band-power feature vs 30 kSps threshold crossings (two points, not a sweep). |
| [ML-guided resolution reconfiguration of neural recording](https://arxiv.org/abs/2510.22924) | arXiv 2025 | iEEG, ECoG, sEMG | ◐ | – | ✓ | – | 3 offline + online sEMG | yes (explicitly) | partly (AFE runtime-configurable, set offline) | Per-channel front-end resolution from decoder importance; channel-selection baseline. |
| [MINDFUL: system-level design of implantable BCIs](https://doi.org/10.1145/3725843.3756032) | MICRO 2025 | implant (1k-8k ch) | F | F | F | – | none (feasibility) | n/a | no | Channels, rate, bits -> power/area feasibility; 'channel dropout'; accuracy not measured. |
| [Swapping-centric neural recording systems](https://arxiv.org/abs/2409.17541) | NVMW 2024 | implant | F | F | – | – | none (feasibility) | n/a | no | Channels x sampling-rate feasible region at a fixed data rate; no task accuracy. |
| HALO / SCALO BCI processors | ISCA 2020 / ISCA 2023 | implant | – | – | – | – | throughput / power | n/a | no (static scheduling) | Channels and rate are throughput parameters (96 ch, 30 kHz, 16 bit); no accuracy sweep. |
| InfiniMind | ISCA 2025 | implant | ◐ | ◐ | – | – | continual learning | n/a | deployment-time loop | Configuration flow adjusts electrodes / sampling rate if accuracy drops (from abstract-level reading). |
| [Embedded EEGNet with reduced channels / sampling](https://arxiv.org/abs/2004.00077) | MeMeA 2020 | scalp EEG (MI) | ◐ | ◐ | – | – | 1 (motor imagery) | yes (per configuration) | no | Channels 64/38/19/8 and downsampling, one factor at a time. |
| [Complexity reduction for wearable seizure EEG](https://arxiv.org/abs/2606.12742) | arXiv 2026 | scalp EEG (seizure) | ◐ | – | – | ◐ | 1 (seizure) | no (zero-filled subsets) | no | Full-channel training, subset inference by zero filling; model quantization. |
| [EEG accuracy trade-offs on embedded heterogeneous processors](https://arxiv.org/abs/2402.09867) | arXiv 2024 | EEG | – | – | – | ✓ | 3 (seizure, sleep, stress) | n/a | compute knobs only | Approximate FFT/Welch + DVFS; sensing untouched. |
| Budget RNN | RTAS 2021 | 7 sensor time-series sets (no EEG) | – | ✓ | – | ✓ | 7 | no | yes | One model; runtime controller sets subsampling + early stop under an energy budget. |
| [SEE: sensor-aware early exit](https://arxiv.org/abs/2407.08715) | arXiv 2024 | biosignals incl. 6-ch EEG | – | – | – | ✓ | 6 | no | yes | Partial windows + early exit; sensors switched off after exit (window length knob). |
| [DynamicSleepNet](https://doi.org/10.3389/fphys.2023.1171467) | Front. Physiol. 2023 | PSG (EEG -> EOG -> EMG) | ✓ | – | – | ✓ | sleep staging | no | yes | Exits ordered by modality; acquisition of extra modalities not switched off. |
| [DANA: dimension-adaptive neural architecture](https://arxiv.org/abs/2008.02397) | IMWUT 2021 | HAR inertial sensors | ✓ | ✓ | – | – | HAR | no (one model) | yes | Training over random sampling rates x sensor subsets; closest precedent of protocol E (no bits axis). |
| [AdaSense](https://arxiv.org/abs/2006.05884) | DAC 2020 | HAR accelerometer | – | ✓ | – | – | HAR | no (one classifier) | yes | Runtime switching of sampling rate / averaging window with fixed-length features. |
| [UR2M](https://arxiv.org/abs/2402.09264) | PerCom 2024 | MCU event detection (EEG only as example) | – | – | – | ✓ | audio / cardiac | no | yes | Uncertainty-aware early exit; no sensing knobs. |
| [DCentNet](https://arxiv.org/abs/2502.17446) | arXiv 2025 | ECG | – | – | – | ✓ | arrhythmia | no | yes | Early exits split across edge / cloud; saves radio, not sensing. |

## Status

Coverage = distinct (channels, fs, bits) points of the 125-point target grid present (ordered montages). Seeds = range of training seeds per point.

| Task | A | B | B_ls | B_ls_bn | E | S | X | ESC |
|---|---|---|---|---|---|---|---|---|
| Ear-SAAD — AAD-SR 30s | 45/125, seeds 1 | 45/125, seeds 1 |  |  |  |  |  |  |
| Ear-SAAD — AAD-SR 5s | 45/125, seeds 1 | 45/125, seeds 1 |  |  |  |  |  |  |
| Ear-SAAD — AAD-SR 60s | 45/125, seeds 1 | 45/125, seeds 1 |  |  |  |  |  |  |
| ICL-ear-AAD — AAD-SR 30s | 20/125, seeds 1 | 20/125, seeds 1 |  |  |  |  |  |  |
| ICL-ear-AAD — AAD-SR 5s | 20/125, seeds 1 | 20/125, seeds 1 |  |  |  |  |  |  |
| ICL-ear-AAD — AAD-SR 60s | 20/125, seeds 1 | 20/125, seeds 1 |  |  |  |  |  |  |
| DTU-HI-AAD — AAD direction L/R | 10/125, seeds 3 | 10/125, seeds 3 |  |  |  |  |  |  |
| KUL-AAD — AAD direction L/R (sim. ear, NC licence) | 5/125, seeds 3 | 5/125, seeds 3 |  |  |  |  |  |  |
| Aarhus-event-AAD — attended-target word | 65/125, seeds 1–3 | 65/125, seeds 1–3 |  |  |  |  |  |  |
| DAAMEE — music oddball P300 | 5/125, seeds 3 | 5/125, seeds 3 |  |  |  |  |  |  |
| Lee2021-mobile — P300 target (standing) | 125/125, seeds 1–3 | 125/125, seeds 1–3 |  |  |  |  |  |  |
| Cao2019-driving — alert vs drowsy (LSO) | 25/125, seeds 1–3 | 25/125, seeds 1–3 |  |  |  |  |  |  |
| COG-BCI — PVT fast vs slow | 10/125, seeds 3 | 10/125, seeds 3 |  |  |  |  |  |  |
| COG-BCI — MATB workload 3-level | 10/125, seeds 3 | 10/125, seeds 3 |  |  |  |  |  |  |
| Muse-PhysioNet — 1-back vs 3-back | 50/125, seeds 1–3 | 50/125, seeds 1–3 | 40/125, seeds 1 | 40/125, seeds 1 | 50/125, seeds 1 | 50/125, seeds 1 | 50/125, seeds 1 | 80 pts |
| Consumer-Muse2 — artifact type 5-class (LSO) | 50/125, seeds 1–3 | 50/125, seeds 1–3 | 40/125, seeds 3 | 40/125, seeds 3 | 50/125, seeds 3 | 50/125, seeds 3 | 50/125, seeds 3 | 96 pts |
| Consumer-DSI24 — artifact type 5-class (LSO) | 10/125, seeds 3 | 10/125, seeds 3 |  |  |  |  |  |  |
| COG-BCI — eyes open vs closed | 10/125, seeds 3 | 10/125, seeds 3 |  |  |  |  |  |  |
| DTU-HI-AAD — eyes open vs closed (LSO) | 10/125, seeds 3 | 10/125, seeds 3 | 100/125, seeds 1 | 100/125, seeds 1 | 125/125, seeds 1 | 125/125, seeds 1 | 125/125, seeds 1 | 80 pts |
| DTU-HI-AAD — 40 Hz ASSR detection (LSO) | 10/125, seeds 3 | 10/125, seeds 3 |  |  |  |  |  |  |
| DTU-HI-AAD — hearing-impaired screening (LSO) | 10/125, seeds 3 | 10/125, seeds 3 |  |  |  |  |  |  |
| ⚠ AV-GC-AAD — AAD direction L/R (sim. ear) | 5/125, seeds 3 | 5/125, seeds 3 |  |  |  |  |  |  |
| ⚠ Lee2021-mobile — SSVEP 3-class (standing) | 125/125, seeds 1–3 | 125/125, seeds 1–3 |  |  |  |  |  |  |
| ⚠ Lee2021-mobile — SSVEP stand<->walk | 125/125, seeds 1–3 | 125/125, seeds 1–3 |  |  |  |  |  |  |
| ⚠ COG-BCI — N-back 0/1/2 | 10/125, seeds 3 | 10/125, seeds 3 |  |  |  |  |  |  |

## Caveats

- Tasks whose full-config A metric is within 0.03 of chance are flagged (⚠) and excluded from headline savings; they are shown for completeness only.
- Several tasks use electrodes picked from a full scalp cap to emulate ear / forehead positions ("simulated"); the reference electrode is still the cap's, not an ear reference.
- Quantization models an ideal uniform mid-tread ADC with per-channel full scale set on the training fold; real ADC noise, nonlinearity and front-end noise are not modelled.
- Metrics are means over subjects (seeds averaged within subject first); 95% CIs are bootstrap over subjects. Savings use point estimates, so a point counted as 'within ε' may have an overlapping but lower CI.
- Partial seeds: some points/protocols have only 1 training seed instead of 3 (14 task instances affected).
- B_ls is only evaluated at fs ≥ 32 Hz; E keeps compute at 256 Hz (low-rate input is upsampled), so E saves sensing / radio bits but not model MACs.
- Minimum-data-rate search takes the best montage at design time (choosing the montage uses labelled validation data); random-subset rows (r0–r2) are not used for savings.
- MACs are per inference window of the evaluated model; ESC MACs are the sum of all rungs evaluated.

Data issues detected at build time:
- 21835 rows in the summary CSV carry the internal cache key as dataset/task name (avgc_aad_lr, consumer5_dsi, daamee_oddball, dtu_aad, dtu_assr, dtu_hi_screen, dtu_rest, kul_aad_lr); public names were taken from the aggregator's NAMES table.

## 中文摘要

本页展示可穿戴 EEG 在“通道数 × 采样率 × ADC 位数”三个维度上能降到多少而任务指标不明显下降，以及已部署模型能否在不重训、不用标签的情况下切换工作点。A = 每个工作点重训（上界）；B_ls = 闭式最小二乘通道折叠 + 时间卷积核重采样 + 测试时量化（无标签、免重训）；B_ls_bn = 再加无标签 BN 统计重估；E = 一次训练的弹性模型；S = 可切换宽度；X = 模型内早退；ESC = 不确定时升级感知配置。

- 有结果的任务实例 25 个，其中 21 个明显高于随机水平（接近随机的任务只放附录，不进总结）。
- 在不比满配置 A 低 0.02 以内，A（每点重训）的数据率中位数可降 6 倍；最好的免重训协议中位数可降 24 倍（3 个任务有免重训结果）。
- 注意：量化是理想 ADC 模型；部分任务的电极是从头皮帽子模拟的；部分协议只有 1 个随机种子；指标为被试平均，置信区间是被试层面 bootstrap。

## Regenerate

From the project root: `python scripts/build_site.py` (reads `results/summary/wearable_factorial.csv`, rewrites this folder).
