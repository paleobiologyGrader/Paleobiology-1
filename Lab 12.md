Nicholas Boulanger
Lab 12

> 20/20

````R
source("https://raw.githubusercontent.com/aazaff/paleobiologyDatabase.R/master/communityMatrix.R")
Lopingian<-downloadPBDB("Brachiopoda","Lopingian","Lopingian")
PostPermian<-downloadPBDB("Brachiopoda","Triassic","Neogene")
Lopingian<-cleanRank(Lopingian,"genus")
PostPermian<-cleanRank(PostPermian,"genus")

TropicalOccs<-subset(Lopingian,Lopingian[,"paleolat"] >= -23.5 & Lopingian[,"paleolat"] <= 23.5)
ExtraTropicalOccs<-subset(Lopingian,Lopingian[,"paleolat"] <= -23.5 | Lopingian[,"paleolat"] >= 23.5)
TropicalGeneraOccs<-subset(TropicalOccs,TropicalOccs[,"genus"]%in%ExtraTropicalOccs[,"genus"]!=TRUE)
ExtraTropicalGeneraOccs<-subset(ExtraTropicalOccs,ExtraTropicalOccs[,"genus"]%in%TropicalOccs[,"genus"]!=TRUE)

TropicalGenera<-unique(TropicalGeneraOccs[,"genus"])
ExtraGenera<-unique(ExtraTropicalGeneraOccs[,"genus"])

TropicalSurvivors<-intersect(TropicalGenera,unique(PostPermian[,"genus"]))
TropicalVictims<-setdiff(TropicalGenera,unique(PostPermian[,"genus"]))

ExtraSurvivors<-intersect(ExtraGenera,unique(PostPermian[,"genus"]))
ExtraVictims<-setdiff(ExtraGenera,unique(PostPermian[,"genus"]))
TropicalOdds<- (length(TropicalSurvivors)/length(TropicalGenera)) / (length(TropicalVictims)/length(TropicalGenera))
ExtraOdds<- (length(ExtraSurvivors)/length(ExtraVictims)) / (length(ExtraVictims)/length(ExtraGenera))
OddsRatio<- TropicalOdds / ExtraOdds
````

Problem Set 1
TriassicSynapsids<-
TriassicDiapsids<-
JurassicDiapsids<-
JurassicSynapsids<-

> this bit is blank?

1)
````R
TriassicSynapsids<-downloadPBDB(Taxa=c("Synapsida"),StartInterval="Anisian",StopInterval="Rhaetian")
TriassicDiapsids<-downloadPBDB(Taxa=c("Diapsida"),StartInterval="Anisian",StopInterval="Rhaetian")
JurassicDiapsids<-downloadPBDB(Taxa=c("Diapsida"),StartInterval="Jurassic",StopInterval="Neogene")
JurassicSynapsids<-downloadPBDB(Taxa=c("Synapsida"),StartInterval="Jurassic",StopInterval="Neogene")
````

2)
````R
TriassicSynapsids<-cleanRank(TriassicSynapsids,"genus")
TriassicDiapsids<-cleanRank(TriassicDiapsids,"genus")
JurassicDiapsids<-cleanRank(JurassicDiapsids,"genus")
JurassicSynapsids<-cleanRank(JurassicSynapsids,"genus")

length(SynapsidGenus)
length(DiapsidGenus)

length(SynapsidGenus)
[1] 116
> length(DiapsidGenus)
[1] 389
````

3)
````R
JSynapsidGenus<-unique(JurassicSynapsids[,"genus"])
JDiapsidGenus<-unique(JurassicDiapsids[,"genus"])
length(JSynapsidGenus)
length(JDiapsidGenus)
DiapsidSurvivors<-intersect(DiapsidGenus,JDiapsidGenus)
length(DiapsidSurvivors)
[1] 37

#Therefore, there must have been 389-37 = 352 victims.
SynapsidSurvivors<-intersect(SynapsidGenus,JSynapsidGenus)
length(SynapsidSurvivors)
[1] 9
#Therefore, there must have been 107 victims.
````

4) 
````R
Odds Ratio:
SynapsidOdds<- ((9/116)/(107/116))
DiapsidOdds<-((37/389)/(352/389))
OddsRatio<-DiapsidOdds/SynapsidOdds

 OddsRatio
[1] 1.249684
> log(OddsRatio)
[1] 0.222891
````

5)
````R
StandardError<-sqrt(1/9+1/107+1/37+1/352)
StandardError
[1] 0.3877175

UpperLimit<-log(OddsRatio) + (StandardError*1.96)
LowerLimit<-log(OddsRatio) - (StandardError*1.96)

UpperLimit
[1] 0.9828172
> LowerLimit
[1] -0.5370353
#Because the lower limit is negative, our results are not statistically significant. 
````

## Problem Set 2
1)
````R
Triassic<-downloadPBDB(Taxa=c("Synapsida","Diapsida"),StartInterval="Anisian",StopInterval="Rhaetian")
Jurassic<-downloadPBDB(Taxa=c("Synapsida","Diapsida"),StartInterval="Jurassic",StopInterval="Neogene")

Jurassic<-cleanRank(Jurassic,"genus")
Triassic<-cleanRank(Triassic,"genus")
````

2)
`MeanLatitudes<-tapply(Triassic[,"paleolat"],Triassic[,"genus"],mean)`

3)
````R
TriassicSurvivors<-subset(Triassic,Triassic[,"genus"]%in%unique(Jurassic[,"genus"])==TRUE)
TriassicSurvivors<-unique(TriassicSurvivors[,"genus"])

TriassicVictims<-subset(Triassic,Triassic[,"genus"]%in%unique(Jurassic[,"genus"])!=TRUE)
TriassicVictims<-unique(TriassicVictims[,"genus"])
````

4)
````R
Synapsids<-intersect(TriassicSynapsids[,"genus"],Triassic[,"genus"])
Diapsids<-intersect(TriassicDiapsids[,"genus"],Triassic[,"genus"])
````

5)
````R
Victims<-array(0,dim=length(TriassicVictims),dimnames=list(TriassicVictims))
UltimateVictims<-merge(MeanLatitudes,Victims,all=TRUE,by="row.names")
UltimateVictims<-transform(UltimateVictims,row.names=Row.names,Row.names=NULL)
colnames(UltimateVictims)<-c("MeanLatitudes","Survivor/Victim")
UltimateVictims[is.na(UltimateVictims[,"Survivor/Victim"]),]<-1

Regression<-glm(UltimateVictims[,"Survivor/Victim"]~UltimateVictims[,"MeanLatitudes"],family="binomial")
summary(Regression)
Coefficients:
                                     Estimate Std. Error z value Pr(>|z|)    
(Intercept)                        -2.3009122  0.1547326  -14.87   <2e-16 ***
UltimateVictims[, "MeanLatitudes"]  0.0007725  0.0051555    0.15    0.881   
#For each degree increase in latitude, the genus in question's log-odds of survival increase by 0.0007725.
````

````R
# Extra Credit
TriassicGenus<-Triassic[,"genus"]
SynapsidSearch<-array(0,dim=length(Synapsids),dimnames=list(Synapsids))
PowerMatrix<-merge(MeanLatitudes, SynapsidSearch,all=TRUE,by = "row.names")
PowerMatrix<-transform(PowerMatrix,row.names=Row.names,Row.names=NULL)
colnames(PowerMatrix)<-c("MeanLatitudes","Synapsid/Diapsid")
PowerMatrix[is.na(PowerMatrix[,"Synapsid/Diapsid"]),]<-1


UltimateRegression<-glm(UltimateVictims[,"Survivor/Victim"]~UltimateVictims[,"MeanLatitudes"]+ PowerMatrix[,"Synapsid/Diapsid"], family = "binomial")
summary(UltimateRegression)
Coefficients:
                                     Estimate Std. Error z value Pr(>|z|)    
(Intercept)                        -2.4738146  0.3518916  -7.030 2.06e-12 ***
UltimateVictims[, "MeanLatitudes"]  0.0001619  0.0052887   0.031    0.976    
PowerMatrix[, "Synapsid/Diapsid"]   0.2204787  0.3956189   0.557    0.577  
#Status as a Synapsid/Diapsid is much more important than average paleolatitude of occurrences for survival. 
````

> But both are insignificant
