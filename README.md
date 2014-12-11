lexicoR
=======

這個套件是用來方便擷取與分析 BIGLEX 資料庫。

##Count the statistics of the scores of manual ratings
##Read in the csv file of the Senti_Lexicon
```{r}
Rating<-read.csv("Emotionword_Rating.csv")
```
##Count the standard deviation, mean and z score of MEAN
```{r}
SD<-apply(Rating,1,sd)
MEAN<-apply(Rating,1,mean)
SCALE<-scale(MEAN)
```
##Combine the three new columns into the original one
```{r}
Rating2<-cbind(Rating,MEAN)
Rating3<-cbind(Rating2,SD)
Senti_Combine<-cbind(Rating3,SCALE)
```

##Combine the collocations of the senti lexicons
##Load in 6 collocation files
```{r}
load(file="/home/amber/senti/afverb")
load(file="/home/amber/senti/afverb2")
load(file="/home/amber/senti/afverb3")
load(file="/home/amber/senti/bfverb1")
load(file="/home/amber/senti/bfverb2")
load(file="/home/amber/senti/bfverb3")
```

##Collapse the elements in the list of the collocation file
```{r}
target<-lapply(afverb,function(x) paste(x,collapse=","))
target2<-lapply(afverb2,function(x) paste(x,collapse=","))
target3<-lapply(afverb3,function(x) paste(x,collapse=","))
target4<-lapply(bfverb1,function(x) paste(x,collapse=","))
target5<-lapply(bfverb2,function(x) paste(x,collapse=","))
target6<-lapply(bfverb3,function(x) paste(x,collapse=","))
```

##Merge the collocations into the charts of senti lexicon
```{r}
Senti_Combine$afverb<-target
Senti_Combine$afverb2<-target2
Senti_Combine$afverb3<-target3
Senti_Combine$bfverb1<-target4
Senti_Combine$bfverb2<-target5
Senti_Combine$bfverb3<-target6
```

##Save the file
```{r}
output <- data.frame(lapply(Senti_Combine, as.character), stringsAsFactors = FALSE)
print(output)
write.csv(output, file = "Senti_Collo_Combined.csv")

```
