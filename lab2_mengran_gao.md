lab2
================
mgao05
February 9, 2017

For Lab 02: Recreate the Random Walk gif in the attached Animations in R tutorial.
----------------------------------------------------------------------------------

``` r
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
```

    ## Executing: 
    ## ""convert" -loop 0 -delay 30 Rplot1.png Rplot2.png Rplot3.png
    ##     Rplot4.png Rplot5.png Rplot6.png Rplot7.png Rplot8.png
    ##     Rplot9.png Rplot10.png Rplot11.png Rplot12.png Rplot13.png
    ##     Rplot14.png Rplot15.png Rplot16.png Rplot17.png Rplot18.png
    ##     Rplot19.png Rplot20.png Rplot21.png Rplot22.png Rplot23.png
    ##     Rplot24.png Rplot25.png Rplot26.png Rplot27.png Rplot28.png
    ##     Rplot29.png Rplot30.png Rplot31.png Rplot32.png Rplot33.png
    ##     Rplot34.png Rplot35.png Rplot36.png Rplot37.png Rplot38.png
    ##     Rplot39.png Rplot40.png Rplot41.png Rplot42.png Rplot43.png
    ##     Rplot44.png Rplot45.png Rplot46.png Rplot47.png Rplot48.png
    ##     Rplot49.png Rplot50.png Rplot51.png Rplot52.png Rplot53.png
    ##     Rplot54.png Rplot55.png Rplot56.png Rplot57.png Rplot58.png
    ##     Rplot59.png Rplot60.png Rplot61.png Rplot62.png Rplot63.png
    ##     Rplot64.png Rplot65.png Rplot66.png Rplot67.png Rplot68.png
    ##     Rplot69.png Rplot70.png Rplot71.png Rplot72.png Rplot73.png
    ##     Rplot74.png Rplot75.png Rplot76.png Rplot77.png Rplot78.png
    ##     Rplot79.png Rplot80.png Rplot81.png Rplot82.png Rplot83.png
    ##     Rplot84.png Rplot85.png Rplot86.png Rplot87.png Rplot88.png
    ##     Rplot89.png Rplot90.png Rplot91.png Rplot92.png Rplot93.png
    ##     Rplot94.png Rplot95.png Rplot96.png Rplot97.png Rplot98.png
    ##     Rplot99.png Rplot100.png "random_walk.gif""

    ## Output at: random_walk.gif

    ## [1] TRUE

``` r
dev.off()  # END OF GIF FUNCTION
```

    ## png 
    ##   3
