**Slim version** of the **4XMM** catalogue — a cleaned-up, compact table where **one row = one unique source**, and only **key columns** are included.

Columns (grouped by purpose), with **simple explanations**:

---

### 🆔 **Source Identification**

| Column    | Meaning                                                            |
| --------- | ------------------------------------------------------------------ |
| `srcid`   | Unique internal source ID assigned in the catalogue                |
| `iauname` | Official IAU-compliant source name (e.g., `4XMM J123456.7+654321`) |

---

### 📍 **Sky Position & Detection Quality**

| Column      | Meaning                                                  |
| ----------- | -------------------------------------------------------- |
| `sc_ra`     | Right Ascension (RA) of the source (J2000, in degrees)   |
| `sc_dec`    | Declination (Dec) of the source (J2000, in degrees)      |
| `sc_poserr` | Positional uncertainty (1σ error, in arcseconds)         |
| `sc_det_ml` | Detection likelihood (higher = more confident detection) |

---

### 💡 **X-ray Fluxes (in different energy bands)**

Each flux is in units of **erg/cm²/s**.

| Column          | Meaning                                        |
| --------------- | ---------------------------------------------- |
| `sc_ep_1_flux`  | Flux in band 1: **0.2–0.5 keV**                |
| `sc_ep_2_flux`  | Flux in band 2: **0.5–1.0 keV**                |
| `sc_ep_3_flux`  | Flux in band 3: **1.0–2.0 keV**                |
| `sc_ep_4_flux`  | Flux in band 4: **2.0–4.5 keV**                |
| `sc_ep_5_flux`  | Flux in band 5: **4.5–12.0 keV**               |
| `sc_ep_8_flux`  | Flux in band 8: **0.2–12.0 keV** (total band)  |
| `sc_ep_9_flux`  | Flux in band 9: **0.5–4.5 keV** (intermediate) |
| `*_err` columns | Corresponding 1σ statistical uncertainty       |

---

### 📈 **Flux Variability (Min/Max)**

| Column         | Meaning                                    |
| -------------- | ------------------------------------------ |
| `sc_ep_8_fmin` | Minimum flux in band 8 over all detections |
| `sc_ep_8_fmax` | Maximum flux in band 8 over all detections |
| `*_err`        | Errors for min/max fluxes                  |

---

### 🌈 **Hardness Ratios (X-ray color)**

These describe **spectral shape** — how "hard" or "soft" the X-ray emission is (i.e., energy balance between bands).

| Column   | Meaning                                     |
| -------- | ------------------------------------------- |
| `sc_hr1` | HR1 = (Band 2 − Band 1) / (Band 2 + Band 1) |
| `sc_hr2` | HR2 = (Band 3 − Band 2) / (Band 3 + Band 2) |
| `sc_hr3` | HR3 = (Band 4 − Band 3) / (Band 4 + Band 3) |
| `sc_hr4` | HR4 = (Band 5 − Band 4) / (Band 5 + Band 4) |
| `*_err`  | 1σ error on hardness ratio                  |

These help distinguish between source types: soft (stars) vs. hard (AGN, X-ray binaries).

---

### 🔍 **Extent (is it point-like or extended?)**

| Column       | Meaning                                          |
| ------------ | ------------------------------------------------ |
| `sc_extent`  | Estimated source radius (in arcseconds)          |
| `sc_ext_err` | Uncertainty on extent                            |
| `sc_ext_ml`  | Likelihood that the source is **not point-like** |

* High values → extended objects (e.g. galaxy clusters)

---

### 🧪 **Variability & Fit Quality**

| Column        | Meaning                                                          |
| ------------- | ---------------------------------------------------------------- |
| `sc_var_flag` | Flag for **variability** across multiple detections              |
| `sc_fvar`     | **Fractional variability** estimate (only meaningful if flagged) |
| `sc_fvarerr`  | Error on fractional variability                                  |
| `sc_chi2prob` | χ² probability of a constant flux model (low = variable)         |

---

### ⚠️ **Flags**

| Column        | Meaning                                                  |
| ------------- | -------------------------------------------------------- |
| `sc_sum_flag` | **Summary flag**: 0 = good, 1–4 = increasing issues      |
| `confused`    | Flag indicating source may be **blended with neighbors** |

---

### 🕒 **Time Information**

| Column         | Meaning                                               |
| -------------- | ----------------------------------------------------- |
| `mjd_first`    | Time of **first detection** (in Modified Julian Date) |
| `mjd_last`     | Time of **last detection**                            |
| `n_detections` | Number of detections contributing to this source      |

---

### 🌐 **Extras**

| Column        | Meaning                                                       |
| ------------- | ------------------------------------------------------------- |
| `webpage_url` | Direct link to the source’s online summary in the XMM archive |

---

### ✅ Summary

You now have a **lightweight catalog of high-confidence sources**, each summarized with:

* Position
* X-ray brightness in multiple bands
* Hardness ratios (like “color” in X-rays)
* Variability indicators
* Extent flags (point-like or extended)
* Timing across multiple detections (if observed multiple times)

---

Let me know if you want help filtering this data (e.g., "show only extended sources" or "only highly variable AGN candidates") or visualizing a light curve or flux distribution!
