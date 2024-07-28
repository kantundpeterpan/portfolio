\title{
Coordinate Reference Systems
}

\author{
WORKING WITH GEOSPATIAL DATA IN PYTHON
}

Joris Van den Bossche

Open source software developer and

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-01.jpg?height=342&width=347&top_left_y=1327&top_left_x=1948)
teacher, GeoPandas maintainer

\section*{Coordinate Reference System (CRS)}

Location of the Eiffel Tower:

POINT (2.2945 48.8584)

\(\rightarrow\) The Coordinate Reference System (CRS) relates the coordinates to a specific location on earth.

\section*{Geographic coordinates}

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-03.jpg?height=1596&width=1579&top_left_y=309&top_left_x=307)

Degrees of latitude and longitude.

E.g. \(48^{\circ} 51^{\prime} N, 2^{\circ} 17^{\prime} \mathrm{E}\)

Used in GPS, web mapping applications...

\section*{Attention!}

in Python we use (lon, lat) and not (lat, long)
- Longitude: \([-180,180]\)
- Latitude: \([-90,90]\)

\section*{Maps are 2D}

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-04.jpg?height=1374&width=3459&top_left_y=409&top_left_x=457)

\section*{Projected coordinates}
![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-05.jpg?height=1270&width=3188&top_left_y=314&top_left_x=513)

\((x, y)\)

\((x, y)\) coordinates are usually in meters or feet

\section*{Projected coordinates - Examples}

Albers Equal Area projection

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-06.jpg?height=1367&width=2274&top_left_y=559&top_left_x=990)

\section*{Projected coordinates - Examples}

Mercator projection

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-07.jpg?height=1302&width=1504&top_left_y=575&top_left_x=1375)

\section*{Projected coordinates - Examples}

Projected size vs actual size (Mercator projection)

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-08.jpg?height=1232&width=1883&top_left_y=773&top_left_x=1207)

\section*{Specifying a CRS}

proj4 string

Example: +proj=longlat +datum=WGS84 +no_defs

Dict representation:

\{'proj': 'longlat', 'datum': 'WGS84', 'no_defs': True\}

EPSG code

Example:

EPSG:4326 = WGS84 geographic CRS (longitude, latitude)

\section*{CRS in GeoPandas}

The .crs attribute of a GeoDataFrame/GeoSeries:

import geopandas

gdf = geopandas.read_file("countries.shp")

print(gdf.crs)

\{'init': 'epsg:4326'\}

\section*{Summary}

- "geographic" (long, lat) versus "projected" ( \(x, y\) ) coordinates

- Coordinates Reference System (CRS) in GeoPandas: .crs attribute

- Most used geographic CRS: WGS84 or EPSG:4326

\section*{Let's practice}

WORKING WITH GEOSPATIAL DATA IN PYTHON

\section*{Working with coordinate systems in GeoPandas}

WORKING WITH GEOSPATIAL DATA IN PYTHON

Joris Van den Bossche

Open source software developer and

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-13.jpg?height=321&width=348&top_left_y=1440&top_left_x=1948)
teacher, GeoPandas maintainer

\section*{CRS information in GeoPandas}

The .crs attribute of a GeoDataFrame/GeoSeries:

import geopandas

gdf = geopandas.read_file("countries.shp")

print(gdf.crs)

\{'init': 'epsg:4326'\}

\section*{Setting a CRS manually}

gdf_noCRS = geopandas.read_file("countries_noCRS.shp")

print(gdf_noCRS.crs)

\{\}

Add CRS information to crs :

```
# Option 1
gdf.crs = {'init': 'epsg:4326'}
# Option 2
gdf.crs = {'proj': 'longlat', 'datum': 'WGS84', 'no_defs': True}
```

\section*{Transforming to another CRS}

import geopandas

gdf = geopandas.read_file("countries_web_mercator.shp")

print(gdf.crs)

\{'init': 'epsg:3857', 'no_defs': True\}

The to_crs() method:

```
# Option 1
gdf2 = gdf.to_crs({'proj': 'longlat', 'datum': 'WGS84', 'no_defs': True})
# Option 2
gdf2 = gdf.to_crs(epsg=4326)
```

\section*{Why converting the CRS?}

1) Sources with a different CRS

```
df1 = geopandas.read_file(...)
df2 = geopandas.read_file(...)
df2 = df2.to_crs(df1.crs)
```

\section*{Why converting the CRS?}

1) Sources with a different CRS

2) Mapping (distortion of shape and distances)

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-18.jpg?height=1205&width=3999&top_left_y=743&top_left_x=117)

\section*{Why converting the CRS?}

1) Sources with a different CRS

2) Mapping (distortion of shape and distances)

3) Distance / area based calculations

\section*{How to choose which CRS to use?}

Tips:

- Use projection specific to the area of your data

- Most countries have a standard CRS

Useful sites:

- http://spatialreference.org/

- https://epsg.io/

\section*{Summary}

- To convert to another CRS: the to_crs() method

- Make sure different datasets have the same CRS

- When calculating distance, area, ... -> use a projected CRS

Useful sites:

- http://spatialreference.org/

- https://epsg.io/

\section*{Let's practice!}

WORKING WITH GEOSPATIAL DATA IN PYTHON

\section*{Spatial operations: creating new geometries}

WORKING WITH GEOSPATIAL DATA IN PYTHON

Joris Van den Bossche

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-23.jpg?height=288&width=330&top_left_y=1473&top_left_x=1962)

Open source software developer and teacher, GeoPandas maintainer

\section*{Spatial operations}

a

b

\section*{Spatial operations: intersection}

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-25.jpg?height=1568&width=1320&top_left_y=464&top_left_x=1505)

\section*{Spatial operations: union}

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-26.jpg?height=1351&width=1622&top_left_y=469&top_left_x=1338)

\[
\text { a.union(b) }
\]

\section*{Spatial operations: difference}

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-27.jpg?height=1357&width=1622&top_left_y=472&top_left_x=1338)

\[
\text { a.difference(b) }
\]

\section*{Spatial operations with GeoPandas}

africa.head()

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-28.jpg?height=962&width=1983&top_left_y=572&top_left_x=110)

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-28.jpg?height=1471&width=1481&top_left_y=317&top_left_x=2385)

\section*{Spatial operations with GeoPandas}

print(box)

POLYGON ((60 10, \(60-10,-20-10,-2010)\)

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-29.jpg?height=1259&width=1432&top_left_y=320&top_left_x=2165)

\section*{Spatial operations with GeoPandas}

africa.intersection(box)
![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-30.jpg?height=1366&width=3180&top_left_y=608&top_left_x=515)

\section*{Spatial operations with GeoPandas}

africa.head()

\begin{tabular}{lrl}
\hline & name & geometry \\
0 & Angola & (POLYGON ((23.90415368011818-11.7222815894063... \\
1 & Burundi & POLYGON \(((29.33999759290035-4.499983412294092 \ldots\) \\
2 & Botswana & POLYGON \(((29.43218834810904-22.09131275806759 \ldots\) \\
\(\ldots\) & & \\
\hline
\end{tabular}

africa.intersection(box)

0 (POLYGON ((13.22332255001795 -10, 13.120987583...

1 POLYGON ((29.33999759290035 -4.499983412294092...

2

()

. .

dtype: object

\section*{Let's practice!}

WORKING WITH GEOSPATIAL DATA IN PYTHON

\section*{Overlaying spatial datasets}

WORKING WITH GEOSPATIAL DATA IN PYTHON

Joris Van den Bossche

Open source software developer and

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-33.jpg?height=326&width=337&top_left_y=1324&top_left_x=1953)
teacher, GeoPandas maintainer

\section*{Intersection with a polygon}
![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-34.jpg?height=1416&width=3302&top_left_y=301&top_left_x=466)

countries.intersection(circle)

\section*{Intersection with a polygon}

Limitations of countries.intersection(circle) :
- Only intersecting a GeoSeries with a single polygon

- Does not preserve attribute information

\section*{Overlaying two datasets}

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-36.jpg?height=1384&width=1259&top_left_y=312&top_left_x=472)

countries.plot()

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-36.jpg?height=1395&width=1270&top_left_y=301&top_left_x=2501)

\[
\text { geologic_regions.plot() }
\]

\section*{Overlaying two datasets}
![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-37.jpg?height=1402&width=3256&top_left_y=307&top_left_x=469)

geopandas.overlay(countries, geologic_regions, how='intersection')

\section*{Overlay vs intersection}

Intersection method (with single polygon)

countries.intersection(geologic_region_A)

\section*{Overlay method}

geopandas.overlay(countries, geologic_regions, how='intersection')

![](https://cdn.mathpix.com/cropped/2024_07_18_30244ada66e736d4cf7dg-38.jpg?height=640&width=1964&top_left_y=868&top_left_x=2160)

\section*{Let's practice!}

WORKING WITH GEOSPATIAL DATA IN PYTHON