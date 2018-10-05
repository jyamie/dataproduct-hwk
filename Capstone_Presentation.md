---
title       : College PRO
subtitle    : Data Products Capstone
author      : Jamie
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [bootstrap]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## College PRO: Welcome!

> ### High school student struggling to decide between a couple colleges?

**Don't fret!!** College PRO provides easy, intuitive side-by-side comparison for as many schools as you'd like. 

All *YOU* have to do is input any U.S. school name, and the tool will automatically populate with graphs and data of:
- Standardized test scores
- Racial diversity

### Shiny Application
- URL: *https://jyamie.shinyapps.io/capstone_app/*

### Reproducible Pitch Presentation
- You're right here!

---

## Server Reactive Function

```r
  get_college_data <- reactive({
    return(scorecard[scorecard$INSTNM %in% input$select_college,])
  })
```
> ### Next, we'll walk through an example where the user inputs Wellesley College and University of California-Berkeley into the application

---

## Calculation & analysis in the server: example



```r
# format race columns
scorecard[,c(race_num)] <- apply(scorecard[,c(race_num)], 2, function(x) as.numeric(as.character(x))*100)
# User inputs college(s) they want to investigate, so we will pull those rows from the data
df <- scorecard[scorecard$INSTNM %in% c("Wellesley College", "University of California-Berkeley"),]

# Get demographics data in desired format
df <- melt(df[,c("INSTNM", race_num)])
df <- merge(df, data.frame(race_num, race_dcde), by.x = 'variable', by.y = 'race_num', all.x = TRUE)
head(df,4)
```

| variable | INSTNM | value | race_dcde |
-----------|--------|-------|-----------|
| UGDS_2MOR | Wellesley College | 5.91 | 2+ Races |
| UGDS_ASIAN | Wellesley College | 21.79 | Asian |
| UGDS_UNKN | Wellesley College | 6.00 | Unknown |
| UGDS_WHITE | Wellesley College | 37.49 | White |

--- &twocol

## Next, we plot our data

*** =right

```r
ggplot(df, aes(INSTNM,value)) + 
  geom_bar(stat = "identity", 
           aes(fill = race_dcde)) +
  theme_classic() + 
  labs(fill = "", x = "", y = "%") +
  scale_fill_brewer(palette = "Set3")
```

*** =left
![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png)

