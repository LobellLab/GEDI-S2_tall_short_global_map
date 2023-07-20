# Annual field-scale maps of tall and short crops at the global scale using GEDI and Sentinel-2

This repo contains code that walks through key steps to create and validate the GEDI-S2 Tall/Short crop Global maps.
These maps classify cropland as either a tall or short crop at global scale from 2019 to 2021 at 10m resolution.

An interecative map is available as [[Earth Engine App](https://stefania.users.earthengine.app/view/gedi-s2tallshortcropmap)]

<p align="center"><img src="https://github.com/LobellLab/GEDI-S2_tall_short_global_map/blob/main/fig_validation.png"></p>



## Dataset

The dataset can be accessed through Google Earth Engine assets:

- 2019 imageCollection [here](https://code.earthengine.google.com/?asset=projects/lobell-lab/gedi_global_folder/predictionsS2_3Mcombined/mapGEDIS2_2019)
- 2020 imageCollection [here](https://code.earthengine.google.com/?asset=projects/lobell-lab/gedi_global_folder/predictionsS2_3Mcombined/mapGEDIS2_2020)
- 2021 imageCollection [here](https://code.earthengine.google.com/?asset=projects/lobell-lab/gedi_global_folder/predictionsS2_3Mcombined/mapGEDIS2_2021)

Each image in the collection has 2 bands:
- classification: 0 = short, 1 = tall
- GCVI_max: grenness index - expressed as maximum GCVI in the 3-month time window. (Scale factor = 10000)

To identify grid-cells with low quality prediction we have falgged them as follow:
- accuracy flag: based on test-Accuracy. Flagged if Accuracy < 0.85.
- coverage flag: based on available months for classification. Flagged if classification is based on one month only - and that month is not the Optimal Month.

GEE code to visualize the imageCollections and to flag the low quality data is provided in this [[Earth Engine script](https://code.earthengine.google.com/f3b5f3241e0dae0a7af639dd4dbb296f)]


## Map creation

1. Train and apply the GEDI model. This model used the GEDI features to classify GEDI locatios as having short crops, tall crops, or trees. [[Earth Engine script](https://code.earthengine.google.com/4a7ad3a9bb72a59e576c4e425a62708a))]
2. Generate 5x5 degree grid-cells [[Earth Engine script](https://code.earthengine.google.com/6d2be67fafd21394db7d6bb9c0b58331)]
3. Determine Optimal Month [[Earth Engine script](https://code.earthengine.google.com/1a732e3c5a14f2b2cd3ce0ec1608ac00)]
4. Export Sentinel-2 harmonics features: [[Earth Engine script](https://code.earthengine.google.com/33365205a751bf1507885c400a1cf4cc)]
5. Train a local GEDI-S2 model for each gridcell based on GEDI predictions in the 3-month time window around the Optimal Month. [[Earth Engine script](https://code.earthengine.google.com/aa975f7a37c52ae87f07b431244f4744)]
6. Mosaic GEDI-S2 predictions into final maps [[Earth Engine script](https://code.earthengine.google.com/f43c776ab4909e122d7de56740439727)]

<!---               
## Map validation and error analysis
1. Export predictins at field locations or random sample for validation analysis ([15.10_validation_atRandomLoc_US])
2. Python Jupyter NOtebook code for running Validation
3. Low Vegetation Index analysis in Canada ([VI_analyis_Canada]), Kenya(VI_analyis_Kenya_SR2021) and malawi(VI_analyis_Malawi)
4. GEDI orbital resonance visualization 2019-2020-2021 (fig02_gediShotsOverFields)
5. Export View Angle information in Time ([00.01_exploreGEDIinGEE_L2B_export])
6. [Jupyter notebook] for View angle analysis in US
-->
