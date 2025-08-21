# XMM-Newton X-ray Source Analysis

A data science analysis of the **4XMM-DR14** X-ray source catalog from the XMM-Newton space observatory, featuring original dimensionality reduction technique and astronomical data preprocessing.

## 🌌 Overview

This repository contains a complete analysis pipeline for exploring X-ray source data from the **XMM-Newton** space telescope's **4th XMM-Newton Serendipitous Source Catalog (4XMM-DR14)**. The project demonstrates the application of data science methodologies to astronomical datasets, with particular emphasis on discovering hidden patterns in X-ray source properties.

### Key Features

- **Comprehensive Data Preprocessing**: Quality filtering, outlier removal, and feature engineering
- **Innovative PCA² Analysis**: Double Principal Component Analysis
- **Astronomical Data Handling**: Proper treatment of X-ray flux measurements, hardness ratios, and temporal data
- **Pattern Discovery**: Identification of distinct source populations and unusual objects

## 📊 Dataset

**Source**: ESA's XMM-Newton Science Archive  
**Catalog**: 4XMM-DR14 (4th XMM-Newton Serendipitous Source Catalog, Data Release 14)  
**Format**: Slim version containing key measurements for unique X-ray sources  
**Size**: ~540,000 X-ray sources (after preprocessing: ~280,000 high-quality sources)

**Note**: The public catalogue is usually screened to remove high-background/solar-flare intervals and also mixes MOS & pn cameras.

### Key Measurements

- **Multi-band X-ray fluxes** (0.2-12.0 keV in various energy bands)
- **Hardness ratios** (spectral hardness indicators HR1-HR4)  
- **Variability parameters** (temporal variability measurements)
- **Source properties** (spatial extent, detection quality, coordinates)
- **Temporal information** (observation dates, detection counts)

### Dependencies

- **pandas** (≥1.3.0): Data manipulation and analysis
- **polar** (≥1.32.0): Data manipulation and analysis
- **matplotlib** (≥3.5.0): Visualization and plotting  
- **scikit-learn** (≥0.24.0): Machine learning and PCA
- **astropy** (≥5.0.0): Astronomical data handling and time conversions

## 📈 Analysis Pipeline

### 1. Data Preprocessing (`preprocessing.ipynb`)

**Objective**: Transform raw catalog data into a clean, analysis-ready dataset.

**Key Steps**:
- ✅ **Quality Filtering**: Remove sources with poor detection flags (`sc_sum_flag ≠ 0`)
- ✅ **Missing Data Handling**: Strategic approach to NaN values (removal vs. imputation)
- ✅ **Hardness Ratio Quality Control**: Ensure complete spectral information
- ✅ **Outlier Removal**: Modified IQR method for error measurements (98th percentile filtering)
- ✅ **Feature Engineering**: Convert MJD timestamps to datetime format
- ✅ **Data Optimization**: Remove uninformative columns and optimize data types

**Outcome**: High-quality dataset with ~280,000 sources containing complete measurements.

### 2. (Simple) Exploratory Data Analysis (`EDA.ipynb`)

**Objective**: Understand data distributions, correlations, and basic patterns.

**Analysis Components**:
- Distribution analysis of X-ray fluxes and hardness ratios
- Correlation analysis between spectral measurements
- Temporal distribution of observations
- Spatial distribution across the sky
- Variability analysis

### 3. Advanced Pattern Discovery (`PCA^2.ipynb`)

**Objective**: Apply innovative PCA² technique to uncover hidden structure in X-ray source data.

#### **The PCA² Innovation**

**PCA²** (PCA-Squared) is a novel two-stage dimensionality reduction technique:

1. **Stage 1**: Standard PCA on original features → captures main population trends
2. **Stage 2**: PCA on reconstruction errors → reveals unique characteristics and anomalies

**Why This Works**:
- **Reconstruction errors** contain information that standard PCA cannot capture
- These errors represent the "uniqueness" of each source
- Applying PCA to errors reveals systematic patterns in what makes sources unusual

#### **Key Discovery: The Cross Pattern**

The most remarkable finding is a **distinctive cross-shaped structure** in the reconstruction error PCA space:

- **Central Region**: "Typical" sources well-represented by standard PCA
- **Four Arms**: Distinct modes of deviation from typical behavior
- **High Concentration**: ~90% of "uniqueness variance" captured in just 2 dimensions
- **Systematic Structure**: Clear evidence of four different types of anomalous sources

![Distinctive Cross Pattern in PCA² Space](/output/cross_sc.png)

*Figure: Visualization of the cross-shaped structure discovered in the PCA² reconstruction error space. Each point represents an X-ray source. The central region contains typical sources, while the four arms highlight distinct anomaly patterns.*

## 🎯 Key Results & Findings

### 1. Data Quality Assessment
- **~65% data retention** after quality filtering (692k → 448k sources)
- **Complete spectral information** for all retained sources
- **Robust error measurements** after outlier removal

### 2. Population Structure Discovery
- **Four distinct anomaly patterns** revealed by PCA² analysis
- **Systematic deviations** from main population trends
- **Potential subpopulations** with unique astrophysical properties

### 3. Anomaly Detection Capability
- **Clear separation** between typical and unusual sources
- **Quantitative outlier scores** based on reconstruction errors
- **Prime candidates** for detailed astrophysical follow-up

---
---
## UPDATE: Cross Pattern Investigation

### Overview
After feedback from domain experts I'm worked on a deeper analysis on the complete dataset, not the slim version. Here are the details the investigation into the "cross" pattern that emerged in the anomaly detection analysis of XMM-Newton data, focusing on instrument-specific effects and their impact on anomaly patterns.

### Methodology

#### 1. Full Dataset Analysis
- **Dataset**: Upgraded from slim version to full dataset
- **New Features**: Separate columns for each instrument and combinations:
  - `pn`: PN camera measurements
  - `mos1`: MOS1 camera measurements  
  - `mos2`: MOS2 camera measurements
  - `ep`: Combined instrument measurements
  - `sc`: Combined instruments and observations

#### 2. Minimal Preprocessing
- **Filter Applied**: Only non-optimal observations removed (`sum_flag > 0`)
- **Purpose**: Ensure cross pattern is not an artifact of preprocessing effects
- **Approach**: Minimal intervention to preserve data integrity

#### 3. Feature Selection Strategy
- **Focus**: Flux columns only
- **Exclusions**: Correlated variables (e.g., hardness ratios)
- **Rationale**: Isolate cross pattern from potential feature correlation/engineering effects

#### 4. Instrument-Specific Analysis
Systematic analysis performed on individual instruments:
- **PN-only analysis**: Isolated PN camera data
- **MOS1-only analysis**: Isolated MOS1 camera data  
- **MOS2-only analysis**: Isolated MOS2 camera data
- **Purpose**: Identify potential instrument-mix effects on anomaly patterns

### Key Findings

#### Cross Pattern Persistence
The cross pattern in PCA space **persists only in PN measurements**, indicating preferred directions for anomalies in the principal component space.

![Cross Pattern only in PN measurements](/output/cross_pn_no_cross_mos1_no_cross_mos2.jpeg)
PN detail with coloring for hardness (output folder)
<img src="/output/cross_pn_hardness_color.png" alt="Cross Pattern in PN with coloring" width="1000" height="600"/>

#### Instrument-Specific Behavior
- **PN Camera**: Clear cross pattern visible in anomaly detection
- **MOS1 Camera**: Cross pattern absent
- **MOS2 Camera**: Cross pattern absent

### Interpretation

Two potential explanations for the PN-specific cross pattern:

#### Hypothesis A: Instrumental Artifact
The PN measurements themselves create the anomaly patterns due to:
- Inherent characteristics of the PN instrument
- Systematic measurement biases
- Instrument-specific noise patterns

#### Hypothesis B: Detection Sensitivity
The MOS measurements failed to capture this anomaly pattern due to:
- Lower sensitivity to the specific anomaly types
- Different instrumental response characteristics
- Insufficient resolution for pattern detection

#### Hypothesis C: Missmatch observations
The PN dataset does not perfectly overlap with MOS observations (sharing only ~63.1% with MOS1 and ~76.1% with MOS2).
- The non-overlapping observations may introduce unique patterns visible only in PN data
- Differences in observation coverage and exposure can steer the anomaly distribution
- The cross pattern could partly arise from PN-only detections rather than purely instrumental effects

### Implications

The instrument-specific nature of the cross pattern suggests:
1. **Anomaly patterns may be instrument-dependent**
2. **PN camera may be more sensitive to certain anomaly types**
3. **Combined instrument analysis may mask instrument-specific patterns**
4. **Future analysis should consider instrument-specific anomaly detection**
---
---

## ⚠️ **Important Limitations & Disclaimers**

### **🚨 CRITICAL: Limited Domain Knowledge**

**This analysis was conducted with LIMITED ASTRONOMICAL DOMAIN EXPERTISE and should be interpreted with significant caution:**

- **⚠️ Simplified Approach**: Uses only basic PCA techniques without incorporating astrophysical priors
- **⚠️ No Physical Modeling**: Does not account for instrumental effects, selection biases, or physical correlations
- **⚠️ Missing Context**: Lacks proper understanding of X-ray source classifications, evolutionary states, and observational systematics
- **⚠️ Statistical Only**: Focuses purely on data patterns without astrophysical interpretation

### **🔬 Technical Limitations**

- **Basic Methodology**: Relies primarily on PCA, a linear technique that may miss complex non-linear relationships
- **No Advanced ML**: Does not employ sophisticated machine learning methods (clustering, neural networks, etc.)
- **Limited Feature Engineering**: Minimal creation of derived astrophysical parameters
- **Preprocessing Choices**: Some decisions (like 98th percentile outlier removal) may be too aggressive for astronomical data

### **📊 Data Limitations**

- **Incomplete Physical Picture**: Uses only tabulated catalog values, not full spectral/timing data
- **Selection Effects**: No correction for XMM-Newton survey selection biases
- **Cross-matching Missing**: No integration with other wavelength catalogs for context
- **Error Propagation**: Simplified treatment of measurement uncertainties

## 🔮 Future Work & Improvements

### **Immediate Next Steps**

1. **🧠 Domain Expert Collaboration**
   - Partner with X-ray astronomers for proper interpretation
   - Validate findings against known source classifications
   - Incorporate astrophysical priors into analysis

2. **🤖 Advanced Machine Learning**
   - Apply clustering algorithms (DBSCAN, HDBSCAN, Gaussian Mixtures)
   - Implement non-linear dimensionality reduction (t-SNE, UMAP)
   - Explore deep learning approaches for pattern recognition

3. **🔗 Enhanced Data Integration**
   - Cross-match with optical catalogs (Gaia, SDSS)
   - Incorporate multiwavelength information
   - Add source classification labels for supervised learning

### **Long-term Research Directions**

4. **🔬 Physical Modeling**
   - Implement proper X-ray spectral fitting
   - Account for instrumental response and systematic errors
   - Develop physically-motivated feature engineering

5. **📈 Advanced Analysis**
   - Time series analysis for variable sources
   - Hierarchical clustering of source populations
   - Anomaly detection with astrophysical context

6. **🎯 Scientific Applications**
   - Systematic search for rare source types
   - Population synthesis modeling
   - Discovery of new X-ray phenomena

## 🤝 Contributing

This project welcomes contributions, especially from:
- **X-ray astronomers** who can provide domain expertise
- **Data scientists** interested in astronomical applications  
- **Students** learning data science with real-world datasets

### How to Contribute
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -am 'Add new analysis'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Create a Pull Request

## 📚 References & Acknowledgments

- **XMM-Newton Science Archive**: [https://www.cosmos.esa.int/web/xmm-newton](https://www.cosmos.esa.int/web/xmm-newton)
- **4XMM Catalog**: Webb et al. (2020), A&A, 641, A136
- **ESA XMM-Newton Project**: For making this invaluable dataset publicly available

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**⚠️ DISCLAIMER**: This analysis represents a data science exploration of astronomical data with limited domain expertise. Results should not be used for scientific publication without proper astronomical validation and collaboration with X-ray astronomy experts. The techniques demonstrated here are educational and methodological in nature, serving as a starting point for more sophisticated astrophysical analysis.
