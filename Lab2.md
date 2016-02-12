# Lab 2 Exercise

> Final Grade 17.8/20

## Part 1 Questions

````R
tinysmooth<-Ammonites[(c(5,14,23,24)),]
tinyribs<-Ammonites[(c(2,6,12,18)),]
mediumsmooth<-Ammonites[(c(3,7,13,15,16,17,22)),]
mediumribs<-Ammonites[(c(4,20,21)),]
largesmooth<-Ammonites[(c(9,10,19,25)),]
largeribs<-Ammonites[(c(1,8,11)),]

tinysmoothUD<-tinysmooth[,"UD"]
tinyribsUD<-tinyribs[,"UD"]
mediumsmoothUD<-mediumsmooth[,"UD"]
mediumribsUD<-mediumribs[,"UD"]
largesmoothUD<-largesmooth[,"UD"]
largeribsUD<-largeribs[,"UD"]

# example calculation
mean(largeribsUD)-mean(mediumribsUD)
[1] 0.09

compareAmmonites<-function(largeribsUD,mediumribsUD,Iterations=100){
  SamplingDistribution<-c(largeribsUD,mediumribsUD)
  ReplicatedMeans<-array(data=NA,dim=Iterations)
  for(Counter in 1:Iterations) {
     NewLargeRibs<-sample(SamplingDistribution,length(largeribsUD),replace=TRUE)
     NewMediumRibs<-sample(SamplingDistribution,length(mediumribsUD),replace=TRUE)
     LargeRibsMean<-mean(NewLargeRibs)
     MediumRibsMean<-mean(NewMediumRibs)
     ReplicatedMeans[Counter]<-LargeRibsMean-MediumRibsMean
     }
   return(ReplicatedMeans)
   }
   
ReplicationResults<-compareAmmonites(largeribsUD,mediumribsUD,100)
length(which(ReplicationResults>0.09))

largeribs,mediumribs: 0 (no statistically discernible difference in shape at  all)
tinysmooth,tinyribs: 3 (no significant difference in shape)
````

> It's great that you showed all of your code, but try commeting at each step so that I can see what you are doing/trying to accomplish. Commenting in R uses a #

1) I recognize four species:

"tiny":5,14,23,24,2,6,12,18
"mediumsmooth":3,7,13,15,16,17,22
"largesmooth":9,10,19,25
"largeribs":1,8,11,4,20,21,

Originally, there were six. Shape-based calculations suggested that "tinysmooth" and "tinyribs" were 
likely the same species, and that "mediumribs" were probably juveniles or females of "largeribs"

2) The distinctions are based on three rough size classes; "tiny","medium", and "large", the presence of ridges on the shell, and some comparisons of shape (umbilical width to diameter).  

3) It is probable that some of the other "tiny" specimens are still in fact juvenile individuals of larger species. 
However, this is difficult to confirm using only simple morphological clues. 

> What about the degree of coiling, complexity of ribbing, and number of sutres?

4) Likewise, it is possible that smaller "species" are in fact females of larger species. Because 
sexual dimorphism in ammonites usually isn't discernible before adulthood, and because determining which
individuals are adults and which are juveniles is difficult, identifying specimens as female (or adult, for
that matter) continues to be an exercise in guesswork. 

> Generally it is the female that is larger than the male. -0.75

## Part 2 Questions Subsection 1

1) 
````R
> names(plethodon)
[1] "land"    "links"   "species" "site"    "outline"
````

2. 
````R
> class(plethodon[[1]])
[1] "array"
````

> You were supposed to give the class of each object in the list, not just the first. -0.25 points.

3) 
````R
> dim(plethodon[[1]])
[1] 12  2 40
````

## Part 2 Questions Subsection 2

1)
````R
> names(hummingbirds)
[1] "land"     "curvepts"
````

> The question was which is the landmark data... -0.5 points

2)   
````R
ProcrustesHummingbirds<-gpagen(hummingbirds[["land"]])
````

3)
````R
plotTangentSpace(ProcrustesHummingbirds[["coords"]],warpgrids=FALSE,verbose=FALSE)
````

4) It's difficult to say how many species are here. I theorize that there are two species, with substantial outliers.

## Part 3 Questions

1) Fangs longer than six inches

2) Sulfurous odor

3) Adorable eyelashes

4) C,D, and E

5) Laser death ray

6) Synapomorphy

7) 
+ Monophyletic
+ Polyphyletic
+ Monophyletic

8) This is inadvisable, as all members of the group are not derived from a common ancestor.

9) 
+ Paraphyletic
+ Monophyletic
+ Paraphyletic 
+ Monophyletic
+ Polyphyletic

## Part 4 Questions
1) 
Paedogenesis


> -0.5 points

2) 
Species E

> You didn't explain your reasoning, also there is no species E, just a specimen E. -0.5 points

3) Paedogenesis
