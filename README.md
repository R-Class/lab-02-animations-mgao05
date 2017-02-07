# lab-02-animations-mgao05
lab-02-animations-mgao05 created by GitHub Classroom
```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
library(ggmap)
library(animation)
library(ggplot2)
library(dplyr)
library(pander)

```

## 1.Follow the steps in the Dates tutoial to read in the code violation data and drop all variables except violation dates, violation types, and their coordinates (lat,lon).


```{r import data and drop }
#import data
data.url <- "https://raw.githubusercontent.com/lecy/code-orange/master/data/code%20violations.csv"

dat <- read.csv( data.url, stringsAsFactors=F)

#subset dataframe
mydat <- dat[ , c("Complaint.Type","Violation.Date","lat","lon") ]
```

#2.Convert the Violation.Date from a character into a date class.
#3.Create new variables for days of the week (Mon, Tues, Wed, etc.), weeks of the year (1-52), months, and years.
```{r date class}
#transform dates

class(mydat$Violation.Date)
mydat$Violation.Date <- as.Date(mydat$Violation.Date, format = "%m/%d/%Y")

#format as years and make a table based on years
as.year <- format(mydat$Violation.Date, format = "%Y")

table(as.year) %>% pander

#format as months, and maka table based on different months

as.months <- format(mydat$Violation.Date, format = "%b")
table(as.months) %>% pander

#format as days of the week and make a table based on days of the week

as.days <- format(mydat$Violation.Date, format = "%A")
table(as.days) %>% pander

#format as weeks of the year
as.weeks <- format(mydat$Violation.Date, format = "%V")
```


##4.Select a category of code violations other than bed bugs and create a new dataset with all other data dropped.

```{r Infestation  violations }
syr <- get_map(location = "syracuse", zoom=13, maptype = "toner-lite")
ggmap(syr)
syr.min.lat <- 42.96
syr.max.lat <- 43.12
syr.min.lon <- -76.25
syr.max.lon <- -76.05
mydat <- mydat [mydat$lat > syr.min.lat & mydat$lat < syr.max.lat, ]
mydat <- mydat [mydat$lon > syr.min.lon & mydat$lon < syr.max.lon, ]

#check complaint numbers for each type
sort(table(mydat$Complaint.Type), decreasing = T)

# filter by count of violation number of types,only the top 10 frequency
top10viola <- names (sort(table(mydat$Complaint.Type), decreasing = T) [1:10])
top10viola <- mydat[mydat$Complaint.Type %in% top10viola , ]
class(top10viola$Complaint.Type)
top10viola$Complaint.Type <- as.factor(top10viola$Complaint.Type)
levels(top10viola$Complaint.Type)
#add legend to violation types 
new.labels <- c("Bed Bugs", "No Permit", "General", "Fire Hazard", "Trash", "Infestation", "Overgrown", "Exterior Maintenance", "Interior Maintenance", "Trash")
levels(top10viola$Complaint.Type) <- new.labels
qmplot(lon, lat, data = top10viola, maptype = "toner-lite", color=Complaint.Type) + facet_wrap(~ Complaint.Type)

#add month and year 
levels(top10viola$Complaint.Type)
as.month <- format( mydat$Violation.Date, "%b" )

as.year <- format(mydat$Violation.Date, "%Y")

mydat$Month <- as.month

mydat$Year <- as.year

qmplot(lon, lat, data=mydat, maptype = "toner-lite", color=I("red"), alpha = 0.3) + theme(legend.position = "none")
qmplot(lon, lat, data=mydat, maptype = "toner-lite", color=I("red"), geom = "density2d")

infestation <- mydat[mydat$Complaint.Type == "Infestation",]
qmplot(lon,lat, data = infestation, maptype = "toner-lite", color =I("red"), alpha= 0.3)


```

# 6.Select one year of data. Using the qmplot() function in the ggmap package, create a plot with one map for each month.

```{r each month}
# Select one year of data. Using the qmplot() function in the ggmap package, create a plot with one map for each month.
as.month <- format( mydat$Violation.Date, "%b" )

as.year <- format(mydat$Violation.Date, "%Y")

mydat$Month <- as.month

mydat$Year <- as.year

year2015 <- mydat[mydat$Year == "2015", ]
theme_set(theme_bw())

#remove nas in year2015 dataframe
year2015 <- na.omit(year2015)

# how to do it in ggplot : 
#ggplot( data=year2015, aes( x=lon, y=lat ) ) + geom_point() + facet_wrap( ~ Month )

#show months in qmplot funtion
qmplot(lon, lat, data = year2015, maptype = "toner-lite", 
      color = I("orange")) +  facet_wrap( ~ Month )



```
## For Lab 02: Recreate the Random Walk gif in the attached Animations in R tutorial.
    

```{r random walk animation}

# random.walk <- cumsum(rnorm(100))
#   
# plot( random.walk, type="l", col="darkred", axes=F, xlab="", ylab="", main="Random Walk" )
# abline( h=0, lty=2, col="gray" )
# 
# 
# 
# 
# 
# dir.create("gifs")
# setwd("gifs")
# 
# 
# library( animation )
# library(magick)
# 
# saveGIF({
# x <- cumsum( rnorm(100) )
# y <- cumsum( rnorm(100) )
# 
# max.x <- max(x)
# min.x <- min(x)
# max.y <- max(y)
# min.y <- min(y)
# 
# par( ask=F )
# par( mar=c(2,2,3,1) )
# 
# for( i in 1:100 )
# {
#   plot( x[i], y[i], pch=19, cex=2, xlim=c(min.x,max.x), ylim=c(min.y,max.y)  )
# }
#   
# }, 
# 
# movie.name = "rando_walk.gif",   # name of your gif
# interval = 0.3,                  # controls the animation speed
# ani.width = 800,                 # size of the gif in pixels
# ani.height = 800 )               # size of the git in pixels
#above is two demensions animation, both x, y axes change, randow walk only changes randomly on y axes.See codes below.
#




random.walk <- cumsum(rnorm(100))

max.y <- max(random.walk)
min.y <- min(random.walk)

dir.create("gifs")
setwd("gifs")


# par( ask=T )
library(animation)

png( )   # START OF GIF FUNCTION



saveGIF({

for( i in 1:100 )
{
   
   plot(  random.walk[1:i], type="l", col="steelblue", axes=F, xlab="", ylab="", main="Random Walk",
          ylim=c(min.y,max.y), xlim=c(0,100) )
   abline( h=0, lty=2, col="gray" )

}


}, 
movie.name= "random_walk.gif", 
interval = 0.3,
ani.width = 1600,
ani.height = 800
)



dev.off()  # END OF GIF FUNCTION









  x <- cumsum(rnorm(100))
  y <- cumsum(rnorm(100))
  
  max.x <- max(x)
  min.x <- min(x)
  max.y <- max(y)
  min.y <- min(y)
  
  par( ask= T)
  par( MAR=C(2,2,3,1))
  for( i in 5:100 )
  {
    plot(x[i], y[i], pch=19, cex=2, xlim = c(min.x, max.x), ylim = c(min.y, max.y))
  }
  











```
