🗿 Discovering Hidden Archaeological Sites in the Amazon
A solo project for the OpenAI Z-Challenge | 2025

![Screenshot 2025-06-14 005324](https://github.com/user-attachments/assets/a9fe77aa-428c-45a6-9d18-a5c999eceabf)


🧠 Abstract
This project tackles the Z-Challenge prompt to discover previously undocumented archaeological sites across the Amazon biome. Through the use of satellite-derived environmental data, historical context, and machine learning, I designed and implemented a full-stack geospatial analysis pipeline to surface new site predictions.

Using only open-source datasets and Earth Engine processing, I constructed a reproducible feature-engineering workflow. This was paired with a Random Forest model trained on curated positive (archaeological sites) and negative (background) examples. Predictions were further validated through interpretive GPT-4 prompts grounded in colonial-era texts and vegetation descriptions.

🎯 Strategy Summary
The core hypothesis behind this project is:

“Unknown archaeological sites are more likely to appear in geographic and ecological conditions similar to known ones.”

The overall strategy involved:

📍 Collecting known site data from archaeological surveys

📡 Extracting environmental predictors (climate, elevation, soil) from Earth Engine

🗺️ Engineering spatial features (distance to river, elevation) locally

🤖 Training a classifier using positive (known) and negative (random Amazonian) points

🔮 Predicting likelihood scores across a 0.05° Amazonian grid

📚 Validating top predictions using historical textual clues interpreted via GPT-4

📁 Data Files Overview
Here’s an explanation of each major file and how it was created:

features_step1.csv
📍 Purpose: Adds spatial features for known archaeological points.
How it was made:

Extracted known sites from selected KML layers:
geoglyphs (#2), mound sites, small geoglyphs, Xingu area lidar located earthworks

Computed distance to river using the HydroRIVERS shapefile + BallTree nearest-neighbor search

Extracted elevation using a local dem_90m.tif with rasterio

🧠 Why: Terrain + proximity to rivers are strongly predictive of pre-Columbian settlements.

site_features_with_climate_fixed.csv
📡 Purpose: Adds Earth Engine–derived climate/soil predictors
How it was made:

Sampled Earth Engine rasters for:

bio04: Temperature Seasonality

bio12: Annual Precipitation

soil_ph: Topsoil pH

elevation_ee: Elevation

Converted .geo → lat/lon in Python

![Screenshot 2025-06-13 233139](https://github.com/user-attachments/assets/1cbce0c3-a04d-4ff2-b80a-bdfbfcc3d05a)


🧠 Why: Climate and soil patterns directly affect vegetation, agriculture, and human settlement viability.

background_points_amazon.csv
🟡 Purpose: Balanced negative training samples
How it was made:

Random points generated within Amazon biome

Sampled Earth Engine variables same as sites

Assigned label 0 to denote non-sites

🧠 Why: Needed for training an ML model that learns what not to classify as a site.

prediction_grid_with_features.csv (intermediate)
🗺️ Purpose: Predict site likelihood across entire Amazon region
How it was made:

0.05° prediction grid created using NumPy

Sampled climate and soil layers via Earth Engine

Estimated dist_to_river_km using training set median

Passed all features into RandomForestClassifier.predict_proba()

🧠 Why: This grid allowed visual identification of new likely site clusters.

🤖 GPT-4 Interpretive Layer
After scoring ~500 grid cells using the trained model, top predictions were run through GPT-4 with historical prompts to narrow down final picks.

📜 Prompt Example:

“Read this excerpt and extract references to rivers, direction, vegetation, and distance to settlements.”

![Screenshot 2025-06-14 002143](https://github.com/user-attachments/assets/fc8f19fa-d1c5-4a4d-ab99-e774448fd5aa)

📌 Top Recommendations:

Madeira & Purús Rivers Zone

Historically rich rubber trails

Still underexplored archaeologically

Guaporé / Southern Mato Grosso Region

Early bandeirante expeditions and gold mines

Moderate elevation, rich soil, ≤5km to river

🧠 Final sites were selected by combining:

High predicted ML probability

GPT-confirmed historical relevance

🧪 ML Modeling Details
Model: RandomForestClassifier

200 trees, max_depth=10

Input features:

dist_to_river_km

elevation, elevation_ee

bio04, bio12

soil_ph

✅ ROC AUC: 1.00 on a balanced validation set

📊 Visuals:

Confusion Matrix

ROC Curve

Feature Importance Plot

Libraries used: scikit-learn, seaborn, matplotlib

🗺️ Visualization
Maps and terrain overlays were key for interpreting model predictions:

Folium for interactive exploration

Matplotlib for static comparison (rivers, sites, terrain)

Tile Sources Used:

"CartoDB Positron": minimalist

"Stamen Terrain": elevation + relief

DEM: 90m resolution from SRTM, shaded with LightSource

📚 References & Acknowledgments
This solo project was inspired and powered by several key open datasets and academic publications:

Walker et al. (2023): Predicting Amazonian Sites with ML

Iriarte et al. (2020): Geometry by Design

Peripato et al. (2023): 10,000 Earthworks

Khan et al. (2017): Lidar UAV Mapping

Remote Sensing & Geodata:

🌍 WorldClim Bioclimatic Variables

🌱 OpenLandMap Soil Database

🌊 HydroSHEDS River Network

🛰️ OpenTopography SRTM

🔍 Final Thoughts
This solo project demonstrates that:

📡 Remote sensing + Open Earth data

🧠 Machine learning + Text interpretation

🌎 Spatial tools + Historical archives

...can collectively uncover the past — and perhaps inspire future archaeological discovery.
