Introduction
------------

Go to the app store on your phone and try to find an app to watch videos
and YouTube surfaces as the top-rated. YouTube is the most popular app
and website to watch any video. I often share YouTube videos on lawn
care and planting gardens. YouTube is an American video-sharing website
with headquartes in San Bruno, California. Chad Hurley, Steve Chen, and
Jawed Karim founded YouTube in February 2005. Google bought the site in
November 2006 for US$1.65 billion, and now the largely branded company
operates as one of Google’s affiliates. YouTube allows you to explore
new content, music, news, and more through an official app or the
website. Subscribing to particular channels that house the users
favorite content, sharing videos with friends, or uploading videos for
public scrutiny are some significant features of YouTube.

YouTube most popular videos are labeled as “trending.” Trending helps
viewers to see the top news, events, and miscellaneous situations in the
video world. Trending videos are collected primarily when a wide range
of viewers would find interesting. Some trends are predictable, like a
new song from a famous artist or a new movie trailer. Others are
surprising, like a viral video. Trending is not personalized and
displays the same list of trending videos in each country to all user.
The list of trending videos is updated roughly every 15 minutes. With
each update, videos may move up, down, but rarely stay in the same
position in the list. YouTube maintains a list of the top trending
videos. YouTube uses a combination of factors, including measuring the
number of views, shares, comments, and likes to make the commercials and
determine trending videos.

This dataset is a daily record of the top trending YouTube videos and
the factors that influences them. Analyzing this dataset, we look at the
main factors that are included in making the videos trending such as
likes, dislikes, and comments. We can view what is needed to make a
video be on the trending list. To analyze this data plan is to use
linear regression with the variables likes, dislike, comments, and
names. The deliverable will be a poster.

The data set that used comes from kaggle.com the original data set has
40,881 observations and 14 variables. This data set includes trending
videos in 2017 and 2018. Since I want to become a YouTube personality, I
explore information regarding how videos start to trend. Evaluating, the
likes, dislikes, views, and comments, the data set seems to have
valuable information. This capstone project encompasses all of the
techniques that were learned in this course. It begins with the data
wrangling process - the process of cleaning the data set. Exploratory
data analysis is the next section of the project. It includes the
inferential knowledge that leads to selecting the independent and
dependent variables. In the machine learning section, linear regression
models are built to predict the number of views necessary for a video to
trend. The results are noted, and future work is described.

We aim to help YouTube to provide users with a threshold so they can
know precisely know what is expected for views, likes, dislikes and
comments for their video to be considered as a trending video. The
information gathered and presented provides clear cut information to
users. It also will help YouTube to determine the YouTube channel that
they can use for advertisement purposes.

The First step in for any project is to install and load the necessary
packages by declaring the libraries. The plyr package is to be used to
select the variables in the clean data and the tidy verse for most of
the other functions. Tidyverse is considered the swiss knife because of
its unlimited capabilities. All packages used in the data wrangling
process and the statistics are loaded into the console.

    library(tidyverse) #The swiss knife!

    ## -- Attaching packages ----------------------------------------------------------------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 3.2.0     v purrr   0.3.2
    ## v tibble  2.1.3     v dplyr   0.8.3
    ## v tidyr   0.8.3     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.4.0

    ## -- Conflicts -------------------------------------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

    library(ggplot2)
    library(rmarkdown)
    library(dplyr)
    library(tidyr)
    library(knitr)
    library(caret)

    ## Loading required package: lattice

    ## 
    ## Attaching package: 'caret'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     lift

To begin the cleaning process, we take a peek at the working directory
with the getwd() function. If the working directory is not set to read
the files, then the working directory has to be changed. The working
directory is confirmed, and the data is imported.

    getwd()

    ## [1] "F:/Lopez"

    df <- read.csv("YouTube.csv")

    str(df)

    ## 'data.frame':    40881 obs. of  14 variables:
    ##  $ video_id              : Factor w/ 24104 levels "--45ws7CEN0",..: 14219 586 2731 6677 1594 847 387 1465 11920 2086 ...
    ##  $ trending_date         : Factor w/ 205 levels "17.01.12","17.02.12",..: 14 14 14 14 14 14 14 14 14 14 ...
    ##  $ title                 : Factor w/ 24573 levels "''Gala Artis 2018'' Le numÃ©ro d'ouverture",..: 7886 16653 17304 10553 7746 11410 22920 23362 20575 8523 ...
    ##  $ channel_title         : Factor w/ 5076 levels "- æ¬¢è¿Žè®¢é\230… -æµ\231æ±Ÿå\215«è§†ã\200\220å¥”è·‘å\220§ã\200‘å®\230æ–¹é¢‘é\201“",..: 1430 2067 3755 3216 1391 1304 4767 810 2675 3905 ...
    ##  $ category_id           : int  10 23 23 24 10 25 23 22 24 22 ...
    ##  $ publish_time          : Factor w/ 23613 levels "2008-01-13T01:32:16.000Z",..: 68 260 180 172 55 228 205 265 188 64 ...
    ##  $ tags                  : Factor w/ 20157 levels "''Hace|\"minuto''\"|\"''hace\"|\"minutos''\"|\"''MÃ¡quinas''\"|\"''animales''\"|\"''mejores''\"|\"tops''\"|\"''"| __truncated__,..: 5897 13838 14461 15239 5761 19 6961 15782 11002 7337 ...
    ##  $ views                 : int  17158579 1014651 3191434 2095828 33523622 1309699 2987945 748374 4477587 505161 ...
    ##  $ likes                 : int  787425 127794 146035 132239 1634130 103755 187464 57534 292837 4135 ...
    ##  $ dislikes              : int  43420 1688 5339 1989 21082 4613 9850 2967 4123 976 ...
    ##  $ comment_count         : int  125882 13030 8181 17518 85067 12143 26629 15959 36391 1484 ...
    ##  $ comments_disabled     : logi  FALSE FALSE FALSE FALSE FALSE FALSE ...
    ##  $ ratings_disabled      : logi  FALSE FALSE FALSE FALSE FALSE FALSE ...
    ##  $ video_error_or_removed: logi  FALSE FALSE FALSE FALSE FALSE FALSE ...

The view function gives a table of all observations and variables. Some
of the variables were easier to understand than others. The varialbes
such as likes, dislikes, comment count, and views were plainly
understood. The other variables did not have units specified, which
makes it hard to interpret. There are missing variables in the data set
labeled as NA. Let’s take a glimpse of the data set.

The glimpse function provides more concise information regarding the
data set. A glimpse of the data set is called to determine the type of
variables and ensure that the data set is correct.

    glimpse(df)

    ## Observations: 40,881
    ## Variables: 14
    ## $ video_id               <fct> n1WpP7iowLc, 0dBIkQ4Mz1M, 5qpjK5DgCt4, ...
    ## $ trending_date          <fct> 17.14.11, 17.14.11, 17.14.11, 17.14.11,...
    ## $ title                  <fct> "Eminem - Walk On Water (Audio) ft. Bey...
    ## $ channel_title          <fct> EminemVEVO, iDubbbzTV, Rudy Mancuso, ni...
    ## $ category_id            <int> 10, 23, 23, 24, 10, 25, 23, 22, 24, 22,...
    ## $ publish_time           <fct> 2017-11-10T17:00:03.000Z, 2017-11-13T17...
    ## $ tags                   <fct> Eminem|"Walk"|"On"|"Water"|"Aftermath/S...
    ## $ views                  <int> 17158579, 1014651, 3191434, 2095828, 33...
    ## $ likes                  <int> 787425, 127794, 146035, 132239, 1634130...
    ## $ dislikes               <int> 43420, 1688, 5339, 1989, 21082, 4613, 9...
    ## $ comment_count          <int> 125882, 13030, 8181, 17518, 85067, 1214...
    ## $ comments_disabled      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS...
    ## $ ratings_disabled       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS...
    ## $ video_error_or_removed <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALS...

The table shows that there is 14 variables and 40,881 observations. The
variables are labelled as factor,integer, and lgl.

Data Wrangling
--------------

Data wrangling is one of the most essential parts of a data science
project. Data wrangling is the process of mapping data from raw to a
format appropriate for the data to be analyzed. Data wrangling consists
of selecting, indexing, and mutating the tables to form them for your
need. This process helps with better decision making, but it takes a
long time to complete. If it is done correctly, then a data scientist
life is made easier. These steps will help to deliver more accurate
results. The data used contained 14 variables and 40,881 observations.
They are multiple techniques used to clean and conform the data before
the analysis. The packages have been loaded in a previous code chunk.
The loaded packages allow syntax that is simple to use when cleaning the
data. The first step is to remove unnecessary variables which are the
columns of the data set. Once the columns are removed, the data set is
reviewed using the glimpse function.

    df <- df[c(-1,-4,-5,-6,-7,-12,-13,-14)]
    glimpse(df)

    ## Observations: 40,881
    ## Variables: 6
    ## $ trending_date <fct> 17.14.11, 17.14.11, 17.14.11, 17.14.11, 17.14.11...
    ## $ title         <fct> "Eminem - Walk On Water (Audio) ft. BeyoncÃ©", "...
    ## $ views         <int> 17158579, 1014651, 3191434, 2095828, 33523622, 1...
    ## $ likes         <int> 787425, 127794, 146035, 132239, 1634130, 103755,...
    ## $ dislikes      <int> 43420, 1688, 5339, 1989, 21082, 4613, 9850, 2967...
    ## $ comment_count <int> 125882, 13030, 8181, 17518, 85067, 12143, 26629,...

    nrow(df)

    ## [1] 40881

The table shows that there is 6 variables and 40,881 observations. The
variables are labelled as factor and integer. By removing insignificant
variables from the data frame, the new data set contains the variables
trending date, title likes, view, dislikes, and comment count. Theses
variable provides information pertinent information regarding the
trending videos.

Next, we determine what to do with missing values. There are several
ways to handle the missing variables. Since the data is large, we omit
the variables with missing data. To do this NA is changed to 0 and then
0 is omitted them. The omit function is used to remove the variable with
missing data values.

Since the data set already comprises only trending videos, and that the
likelihood of observing a trending video, opened for comments, receiving
zero comments is minimal. This may no be the best way to handle this
situation but it is a way.

    df[df == 0] <- NA
    df <- na.omit(df)
    glimpse(df)

    ## Observations: 39,924
    ## Variables: 6
    ## $ trending_date <fct> 17.14.11, 17.14.11, 17.14.11, 17.14.11, 17.14.11...
    ## $ title         <fct> "Eminem - Walk On Water (Audio) ft. BeyoncÃ©", "...
    ## $ views         <int> 17158579, 1014651, 3191434, 2095828, 33523622, 1...
    ## $ likes         <int> 787425, 127794, 146035, 132239, 1634130, 103755,...
    ## $ dislikes      <int> 43420, 1688, 5339, 1989, 21082, 4613, 9850, 2967...
    ## $ comment_count <int> 125882, 13030, 8181, 17518, 85067, 12143, 26629,...

Since the missing data values have been deleted the number of
observations are minimized. The table shows that there is 6 variables
and 39,924 observations. The variables types are factor and integer. The
data is numerical. Thus we look at a summary of the data.

    mean(df$views)

    ## [1] 1153527

    summary(df$views)

    ##      Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
    ##      1023    148818    377166   1153527    970374 137843120

The mean number of views is 1,153,527. The clean data set is df and will
be used for the remainder of the project.

Dealing with Outliers
---------------------

The data set has a minimum value of 1023 and a maximum value of
137,843,120. The range of these values is large. The value 1023 doesn’t
seem logical for a trending video. Thus a filter is created to include
only observations at a certain threshold. This threshold is 148,818, the
mean of the first quartile. The new data frame is called df\_1. The
technique used here was found on a webpage dedicated to methods for
dealing with outliers. Mr. Goran is not familiar with this method. Thus
we will use the data frame df as the clean data frame for further
analysis. Additionally, dealing with outliers is beyond the scope of
this course.

    df_1<-df %>% filter(views>=148818)
    glimpse(df_1)

    ## Observations: 29,943
    ## Variables: 6
    ## $ trending_date <fct> 17.14.11, 17.14.11, 17.14.11, 17.14.11, 17.14.11...
    ## $ title         <fct> "Eminem - Walk On Water (Audio) ft. BeyoncÃ©", "...
    ## $ views         <int> 17158579, 1014651, 3191434, 2095828, 33523622, 1...
    ## $ likes         <int> 787425, 127794, 146035, 132239, 1634130, 103755,...
    ## $ dislikes      <int> 43420, 1688, 5339, 1989, 21082, 4613, 9850, 2967...
    ## $ comment_count <int> 125882, 13030, 8181, 17518, 85067, 12143, 26629,...

The table shows that the data set df\_1 has 6 variables and 29,943
observations. Let’s take a look at the summary of df\_1 to view the
changes in the mean number of views.

    summary(df_1$views)

    ##      Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
    ##    148823    304129    586076   1512629   1325222 137843120

There is evidence that some trending videos are more popular than
others. A popularity index is created to distinguish between highly
trending and trending videos based on the number of views. The threshold
is selected to be 586,076 the medeian number of views.

    df_1$Popularity_Index <- ifelse(( df_1$views >= 586076), '1', '0')

We take one last look at the clean data set and write it to the console.

    glimpse(df_1)

    ## Observations: 29,943
    ## Variables: 7
    ## $ trending_date    <fct> 17.14.11, 17.14.11, 17.14.11, 17.14.11, 17.14...
    ## $ title            <fct> "Eminem - Walk On Water (Audio) ft. BeyoncÃ©"...
    ## $ views            <int> 17158579, 1014651, 3191434, 2095828, 33523622...
    ## $ likes            <int> 787425, 127794, 146035, 132239, 1634130, 1037...
    ## $ dislikes         <int> 43420, 1688, 5339, 1989, 21082, 4613, 9850, 2...
    ## $ comment_count    <int> 125882, 13030, 8181, 17518, 85067, 12143, 266...
    ## $ Popularity_Index <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", ...

    write.csv(df_1, file = "df_1clean.csv")

Statistical Analysis
--------------------

Now that the data set is cleaned, it is time for the statistical
analysis. This analysis is the exploratory portion of the project. At
this time, we are trying to find trends in the project data based on the
facts and develop a solid hypothesis. In this portion of the coding in
R, plots are created and analyzed. A deep analysis of the trending
videos is investigated.

The necessary libraries needed for the Statistical Analysis portion were
installed and are loaded here.

All of the packages are loaded. Therefore, we load the data into R and
call the data frame dta.

When reading in the data file, an additional variable appears. That
variable is removed. A glimpse of the data set is called to determine
the type of variables and ensure that the data set is correct.

    glimpse(df)

    ## Observations: 39,924
    ## Variables: 6
    ## $ trending_date <fct> 17.14.11, 17.14.11, 17.14.11, 17.14.11, 17.14.11...
    ## $ title         <fct> "Eminem - Walk On Water (Audio) ft. BeyoncÃ©", "...
    ## $ views         <int> 17158579, 1014651, 3191434, 2095828, 33523622, 1...
    ## $ likes         <int> 787425, 127794, 146035, 132239, 1634130, 103755,...
    ## $ dislikes      <int> 43420, 1688, 5339, 1989, 21082, 4613, 9850, 2967...
    ## $ comment_count <int> 125882, 13030, 8181, 17518, 85067, 12143, 26629,...

The table shows that there is 7 variables and 29,943 observations. The
variables are labelled as factor and integer.

Since there is an enormous amount of data for trending videos, the means
of each variable is explored. The code to find the means is written
below.

    daily_means <- df  %>%
      group_by(trending_date) %>%
      summarise(n_title = n(),
                mean_likes = mean(likes),
                 mean_views = mean(views),
                 mean_dislikes = mean(dislikes),
                mean_comment_count = mean(comment_count))
    daily_means

    ## # A tibble: 205 x 6
    ##    trending_date n_title mean_likes mean_views mean_dislikes
    ##    <fct>           <int>      <dbl>      <dbl>         <dbl>
    ##  1 17.01.12          195     41083.   1134989.         2162.
    ##  2 17.02.12          196     52468.   1239664.         1813.
    ##  3 17.03.12          194     60609.   1531996.         2225.
    ##  4 17.04.12          193     63517.   1793375.         2683.
    ##  5 17.05.12          190     64213.   1829766.         2548.
    ##  6 17.06.12          192     51453.   1488403.         1552.
    ##  7 17.07.12          192     55936.   1760302.         4232.
    ##  8 17.08.12          196     36736.   1206701.         5980.
    ##  9 17.09.12          196     49789.   1598892.         7652.
    ## 10 17.10.12          198     50658.   1542022.         8214.
    ## # ... with 195 more rows, and 1 more variable: mean_comment_count <dbl>

The mean for the variables, likes, dislikes, views, and comment\_count
is collect for each trending date. The table also shows the number of
videos per date. A graphical analysis of the means is annotated in the
code below. We conduct this anlysis as it reduces the number of data
points and patterns can be seen.

    ggplot(daily_means, aes(y = mean_views, x = mean_likes, color=mean_dislikes)) + geom_point()

![](Capstone_Project_Update_files/figure-markdown_strict/unnamed-chunk-14-1.png)

    ggplot(daily_means, aes(y = mean_views, x = mean_likes, color=(mean_comment_count))) + geom_point() 

![](Capstone_Project_Update_files/figure-markdown_strict/unnamed-chunk-14-2.png)

    ggplot(daily_means, aes(y = mean_views, x = mean_comment_count,  color=mean_dislikes)) + geom_point() 

![](Capstone_Project_Update_files/figure-markdown_strict/unnamed-chunk-14-3.png)

    ggplot(daily_means, aes(y = mean_views, x = mean_dislikes,color=mean_comment_count)) + geom_point() 

![](Capstone_Project_Update_files/figure-markdown_strict/unnamed-chunk-14-4.png)
The plots show strong positive relationships between the means of the
variables. Thus there may be a strong correlation between the actual
variables. Note that a trending video that has an extremely large number
of likes and views it also has a vast amount of dislikes and comments.
Plots for additional anlysis betwen the means are below. The ralations
hip beteen the variables is strong.

    ggplot(daily_means, aes(y = mean_dislikes, x = mean_likes, color=(mean_comment_count))) + geom_point() 

![](Capstone_Project_Update_files/figure-markdown_strict/unnamed-chunk-15-1.png)

    ggplot(daily_means, aes(x =mean_likes , y = mean_comment_count, )) + geom_point()

![](Capstone_Project_Update_files/figure-markdown_strict/unnamed-chunk-15-2.png)

    ggplot(daily_means, aes(y = mean_dislikes, x =mean_comment_count , )) + geom_point() 

![](Capstone_Project_Update_files/figure-markdown_strict/unnamed-chunk-15-3.png)

    ggplot(df, aes(y = views, x = likes, color=dislikes)) + geom_point() 

![](Capstone_Project_Update_files/figure-markdown_strict/unnamed-chunk-16-1.png)

    ggplot(df, aes(y = views, x = likes, color=comment_count)) + geom_point() 

![](Capstone_Project_Update_files/figure-markdown_strict/unnamed-chunk-16-2.png)

    ggplot(df, aes(y = views, x = comment_count, color=dislikes)) + geom_point()

![](Capstone_Project_Update_files/figure-markdown_strict/unnamed-chunk-16-3.png)

    ggplot(df, aes(y = views, x = dislikes, color=comment_count)) + geom_point() 

![](Capstone_Project_Update_files/figure-markdown_strict/unnamed-chunk-16-4.png)

To the naked eye, the relationship between the actual variables does not
appear to have a strong linear relationship. A large amount of the data
is clumped around the (0,0) point. Nevertheless, there is a positive
relationship between the variables in the clean data set. It is
worthwhile to note that videos that have a low number of dislikes and a
moderately high number of views have high levels of comments. Lets view
the box plots for the clean data set.

Let’s take a look at the Popularity\_Index variable that was created. I
can remove this if you like.

    df_1$Popularity_Index <- ifelse(df_1$Popularity_Index == 1, 'Highly Popular', 'Popular')
    ggplot(df_1, aes(x = likes, y = views, color=Popularity_Index)) +
      geom_point() 

![](Capstone_Project_Update_files/figure-markdown_strict/unnamed-chunk-17-1.png)

    ggplot(df_1, aes(x = dislikes, y = views, color=Popularity_Index)) +
      geom_point() 

![](Capstone_Project_Update_files/figure-markdown_strict/unnamed-chunk-17-2.png)

    ggplot(df_1, aes(x = comment_count, y = views, color=Popularity_Index)) +
      geom_point() 

![](Capstone_Project_Update_files/figure-markdown_strict/unnamed-chunk-17-3.png)

Recall that the variables Popularity \_Index was determined by the mean
of views in the original data set. It seems as if the clean data set
includes the majority of highly popular videos. Looking at the plots
there are only a few popular trending videos as indicated by the small
splash of mint green.

    write.csv(daily_means, file = "daily_means.csv")

Machine Learning
----------------

The plots created, and a clearer understanding of the data set has been
achieved. The models below explores the relationship between the
dependent variables and the independent variables. After creating
multiple models, we conclude that the model of best fit has a dependent
variable is views, and the independent variables are likes, dislikes,
and comments. Since the dependent variable is a numerical value,
multiple linear regression will be used as the machine learning
technique.For the machine learning portion of the capstone project, the
number of views is predicted given the number of likes, dislikes, and
comments.

Before the linear model is created, the data is divided into testing and
training data. The divide used is 80% to train the model and 20% of the
data to test the model. To determine if the model of best fit is a good
model the metrics R-squared RMSE, and p-value are observed. The model is
built using the original data and not the daily means of the the data.
The code below splits the data set. Set seed gives a starting point for
splitting the data.

    set.seed(7)
    validation_index <- createDataPartition(df$views, p=0.80, list=FALSE)
    testing <- df[-validation_index,]
    # use the remaining 80% of data to training and testing the models
    training <- df[validation_index,]

Since the method of choice is multiple linear regression, there are four
model choices with views as the dependent variable. The models will
include a combination of independent variables in the form, (1) likes
and dislikes, (2) comment\_count and likes, (3) comment\_count and
dislikes, and (4) comment\_count, dislikes, and likes.

Model1
------

In model1, the dependent variable is views, and the dependent variable
is dislikes. The independent variable is statistically significant but
*R*<sup>2</sup> is only 0.3384. We can produce a better model.

    model1 <- lm(views ~ dislikes, likes , data = training)
    summary(model1)

    ## 
    ## Call:
    ## lm(formula = views ~ dislikes, data = training, subset = likes)
    ## 
    ## Residuals:
    ##       Min        1Q    Median        3Q       Max 
    ## -26420216   -831513   -639197   -104119  75658661 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 9.598e+05  1.841e+04   52.14   <2e-16 ***
    ## dislikes    7.610e+01  6.813e-01  111.70   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2864000 on 24400 degrees of freedom
    ##   (7538 observations deleted due to missingness)
    ## Multiple R-squared:  0.3384, Adjusted R-squared:  0.3383 
    ## F-statistic: 1.248e+04 on 1 and 24400 DF,  p-value: < 2.2e-16

Model2
------

In model1, the dependent variable is views, and the dependent variable
is dislikes. The independent variable is statistically significant but
*R*<sup>2</sup> is only 0.5154. We can produce a better model.

    model2 <- lm(views ~ comment_count, likes , data = training)
    summary(model2)

    ## 
    ## Call:
    ## lm(formula = views ~ comment_count, data = training, subset = likes)
    ## 
    ## Residuals:
    ##       Min        1Q    Median        3Q       Max 
    ## -41892227   -559013   -435573    -70504  57849363 
    ## 
    ## Coefficients:
    ##                Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   5.926e+05  1.606e+04    36.9   <2e-16 ***
    ## comment_count 1.017e+02  6.314e-01   161.1   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2451000 on 24400 degrees of freedom
    ##   (7538 observations deleted due to missingness)
    ## Multiple R-squared:  0.5154, Adjusted R-squared:  0.5154 
    ## F-statistic: 2.595e+04 on 1 and 24400 DF,  p-value: < 2.2e-16

Model3
------

In model3, the dependent variable is views, and the independent
variables are dislikes, and comments. The independent variables are
statistically significant but *R*<sup>2</sup> is only 0.4954. We can
produce a better model.

    model3 <- lm(views ~ dislikes + comment_count , data = training)
    summary(model3)

    ## 
    ## Call:
    ## lm(formula = views ~ dislikes + comment_count, data = training)
    ## 
    ## Residuals:
    ##       Min        1Q    Median        3Q       Max 
    ## -48738036   -589848   -457070    -88300  64616809 
    ## 
    ## Coefficients:
    ##                Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   6.310e+05  1.381e+04   45.69   <2e-16 ***
    ## dislikes      3.162e+01  9.911e-01   31.90   <2e-16 ***
    ## comment_count 8.944e+01  7.808e-01  114.55   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2402000 on 31937 degrees of freedom
    ## Multiple R-squared:  0.4954, Adjusted R-squared:  0.4953 
    ## F-statistic: 1.568e+04 on 2 and 31937 DF,  p-value: < 2.2e-16

Model4
------

In model4, the dependent variable is views, and the independent
variables are likes, dislikes, and comment\_count. The independent
variables are statistically significant but *R*<sup>2</sup> is only
0.7496. This is the best model for all possible combinations of more
than one independent variable from the choice of, likes, dislikes, and
comment count.

It is also worth noting that the model created is better than the model
in the first capstone submission. Model4 from the first submission had a
*R*<sup>2</sup> = .7293. The metods, I used to filter the outlier were
not reliable as suggested by Mr. Goran.

    model4 <- lm(views ~ dislikes + comment_count + likes, data = training)
    summary(model4)

    ## 
    ## Call:
    ## lm(formula = views ~ dislikes + comment_count + likes, data = training)
    ## 
    ## Residuals:
    ##       Min        1Q    Median        3Q       Max 
    ## -26374458   -309543   -201520     54105  46021268 
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)    3.185e+05  9.882e+03   32.23   <2e-16 ***
    ## dislikes       5.366e+01  7.087e-01   75.71   <2e-16 ***
    ## comment_count -4.177e+01  9.128e-01  -45.76   <2e-16 ***
    ## likes          2.330e+01  1.294e-01  180.10   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1692000 on 31936 degrees of freedom
    ## Multiple R-squared:  0.7496, Adjusted R-squared:  0.7496 
    ## F-statistic: 3.188e+04 on 3 and 31936 DF,  p-value: < 2.2e-16

Model of Best Fit (Model4)
--------------------------

This is the fourth model and it is the model of best fit.
*y* = 53.66(*d**i**s**l**i**k**e**s*) − 41.77(*c**o**m**m**e**n**t*\_*c**o**u**n**t*) + 23.30(*l**i**k**e**s*)
We are interested in whether the total number of views for a trending
video can be predicted given the number of likes and dislikes.  
The p-value helps to determine if each independent variable is
statistically significant. A p-value is statistically significant and
will be indicated by a darkened dot, *, **, or *** if the p-value is
between 0 and 0.01. The independent variables, likes, dislikes, and
comment\_count in the summary table have p values in the following
interval 0 &lt; *p* − *v**a**l**u**e* &lt; 0.01. Thus these variables
are high statistically significant.

We now address the quality of the linear regression fit using
*R*<sup>2</sup> and the residual standard error (RSE). The symbol from
RSE is “*σ*<sup>2</sup>.” The RSE is an estimate of standard error
deviation. That is, it is the average deviation from the true regression
line. In the resulting output, the RSE is 1692000. This value reflects
that the actual number of views deviates from the true regression line
an average of approximately 1692000 views. The RSE indicates that the
model has variability in fitting the data.

The *R*<sup>2</sup> statistic provides an alternative measure of fit. It
measures the proportion of the variability in *Y* that can be explained
using *X*. The *R*<sup>2</sup> statistic takes on a value,
0 ≤ *R*<sup>2</sup> ≤ 1. An *R*<sup>2</sup> value near one indicates
that the regression has explained a large proportion of the variability
in the response. In the outcome summary for the model,
*R*<sup>2</sup> = 0.7496. Since this value is close to 1, we can
conclude that the regression did explain much of the variability in the
number of views.

Information from the *R*<sup>2</sup> and *σ*<sup>2</sup> statistics
determine that the model was a good fit but has room to be better.
Selecting additional independent variables and/or removing independent
variables may have a significant impact on the model. Finding a best
practice for removing outliers may be helpful in making the model
better.

    model5 <- lm(likes ~ dislikes + comment_count + views, data = testing)
    summary(model5)

    ## 
    ## Call:
    ## lm(formula = likes ~ dislikes + comment_count + views, data = testing)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -924642   -6667    -347    3223  909518 
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   -2.036e+03  5.657e+02  -3.598 0.000322 ***
    ## dislikes      -1.817e+00  3.199e-02 -56.786  < 2e-16 ***
    ## comment_count  3.889e+00  4.472e-02  86.965  < 2e-16 ***
    ## views          2.211e-02  2.411e-04  91.711  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 47040 on 7980 degrees of freedom
    ## Multiple R-squared:  0.8541, Adjusted R-squared:  0.854 
    ## F-statistic: 1.557e+04 on 3 and 7980 DF,  p-value: < 2.2e-16

\`\`\` Running the same model on the testing data set shows that
*R*<sup>2</sup> = 0.8541 and the RSE is significantly lower. This shows
that there is inconsistency in the training and testing models. In both
models the indepedent variables have the same level of significance.

Recommendations
---------------

1.  The model is a great predicator for determining the number of views
    given likes, dislikes, and comments. However becasue there is some
    inconsitency with the model for the training and testing data set,
    the counter for the code to split the data set should be changed.
    This could give a more reliable training and testing data set.
2.  Use classification models to make sense of the more massive amount
    of categorical data in the original data set instead of removing the
    data. Significant infromation can be determined from all aspects of
    the data in the data set.
3.  Perform the same analysis to determine if the data is consistent
    for 2019. Performing the same test on different years can determine
    if the independent variables, likes, dislikes, and comments are
    still statistically significant. We can also view the
    *R*<sup>2</sup> value to learn if the independent variables are
    still good predictors of views-dependent variable.

Future Work
-----------

What will happen if the model is built for categorical values? Will the
data stil be a perfect match? These are all questions availabe for
fututre analysis. Building more efficient models and developing sound
reasoning for selecting covariates for the models is an aspect that can
enhance this project. Using likes, dislikes, or comments as a dependent
variable instead of views may have a significant impact on the model.
