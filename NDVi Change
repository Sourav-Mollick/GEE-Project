/**** Start of imports. If edited, may not auto-convert in the playground. ****/
var roi = 
    /* color: #d63000 */
    /* shown: false */
    /* displayProperties: [
      {
        "type": "rectangle"
      }
    ] */
    ee.Geometry.Polygon(
        [[[90.45792361255951, 23.762733284161076],
          [90.45792361255951, 21.950698390179234],
          [91.64444705005951, 21.950698390179234],
          [91.64444705005951, 23.762733284161076]]], null, false),
    imageCollection = ee.ImageCollection("LANDSAT/LC08/C02/T1_TOA");
/***** End of imports. If edited, may not auto-convert in the playground. *****/

Map.centerObject(roi);

var time_start = '2023-12', time_end = '2024-11'

var before = ee.ImageCollection("LANDSAT/LC08/C02/T1_L2")
.select('SR.*')
.filterDate(time_start, time_end)
.filterBounds(roi)
.filter(ee.Filter.calendarRange(2,2,'month'))
.median().multiply(0.0000275).add(-0.2);

var ndvi_before = before.normalizedDifference(['SR_B5','SR_B4'])


Map.addLayer(ndvi_before.clip(roi),[],'before', false);

var after = ee.ImageCollection("LANDSAT/LC08/C02/T1_L2")
.select('SR.*')
.filterDate(time_start, time_end)
.filterBounds(roi)
.filter(ee.Filter.calendarRange(4,4,'month'))
.median().multiply(0.0000275).add(-0.2);

var ndvi_after = after.normalizedDifference(['SR_B5','SR_B4']);

Map.addLayer(ndvi_after.clip(roi), [], 'ndvi_after', false )


var ndvi_change = ndvi_before.subtract(ndvi_after);

Map.addLayer(ndvi_change.clip(roi),[],'ndvi_change', false)

var change_percent = ((ndvi_after.subtract(ndvi_before)).divide(ndvi_before)).multiply(100)
var change_thr = change_percent.lt(0);
var change_mask = change_percent.updateMask(change_thr);

Map.addLayer(change_mask.clip(roi),[],'change_percent', false)