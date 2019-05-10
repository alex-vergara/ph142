# ph142

---
---
title: "Sarbicola_GDark"
output: html_document
---

Loading libraries
```{r}
library(ggplot2)
library(tidyr)
library(dplyr)
ibrary(dplyr)
library(forcats)
library(broom)
```

Opening documents
```{r message=FALSE, warning=FALSE}
setwd("~/Dropbox/Research/Schefflera/Gdark_leaflets/Clean/")
ArtDark<-read.csv("GDARK_ArtificialDark.csv", header = T)
Predawn<-read.csv("GDARK_Predawn.csv", header = T)
#GMIN<-na.omit(GMINraw)
attach(ArtDark)
attach(Predawn)

setwd("~/Dropbox/Research/Schefflera/Gmin_leaflets/arboricola/Clean Data/")
Midday<-read.csv("GMIN_Sarbicola_Midday_Wholeleaf.csv", header = T)
attach(Midday)
na.exclude(Midday)
ArtDark$treatment= "Artificial"
Predawn$treatment= "Predawn"
Midday$treatment= "Midday"

is.factor(GDARKtemp$Time)
is.factor(Midday$Time)
```

```{r}
GDARKtemp=full_join(ArtDark,Predawn)
Midday.clean<-Midday %>% dplyr::select(-Time, -LoggerDate, - LoggerTime)
GDARKall=full_join(GDARKtemp,Midday.clean)
```

```{r}
GDARKall_final <- GDARKall %>% 
  mutate(reordered = fct_relevel(treatment, "Midday", "Artificial", "Predawn"))

ggplot(data=GDARKall_final, aes(x=reordered, y=FINALgmin_mmol_m.2s.1))+
  geom_boxplot(aes(fill=reordered))

```
