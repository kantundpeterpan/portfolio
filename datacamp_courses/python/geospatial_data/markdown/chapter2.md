\title{
Shapely geometries and spatial relationships
}

WORKING WITH GEOSPATIAL DATA IN PYTHON

\section*{Dani Arribas-Bel}

![](https://cdn.mathpix.com/cropped/2024_07_18_15ab3fd3c0a3cbf3fa00g-01.jpg?height=298&width=309&top_left_y=1457&top_left_x=1962)

Geographic Data Science Lab (University of Liverpool)

\section*{Scalar geometry values}

```
cities = geopandas.read_file("ne_110m_populated_places.shp")
cities.head()
```

\begin{tabular}{|rrr|r|}
\hline & name & \\
0 & Vatican City & POINT (12.45338654497177 41.90328217996012) \\
1 & San Marino & POINT \((12.4417701578001443 .936095834768)\) \\
2 & Vaduz & POINT \((9.51666947290726747 .13372377429357)\) \\
3 & Lobamba & POINT (31.19999710971274 -26.46666746135247\()\) \\
4 & Luxembourg & POINT (6.130002806227083 49.61166037912108) \\
\hline
\end{tabular}

```
brussels = cities.loc[170, 'geometry']
print(brussels)
```

POINT (4.33137074969045 50.83526293533032)

\section*{Scalar geometry values}

brussels = cities.loc[170, 'geometry']

print(brussels)

POINT (4.33137074969045 50.83526293533032)

type(brussels)

shapely.geometry.point.Point

\section*{The Shapely python package}

type(brussels)

shapely.geometry.point.Point

\section*{Shapely}
- Python Package for the manipulation and analysis of geometric objects
- Provides the Point, LineString and Polygon objects
- GeoSeries (GeoDataFrame 'geometry' column) consists of shapely objects

\section*{Geometry objects}

Accessing from a GeoDataFrame:

```
brussels = cities.loc[170, 'geometry']
paris = cities.loc[235, 'geometry']
belgium = countries.loc[countries['name'] == 'Belgium', 'geometry'].squeeze()
france = countries.loc[countries['name'] == 'France', 'geometry'].squeeze()
uk = countries.loc[countries['name'] == 'United Kingdom', 'geometry'].squeeze()
```

Creating manually:

from shapely.geometry import Point

\(\mathrm{p}=\) Point \((1,2)\)

print(p)

POINT (1 2)

\section*{Spatial methods}

The area of a geometry:

belgium.area

\subsection*{3.8299974609075753}

The distance between 2 geometries:

brussels.distance(paris)

2.8049127723186214

And many more! (e.g. centroid, simplify , ...)

\section*{Spatial relationships}

geopandas.GeoSeries([belgium, france, uk, paris, brussels, line]).plot()

![](https://cdn.mathpix.com/cropped/2024_07_18_15ab3fd3c0a3cbf3fa00g-07.jpg?height=1487&width=1492&top_left_y=553&top_left_x=1370)

\section*{Spatial relationships}

![](https://cdn.mathpix.com/cropped/2024_07_18_15ab3fd3c0a3cbf3fa00g-08.jpg?height=439&width=1958&top_left_y=345&top_left_x=128)

france.contains(brussels)

\section*{False}

brussels.within(belgium)

True

True

line.intersects(france)

True

line.intersects(uk)

False

\section*{Let's practice!}

WORKING WITH GEOSPATIAL DATA IN PYTHON

\section*{Spatial relationships with GeoPandas}

WORKING WITH GEOSPATIAL DATA IN PYTHON

\section*{Dani Arribas-Bel}

Geographic Data Science Lab

![](https://cdn.mathpix.com/cropped/2024_07_18_15ab3fd3c0a3cbf3fa00g-10.jpg?height=353&width=347&top_left_y=1321&top_left_x=1948)
(University of Liverpool)

\section*{Element-wise spatial relationship methods}

brussels.within(france)

False

paris.within(france)

True

\section*{Element-wise spatial relationship methods}

brussels.within(france)

False

For full GeoDataFrame?

cities.head()

name

geometry

0 Vatican City POINT (12.45338654497177 41.90328217996012)

1 San Marino POINT (12.44177015780014 43.936095834768)

2 Vaduz POINT (9.516669472907267 47.13372377429357)

3 Lobamba POINT (31.19999710971274 -26.46666746135247)

\section*{Element-wise spatial relationship methods}

The within() operation for each geometry in cities :

cities.within(france)

cities['geometry'][0].within(france)

![](https://cdn.mathpix.com/cropped/2024_07_18_15ab3fd3c0a3cbf3fa00g-13.jpg?height=950&width=1965&top_left_y=914&top_left_x=119)

False

cities['geometry'][1].within(france)

\section*{False}

cities['geometry'][2].within(france)

False

...

\section*{Filtering by spatial relation}

Filter cities depending on the within() operation:

cities[cities.within(france)]

\begin{tabular}{|c|c|c|}
\hline & name & geometry \\
\hline 10 & Monaco & POINT (7.406913173465057 43.73964568785249\()\) \\
\hline 13 & Andorra & POINT (1.51648596050552 42.5000014435459\()\) \\
\hline 235 & Paris & POINT (2.33138946713035 48.86863878981461\()\) \\
\hline
\end{tabular}

\section*{Filtering by spatial relation}

Which countries does the Amazon flow through?

rivers = geopandas.read_file("ne_50m_rivers_lake_centerlines.shp")

rivers.head()

![](https://cdn.mathpix.com/cropped/2024_07_18_15ab3fd3c0a3cbf3fa00g-15.jpg?height=690&width=4007&top_left_y=876&top_left_x=118)

```
amazon = rivers[rivers['name'] == 'Amazonas'].geometry.squeeze()
mask = countries.intersects(amazon)
```

\section*{Filtering by spatial relation}

countries[mask]

\begin{tabular}{lrrrrr}
\hline \multicolumn{4}{c}{ name } & continent & \\
geometry \\
22 & Brazil & South America & POLYGON \(((-57.63-30.22,-56.29-28 \ldots\) \\
35 & Colombia & South America & POLYGON \(((-66.881 .25,-67.071 .13, \ldots\) \\
124 & Peru & South America & POLYGON \(((-69.53-10.95,-68.67-12 \ldots\) \\
\hline
\end{tabular}
- within

- contains
- intersects

More at https://shapely.readthedocs.io/en/latest/

\section*{Shapely objects}

paris.within(france)

True

france.intersects(amazon)

False

\section*{GeoPandas}

cities.within(france)

![](https://cdn.mathpix.com/cropped/2024_07_18_15ab3fd3c0a3cbf3fa00g-17.jpg?height=548&width=1953&top_left_y=562&top_left_x=2160)

countries.intersects(amazon)

\(\begin{array}{ll}0 & \text { False } \\ 1 & \text { False } \\ 2 & \text { False }\end{array}\)

WORKING WITH GEOSPATIAL DATA IN PYTHON

\section*{Let's practice!}

WORKING WITH GEOSPATIAL DATA IN PYTHON

\section*{The "spatial join" operation}

WORKING WITH GEOSPATIAL DATA IN PYTHON

\section*{Dani Arribas-Bel}

Geographic Data Science Lab

![](https://cdn.mathpix.com/cropped/2024_07_18_15ab3fd3c0a3cbf3fa00g-19.jpg?height=326&width=342&top_left_y=1329&top_left_x=1945)
(University of Liverpool)

\section*{Spatial relationships I}

![](https://cdn.mathpix.com/cropped/2024_07_18_15ab3fd3c0a3cbf3fa00g-20.jpg?height=1411&width=3560&top_left_y=374&top_left_x=385)

\section*{Spatial relationships II}

Which cities are located within Brazil?

```
brazil = countries.loc[22, 'geometry']
cities[cities.within(brazil)]
```

![](https://cdn.mathpix.com/cropped/2024_07_18_15ab3fd3c0a3cbf3fa00g-21.jpg?height=656&width=4000&top_left_y=947&top_left_x=122)

But what if we want to know for each city in which country it is located?

\section*{The Spatial Join}

![](https://cdn.mathpix.com/cropped/2024_07_18_15ab3fd3c0a3cbf3fa00g-22.jpg?height=1737&width=4058&top_left_y=298&top_left_x=109)

\section*{The spatial join with GeoPandas}

```
joined = geopandas.sjoin(cities,
    countries[['name', 'geometry']],
    op="within")
```

joined.head()

\begin{tabular}{|lrrrr|}
\hline & name_left & & geometry name_right \\
0 & Vatican City & POINT (12.45338654497177 41.90328217996012) & Italy \\
1 & San Marino & POINT \((12.4417701578001443 .936095834768)\) & Italy \\
226 & Rome & POINT \((12.481312562874\) & \(41.89790148509894)\) & Italy \\
2 & VaduZ & POINT \((9.51666947290726747 .13372377429357)\) & Austria \\
212 & Vienna & POINT (16.36469309674374 48.20196113681686\()\) & Austria \\
\hline
\end{tabular}

\section*{Let's practice!}

WORKING WITH GEOSPATIAL DATA IN PYTHON

\section*{Choropleths: Mapping data over \\ space}

WORKING WITH GEOSPATIAL DATA IN PYTHON

\section*{Dani Arribas-Bel}

![](https://cdn.mathpix.com/cropped/2024_07_18_15ab3fd3c0a3cbf3fa00g-25.jpg?height=283&width=309&top_left_y=1470&top_left_x=1962)

Geographic Data Science Lab (University of Liverpool)

\section*{Choropleths}

countries.plot(column='gdp_per_cap', legend=True)

![](https://cdn.mathpix.com/cropped/2024_07_18_15ab3fd3c0a3cbf3fa00g-26.jpg?height=1433&width=3706&top_left_y=602&top_left_x=415)

\section*{Choropleths}

Specifying a column:

locations.plot(column='variable')

Choropleth with classification scheme:

locations.plot(column='variable', scheme='quantiles', k=7, cmap='viridis')

Key choices:

- Number of classes ( k )

- Classification algorithm ( scheme )

- Color palette ( cmap )

\section*{Number of classes ("k")}

locations.plot(column='variable', scheme='Quantiles', k=7, cmap='viridis')

Choropleths necessarily imply information loss (but that's OK)

Tension between:

- Maintaining detail and granularity from original values (higher k )

- Abstracting information so it is easier to process and interpret (lower k)

Rule of thumb: 3 to 12 classes or "bins"

\section*{Classiffication algorithms ("scheme")}

locations.plot(column='variable', scheme='quantiles', k=7, cmap='viridis')

How do we allocate every value in our variable into one of the \(k\) groups?

Two (common) approaches for continuous variables:

- Equal Intervals ('equal_interval')

- Quantiles ( 'quantiles')

\section*{Equal Intervals}

locations.plot(column='variable', scheme='equal_interval', k=7, cmap='Purples')

![](https://cdn.mathpix.com/cropped/2024_07_18_15ab3fd3c0a3cbf3fa00g-30.jpg?height=1362&width=2360&top_left_y=610&top_left_x=849)

\section*{Quantiles}

locations.plot(column='variable', scheme='quantiles', k=7, cmap='Purples')

quantiles

![](https://cdn.mathpix.com/cropped/2024_07_18_15ab3fd3c0a3cbf3fa00g-31.jpg?height=1275&width=1270&top_left_y=708&top_left_x=819)

Geographical distribution

![](https://cdn.mathpix.com/cropped/2024_07_18_15ab3fd3c0a3cbf3fa00g-31.jpg?height=1145&width=836&top_left_y=773&top_left_x=2382)

\section*{Color}

Categories, non-ordered

```
locations.plot(column='variable',
    categorical=True, cmap='Purples')
```

Graduated, sequential

locations.plot(column='variable', \(\mathrm{k}=5, \mathrm{cmap}=\) 'RdPu')

Graduated, divergent

locations.plot(column='variable', \(k=5\), cmap='RdY̌Gn')

IMPORTANT: Align with your purpose

\section*{Let's practice!}

WORKING WITH GEOSPATIAL DATA IN PYTHON