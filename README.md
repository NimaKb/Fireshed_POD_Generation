# Fireshed Characterization and POD Development

## Project Overview
This project aims to develop a comprehensive fireshed characterization and Potential Operational Delineations (PODs) for wildfire risk management in the Cariboo Region. Firesheds are spatial units that delineate areas where wildfires are likely to ignite and spread, potentially impacting values such as communities, habitats, tourism areas, and infrastructure. PODs are operational units designed for pre-planning wildfire suppression and prevention activities, incorporating local fire knowledge with advanced spatial analytics.

## Key Objectives
1. Characterize firesheds and calculate cumulative fire risk scores using multicriteria decision analysis (MCDA).
2. Develop POD boundaries based on critical control features and integrate key data layers.
3. Calibrate fireshed scoring using wildfire simulation data and historical burn counts.
4. Generate and refine PODs by integrating ecological, topographical, and anthropogenic data.
5. Develop scripts for cumulative score calibration with burn count and for generating HTML files for refined POD boundaries.

## Methods and Approach
### Fireshed Characterization
#### Data Compilation
Many data sources were gathered to characterize the fire landscape in the Cariboo Region. These include fire behavior data, fuel distribution and disturbance data, terrain attributes, and climate data. The detailed list of datasets used is provided in Appendix 1: Data Sets.

#### Multicriteria Decision Analysis (MCDA)
Since multiple factors must be considered to identify and score firesheds, a Multicriteria Decision Analysis (MCDA) was implemented. The key goal of MCDA is to standardize all factors within a consistent range (fuzzy membership values from 0 to 1), enabling cumulative aggregation and generating a comprehensive fireshed score. The cumulative score reflects the relative influence of each factor on wildfire spread, intensity, and overall threat.

##### Scoring System
The scoring system uses fuzzy membership scaling to normalize layer values between 0 and 1. Each layer contributes to the cumulative score, with weights assigned equally to the major categories:
- **Fire Behavior and Threat Analysis:** Includes Fire Threat Rating and Historical Fire data.
- **Fuel Distribution and Disturbance:** Comprises FBP Fuel Type, BEC Natural Disturbance, Fuel Treatments, and Human Disturbance.
- **Terrain:** Includes Slope, Elevation, and Aspect.
- **Climate and Human Fire Influence:** Covers Drought Levels, PSTA Lightning Fire Density, and PSTA Human Fire Density.

For example:
- **Fire Threat Rating:** Linear increasing values from 0 (lowest) to 1 (highest).
- **Historical Fires:** Values based on fire year and the likelihood of reburn, ranging from 0.2 for recent fires to 1.0 for fully regrown areas.
- **Fuel Distribution:** Different vegetation types and disturbances (e.g., coniferous forests, mixedwood, deciduous, and grasslands) are assigned scores based on fire potential.
- **Terrain Attributes:** Slope, aspect, and elevation are evaluated to represent fire spread tendencies.
- **Climate and Human Influence:** Includes drought levels, lightning fire density, and human fire density, each scored based on recorded severity and frequency.

##### Cumulative Scoring
Each of the four major categories is weighted equally (0.25 each), and within each category, individual layers are weighted equally. The final cumulative score for each fireshed ranges from 0 to 1. Scores are calculated using the following formula:

```
Cumulative Score = 0.25 * (Fire Behavior + Fuel Distribution + Terrain + Climate)
```

### Calibration and Validation
Calibration of fireshed scoring was conducted using historical wildfire data, simulation outputs from Forsite projects, and burn count analysis. Calibration areas include Barkerville, Fox Creek, and Yunesit’in. The relationship between burn count and cumulative score was assessed, with findings indicating that higher burn counts correspond to higher cumulative scores, reinforcing the reliability of the scoring model.

Scripts for cumulative score calibration with burn count are available in the repository.

### POD Development
#### POD Generation Approach
Potential Operational Delineations (PODs) are spatial units created for pre-planning wildfire prevention and suppression. Initial POD boundaries were delineated using key geographic features such as:
- **Railways**
- **Road networks**
- **Landscape units**
- **Critical corridors**

The Multipart to Single Part tool was used to separate individual polygons, followed by iterative merging of small polygons (less than 1000 ha) using the Eliminate tool, especially when they did not touch critical corridors. Manual adjustments ensured the integrity of intersections and prevented overlaps.
In addtion, water bodies from BC catalogue (WSA-Water Polygon Feature (50,000), can be found here: https://catalogue.data.gov.bc.ca/dataset/wsa-water-polygon-features-50-000), erased from the AOI as their boundaries can function as POD boundaries as well.

#### POD Boundary Refinement
After the initial generation, the POD boundaries were further refined in collaboration with the BC Wildfire Service (BCWS). Additional factors considered during the refinement process included:
- **Wildland-Urban Interface (WUI) Areas:** Smaller POD sizes were created near populated areas (e.g., proximity to Quesnel) to enhance precision and operational planning.
- **Treatment Areas:** POD boundaries were aligned with vegetation treatment zones to incorporate fire mitigation efforts effectively.
- **Presence of Interests:** Adjustments were made to account for areas with significant social, economic, or ecological value.

#### HTML Map Conversion
To enhance accessibility for BCWS staff and stakeholders, the refined POD boundaries for the Quesnel area were converted into an HTML map format. This interactive map allows users to view and assess POD boundaries without the need for specialized GIS software, promoting more efficient decision-making and collaboration.

Scripts for generating HTML files from POD boundaries are available in the repository.

### Integration and Refinement
To enhance delineation accuracy, water bodies from the BC Catalogue were erased from the area of interest (AOI), as they serve as natural POD boundaries. The resulting smaller polygons were merged to form cohesive PODs. Additional attributes were calculated using Spatial Join and Zonal Statistics to integrate the following factors:
- **Fire Behavior**
- **Fuel Distribution and Disturbance**
- **Terrain Attributes**
- **Climate and Anthropogenic Influence**
- **BEC Zones**
- **WUI and First Nations Reserve Lands**

#### Final Validation and Stakeholder Input
After generating the draft POD boundaries, stakeholder validation workshops were conducted to refine and confirm the accuracy of delineations. Local fire knowledge and experience were incorporated to enhance the reliability and practicality of POD boundaries.

## Data Sources
Key datasets include:
- **Fire Threat Rating (BC Wildfire Service)**
- **Historical Fire Data (Forsite Fire)**
- **FBP Fuel Type and Vegetation Layers (BEC classifications)**
- **Climate Data (Drought Levels and Fire Density)**
- **Terrain Data (Slope, Aspect, Elevation)**
- **Human Impact Data (Human Fire Density and Disturbance)**

## Results and Implications
The cumulative fireshed score demonstrated strong predictive power, as areas with higher burn counts consistently correlated with higher cumulative scores. The delineation of PODs enhanced operational planning by integrating local fire management expertise and spatial analysis, aiding decision-making for wildfire prevention and response. The HTML map of refined POD boundaries significantly increased accessibility and usability for BCWS staff and collaborators.


For further questions or collaboration, contact Nima Karimi at nima.karimi@forsite.ca.
