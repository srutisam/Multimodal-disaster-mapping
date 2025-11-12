This project implements a dual-encoder deep learning architecture that fuses Sentinel-1 (SAR) and Sentinel-2 (optical) satellite imagery to detect post-conflict infrastructure damage.
It focuses on rapid disaster mapping in war-affected regions — specifically the Kyiv–Bucha area (Feb–Apr 2022) — and evaluates both classical and deep learning approaches for change detection.

<h1>Objectives</h1>

Fuse multi-modal remote sensing data (SAR + optical) for high-resolution change detection.

Replicate baseline algorithms (SAR log-ratio, GLCM texture analysis) and compare with deep models.

Develop a Dual-Encoder Cross-Attention U-Net architecture for multimodal feature fusion.

Evaluate performance using F1-score, IoU, and qualitative visual analysis.

Demonstrate generalization across other AOIs such as Mariupol.

<h1>Dataset & Inputs</h1>
Data Source	Description	Resolution
Sentinel-1 GRD (VV, VH)	SAR intensity (pre/post invasion)	10 m
Sentinel-2 L2A (RGB + NIR)	Optical reflectance imagery (pre/post invasion)	10 m
Google Open Buildings (Vector)	Building footprints for mask creation	—
UNOSAT Damage Assessment (Vector)	Ground-truth damage labels.

All data are publicly available via Copernicus Open Access Hub, Planetary Computer STAC, or UNOSAT portals.

<h1>Methodology</h1>
<h2>1. Data Preparation</h2>

Collect pre- and post-event Sentinel-1/2 imagery.

Align projections (UTM zone 36N) and resample to 10 m resolution.

Generate binary ground-truth masks from vector overlays (UNOSAT × OpenBuildings).

Stack SAR + optical bands into multi-channel rasters.

<h2>2. Baseline Replication</h2>

SAR Log-Ratio: 
𝐿=log 10(𝐼_𝑝𝑜𝑠𝑡/_𝐼𝑝𝑟𝑒)

Optical Texture (GLCM): Compute homogeneity, contrast, entropy.

Combine via threshold fusion for damage mapping.

<h2>3. Deep Learning Architecture</h2>

Dual-Encoder Cross-Attention U-Net

Encoder-A: Sentinel-1 channels (VV, VH)

Encoder-B: Sentinel-2 channels (RGB + NIR)

Cross-attention fusion at bottleneck

Decoder with skip connections and 1×1 output head

Loss: Dice Loss (binary segmentation)

Optimizer: Adam (lr = 1e-4)

Framework: PyTorch + segmentation-models-pytorch

<h2>4. Training Setup</h2>

Patch size: 256×256

Batch size: 8

Epochs: 50

GPU: NVIDIA T4 (Google Colab Pro)

<h2>5. Evaluation</h2>

Quantitative: F1-score, IoU

Qualitative: Pre/post image + predicted mask visualization

Optional generalization to Mariupol AOI for robustness testing.
