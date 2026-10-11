# Dimensions³ — Jebel Ali Flood Evidence PoC

**Team Dimensions³ · Climate-induced Disasters · United Arab Emirates**

Dimensions³ is a multi-source Earth Observation proof of concept that combines satellite imagery, rainfall data, official reports, and public evidence to screen for possible flood-related surface changes around Jebel Ali, Dubai.

[![Open in Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/vfatimaalkaabi/dimensions3-flood-poc/blob/main/notebooks/02_main_analysis.ipynb)

## Live Demonstration

- **Dimensions³ web portal:**  
  https://vfatimaalkaabi.github.io/dimensions3-flood-poc/

- **Jebel Ali flood-evidence packet:**  
  https://vfatimaalkaabi.github.io/dimensions3-flood-poc/jebel-ali-packet.html

- **GitHub repository:**  
  https://github.com/vfatimaalkaabi/dimensions3-flood-poc

The Jebel Ali packet presents the reproduced proof-of-concept results. The wider portal demonstrates the proposed customer workflow; some alerts, city-level functions and future analytical capabilities are illustrative prototype features.

## 1. Business Use Case

Dimensions³ is designed for:

- Municipalities and emergency-management teams
- Civil defence and disaster-response authorities
- Ports and infrastructure operators
- Environmental and climate agencies
- Urban planners
- Insurers, researchers and resilience planners
- Decision-makers without specialist remote-sensing knowledge

After extreme rainfall, these users need to identify locations that may require inspection, understand the available evidence and prioritize response activities.

The platform converts multiple technical datasets into one evidence packet containing satellite comparisons, rainfall information, supporting reports and a transparent screening result.

## 2. Problem

Arid regions such as the United Arab Emirates can experience sudden intense rainfall and flash flooding. Relevant satellite imagery, weather measurements and event reports are available, but they are distributed across different platforms and require specialist processing.

A single optical image can also be affected by cloud, shadows, acquisition timing or permanent water. Radar imagery is more reliable during cloudy conditions, but it can still respond to wet soil, roughness and geometric effects.

Dimensions³ addresses this problem by combining several independent sources rather than relying on one image or sensor.

## 3. Data Used

### Sentinel-1 SAR

- **Provider:** European Space Agency and Copernicus Data Space Ecosystem
- **Product:** Sentinel-1A Interferometric Wide Swath GRD
- **Processing level:** Level-1 GRD
- **Polarizations:** VV and VH
- **Before:** 12 April 2024
- **After:** 24 April 2024
- **Purpose:** Radar-backscatter change and candidate-water screening
- **Licence:** Copernicus Sentinel Data Terms and Conditions

### Sentinel-2

- **Provider:** European Space Agency/Copernicus, accessed through Microsoft Planetary Computer
- **Product:** Sentinel-2 MSI Level-2A surface reflectance
- **Tile:** 40RBN
- **Before:** 12 April 2024
- **After:** 22 April 2024
- **Bands used:** Blue, green, red, near-infrared and short-wave infrared
- **Purpose:** Natural-colour comparison and MNDWI water-change screening
- **Licence:** Copernicus Sentinel Data Terms and Conditions

### Landsat 9

- **Provider:** United States Geological Survey, accessed through Microsoft Planetary Computer
- **Product:** Landsat Collection 2 Level-2 surface reflectance
- **Path/row:** 160/043
- **Before:** 3 April 2024
- **After:** 19 April 2024
- **Purpose:** Independent optical water-change evidence using MNDWI
- **Licence:** Landsat data are in the public domain; source acknowledgement is requested

### NASA POWER

- **Provider:** NASA Prediction of Worldwide Energy Resources
- **Period:** 1–30 April 2024
- **Location:** Approximately 24.98°N, 55.05°E
- **Variables:** Daily precipitation, temperature and wind speed
- **Purpose:** Rainfall-event context and temporal corroboration
- **Licence:** NASA POWER data-access and attribution guidance

### Supporting Evidence

The PoC includes six official/news records and three public-content records associated with the April 2024 UAE rainfall event.

Supporting reports are used only as regional and temporal corroboration. They are not treated as pixel-level flood ground truth.

### Hyperspectral Data

Hyperspectral imagery was not used in the current flood-screening result.

Future versions may investigate hyperspectral data for surface-moisture characterization, material identification and water-quality assessment where suitable imagery and validation data are available.

## 4. Technical Approach

The processing workflow is:

1. Define the Jebel Ali study area and April 2024 event period.
2. Collect before-and-after Sentinel-1, Sentinel-2 and Landsat 9 observations.
3. Inspect scene dates, cloud cover, processing levels and spatial coverage.
4. Calculate optical water-related change using MNDWI.
5. Compare Sentinel-1 VV and VH radar backscatter before and after the event.
6. Remove permanent water, including the sea and port basins.
7. Filter isolated candidate pixels and obvious radar noise.
8. Resize the candidate masks to a common `374 × 341` analysis grid.
9. Identify high-agreement candidate pixels supported by at least two sensors.
10. Add rainfall, official reports and public corroborating evidence.
11. Produce a transparent combined screening score and evidence dashboard.

The Modified Normalized Difference Water Index is:

```text
MNDWI = (Green - SWIR) / (Green + SWIR)
```

The internal spatial fusion uses:

```text
40% Sentinel-1 + 30% Sentinel-2 + 30% Landsat 9
```

The final evidence score uses:

```text
50% spatial support + 30% rainfall support + 20% report support
```

The current PoC uses transparent rules and weighted evidence fusion. CNN segmentation, deep learning and AI-agent automation are proposed future improvements and are not presented as completed validation.

## 5. Headline Result

The reproduced proof-of-concept result is:

| Measure | Result |
|---|---:|
| Screening label | **MEDIUM** |
| Combined evidence score | **0.574** |
| High-agreement candidate area | **0.74%** |
| High-agreement pixels | **873** |
| Peak daily rainfall | **53.39 mm** |
| Peak rainfall date | **16 April 2024** |
| Supporting evidence records | **9** |

The `0.574` value is an evidence-screening score, not a formal accuracy percentage.

The `0.74%` result represents a preliminary candidate flood-related area supported by multiple sensors after permanent-water removal. It is not a confirmed flood-extent measurement.

## 6. Installation

Python 3.11 is recommended.

```bash
git clone https://github.com/vfatimaalkaabi/dimensions3-flood-poc.git
cd dimensions3-flood-poc

python -m venv .venv
source .venv/bin/activate

pip install -r requirements.txt
```

On Windows, activate the environment with:

```bash
.venv\Scripts\activate
```

Then launch Jupyter:

```bash
jupyter lab
```

No API credentials are required for the committed reproducible sample.

## 7. How to Run

### Recommended Reviewer Workflow

Open:

```text
notebooks/02_main_analysis.ipynb
```

Run all cells from the beginning.

The notebook:

1. Loads the committed sample input.
2. Reconstructs the multi-source spatial evidence.
3. Adds rainfall and supporting evidence.
4. Reproduces the combined score.
5. Saves the headline visualization and summary.

Expected runtime is approximately 2–5 minutes on a standard laptop or Google Colab. No GPU is required.

The expected final output is:

```text
Screening label: MEDIUM
Combined evidence score: 0.574
Candidate area: 0.74%
Peak rainfall: 53.39 mm
Supporting evidence records: 9
```

The full development and data-collection workflow is also documented in:

```text
Dimensions3_FinalCode.ipynb
```

The full notebook accesses public satellite catalogues and may take longer because it searches, downloads and processes larger remote datasets.

## 8. Example Input and Output

### Example Input

The small reproducible input is committed at:

```text
data/sample_input/dimensions3_sample_input.npz
```

It contains processed and aligned sample masks derived from Sentinel-1, Sentinel-2 and Landsat 9, together with the rainfall and supporting-evidence values required to reproduce the headline result.

This sample avoids committing complete satellite scenes or restricted imagery.

### Example Output

The reproduced output is available at:

```text
results/reproducible_flood_evidence_result.png
```

![Dimensions³ reproduced flood-evidence result](results/reproducible_flood_evidence_result.png)

Additional figures in `results/` include:

- Sentinel-1 radar before/after and change analysis
- Sentinel-2 natural-colour comparison
- Landsat 9 MNDWI analysis
- Rainfall visualization
- Multi-source fusion result
- Supporting-evidence timeline
- Improved screening after permanent-water removal

The same evidence is presented interactively on the [Jebel Ali flood-evidence page](https://vfatimaalkaabi.github.io/dimensions3-flood-poc/jebel-ali-packet.html).

## 9. Validation and Limitations

### What Was Verified

- A fresh Google Colab run reproduced the `0.574` score and `MEDIUM` label.
- The committed sample input reproduces the result without downloading full satellite scenes.
- The three satellite masks use the same `374 × 341` analysis-array dimensions.
- High-agreement pixels require support from at least two sensors.
- Permanent-water pixels and isolated radar noise were reduced.
- Inputs, processing rules, weights and outputs are documented.
- The live website displays the reproduced values and analytical figures.

### Current Limitations

- No independent georeferenced flood-extent map was available as ground truth.
- Equal array dimensions do not prove exact geographic co-registration.
- The Sentinel-1 GRD measurements were used for preliminary change screening.
- Operational validation should use fully calibrated, terrain-corrected and georeferenced radar products.
- Radar shadows, wet soil, coastline effects, construction and image-registration differences can create false positives.
- Clouds and different acquisition dates can affect Sentinel-2 and Landsat comparisons.
- Rainfall and reports support the event context but do not identify exact flooded pixels.
- Public social-media content is used only as regional corroboration.
- Precision, recall, F1 score and Intersection over Union have not yet been measured.
- The combined score is a transparent heuristic screening score, not a trained-model probability.
- Some wider portal functions are illustrative prototype features.

The result should be used to prioritize inspection and further validation, not to declare confirmed flooded locations.

## 10. Impact and Next Steps

### Expected Impact

Dimensions³ can help organizations:

- Screen large areas faster after extreme rainfall
- Combine technical evidence in one understandable interface
- Prioritize roads, industrial sites and infrastructure for inspection
- Preserve traceable evidence for reporting and planning
- Make Earth Observation information accessible to non-specialist users

### Next Steps for Incubation

1. Process Sentinel-1 data to calibrated, terrain-corrected RTC products.
2. Perform exact geospatial co-registration instead of array-only resizing.
3. Validate results against authoritative flood maps or field observations.
4. Add elevation, slope, drainage and road-network information.
5. Analyse longer pre-event and post-event time series.
6. Calculate precision, recall, F1 score and IoU.
7. Train and validate CNN-based flood-segmentation models when labelled data are available.
8. Investigate suitable hyperspectral datasets.
9. Connect the notebook output to an operational web API.
10. Develop validated automated alerts and scalable processing for additional locations and hazards.

## Repository Structure

```text
dimensions3-flood-poc/
├── README.md
├── LICENSE
├── requirements.txt
├── Dimensions3_FinalCode.ipynb
├── index.html
├── analysis.html
├── timeline.html
├── city-map.html
├── alerts.html
├── report.html
├── jebel-ali-packet.html
├── support.js
├── notebooks/
│   └── 02_main_analysis.ipynb
├── data/
│   └── sample_input/
│       └── dimensions3_sample_input.npz
└── results/
    ├── reproducible_flood_evidence_result.png
    ├── sentinel1-radar.png
    ├── sentinel1-screening.png
    ├── sentinel2-rgb.png
    ├── landsat9-mndwi.png
    ├── rainfall.png
    ├── evidence-count.png
    ├── evidence-timeline.png
    ├── fusion.png
    └── improved-screening.png
```

## Presentation

The final Dimensions³ presentation presents the problem, business use case, datasets, methodology, actual analytical outputs, validation, limitations, impact and future incubation plan.

[Download the final presentation slides](docs/slides.pdf)

The presentation includes:

- Project title, team, theme and country
- Problem and affected users
- Business use case and target customers
- Sentinel-1, Sentinel-2, Landsat 9 and rainfall data
- Hyperspectral-data status and potential future use
- Multi-source flood-screening workflow
- Actual notebook outputs and portal screenshots
- Reproduced `MEDIUM` screening result and `0.574` score
- Validation and limitations
- Expected impact and next steps

## Demo Video

A 2–3 minute demonstration video has been included in the optional supporting archive attached to the official submission form.

The video demonstrates:

- The Dimensions³ web portal and user workflow
- Satellite-data packet creation
- Sentinel-1, Sentinel-2 and Landsat 9 analysis
- The Jebel Ali flood-evidence results
- The `MEDIUM` screening label and `0.574` combined score
- Rainfall and supporting evidence
- Proposed alerts, reporting and future platform capabilities

Some wider portal capabilities shown in the demonstration are illustrative prototype features. The reproducible Jebel Ali result is generated by the committed notebook and sample data.

## Team Members and Roles

- **Alia Alblooshi — Website and integration:** Developed the platform interface and connected the analytical outputs to the website.
- **Fatima Alkaabi — Data pipeline and analysis:** Collected and processed the satellite, rainfall and supporting evidence; developed the notebook and multi-source screening workflow.
- **Wadha Alkaabi — Satellite imagery:** Supported Sentinel-1, Sentinel-2 and Landsat data preparation and interpretation.
- **Fatima Alnuaimi — Evidence and validation:** Reviewed weather information, event references, analytical results and project limitations.
- **Mariam Alneyadi — Reproducibility and presentation:** Prepared repository documentation, tested the reviewer workflow and developed the submission presentation.

The team collaborated on the project concept, workflow design, interpretation, validation, platform development and final submission.

## Licence

The project code and original documentation are released under the licence provided in the repository’s `LICENSE` file.

Satellite imagery, weather information, reports and public evidence remain subject to their original providers’ terms and licences. No ownership of third-party data is claimed.

## Attribution

This project uses data and services from:

- European Space Agency
- Copernicus Programme
- Copernicus Data Space Ecosystem
- Microsoft Planetary Computer
- United States Geological Survey
- NASA POWER
- OpenStreetMap contributors
- Original publishers of cited official, news and public evidence

## Responsible Use

Dimensions³ is a proof-of-concept screening system. Its outputs are intended to support investigation, inspection prioritization and research.

They should not be used as the sole basis for emergency declarations, public warnings, insurance decisions or operational response without authoritative validation.
