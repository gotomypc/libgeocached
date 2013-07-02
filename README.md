## libgeocached 
A fast in-memory geospatial index and query library in C++. The library supports the following

* indexing of arbitray object types with geolocation tag via C++ template
* efficient [geohash](https://en.wikipedia.org/wiki/Geohash)-based spatial partitioning and search tree impplementation for geospatial query
* optional persistency support for NoSQL library including [Redis](http://redis.io/) and [MongoDB](http://www.mongodb.org/)



### Usage
To create a new geocache matrix, do 
```cpp
#include <matrix.hpp>

libgeocached::Matrix<DataObject> matrix; //DataObject can be any type
```

To add an object into the matrix and associate it with an id and location tag:

```cpp
//data insertion
ObjectID id1 = ObjectIDNew();
DataObject obj("dummy");
GCLocation location =  GCLocationMake(23.23234, -123.34324);
matrix.insert(id1, obj, location);
```

To query for all objects within a circular geographical area 
```cpp
#include <vector>
...
//define a circle area with center lat/lng and 1000 meter radius
GCCircle circle = GCCircleMake(GCLocationMake(23.23234, -123.34324), 1000);
std::vector<DataObject> objs;

//circular query
matrix.objs_in_circle(circle, objs);

for(DataObject& obj : objs)
    cout << obj << endl;
```


### GCTree 
GCTree is an efficient search tree implementation for hierarchical partitioning of geographical space and cell indexing. Each tree node represents a rectangular geo cell with bounding latitude (north,south) and longitude (west,east) edges. It uses a binary representation of the geohash values for both lat and lng. For example, a 15bit precision binary geohash
```cpp
latitude:  0b101000010000101
longitude: 0b001010000100101

```

represents a rectangle bounding box of the following edge values

```cpp
lat_south:	23.2305908203125	23.2305908203125
lat_north:	23.236083984375	23.236083984375
lng_west:	-123.343505859375	-123.343505859375
lng_east:	-123.33251953125	-123.33251953125
```

Each internal tree node has a branching degree of 4, with each child representing adding an additional lowest bit to its latitude and longitude values
```cpp
lat lng
0   0    south half along latitude, west half along longitude
0   1    south half along latitude, east half along longitude
1   0    north half along latitude, west half along longitude
1   1    north half along latitude, east half along longitude
```

Thesefore, a given geo cell is partitioned into four sub regions by halfing at the centre line along both latitude and longitude directions. Internal node at tree level K represents a geo rectangle having K bit precision (assuming 0 bit at root node). 




## Performance
[TBD]

## Road Map
[TBD]

## Dependencies

* Geographiclib <http://geographiclib.sourceforge.net/>
* libgeohash <https://github.com/lyokato/libgeohash>
* libspatialindex <http://libspatialindex.github.io/>
* RTree <http://superliminal.com/sources/sources.htm#C%20&%20C++%20Code>


## Author 
Denny C. Dai <dennycd@me.com> or visit <http://dennycd.me>

## License 
The MIT License (MIT) 
<http://opensource.org/licenses/MIT>
