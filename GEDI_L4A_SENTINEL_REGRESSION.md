```Javascript

// VISUALISATIE VAN GEDI LEVEL-4A BIOMASSA DATA

var GEDI_L4A = ee.ImageCollection("LARSE/GEDI/GEDI04_A_002_MONTHLY")
var   ROI = ee.Geometry.Polygon(
        [[[-56.914434661033155, 6.04954064439789],
          [-56.914434661033155, 5.7052910346699734],
          [-55.804815520408155, 5.7052910346699734],
          [-55.804815520408155, 6.04954064439789]]], null, false);

// 1. GEDI filteren op bounds en tijd

var GEDI_L4A = GEDI_L4A.filterBounds(ROI)
                        .filterDate('2020-01-01','2021-12-31')

// 2. Quality filter
var qualityMask = function(im) {
  return im.updateMask(im.select('l4_quality_flag').eq(1))
      .updateMask(im.select('degrade_flag').eq(0));
};

var GEDI_L4A =  GEDI_L4A.map(qualityMask)

// Aggregeren van GEDI_L4A data:
var GEDI_L4A = GEDI_L4A.mean()

// 3. Sentinel-beeld toevoegen
// Inladen van een vooraf aangemaakt Sentinel-1/2 beeld (dit kun je vervangen door je eigen beeld)
var Sentinel_2021 = ee.Image('projects/ee-mangroves-suriname/assets/Sentinel_2021')

// Visualisatie van een RGB-beeld:
Map.addLayer(Sentinel_2021,{min:0, max:0.3, bands: ['B4','B3','B2']},'Sentinel 2021')

// 4. rasterpixels omzetten naar punten
var GEDI_L4A = GEDI_L4A.select('agbd').sample({
  region : ROI,
  geometries : true,
  scale: 25,
  tileScale: 16
})

// 4. GEDI-shots reduceren met random kolom
var GEDI_L4A =  GEDI_L4A.randomColumn('random')
var GEDI_L4A = GEDI_L4A.filter(ee.Filter.lt('random',0.25))

print('Gereduceerde GEDI_L4A set: ', GEDI_L4A.size())

// 5. Sentinel-GEDI dataset aanmaken
var GEDI_S2 = Sentinel_2021.sampleRegions({
  collection: GEDI_L4A,
  properties: ['agbd'],
  scale: 25 ,
  geometries : true,
})

//6. Regressiemodel aanmaken
// Random Forest Regressiemodel:
var bands = Sentinel_2021.bandNames()
print('bands : ', bands)

// Build Random Forest regressor
var classifier = ee.Classifier.smileRandomForest(100, null, 1, 0.5, null, 0)
  .setOutputMode('REGRESSION')
  .train({
    features: GEDI_S2,
    classProperty: 'agbd',
    inputProperties: bands
    });

var regression = Sentinel_2021.classify(classifier, 'predicted');

Map.addLayer(regression, {min:0,max:300}, 'AboveGround Biomass Density (Mg/ha)')
```