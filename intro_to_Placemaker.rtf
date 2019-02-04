{\rtf1\ansi\ansicpg1252\cocoartf1561\cocoasubrtf400
{\fonttbl\f0\fnil\fcharset0 HelveticaNeue-Bold;\f1\fnil\fcharset0 HelveticaNeue;}
{\colortbl;\red255\green255\blue255;\red0\green0\blue0;}
{\*\expandedcolortbl;;\cssrgb\c0\c0\c0;}
\margl1440\margr1440
\deftab720
\pard\pardeftab720\partightenfactor0

\f0\b\fs32 \cf2 \expnd0\expndtw0\kerning0
\up0 \nosupersub \ulnone \outl0\strokewidth0 \strokec2 Simplifying Open Street Map Data Analytics \uc0\u8232 with OmniSci and Placemaker\
\pard\pardeftab720\partightenfactor0

\f1\b0\fs22 \cf2 \strokec2 \
As a GIS professional, one of the most common questions I get is \'93how do I get basic data for my study area?\'94  In most places around the world, my short answer to this is usually \'93OSM\'94 or Open Street Map.  For those of you not familiar, OSM is a great crowd-sourced project which for many years has been generating truly open datasets of roads, buildings, hydrology, land use and other features across the planet.  Because it is crowd-sourced, its quality does vary from place to place, so a best practice is to sanity check against other available sources where they exist.  But in most of the world, OSM compares favorably with authoritative datasets.\
\
One issue that many folks have with GIS data in general, and OSM in particular, is that it is mostly available in rather unusual data formats.  OSM\'92s native formats are unique to that specific project - PDB (binary) and OSM (xml).  It uses a tagging structure which is rather different than conventional GIS attribute tables, and requires a bit of tweaking to get into a SQL relational database.  This in turn requires a set of dedicated tools just to get this specific data into common GIS formats. \
\
A second issue is just spatial extent.  For example, a major download site is \'91{\field{\*\fldinst{HYPERLINK "http://geofabrik.de"}}{\fldrslt \cf2 \ul \ulc2 \strokec2 geofabrik.de}}', but OSM data there are only available by continent.\
\
https://download.geofabrik.de\
\
For typical geospatial analysis work, what we typically want is data for one town or region as \'93analysis ready data.\'94  Its obviously wasteful to download all of North America if you just want a single town.  There are some other secondary OSM sites which allow extracting smaller areas, but that basically requires going through a custom interface jus to \'93clip, zip and ship.\'94\
\
A few year\'92s ago, I ran across two Python-based tools which changed my professional practice: Geoff Boing\'92s OSMNX library, which makes it easy to specify geographic places and download their OSM data, and GeoPandas, which is a Swiss army knife for geodata.\
\
I\'92ve combined those two libraries into an open source script/Jupyter Notebook I call \'93Placemaker.\'94  The outputs could be used in any GIS package, but for larger areas, I like to explore in OmniSci\'92s \'93Immerse\'94 online platform.  This allows you to interactively display huge OSM datasets, and understand which attributes it has.  This is particularly useful with OSM data, where these can vary place to place.\
\pard\pardeftab720\sa360\partightenfactor0
\cf2 \strokec2 \
\pard\pardeftab720\sl288\slmult1\sa40\partightenfactor0

\fs28 \cf2 \kerning1\expnd1\expndtw5
\strokec2 How To Use Placemaker\
\pard\pardeftab720\partightenfactor0

\fs22 \cf2 \expnd0\expndtw0\kerning0
\strokec2 \
1. Go to our GitHub site below and grab a copy of the placemaker.ipynb script:\
\
	http://github.com/omnisci/community/placemaker.ipynb\
\
2. Put the script on a machine which already has its required Python libraries, or execute the \'93Install\'94 block to run conda installs. \
\
3. Replace the sample town, \'93Paraty, Brazil\'94 with any other place in the world of your choice.\
\
4. Shift-return over cells to execute step by step.  The defaults will give you three OSM tables: bounds, roads and buildings.\
\
\pard\pardeftab720\sa360\partightenfactor0
\cf2 \strokec2 \
\pard\pardeftab720\sl288\slmult1\sa40\partightenfactor0

\fs28 \cf2 \kerning1\expnd1\expndtw5
\strokec2 How It Works - Overview\
\pard\pardeftab720\partightenfactor0

\fs22 \cf2 \expnd0\expndtw0\kerning0
\strokec2 \
The workflow starts with a place name.  This is translated by OSMNX into a geographic polygon, and a set of geographic objects representing roads and buildings.  These are then converted in-memory to formats compatible with SQL geodatabases.  They can then either be exported into many common GIS file formats, or directly uploaded to an OmniSci database instance.\
\
While Placemaker extracts data from some very specific sources, it uses methods which should be of general applicability to a wide variety of GIS data. This code shows how to load geospatial data to Pandas or GeoPandas data frames, and then to your favorite geovisualization or geoanalytics tool.  It also shows a nice trick or two to get GeoPandas data into OmniSci in-memory.  This avoids having to write shape files or geojson to disk which both saves time and avoids some data type confusion issues.  \
\
The outputs are a set of standard SQL-compatible geodatabase files.  These represent specific combinations of OSM tags as \'93layers\'94 more similar to conventional CAD and GIS, with one table for \'93roads\'94 and another for \'93buildings\'94 for example.\
\pard\pardeftab720\sa360\partightenfactor0
\cf2 \strokec2 \
\pard\pardeftab720\sl288\slmult1\sa40\partightenfactor0

\fs28 \cf2 \kerning1\expnd1\expndtw5
\strokec2 The Details\
\pard\pardeftab720\partightenfactor0

\fs22 \cf2 \expnd0\expndtw0\kerning0
\strokec2 \
Placemaker gets boundary definition data from Open Street Map using the OSMNX library.  Under the hood, this calls the Nominatum API to translate place names to geographies.  For example, if you set the place name to \'93Paraty, Brazil\'94, this library returns a GeoDataFrame containing the bounding box and detailed geography representing the polygonal boundary of that place.  \
\
This works without a hitch much of the time, but the most common problem is with place names which are not unique.  If your place name is very common, like \'91Springfield\'92 in the US, you may have to further specify the name a bit.   Also, in some cases, OSM has only point data representing a place.  In a case like that, you can further specify a \'93buffer distance\'94 so that you generate an area.\
\
Speaking of buffer distances, OSMNX allows multiple clever and important network distance measures.  In particular, you can specify travel times by mode along networks.  The software is smart enough to understand non-drivable roads or walking times.  This is well explained in the OSMNX docs if you need it.\
\
OSMNX is designed for network analysis, and so returns road data as a directed graph.  We loop through the edges of this graph and recover a Pandas data frame where lines are represented as \'91well known text.\'92  Note that this is slightly different than other OSMNX routines which return GeoPandas data frames.  GeoPandas geometry columns contain binary \'93Shapely\'94 representations of geometry (even though they have a user-friendly text representation when you display them).  \
\
So for roads, the Pandas dataframe is then uploaded to OmniSci using the pymapd library\'92s row-wise loader.  This specific loader (and currently *only** this loader) supports WKT conversion to OmniSci geometry features in-memory.   \
\
The code for buildings and bounds is slightly different, since these are returned by OSMNX as GeoPandas data frames.   In particular, look at \'93row2wkt\'94 which is broken out into a separate function just to emphasize it.  Shapely objects have a \'93.to_wkt()\'94 which we then call to convert them in place.  This essentially converts GeoPandas to regular Pandas, which pymapd then understands.\
\
The pymapd library is well documented, but I found a couple of points tricky.  The first is that the row-wise loader method wants a Python iterator over un-named and un-indexed tuples.  But Pandas by default names and indexes things, so you need to turn off those enhancements before passing on the data.  The relevant line is:\
\
	con.load_table_rowwise(table_name, gdf.itertuples(index=False, name=None))\
\
\
Last but not least, I used the current list of reserved column names from OmniSci, and not a more-general SQL list.  OSM taggers are incredibly inventive, and so name collisions are a common hazard in getting such data into any database.  I opted for some relatively simple rules for column naming, yielding understandable-but-verbose names.  Depending on your needs you might want to tighten up on those, or for interoperability to include SQL keywords used by other databases.  The method which generates these mappings is:\
\
	ddl_from_cols()\
\
This also attempts to guess appropriate column types.  In that, it sacrifices some efficiency for robustness, so there is undoubtedly room for improvement on specific OSM columns.\
\
Conclusions\
\
I hope you find the \'93Placemaker\'94 utility script useful, either specifically for generating geospatial data projects in OmniSci, or more generally for getting OSM data into more-standard GIS form.  Routines like \'93gdf_to_omnisci()\'94 provide a pretty-generic new pathway for getting oddball geo formats into OmniSci.  In addition to OSM shown here, this approach should work for file formats not currently directly supported by OmniSci, including multi-layer archive formats such as ESRI\'92s file geodatabases. \
\
OmniSci is committed to contributing to the OSM community, so look for some announcements soon about further steps in increasing access to this important dataset.\
\
\
\
}