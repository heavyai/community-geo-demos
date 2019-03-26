
<h1>Simplifying Open Street Map Data Analytics with OmniSci and Placemaker</h1>
<hr>Dr. Mike Flaxman, March 2019</hr>

<br>
As a GIS professional, one of the most common questions I get is "how do I get basic data for my study area?"  In most places around the world, my short answer to this is usually 'OSM' or Open Street Map.  For those of you not familiar, OSM is a great crowd-sourced project which for many years has been generating truly open datasets of roads, buildings, hydrology, land use and other features across the planet.  Because it is crowd-sourced, its quality does vary from place to place, so a best practice is to sanity check against other available sources where they exist.  But in most of the world, OSM compares favorably with authoritative datasets.

One issue that many folks have with GIS data in general, and OSM in particular, is that it is mostly available in rather unusual data formats.  OSM's native formats are unique to that specific project - PDB (binary) and OSM (xml).  It uses a tagging structure which is rather different than conventional GIS attribute tables, and requires a bit of tweaking to get into a SQL relational database.  This in turn requires a set of dedicated tools just to get this specific data into common GIS formats.

A second issue with using OSM is just the size/spatial extent of commonly avaiable downloads.  For example, a major download site is [https://download.geofabrik.de\], but OSM data there are only available by continent.

For typical geospatial analysis work, what we typically want is data for one town or region as "analysis ready data."  Its obviously wasteful to download all of North America if you just want a single town.  There are some other secondary OSM sites which allow extracting smaller areas, but that basically requires going through a custom interface just to "clip, zip and ship."

A few year ago, I ran across two Python-based tools which changed my professional practice: the OSMNX library (Boeing 2017), which makes it easy to specify geographic places and download their OSM data, and GeoPandas, which is a Swiss army knife for geodata.

I've combined those two libraries into an open source script/Jupyter Notebook I call 'Placemaker.'  The outputs could be used in any GIS package, but for larger areas, I like to explore in OmniSci's Immerse online platform.  This allows you to interactively display huge OSM datasets, and understand which attributes it has.  This is particularly useful with OSM data, where these can vary place to place.

<h3>How To Use Placemaker</h3>

1. Go to our GitHub site below (if you're not already there) and grab a copy of the placemaker.ipynb script:

	[https://github.com/omnisci/community-geo-demos/blob/master/osm/placemaker_v01.ipynb]
	
2. Put the script on a machine which already has its required Python libraries, or execute the Instal block to run conda installs. 

3. Replace the sample town, Paraty, Brazil with any other place in the world of your choice.

4. From the Cell menu, select "Run All" Alternatiecly, you can shift-return over cells to execute step by step.  The defaults will give you three OSM tables: bounds, roads and buildings.

<h3>How It Works - Overview</h3>

The workflow starts with a place name.  This is translated by OSMNX into a geographic polygon, and a set of geographic objects representing roads and buildings.  These are then converted in-memory to formats compatible with SQL geodatabases.  They can then either be exported into many common GIS file formats, or directly uploaded to an OmniSci database instance.

While Placemaker extracts data from some very specific sources, it uses methods which should be of general applicability to a wide variety of GIS data. This code shows how to load geospatial data to Pandas or GeoPandas data frames, and then to your favorite geovisualization or geoanalytics tool.  It also shows a nice trick or two to get GeoPandas data into OmniSci in-memory.  This avoids having to write shape files or geojson to disk which both saves time and avoids some data type confusion issues.  

The outputs are a set of standard SQL-compatible geodatabase files.  These represent specific combinations of OSM tags as 'layers' more similar to conventional CAD and GIS, with one table for 'roads' and another for 'buildings' for example.

<h3>The Details</h3>

Placemaker gets boundary definition data from Open Street Map using the OSMNX library.  Under the hood, this calls the Nominatum API to translate place names to geographies.  For example, if you set the place name to 'Paraty, Brazil', this library returns a GeoDataFrame containing the bounding box and detailed geography representing the polygonal boundary of that place.  

This works without a hitch much of the time, but the most common problem is with place names which are not unique.  If your place name is very common, like 'Springfield' in the US, you may have to further specify the name a bit.   Also, in some cases, OSM has only point data representing a place.  In a case like that, you can further specify a 'buffer distance' so that you generate an area.

Speaking of buffer distances, OSMNX allows multiple clever and important network distance measures.  In particular, you can specify travel times by mode along networks.  The software is smart enough to understand non-drivable roads or walking times.  This is well explained in the OSMNX docs if you need it.

OSMNX is designed for network analysis, and so returns road data as a directed graph.  We loop through the edges of this graph and recover a Pandas data frame where lines are represented as 'well known text.'  Note that this is slightly different than other OSMNX routines which return GeoPandas data frames.  GeoPandas geometry columns contain binary 'Shapely' representations of geometry (even though they have a user-friendly text representation when you display them).  

So for roads, the Pandas dataframe is then uploaded to OmniSci using the pymapd library's row-wise loader.  This specific loader (and currently *only** this loader) supports WKT conversion to OmniSci geometry features in-memory.   

The code for buildings and bounds is slightly different, since these are returned by OSMNX as GeoPandas data frames.   In particular, look at 'row2wkt' which is broken out into a separate function just to emphasize it.  Shapely objects have a '.to_wkt()' which we then call to convert them in place.  This essentially converts GeoPandas to regular Pandas, which pymapd then understands.

The pymapd library is well documented, but I found a couple of points tricky.  The first is that the row-wise loader method wants a Python iterator over un-named and un-indexed tuples.  But Pandas by default names and indexes things, so you need to turn off those enhancements before passing on the data.  The relevant line is:

	con.load_table_rowwise(table_name, gdf.itertuples(index=False, name=None))

Last but not least, I used the current list of reserved column names from OmniSci, and not a more-general SQL list.  OSM taggers are incredibly inventive, and so name collisions are a common hazard in getting such data into any database.  I opted for some relatively simple rules for column naming, yielding understandable-but-verbose names.  Depending on your needs you might want to tighten up on those, or for interoperability to include SQL keywords used by other databases.  The method which generates these mappings is:

	ddl_from_cols()

This also attempts to guess appropriate column types.  In that, it sacrifices some efficiency for robustness, so there is undoubtedly room for improvement on specific OSM columns.

<h3>Conclusions</h3>

I hope you find the 'Placemaker' utility script useful, either specifically for generating geospatial data projects in OmniSci, or more generally for getting OSM data into more-standard GIS form.  Routines like **gdf_to_omnisci** provide a pretty-generic new pathway for getting oddball geo formats into OmniSci.  In addition to OSM shown here, this approach should work for file formats not currently directly supported by OmniSci, including multi-layer archive formats such as ESRI's file geodatabases. 

OmniSci is committed to contributing to the OSM community, so look for some announcements soon about further steps in increasing access to this important dataset.

<h3>References</h3>

OSMNX Documentation.  [https://osmnx.readthedocs.io/en/stable/]

Boeing, G. 2017. “OSMnx: New Methods for Acquiring, Constructing, Analyzing, and Visualizing Complex Street Networks.” Computers, Environment and Urban Systems 65, 126-139. doi:10.1016/j.compenvurbsys.2017.05.004
