
# Region-Based Hierarchical Clustering Color Quantization (RHCCQ)

**RHCCQ** is an adaptive, high-efficiency image compression framework. It treats an image as a hierarchy of importance rather than a uniform grid of pixels, prioritizing visual fidelity in critical regions while aggressively optimizing background data.

## 🚀 The Core Philosophy

Traditional compression (like standard JPEG) applies uniform loss across the entire image. RHCCQ introduces **Region of Interest (ROI) Awareness**, allowing for "Selective Fidelity"—spending the bit-budget where it matters most.

---

## 🛠 How It Works: The Pipeline

### 1. ROI Detection (Saliency Feature Initialization)

The algorithm identifies significant areas using edge density analysis.

* **Feature Mapping:** Computes spatial edge density and establishes adaptive thresholds.
* **Geometric Consolidation:** Protects natural borders and fills gaps to create solid, unified masks for the encoder.

### 2. Subregion Identification (SLIC)

To manage complexity, identified regions are broken down into superpixels.

* **Adaptive Complexity Scoring:** Calculates color variance to determine the optimal number of segments.
* **SLIC Partitioning:** Divides the image into perceptually meaningful clusters that respect natural boundaries.

### 3. Adaptive Color Quantization (The Novel Approach)

Unlike K-Means, which requires a fixed  and often "absorbs" rare ROI colors, RHCCQ utilizes a **Density-Based Fusion (DBSCAN)** method.

* **JPEG Normalization:** Automatically derives **Epsilon** and **Max Cluster Size** by mapping user-defined quality levels to mathematical constraints.
* **Hierarchical Merging:**
* **Local:** Subregions are fused to establish a specific ROI Palette.
* **Global:** ROIs are fused to finalize a unified Full Image Palette.



### 4. Encoding & The .rhccq Format

The final data is serialized into a custom format designed for the intersection of hierarchical clustering and ROI processing.

* **Bit-Budgeting:** Minimal bounding boxes isolate subregions; "black" padding is strategically ignored during clustering to prevent background interference.
* **Compression:** Employs **zlib** to compress color indices and palettes efficiently.
* **Format:** Generates `.rhccq` files, a specialized framework for region-aware data.

---

## 📂 The Decoder

The `.rhccq` decoder performs high-speed reconstruction:

1. **Ingestion:** Loads the compressed data stream.
2. **Unpacking:** Uses **zlib** to restore the optimized color palette and pixel indices.
3. **Mapping:** Reconstructs the image by mapping indices back to the hierarchical region coordinates.

---

## 🌟 Technical Advantages

* **Novel Methodology:** Introduces a density-based color fusion technique not currently documented in standard image processing research.
* **Detail Preservation:** Eliminates color banding by prioritizing high-entropy areas.
* **Storage Efficiency:** Resolves the "Overhead Paradox" by using hierarchical merging to reduce metadata.
* **Mathematical Versatility:** The framework can be applied to any data type in a discrete finite space, not just RGB images.

---

## 📊 Potential Applications

* **Medical Imaging:** Preserve perfect detail in diagnostic regions while compressing healthy tissue.
* **Satellite Imagery:** Focus bandwidth on urban centers or specific coordinates.
* **Remote Surveillance:** Maintain high-fidelity faces/license plates in low-bandwidth streams.
