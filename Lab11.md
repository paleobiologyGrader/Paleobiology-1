Nicholas Boulanger
Lab Exercise 11

> 19/20

`source("https://raw.githubusercontent.com/aazaff/paleobiologyDatabase.R/master/communityMatrix.R")`

## Problem Set 1

1)
`TriassicUnits<-read.csv("https://macrostrat.org/api/units?interval_name=Triassic&format=csv")`

2)
````R
dim(TriassicUnits)
[1] 838  17
838 units.
````

3)
`TriassicUnits[1:10,]`

These are mostly Igneous and Metamorphic rocks, which are unlikely to contain fossils. Volcanic activity tends
to destroy rather than preserve organic material, and metamorphic rocks also are not good at preserving fossils.

4)
t_age, the ending age, and b_age, the starting age.

5)
Some range through the Triassic.

## Problem Set 2

1) 
`TriassicUnits<-read.csv("https://macrostrat.org/api/units?interval_name=Triassic&format=csv&lith_class=sedimentary")`

2) 
`TriassicUnits<-subset(TriassicUnits,t_age>=201.3 & b_age<=252.17)`

3)
````R
CretaceousUnits<-read.csv("https://macrostrat.org/api/units?interval_name=Cretaceous&format=csv&lith_class=sedimentary")
JurassicUnits<-read.csv("https://macrostrat.org/api/units?interval_name=Jurassic&format=csv&lith_class=sedimentary")
PermianUnits<-read.csv("https://macrostrat.org/api/units?interval_name=Permian&format=csv&lith_class=sedimentary")
CarboniferousUnits<-read.csv("https://macrostrat.org/api/units?interval_name=Carboniferous&format=csv&lith_class=sedimentary")
DevonianUnits<-read.csv("https://macrostrat.org/api/units?interval_name=Devonian&format=csv&lith_class=sedimentary")
SilurianUnits<-read.csv("https://macrostrat.org/api/units?interval_name=Silurian&format=csv&lith_class=sedimentary")
OrdovicianUnits<-read.csv("https://macrostrat.org/api/units?interval_name=Ordovician&format=csv&lith_class=sedimentary")

CretaceousUnits<-subset(CretaceousUnits,t_age>=66 & b_age<=145.5)
JurassicUnits<-subset(JurassicUnits,t_age>=145.0 & b_age<=201.3)
PermianUnits<-subset(PermianUnits,t_age>=252.17 & b_age<=298.9)
CarboniferousUnits<-subset(CarboniferousUnits,t_age>=298.9 & b_age<=358.9)
DevonianUnits<-subset(DevonianUnits,t_age>=358.9 & b_age<=419.2)
SilurianUnits<-subset(SilurianUnits,t_age>=419.2 & b_age<=443.4)
OrdovicianUnits<-subset(OrdovicianUnits,t_age>=443.4 & b_age<=485.5)
````

4)
````R
dim(CretaceousUnits)
dim(JurassicUnits)
dim(PermianUnits)
dim(CarboniferousUnits)
dim(DevonianUnits)
dim(SilurianUnits)
dim(OrdovicianUnits)

dim(CretaceousUnits)
[1] 4589   17
> dim(JurassicUnits)
[1] 998  17
> dim(PermianUnits)
[1] 903  17
> dim(CarboniferousUnits)
[1] 3020   17
> dim(DevonianUnits)
[1] 2059   17
> dim(SilurianUnits)
[1] 1101   17
> dim(OrdovicianUnits)

Xfreq<-c("Cretaceous","Jurassic","Permian","Carboniferous","Devonian","Silurian","Ordovician")
Yfreq<-c(4589,998,903,3020,2059,1101,2161)
names(Yfreq)<-Xfreq
UnitFreqs<-Yfreq
````

5)
````R
> mean(UnitFreqs)
[1] 2118.714
> sd(UnitFreqs)
[1] 1334.771

2118-838
[1] 1280
````

The Triassic is less than one  standard deviation away from the mean.

> It is slight more than one. -1 Points

6)
There is not statistical evidence that the Triassic is significantly different from the other periods, because
it is well within a normal distribution of number of units.

7)
````R
Xfreq<-c("Cretaceous","Permian","Carboniferous","Devonian","Silurian","Ordovician")
Yfreq<-c(4589,903,3020,2059,1101,2161)
names(Yfreq)<-Xfreq
UnitFreqs<-Yfreq

mean(UnitFreqs)
[1] 2305.5
> sd(UnitFreqs)
[1] 1358.26
````

Even with the Triassic and Jurassic both excluded from the distribution, they both still fall well within two
standard distributions. Therefore, they are not outliers.

## Part 2
````R
URL<-"https://macrostrat.org/api/columns?format=geojson_bare&interval_name=Albian&project_id=1"
GotURL<-getURL(URL)
AlbianMap<-readOGR(GotURL,"OGRGeoJSON",verbose=FALSE)
plot(AlbianMap,col="#CCEA97")
````

1)

````R
URL<-"https://macrostrat.org/api/columns?format=geojson_bare&project_id=1"
GotURL<-getURL(URL)
Map<-readOGR(GotURL,"OGRGeoJSON",verbose=FALSE)
plot(Map)
````

2)
````R
NewURL<-"https://macrostrat.org/api/columns?format=geojson_bare&project_id=1&age_top=242&age_bottom=252.2"
NewGotURL<-getURL(NewURL)
NewMap<-readOGR(NewGotURL,"OGRGeoJSON",verbose=FALSE)
plot(NewMap,col="#B051A5",add=TRUE)
````

3)
````R
Induan<-downloadPBDB(Taxa=c("Animalia"),StartInterval="Induan",StopInterval="Cenozoic")
Thatsright<-Induan[,"lat"]
Anisian<-Induan[,"lng"]
points(Anisian,Thatsright)
````

4)
````R
windows()
plot(Map)

poop<-"https://macrostrat.org/api/columns?format=geojson_bare&project_id=1&interval_name=Lopingian"
poopy<-getURL(poop)
poopmap<-readOGR(poopy,"OGRGeoJSON",verbose=FALSE)
plot(poopmap,col="#FBA794",add=TRUE)
````

> That's not a very nice object name.


5)
````R
Lopingian<-downloadPBDB(Taxa=c("Animalia"),StartInterval="Lopingian",StopInterval="Lopingian")
Lopingianlat<-Lopingian[,"lat"]
Lopingianlong<-Lopingian[,"lng"]
points(Lopingianlong,Lopingianlat)
````

6)
There does seem to be a drop in the extent of sedimentary units. Even more substantial was the drop
in the reported fossils. The drop in fossils is so dramatic that it seems unlikely that lower diversity
in the Early Triassic is an artefact of poor sedimentary rock availability. However, poor fossil sampling,
if it was poor enough, could still explain the discrepancy; many of the deposits on the second map are completely
unsampled. 








