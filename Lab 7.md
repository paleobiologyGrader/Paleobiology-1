> 19/20

````R
source("https://raw.githubusercontent.com/aazaff/paleobiologyDatabase.R/master/communityMatrix.R")
DataPBDB<-downloadPBDB(Taxa=c("Bivalvia"),StartInterval="Cenozoic",StopInterval="Cenozoic")
DataPBDB<-cleanGenus(DataPBDB)
````

## Problem Set 1

1) 
The min_ma and max_ma columns represent filters on samples; they specify that records must be at least or at most 
a given age.

2)
`tapply(DataPBDB[,"max_ma"],DataPBDB[,"genus"],max)`

3)
`tapply(DataPBDB[,"min_ma"],DataPBDB[,"genus"],min)`

4)
````R
GenusFrequencies<-DataPBDB[,"genus"]
GenusTable<-table(GenusFrequencies)
sort(GenusTable)
#Anadara is the most frequent genus.
````

5)
````R
Anadara<-(DataPBDB[(DataPBDB[,"genus"]=="Anadara"),])
min(tapply(Anadara[,"min_ma"],Anadara[,"occurrence_no"],min))
[1] 0
max(tapply(Anadara[,"max_ma"],Anadara[,"occurrence_no"],max))
[1] 66
#This genus is first found in 66-million-year-old rocks and is extant.
````

## Problem Set 2

````R
Lucina<-subset(DataPBDB,DataPBDB[,"genus"]=="Lucina")
OriginalMean<-mean(Lucina[,"paleolat"])

 
Repeat<-1:1000
ResampledMeans<-array(NA,dim=length(Repeat))
set.seed(100)
for (counter in Repeat) {
   ResampledMeans[counter]<-mean(sample(Lucina[,"paleolat"],length(Lucina[,"paleolat"]),replace=TRUE))
   }
 head(ResampledMeans)
#NOTE: set.seed appears to be non-functional here. I am continuing despite this.
````

1)

+ mean(): take the mean of ().
+ sample(): take a random sample from ().
+ Lucina[,"paleolng"]: we are taking a random sample from the "paleolng" column of Lucina.
+ length(Lucina[,"paleolng"]: we are using the entire column as our sample size.
+ replace=TRUE: we are allowing the same element to be used more than once in our sample.

Our overarching goal is to find what mean we would get IF we took a random sample of Lucina as long as the one we
have.

2)
````R
plot(density(ResampledMeans))
#It does look approximately Gaussian. We have a bell with definite tails on each side.
````

3)
````R
mean(ResampledMeans)
[1] 24.16008
#This mean is very similar to the original mean of Lucina.
````

4)
````R
ResampledMans<-sort(ResampledMeans)
````

5)
````R
#2.5th percentile:
0.025*1000
[1] 25
ResampledMeans[25]
[1] 21.81826
#97.5th percentile:
0.975*1000
[1] 975
ResampledMeans[975]
[1] 26.2861
````

## Problem Set 3

````R
estimateExtinction <- function(OccurrenceAges, ConfidenceLevel=.95)  {
 NumOccurrences<-length(unique(OccurrenceAges))-1
  Alpha<-((1-ConfidenceLevel)^(-1/NumOccurrences))-1
  Lower<-min(OccurrenceAges)
  Upper<-min(OccurrenceAges)-(Alpha*10)
  return(setNames(c(Lower,Upper),c("Earliest","Latest")))
  }

estimateExtinction(Lucina[,"min_ma"],0.95)
 Earliest    Latest 
 0.000000 -1.221208 
````

1)
It is likely that Lucina is still among us, because the Earliest occurrences are modern.

2)
````R
Dallarca<-subset(DataPBDB,DataPBDB[,"genus"]=="Dallarca")
estimateExtinction(Dallarca[,"min_ma"],0.095)
Earliest   Latest 
2.588000 2.420241 
````

> I'm not sure what went wrong, but this is not correct. -1 Points

3)
It appears very, very unlikely that Dallarca managed to survive into the present without our noticing it.

4)
I think we ought to trust the pure reading of the fossil record. There is no concrete evidence that Dallarca
survived into the Quaternary, let alone into the present day. Statistics is not a substitute for actual data.

## Problem Set 4

1)
Fossil occurrences are not randomly distributed in time because the organisms that left them were not randomly
distributed in time. Populations grow, shrink, or disperse over time, often in nonrandom ways.

2)
Fossil occurences are not randomly distributed in time because the conditions for fossilization were not randomly
distributed in time. The likelihood of an organism being preserved changes based on the conditions it finds itself
in at the time of death, and these conditions are nonrandom.

## Problem Set 5

````R
ExtantBivalves<-read.csv("https://raw.githubusercontent.com/aazaff/teachPaleobiology/master/Lab7Figures/ExtantBivalves.csv",row.names=1,header=TRUE)
ExtantData<-subset(DataPBDB,DataPBDB[,"genus"]%in%ExtantBivalves[,"Extant"]==TRUE)
````

1)

````R
dim(DataPBDB)
[1] 67617    26
dim(ExtantData)
[1] 59096    26
67617-59096
[1] 8521
#We lost 8521 occcurrences by using only extant bivalves.
````

2)
````R
UniqueData<-DataPBDB[,"genus"]
UniqueExtant<-ExtantData[,"genus"]
unique(UniqueData)
unique(UniqueExtant)
length(unique(UniqueData))
[1] 1018
length(unique(UniqueExtant))
[1] 532
532/1018
[1] 0.5225933
#Approximately 52% of genera in the PBDB are alive today.
````

3)
````R
Bivalves<-tapply(ExtantData[,"min_ma"],ExtantData[,"genus"],min)
tapply(ExtantData[,"max_ma"],ExtantData[,"genus"],max)
````

4)
````R
ExtantBivalves<-Bivalves[which(Bivalves>0)]
````

5)
````R
Scrobicularia<-subset(DataPBDB,DataPBDB[,"genus"]=="Scrobicularia")
Meiocardia<-subset(DataPBDB,DataPBDB[,"genus"]=="Meiocardia")
Dimya<-subset(DataPBDB,DataPBDB[,"genus"]=="Dimya")
Digitaria<-subset(DataPBDB,DataPBDB[,"genus"]=="Digitaria")
Cuspidaria<-subset(DataPBDB,DataPBDB[,"genus"]=="Cuspidaria")
Arctica<-subset(DataPBDB,DataPBDB[,"genus"]=="Arctica")
Aloides<-subset(DataPBDB,DataPBDB[,"genus"]=="Aloides")
Kurtiella<-subset(DataPBDB,DataPBDB[,"genus"]=="Kurtiella")
Gouldia<-subset(DataPBDB,DataPBDB[,"genus"]=="Gouldia")
Acrosterigma<-subset(DataPBDB,DataPBDB[,"genus"]=="Acrosterigma")

estimateExtinction(Scrobicularia[,"min_ma"],0.95)
 Earliest    Latest 
  0.01170 -34.70966 
> estimateExtinction(Meiocardia[,"min_ma"],0.95)
 Earliest    Latest 
 0.011700 -5.329574 
> estimateExtinction(Dimya[,"min_ma"],0.95)
 Earliest    Latest 
 0.781000 -2.054688 
> estimateExtinction(Digitaria[,"min_ma"],0.95)
 Earliest    Latest 
 0.781000 -3.761154 
> estimateExtinction(Cuspidaria[,"min_ma"],0.95)
 Earliest    Latest 
2.5880000 0.7771965 
> estimateExtinction(Arctica[,"min_ma"],0.95)
 Earliest    Latest 
 0.011700 -1.799104 
> estimateExtinction(Aloides[,"min_ma"],0.95)
Earliest   Latest 
   5.333     -Inf 
> estimateExtinction(Kurtiella[,"min_ma"],0.95)
 Earliest    Latest 
  0.01170 -34.70966 
> estimateExtinction(Gouldia[,"min_ma"],0.95)
 Earliest    Latest 
 0.011700 -2.047386 
> estimateExtinction(Acrosterigma[,"min_ma"],0.95)
 Earliest    Latest 
 0.011700 -3.481128 
#None of these taxa have intervals beginning with zero. 
#However,six genera have intervals beginning with 0.011700.
#Let's suppose this is close enough to the present that they may be extant.
6/10
[1] 0.6
#We could say that sixty percent of these taxa may still be extant.
````
