\title{
Introduction to the dataset
}

\author{
WORKING WITH GEOSPATIAL DATA IN PYTHON
}

Joris Van den Bossche

Open source software developer and

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-01.jpg?height=304&width=304&top_left_y=1340&top_left_x=1959)
teacher, GeoPandas maintainer

\section*{Artisanal mining site data from IPIS}

IPIS: International Peace Information Service

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-02.jpg?height=1253&width=1238&top_left_y=513&top_left_x=1508)

Image: Connormah, CC BY-SA 3.0, from Wikimedia Commons

\section*{Artisanal mining site data from IPIS}

IPIS: International Peace Information Service

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-03.jpg?height=1421&width=1281&top_left_y=440&top_left_x=1492)

Image: G.A.O, public domain, from Wikimedia Commons

\section*{Artisanal mining site data from IPIS}

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-04.jpg?height=1444&width=2813&top_left_y=293&top_left_x=715)

More analysis (re. social \& security)

\section*{Geospatial file formats}

Reading files: geopandas.read_file("path/to/file.geojson")

Supported formats:

- ESRI Shapefile
- One "file" consists of multiple files! ( .shp , .dbf, .shx , .prj , ...)

- GeoJSON

- GeoPackage ( .gpkg )
- ...

\(\&\) PostGIS databases!

\section*{Writing to geospatial file formats}

Writing a GeoDataFrame to a file with the to_file() method:

\# Writing a Shapefile file

geodataframe.to_file("mydata.shp", driver='ESRI Shapefile')

\# Writing a GeoJSON file

geodataframe.to_file("mydata.geojson", driver='GeoJSON')

\# Writing a GeoPackage file

geodataframe.to_file("mydata.gpkg", driver='GPKG')

\section*{Let's practice!}

WORKING WITH GEOSPATIAL DATA IN PYTHON

\title{
Additional spatial operations
}

\author{
WORKING WITH GEOSPATIAL DATA IN PYTHON
}

Joris Van den Bossche

Open source software developer and

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-08.jpg?height=309&width=331&top_left_y=1332&top_left_x=1956)
teacher, GeoPandas maintainer

\section*{Overview of spatial operations}

Spatial relationships:

- intersects

- within

- contains

Join attributes based on spatial relation:

- geopandas.sjoin
Geometry operations:

- intersection

- union

- difference

- ...

Combine datasets based on geometry operation:

- geopandas.overlay

\section*{Unary union}

Convert a series of geometries to a single union geometry

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-10.jpg?height=1362&width=1151&top_left_y=600&top_left_x=808)

(GeoSeries of aeometries)

\section*{Unary union}

Convert a series of geometries to a single union geometry:

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-11.jpg?height=1378&width=1210&top_left_y=602&top_left_x=784)

(GeoSeries of aeometries)

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-11.jpg?height=1227&width=1151&top_left_y=602&top_left_x=2284)

africa.unary_union

l-inaln nonmotmu

\section*{Buffer operation}

point

\section*{Buffer operation}

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-13.jpg?height=1427&width=1227&top_left_y=545&top_left_x=1546)

\section*{Buffer operation}

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-14.jpg?height=955&width=1231&top_left_y=483&top_left_x=513)

line.buffer(distance)

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-14.jpg?height=1118&width=1394&top_left_y=477&top_left_x=2461)

WORKING WITH GEOSPATIAL DATA IN PYTHON

\section*{Let's practice!}

WORKING WITH GEOSPATIAL DATA IN PYTHON

\section*{Applying custom spatial operations}

WORKING WITH GEOSPATIAL DATA IN PYTHON

Joris Van den Bossche

Open source software developer and

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-16.jpg?height=309&width=337&top_left_y=1332&top_left_x=1953)
teacher, GeoPandas maintainer

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-17.jpg?height=1959&width=2696&top_left_y=84&top_left_x=779)

\section*{Total river length within 50 km of each city?}

```
For a single point ( cairo ):
area = cairo.buffer(50000)
rivers_within_area = rivers.intersection(area)
print(rivers_within_area.length.sum() / 1000)
```

186.397219642

\section*{The apply() method}

Series.apply() : call a function on each of the values of the Series

\section*{Series.apply(function, **kwargs)}

- function : the function being called on each value; the value is passed as the first argument

- **kwargs : additional arguments passed to the function

For a GeoSeries, the function is called as function(geom, **kwargs) for each geom in the GeoSeries

\section*{Applying a custom spatial operation}

The function to apply:

```
def river_length(geom, rivers):
    area = geom.buffer(50000)
    rivers_within_area = rivers.intersection(area)
    return rivers_within_area.length.sum() / 1000
```

Call function on the single geometry:

river_length(cairo, rivers=rivers)

186.3972196423455

\section*{Applying a custom spatial operation}

Applying on all cities:

cities.geometry.apply(river_length, rivers=rivers)

\(\begin{array}{lr}0 & 0.000000 \\ 1 & 0.000000 \\ 2 & 106.072198\end{array}\)

\section*{Applying a custom spatial operation}

Applying on all cities and assigning result to new column:

```
cities['river_length'] = cities.geometry.apply(river_length, rivers=rivers)
cities.head()
```

name

geometry river_length

0 Vatican City

POINT (1386304.6 5146502.5)

0.000000

1 San Marino

POINT (1385011.5 5455558.1)

0.000000

2

Vaduz

POINT (1059390.7 5963928.5) 106.072198

\section*{Let's practice!}

WORKING WITH GEOSPATIAL DATA IN PYTHON

\section*{Working with raster data}

WORKING WITH GEOSPATIAL DATA IN PYTHON

Joris Van den Bossche

Open source software developer and

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-24.jpg?height=320&width=309&top_left_y=1327&top_left_x=1962)
teacher, GeoPandas maintainer

Raster

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-25.jpg?height=1563&width=1595&top_left_y=255&top_left_x=1286)

Image source: QGIS documentation

\section*{Raster data}
![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-26.jpg?height=1504&width=3982&top_left_y=300&top_left_x=109)

\section*{Raster data with multiple bands}

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-27.jpg?height=1243&width=2594&top_left_y=507&top_left_x=808)

\section*{The rasterio package}

import rasterio

- "Pythonic" bindings to GDAL

- Reading and writing raster files

- Processing tools (masking, reprojection, resampling, ..)

https://rasterio.readthedocs.io/en/latest/

\section*{Opening a raster file}

```
import rasterio
src = rasterio.open("DEM_world.tif")
Metadata:
```

src.count

src.width, src.height

1

\((4320,2160)\)

\section*{Raster data \(=\) numpy array}

```
array = src.read()
Standard numpy array:
array
array([[[-4290, -4290, -4290, ..., -4290, -4290, -4290],
    [-4278, -4278, -4278, ..., -4278, -4278, -4278],
    [-4269, -4269, -4269, ..., -4269, -4269, -4269],
    [ 2804, 2804, 2804, ..., 2804, 2804, 2804],
    [ 2804, 2804, 2804, ..., 2804, 2804, 2804],
    [ 2804, 2804, 2804, ..., 2804, 2804, 2804]]], dtype=int16)
```

\section*{Plotting a raster dataset}

Using the rasterio.plot.show() method:

import rasterio.plot

rasterio.plot.show(src, cmap='terrain')

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-31.jpg?height=819&width=1584&top_left_y=1110&top_left_x=1324)

\section*{Extracting information based on vector data}

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-32.jpg?height=1221&width=2360&top_left_y=355&top_left_x=947)

rasterstats : Summary statistics of geospatial raster datasets based on vector geometries (https://github.com/perrygeo/python-rasterstats)

\section*{Extract raster values with rasterstats}

- For point vectors:

\[
\begin{array}{r}
\text { rasterstats.point_query(geometries, "path/to/raster", } \\
\text { interpolation='nearest'|'bilinear') }
\end{array}
\]

- For polygon vectors:

rasterstats.zonal_stats(geometries, "path/to/raster",

\[
\text { stats=['min', 'mean', 'max']) }
\]

\section*{Extract raster values with rasterstats}

```
result = rasterstats.zonal_stats(countries.geometry, "DEM_gworld.tif",
    stats=['mean'])
countries['mean_elevation'] = pd.DataFrame(result)
countries.sort_values('mean_elevation', ascending=False).head()
```

\begin{tabular}{lrrrrr}
\hline & name & continent & & geometry & mean_elevation \\
157 & Tajikistan & Asia & POLYGON ((74.98 37.41, . . & 3103.231105 \\
85 & Kyrgyzstan & Asia & POLYGON \(((80.2542 .34, \ldots\) & 2867.717142 \\
24 & Bhutan & Asia & POLYGON \(((91.6927 .77, \ldots\) & 2573.559846 \\
119 & Nepal & Asia & POLYGON \(((81.1130 .18, \ldots\) & 2408.907816 \\
6 & Antarctica & Antarctica & (POLYGON \(((-59.57-80.04 \ldots\) & 2374.075028 \\
\(\ldots\) & \(\ldots\) & \(\ldots\) & \(\ldots\) & \\
\hline
\end{tabular}

\section*{Let's practice!}

WORKING WITH GEOSPATIAL DATA IN PYTHON

\section*{The end}

WORKING WITH GEOSPATIAL DATA IN PYTHON

Instructors

Joris Van den Bossche \& Dani Arribas-

![](https://cdn.mathpix.com/cropped/2024_07_18_b6eed2ad89bbe676a47dg-36.jpg?height=320&width=309&top_left_y=1207&top_left_x=1983)
Bel

\section*{Taking the next steps ...}

More on GeoPandas:

- GeoPandas docs and example gallery: https://geopandas.readthedocs.io/

- Other online sources, e.g.: https://automating-gis-processes.github.io/2018/

Looking for spatial statistics? Check PySAL

Working with multi-dimensional gridded data? Check xarray

Want to create interactive web maps? Check folium, ipyleaflet or geoviews

Make matplotlib plots with projection support? Check cartopy

\section*{Good luck!}

WORKING WITH GEOSPATIAL DATA IN PYTHON