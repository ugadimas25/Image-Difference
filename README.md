# Image-Difference
Image Difference menggunakan data landsat

```javascript

// 05 IMAGE DIFFERENCE

// Pusat peta
Map.setCenter(108.85, -7.68, 12); // Segara Anakan, Cilacap
//Map.setCenter(108.7752969,-6.8054676,12); // Tawangsari, Cirebon

// Muat dua komposit Landsat 7 5-tahunan.
var landsat1999 = ee.Image('LANDSAT/LE7_TOA_5YEAR/1999_2003');
var landsat2008 = ee.Image('LANDSAT/LE7_TOA_5YEAR/2008_2012');

// Hitung NDVI dengan cara yang sulit.
var ndvi1999 = landsat1999.select('B4').subtract(landsat1999.select('B3'))
    .divide(landsat1999.select('B4').add(landsat1999.select('B3')));

// Hitung NDVI dengan cara mudah.
var ndvi2008 = landsat2008.normalizedDifference(['B4', 'B3']);

Map.addLayer(ndvi1999, '', 'ndvi1999');
Map.addLayer(ndvi2008, '', 'ndvi2008');

// Hitung citra perbedaan multi-band.
var diff = landsat2008.subtract(landsat1999);
Map.addLayer(diff, { bands: ['B4', 'B3', 'B2'], min: -32, max: 32 },
    'difference');

// Hitung perbedaan kuadrat di setiap band.
var squaredDifference = diff.pow(2);
Map.addLayer(squaredDifference, { bands: ['B4', 'B3', 'B2'], max: 1000 },
    'squared diff.');
