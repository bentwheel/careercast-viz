Be an Actuary: Low stress, high pay. Case closed?
-------------------------------------------------

At the end of 2013, I decided prior to beginning my junior year at the
University of Pennsylvania that I’d wanted to be an actuary. One of the
many compelling forces that had brought me to this particular career
strategy was the widely cited career outlook for actuaries around that
period of time. For example, from
[BeAnActuary.org](https://beanactuary.org/why/?fa=a-top-ranked-job):

(./topjob.JPG)

The link whisks you off to a list of articles, not a single one newer
than 2016, promoting the high-pay/low-stress benefits of becoming an
actuary. It sure is a little stale, and so much has changed with respect
to the bucket of functions we use to map data to decisions since 2016.
So I took to the web to see how the “Actuary” career was faring in 2019
on the same career ranking site that was boasting top billing for the
profession back in 2013.

And, well, things have changed.

Web Scraping: Now (mostly) legal, still the same amount of fun as always
------------------------------------------------------------------------

The web is rich with info, and I love to scrape it. And thanks to a
[relatively recent 9th Circuit
case](https://www.vice.com/en/article/9kek83/linkedin-data-scraping-lawsuit-shot-down),
provided there’s no specific legislation barring the practice or
conflicting circuit court of appeals ruling that requires SCOTUS
intervention, it’s now legal (at least, for non-commercial reasons) and
is likely to remain that way.

Scraping the web is easy to do, and is a great way to make an
un-automated task sail by pretty quickly. If you’re a Python person, you
can use the
[BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/) package
(`pip install beautifulsoup4`). If you’re an R person, then
[rvest](https://github.com/tidyverse/rvest) is the package for you.
Since I like to generally scrape data to `ggplot` it, I almost always
use the `tidyverse`’s `rvest` package (as in “harvest”), but you should
live your best life.

Also, it’s helpful to have a [CSS
SelectorGadget](https://selectorgadget.com/) or similar browser
extension to give your webscraping efforts more surgical precision, but
sometimes it’s easier to look at the page source, or grab the HTML
broadly and whittle it down in code.

CareerCast.com: Does the Actuary gig still get top billing?
-----------------------------------------------------------

I turned my attention to the [CareerCast Top 200 Careers of
2013](https://www.careercast.com/jobs-rated/best-worst-jobs-2013)
report, which ranks careers on an overall index based on job stress
levels, projected career growth outlook, and work environment. Sure
enough, in 2013, there it was in the number one slot:

[](./topjob2013.JPG)

I love being an actuary, and I think most actuaries love being
actuaries, so I shimmied on over to the [CareerCast Top 200 Careers of
2019](https://www.careercast.com/jobs-rated/2019-jobs-rated-report)
report, and there it was, halfway down the Top 20 at \#10.

[](./actuaryjob2019.JPG)

What happened? And what had taken the top spot? Was it… no.. it couldn’t
be. Could it?

[](./dsjob2019.JPG)

Of course it’s No. 1. *Touché*.

[](./newman.jpg)

    library(tidyverse)

    ## -- Attaching packages -------------------------------------------------------------------- tidyverse 1.3.0 --

    ## v ggplot2 3.3.2     v purrr   0.3.4
    ## v tibble  3.0.3     v dplyr   1.0.2
    ## v tidyr   1.1.2     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.5.0

    ## -- Conflicts ----------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

    library(rvest)

    ## Loading required package: xml2

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     pluck

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

    library(xml2)

    scrape_careercast_page <- function(url) 
    {
      # Scrape HTML file from 2019 job report
      
      rankings_html <- read_html(url)
      
      get_rankings_data <- as_list(xml2::xml_find_all(rankings_html,'//*[contains(concat( " ", @class, " " ), concat( " ", "aiNormalizedNumber", " " ))] | //*[contains(concat( " ", @class, " " ), concat( " ", "views-field-field-name", " " ))]')) %>% 
        flatten() %>% 
        flatten() %>% 
        flatten() %>% 
        tibble(data = str_trim(.)) %>% 
        select(data) %>% 
        filter(data != "") %>% 
        mutate(n = (row_number() - 1) %% 6,
               label = case_when(n == 0 ~ "rank",
                                 n == 1 ~ "career",
                                 n == 2 ~ "overall_rank",
                                 n == 3 ~ "work_environment_rank",
                                 n == 4 ~ "stress_rank",
                                 n == 5 ~ "proj_growth_rank")) %>% 
        filter(n != 0) %>% 
        arrange(n) %>% 
        mutate(n = (row_number() - 1) %% 20) %>% 
        group_by(n) %>% 
        pivot_wider(names_from = label, values_from = data) %>% 
        ungroup() %>% 
        select(-n)

        return(get_rankings_data)  
    }

    rankings_url <- "https://www.careercast.com/jobs-rated/2019-jobs-rated-report?page=0"
    scrape_careercast_page(rankings_url)

    ## # A tibble: 20 x 5
    ##    career           overall_rank work_environment_~ stress_rank proj_growth_rank
    ##    <chr>            <chr>        <chr>              <chr>       <chr>           
    ##  1 Data Scientist   1/200        4/200              42/200      37/200          
    ##  2 Statistician     2/200        11/200             43/200      7/200           
    ##  3 University Prof~ 3/200        1/200              5/200       54/200          
    ##  4 Occupational Th~ 4/200        6/200              34/200      22/200          
    ##  5 Genetic Counsel~ 5/200        25/200             15/200      12/200          
    ##  6 Medical Service~ 6/200        39/200             20/200      32/200          
    ##  7 Information Sec~ 7/200        44/200             26/200      15/200          
    ##  8 Mathematician    8/200        55/200             32/200      7/200           
    ##  9 Operations Rese~ 9/200        47/200             8/200       18/200          
    ## 10 Actuary          10/200       8/200              80/200      28/200          
    ## 11 Software Develo~ 11/200       68/200             27/200      22/200          
    ## 12 Speech Patholog~ 12/200       46/200             22/200      41/200          
    ## 13 Dental Hygienist 13/200       48/200             19/200      32/200          
    ## 14 Audiologist      14/200       68/200             4/200       29/200          
    ## 15 Radiation Thera~ 15/200       8/200              67/200      72/200          
    ## 16 Web Developer    16/200       32/200             49/200      54/200          
    ## 17 Application Sof~ 17/200       141/200            27/200      9/200           
    ## 18 Physical Therap~ 18/200       105/200            47/200      15/200          
    ## 19 Physicist        18/200       44/200             78/200      63/200          
    ## 20 Optometrist      20/200       78/200             87/200      41/200

There’s a reason I made this a function - it’s so I could scrape the
CareerCast site from the last ten years, and see what careers have risen
and fallen in their ranking methodology, which has remained completely
consistent and stable over the last decade. My only hope is that their
web design philosophy and CSS naming conventions have remained just as
consistent!

Since it’s only ten years, I’ve chosen to do it by hand, but if we were
doing any more than a decade, this is the kind of stuff `purrr::map()`
is made for.

    # 2019
    rankings_url <- "https://www.careercast.com/jobs-rated/2019-jobs-rated-report?page=0"
    data <- scrape_careercast_page(rankings_url) %>% 
      mutate(year = 2019)

    # 2018 - there is an issue here that has the overall ranking starting at 2 instead of 1. I've corrected for this.
    rankings_url <- "https://www.careercast.com/jobs-rated/2018-jobs-rated-report"
    data2018 <- scrape_careercast_page(rankings_url) %>% 
      mutate(year = 2018)
      
    data2018 <- data2018 %>% 
      separate(overall_rank,  sep="/", into=c("num","den")) %>% 
      mutate(num = as.character(as.numeric(num) - 1)) %>% 
      unite(col="overall_rank", num, den, sep="/")

    data <- data %>% 
      union_all(data2018)

    # 2017 - no issues
    rankings_url <- "https://www.careercast.com/jobs-rated/2017-jobs-rated-report"
    data2017 <- scrape_careercast_page(rankings_url) %>% 
      mutate(year = 2017)

    data <- data %>% 
      union_all(data2017)

    # 2016 - URL format is slightly different.
    rankings_url <- "https://www.careercast.com/jobs-rated/jobs-rated-report-2016-ranking-200-jobs"
    data2016 <- scrape_careercast_page(rankings_url) %>% 
      mutate(year = 2016)

    data <- data %>% 
      union_all(data2016)

    # 2015 - again slightly different URL
    rankings_url <- "https://www.careercast.com/jobs-rated/jobs-rated-report-2015-ranking-top-200-jobs"
    data2015 <- scrape_careercast_page(rankings_url) %>% 
      mutate(year = 2015)

    data <- data %>% 
      union_all(data2015)

    # 2014 - again a slightly different URL
    rankings_url <- "https://www.careercast.com/jobs-rated/jobs-rated-2014-ranking-200-jobs-best-worst"
    data2014 <- scrape_careercast_page(rankings_url) %>% 
      mutate(year = 2014)

    data <- data %>% 
      union_all(data2014)

    # 2013 - again a slightly different URL than last year
    rankings_url <- "https://www.careercast.com/jobs-rated/best-worst-jobs-2013"
    data2013 <- scrape_careercast_page(rankings_url) %>% 
      mutate(year = 2013)

    data <- data %>% 
      union_all(data2013)

    # 2012 - again a slightly different URL than last year
    rankings_url <- "https://www.careercast.com/jobs-rated/2012-ranking-200-jobs-best-worst"
    data2012 <- scrape_careercast_page(rankings_url) %>% 
      mutate(year = 2012)

    data <- data %>% 
      union_all(data2012)

    # 2011 - the same URL format as the 2012 report! huzzah!
    rankings_url <- "https://www.careercast.com/jobs-rated/2011-ranking-200-jobs-best-worst"
    data2011 <- scrape_careercast_page(rankings_url) %>% 
      mutate(year = 2011)

    data <- data %>% 
      union_all(data2011)

    # 2010 is a 404 error, so we'll stop there.

    # As usual, we'll set up a plot function that gives a plot of the ranking we want - overall,
    # job growth, stress, etc.

    plotRanking <- function(metric)
    {
      
      metricLabel = ifelse(metric == "overall_rank","Overall Rank",
                     ifelse(metric == "work_environment_rank","Work Environment Rank",
                            ifelse(metric == "stress_rank","Stress Rank", "Projected Growth Rank")))
      
      plot.data <- data %>% 
        select(career, eval(metric), year) %>% 
        separate(eval(metric), into=c("num","denom"), sep="/") %>% 
        mutate(num = as.numeric(num))
      
      plot.data <- plot.data %>% 
        group_by(career) %>% 
        summarize(n = n()) %>% 
        filter(n >= 4) %>% 
        left_join(plot.data)
      
      plot <- plot.data %>% 
        ggplot(aes(x = year, y = num, color = career)) + 
        geom_line(size = 1.5) +
        geom_point(size = 2) + 
        scale_y_reverse(labels = scales::label_ordinal(), breaks=seq(1:20)) +
        labs(x = "Year",
             y = "Rank",
             title = paste0(metricLabel, " over the Last Decade-ish"),
             subtitle = "CareerCast.com Career Rankings, 2011 - 2019") +
        theme_minimal()
        
      plot
    }

    plotRanking("overall_rank")

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## Joining, by = "career"

![](README_files/figure-markdown_strict/plotting-1.png)
