🗿 Discovering Hidden Archaeological Sites in the Amazon
A solo project for the OpenAI Z-Challenge | 2025
![Screenshot 2025-06-14 104140](https://github.com/user-attachments/assets/74520313-9e27-4079-aee9-5327c1851afb)
![Screenshot 2025-06-14 104029](https://github.com/user-attachments/assets/ca96ab13-ae31-4396-8f20-f345c83ace97)


🧠 Abstract
This project tackles the Z-Challenge prompt to discover previously undocumented archaeological sites across the Amazon biome. Through the use of satellite-derived environmental data, historical context, and machine learning, I designed and implemented a full-stack geospatial analysis pipeline to surface new site predictions.

Using only open-source datasets and Earth Engine processing, I constructed a reproducible feature-engineering workflow. This was paired with a Random Forest model trained on curated positive (archaeological sites) and negative (background) examples. Predictions were further validated through interpretive GPT-4 prompts grounded in colonial-era texts and vegetation descriptions.

🎯 Strategy Summary
The core hypothesis behind this project is:

“Unknown archaeological sites are more likely to appear in geographic and ecological conditions similar to known ones.”

The overall strategy involved:

Collecting known site data from archaeological surveys

Extracting environmental predictors (climate, elevation, soil) from Earth Engine

Engineering spatial features (distance to river, elevation) locally

Training a classifier using positive (known) and negative (random Amazonian) points

Predicting likelihood scores across a 0.05° Amazonian grid

Validating top predictions using historical textual clues interpreted via GPT-4

📁 Data Files Overview
Here is an explanation of each major CSV/GeoJSON file used in the project and how they were created:

1. features_step1.csv
📍 Purpose: Adds spatial features for known archaeological points.

How it was made:

Extracted known sites from selected KML layers:

geoglyphs (#2), mound sites, small geoglyphs, Xingu area lidar located earthworks.

Computed distance to river using the HydroRIVERS shapefile + BallTree nearest-neighbor algorithm.

Extracted elevation values from a local dem_90m.tif using rasterio.

🧠 Why:
These spatial features capture terrain placement and access to water, which are highly predictive of pre-Columbian settlement choices.

2. site_features_with_climate_fixed.csv
📡 Purpose: Adds Earth Engine-derived environmental predictors to known sites.

How it was made:

Used Earth Engine to sample environmental variables at site coordinates:

bio04: temperature seasonality

bio12: annual precipitation

soil_ph: topsoil pH

elevation_ee: GEE elevation band

Exported with .geo column → parsed into latitude/longitude using Python.

🧠 Why:
Adds ecological constraints — vegetation, soil fertility, and elevation — known to influence past Amazonian habitation patterns.

3. background_points_amazon.csv
🟡 Purpose: Provides balanced negative samples for ML training.

How it was made:

Random points were generated inside the Amazon biome (excluding protected areas or known site buffers).

Same Earth Engine export method as sites was used to sample predictors.

Label 0 assigned to all background points.

🧠 Why:
Provides the Random Forest with “what is not a site” — essential for building a strong classifier.

4. prediction_grid_with_features.csv (intermediate)
🗺️ Purpose: Scores each 0.05° grid cell in the Amazon with probability of archaeological interest.

How it was made:

Created lat/lon grid using numpy over EE rectangle bounding box.

Exported EE climate and soil variables for each grid point.

Estimated missing dist_to_river_km using median from training set (to avoid re-running spatial joins).

Passed all features to predict_proba() using trained model.

🧠 Why:
This is the predictive layer — every cell gets a likelihood score, making it possible to surface top candidates.

🧠 GPT-4 Interpretation
After ML scoring, top ~50 high-confidence predictions were passed into GPT-4 using prompts inspired by colonial-era texts. Example prompt:

"Read this excerpt and extract references to rivers, direction, vegetation, and distance to settlements."
![Screenshot 2025-06-14 002143](https://github.com/user-attachments/assets/6ec0eab5-cc0d-4a72-9be2-d4d3776bf27e)


The GPT model suggested prioritizing zones:

Near the Madeira and Purús rivers, due to rubber-era trails.

Around Guaporé and Mato Grosso, based on gold-rush expeditions.

📌 Final shortlisted coordinates reflect both:

High ML probability

GPT alignment with geographic-historical clues

🧪 ML Modeling Details
Algorithm: RandomForestClassifier (200 estimators, max_depth=10)

Features used:

dist_to_river_km

elevation

bio04, bio12

soil_ph

Evaluation: ROC AUC = 1.00 (balanced test set of 59 samples)

Tools: scikit-learn, matplotlib, seaborn

📊 Visuals:

Confusion matrix, ROC curve

Feature importance plot

🗺️ Visualization
Maps were generated using both:

📌 Folium for interactive zoomed-in display

🖼️ Matplotlib for static site-vs-river plots and DEM overlays

Tiles used: "CartoDB Positron", "Stamen Terrain"
DEM: dem_90m.tif shaded relief added via LightSource.

📚 References & Acknowledgments
This project builds upon publicly available academic work:

Walker et al. (2023): Predicting Amazonian Sites with ML

Iriarte et al. (2020): Geometry by Design

Peripato et al. (2023): 10,000 Pre-Columbian Earthworks

Khan et al. (2017): Lidar & UAV Mapping in the Amazon

OpenLandMap Soil Database

WorldClim Bioclimatic Variables

HydroSHEDS River Network

OpenTopography SRTM

🔍 Final Takeaways
This solo project demonstrates how:

Remote sensing + open data

Machine learning + historical context

Geospatial tools + AI models (GPT)

...can collectively guide real-world archaeological discovery.

🧠 Future extensions:

Apply CNNs to Sentinel-2 surface textures

Extract trails using slope + canopy dips

Partner with local archaeologists to validate predictions
