Antibiotics
================
ariel chen
2025/3/9

*Purpose*: Creating effective data visualizations is an *iterative*
process; very rarely will the first graph you make be the most
effective. The most effective thing you can do to be successful in this
iterative process is to *try multiple graphs* of the same data.

Furthermore, judging the effectiveness of a visual is completely
dependent on *the question you are trying to answer*. A visual that is
totally ineffective for one question may be perfect for answering a
different question.

In this challenge, you will practice *iterating* on data visualization,
and will anchor the *assessment* of your visuals using two different
questions.

*Note*: Please complete your initial visual design **alone**. Work on
both of your graphs alone, and save a version to your repo *before*
coming together with your team. This way you can all bring a diversity
of ideas to the table!

<!-- include-rubric -->

# Grading Rubric

<!-- -------------------------------------------------- -->

Unlike exercises, **challenges will be graded**. The following rubrics
define how you will be graded, both on an individual and team basis.

## Individual

<!-- ------------------------- -->

| Category | Needs Improvement | Satisfactory |
|----|----|----|
| Effort | Some task **q**’s left unattempted | All task **q**’s attempted |
| Observed | Did not document observations, or observations incorrect | Documented correct observations based on analysis |
| Supported | Some observations not clearly supported by analysis | All observations clearly supported by analysis (table, graph, etc.) |
| Assessed | Observations include claims not supported by the data, or reflect a level of certainty not warranted by the data | Observations are appropriately qualified by the quality & relevance of the data and (in)conclusiveness of the support |
| Specified | Uses the phrase “more data are necessary” without clarification | Any statement that “more data are necessary” specifies which *specific* data are needed to answer what *specific* question |
| Code Styled | Violations of the [style guide](https://style.tidyverse.org/) hinder readability | Code sufficiently close to the [style guide](https://style.tidyverse.org/) |

## Submission

<!-- ------------------------- -->

Make sure to commit both the challenge report (`report.md` file) and
supporting files (`report_files/` folder) when you are done! Then submit
a link to Canvas. **Your Challenge submission is not complete without
all files uploaded to GitHub.**

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(ggrepel)
```

*Background*: The data\[1\] we study in this challenge report the
[*minimum inhibitory
concentration*](https://en.wikipedia.org/wiki/Minimum_inhibitory_concentration)
(MIC) of three drugs for different bacteria. The smaller the MIC for a
given drug and bacteria pair, the more practical the drug is for
treating that particular bacteria. An MIC value of *at most* 0.1 is
considered necessary for treating human patients.

These data report MIC values for three antibiotics—penicillin,
streptomycin, and neomycin—on 16 bacteria. Bacteria are categorized into
a genus based on a number of features, including their resistance to
antibiotics.

``` r
## NOTE: If you extracted all challenges to the same location,
## you shouldn't have to change this filename
filename <- "./data/antibiotics.csv"

## Load the data
df_antibiotics <- read_csv(filename)
```

    ## Rows: 16 Columns: 5
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): bacteria, gram
    ## dbl (3): penicillin, streptomycin, neomycin
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
df_antibiotics %>% knitr::kable()
```

| bacteria                        | penicillin | streptomycin | neomycin | gram     |
|:--------------------------------|-----------:|-------------:|---------:|:---------|
| Aerobacter aerogenes            |    870.000 |         1.00 |    1.600 | negative |
| Brucella abortus                |      1.000 |         2.00 |    0.020 | negative |
| Bacillus anthracis              |      0.001 |         0.01 |    0.007 | positive |
| Diplococcus pneumonia           |      0.005 |        11.00 |   10.000 | positive |
| Escherichia coli                |    100.000 |         0.40 |    0.100 | negative |
| Klebsiella pneumoniae           |    850.000 |         1.20 |    1.000 | negative |
| Mycobacterium tuberculosis      |    800.000 |         5.00 |    2.000 | negative |
| Proteus vulgaris                |      3.000 |         0.10 |    0.100 | negative |
| Pseudomonas aeruginosa          |    850.000 |         2.00 |    0.400 | negative |
| Salmonella (Eberthella) typhosa |      1.000 |         0.40 |    0.008 | negative |
| Salmonella schottmuelleri       |     10.000 |         0.80 |    0.090 | negative |
| Staphylococcus albus            |      0.007 |         0.10 |    0.001 | positive |
| Staphylococcus aureus           |      0.030 |         0.03 |    0.001 | positive |
| Streptococcus fecalis           |      1.000 |         1.00 |    0.100 | positive |
| Streptococcus hemolyticus       |      0.001 |        14.00 |   10.000 | positive |
| Streptococcus viridans          |      0.005 |        10.00 |   40.000 | positive |

# Visualization

<!-- -------------------------------------------------- -->

### **q1** Prototype 5 visuals

To start, construct **5 qualitatively different visualizations of the
data** `df_antibiotics`. These **cannot** be simple variations on the
same graph; for instance, if two of your visuals could be made identical
by calling `coord_flip()`, then these are *not* qualitatively different.

For all five of the visuals, you must show information on *all 16
bacteria*. For the first two visuals, you must *show all variables*.

*Hint 1*: Try working quickly on this part; come up with a bunch of
ideas, and don’t fixate on any one idea for too long. You will have a
chance to refine later in this challenge.

*Hint 2*: The data `df_antibiotics` are in a *wide* format; it may be
helpful to `pivot_longer()` the data to make certain visuals easier to
construct.

#### Visual 1 (All variables)

In this visual you must show *all three* effectiveness values for *all
16 bacteria*. This means **it must be possible to identify each of the
16 bacteria by name.** You must also show whether or not each bacterium
is Gram positive or negative.

``` r
# WRITE YOUR CODE HERE
summary(df_antibiotics)
```

    ##    bacteria           penicillin        streptomycin       neomycin     
    ##  Length:16          Min.   :  0.0010   Min.   : 0.010   Min.   : 0.001  
    ##  Class :character   1st Qu.:  0.0065   1st Qu.: 0.325   1st Qu.: 0.017  
    ##  Mode  :character   Median :  1.0000   Median : 1.000   Median : 0.100  
    ##                     Mean   :217.8781   Mean   : 3.065   Mean   : 4.089  
    ##                     3rd Qu.:275.0000   3rd Qu.: 2.750   3rd Qu.: 1.700  
    ##                     Max.   :870.0000   Max.   :14.000   Max.   :40.000  
    ##      gram          
    ##  Length:16         
    ##  Class :character  
    ##  Mode  :character  
    ##                    
    ##                    
    ## 

``` r
glimpse(df_antibiotics)
```

    ## Rows: 16
    ## Columns: 5
    ## $ bacteria     <chr> "Aerobacter aerogenes", "Brucella abortus", "Bacillus ant…
    ## $ penicillin   <dbl> 870.000, 1.000, 0.001, 0.005, 100.000, 850.000, 800.000, …
    ## $ streptomycin <dbl> 1.00, 2.00, 0.01, 11.00, 0.40, 1.20, 5.00, 0.10, 2.00, 0.…
    ## $ neomycin     <dbl> 1.600, 0.020, 0.007, 10.000, 0.100, 1.000, 2.000, 0.100, …
    ## $ gram         <chr> "negative", "negative", "positive", "positive", "negative…

``` r
df_long <-df_antibiotics %>% 
  pivot_longer(cols = c(penicillin, streptomycin, neomycin), names_to = "antibiotics", values_to = "MIC" ) 
df_long%>%
  ggplot( aes(x = bacteria, y = MIC, color = antibiotics, shape = gram)) +
  scale_y_log10() +
  geom_point(size = 4) +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) 
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.1-1.png)<!-- -->

#### Visual 2 (All variables)

In this visual you must show *all three* effectiveness values for *all
16 bacteria*. This means **it must be possible to identify each of the
16 bacteria by name.** You must also show whether or not each bacterium
is Gram positive or negative.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# WRITE YOUR CODE HERE
df_long %>% 
  ggplot( aes(x = bacteria, y = MIC, fill = antibiotics, color = gram)) +
  geom_bar(stat = "identity", position = "dodge") +
  scale_y_log10() +
  scale_color_manual(values = c("positive" = "blue", "negative" = "red")) +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.2-1.png)<!-- -->

#### Visual 3 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# WRITE YOUR CODE HERE
df_long %>% 
  filter(antibiotics == "penicillin") %>% 
  ggplot(aes(x = bacteria, y = MIC, fill = gram))+
  geom_bar(stat = "identity")+
  scale_y_log10()+
  theme_minimal() +
  labs(title = "MIC values across bactaerias with penicillin")+
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.3-1.png)<!-- -->

#### Visual 4 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# WRITE YOUR CODE HERE
df_long %>% 
  filter(antibiotics == "streptomycin") %>% 
  ggplot(aes(x = bacteria, y = MIC, fill = gram))+
  geom_bar(stat = "identity")+
  scale_y_log10()+
  theme_minimal() +
  labs(title = "MIC values across bactaerias with streptomycin")+
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.4-1.png)<!-- -->

#### Visual 5 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# WRITE YOUR CODE HERE
df_long %>% 
  filter(antibiotics == "neomycin") %>% 
  ggplot(aes(x = bacteria, y = MIC, fill = gram))+
  geom_bar(stat = "identity")+
  scale_y_log10()+
  theme_minimal() +
  labs(title = "MIC values across bactaerias with neomycin")+
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.5-1.png)<!-- -->

``` r
df_long %>% 
  ggplot(aes(x = antibiotics, y = MIC, color = bacteria))+
  scale_y_log10()+
  geom_point(size = 3, position = position_jitter(width = 0.2, height = 0))
```

![](c05-antibiotics-assignment_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

``` r
# Convert data to wide format for PCA
bacteria_pca <- df_long %>%
  select(bacteria, antibiotics, MIC) %>%
  spread(key = antibiotics, value = MIC)
bacteria_pca
```

    ## # A tibble: 16 × 4
    ##    bacteria                        neomycin penicillin streptomycin
    ##    <chr>                              <dbl>      <dbl>        <dbl>
    ##  1 Aerobacter aerogenes               1.6      870             1   
    ##  2 Bacillus anthracis                 0.007      0.001         0.01
    ##  3 Brucella abortus                   0.02       1             2   
    ##  4 Diplococcus pneumonia             10          0.005        11   
    ##  5 Escherichia coli                   0.1      100             0.4 
    ##  6 Klebsiella pneumoniae              1        850             1.2 
    ##  7 Mycobacterium tuberculosis         2        800             5   
    ##  8 Proteus vulgaris                   0.1        3             0.1 
    ##  9 Pseudomonas aeruginosa             0.4      850             2   
    ## 10 Salmonella (Eberthella) typhosa    0.008      1             0.4 
    ## 11 Salmonella schottmuelleri          0.09      10             0.8 
    ## 12 Staphylococcus albus               0.001      0.007         0.1 
    ## 13 Staphylococcus aureus              0.001      0.03          0.03
    ## 14 Streptococcus fecalis              0.1        1             1   
    ## 15 Streptococcus hemolyticus         10          0.001        14   
    ## 16 Streptococcus viridans            40          0.005        10

``` r
# Perform PCA (excluding bacteria column)
pca_result <- prcomp(bacteria_pca[, -1], scale. = TRUE)#excludes the first column of bacteria names 
#prcomp: function for priciple component analysis

# Create a dataframe for plotting
pca_df <- data.frame(bacteria = bacteria_pca$bacteria, #adds the bacteria names back
                     PC1 = pca_result$x[, 1], #extracts first pca vector 
                     PC2 = pca_result$x[, 2], #extracts second
                     gram = df_long$gram[match(bacteria_pca$bacteria, df_long$bacteria)])  # Add Gram type back
pca_df
```

    ##                           bacteria        PC1         PC2     gram
    ## 1             Aerobacter aerogenes -0.9578654  1.52888294 negative
    ## 2               Bacillus anthracis -0.5731209 -0.78205432 positive
    ## 3                 Brucella abortus -0.2749643 -0.67396781 negative
    ## 4            Diplococcus pneumonia  1.7485195 -0.05183599 positive
    ## 5                 Escherichia coli -0.5820740 -0.50315172 negative
    ## 6            Klebsiella pneumoniae -0.9537631  1.47917450 negative
    ## 7       Mycobacterium tuberculosis -0.2802382  1.56672423 negative
    ## 8                 Proteus vulgaris -0.5555620 -0.76820466 negative
    ## 9           Pseudomonas aeruginosa -0.8745372  1.51259914 negative
    ## 10 Salmonella (Eberthella) typhosa -0.5153844 -0.75883123 negative
    ## 11       Salmonella schottmuelleri -0.4565649 -0.71332191 negative
    ## 12            Staphylococcus albus -0.5600531 -0.77736455 positive
    ## 13           Staphylococcus aureus -0.5705529 -0.78101044 positive
    ## 14           Streptococcus fecalis -0.4193092 -0.72570692 positive
    ## 15       Streptococcus hemolyticus  2.1977884  0.10693824 positive
    ## 16          Streptococcus viridans  3.6276816  0.34113052 positive

``` r
# Plot PCA results
ggplot(pca_df, aes(x = PC1, y = PC2, color = gram, label = bacteria)) +
  geom_point(size = 3) +
  geom_text_repel(size = 3, max.overlaps = 20) +  # Prevent text overlap
  # geom_text(vjust = 1, hjust = 1, size = 3) +
  labs(title = "PCA of Bacteria Based on MIC Values",
       x = "Principal Component 1",
       y = "Principal Component 2",
       color = "Gram Type") +
  theme_minimal()
```

![](c05-antibiotics-assignment_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
library(ggplot2)
library(ggrepel)

# Define target bacteria
target_bacteria <- c("Streptococcus fecalis", "Diplococcus pneumonia")

# Create a new column for highlighting (keep original Gram colors for others)
pca_df$highlight <- ifelse(pca_df$bacteria %in% target_bacteria, pca_df$bacteria, pca_df$gram)

# Define custom colors for the two bacteria, keeping others unchanged
custom_colors <- c("Streptococcus fecalis" = "blue", 
                   "Diplococcus pneumonia" = "red", 
                   "positive" = "darkorange",  # Default color for Gram-positive
                   "negative" = "purple")      # Default color for Gram-negative

# Plot
ggplot(pca_df, aes(x = PC1, y = PC2, label = bacteria, color = highlight)) +
  geom_point(size = 3) +  
  geom_text_repel(size = 3, max.overlaps = 20) +  
  scale_color_manual(values = custom_colors) +  # Apply custom colors only to target bacteria
  labs(title = "PCA of Bacteria Based on MIC Values",
       x = "Principal Component 1",
       y = "Principal Component 2",
       color = "Bacteria Type") +
  theme_minimal()
```

![](c05-antibiotics-assignment_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

### **q2** Assess your visuals

There are **two questions** below; use your five visuals to help answer
both Guiding Questions. Note that you must also identify which of your
five visuals were most helpful in answering the questions.

*Hint 1*: It’s possible that *none* of your visuals is effective in
answering the questions below. You may need to revise one or more of
your visuals to answer the questions below!

*Hint 2*: It’s **highly unlikely** that the same visual is the most
effective at helping answer both guiding questions. **Use this as an
opportunity to think about why this is.**

#### Guiding Question 1

> How do the three antibiotics vary in their effectiveness against
> bacteria of different genera and Gram stain?

*Observations* - What is your response to the question above?
-*penicillin*: We can see that in the bar plot with penicillin isolated,
if we make MIC value 1 the bases of comparison, all bacteria gram type
negative has higher MIC values while all bacteria gram type positive has
lower MIC values. This tells me that bacteria gram type negative is more
resistant to *penicillin* than type positive. -*streptomycin*: For
streptomycin, I’d say there isn’t a very obvious relationship between
Gram type and MIC values. However, I did notice that Gram-negative
bacteria tend to have more stable MIC values, mostly fluctuating around
1, while Gram-positive bacteria show greater variation. This suggests
that streptomycin might generally be more effective against
Gram-negative bacteria, though the trend isn’t entirely clear.
-*neomycin*: If we once again make MIC value 1 our basis of comparison,
we can see that neomycin is generally effective across the board (with a
couple of exceptions) especially in the staphyloccocus group. I also
don’t see much correlation between the gram type and the MIC values.

- Which of your visuals above (1 through 5) is **most effective** at
  helping to answer this question?
  - 3 4 and 5 where I isolated the three different antibiotic tested in
    plotting.
- Why?
  - It is easier to see the relationship between gram types and
    different antibiotics because there are less information presented
    at once and less prone to being overwhelmed by all the
    information(plot 1 and 2).

#### Guiding Question 2

In 1974 *Diplococcus pneumoniae* was renamed *Streptococcus pneumoniae*,
and in 1984 *Streptococcus fecalis* was renamed *Enterococcus fecalis*
\[2\].

> Why was *Diplococcus pneumoniae* was renamed *Streptococcus
> pneumoniae*?

*Observations* - What is your response to the question above? - Ok so at
first when I was reading the question I understood that I will probably
have to derive some sort of clustering between the bacteria based on
their MIC values to different antibiotics. I tried by using scatter plot
with the x axis being different antibiotics and the y axis with the MIC
values, it was very ahrd to see any relationships and grouping because
there were too many bacteria. I guess you can sort of see some bacteria
having similar reactions to antibiotics in plot 2 of the bar plot,
however I am still not getting the clustering and clear mapping of
groups of bacteria I wanted to get. I ultimately consulted chatgpt for
PCA and it produced the PCA plots. I went and understood every single
line of code and added comments. We can hypothesize that the reason why
*Diplococcus pneumoniae* was renamed *Streptococcus pneumoniae* is
because that in our PCA plot you can see that Diplococcus pneumoniae is
clustering with *Streptococcus hemolyticus* and *Streptococcus
viridans*. This suggests that *Diplococcus pneumonia* shares structural
or behavioral similarities with the *Streptococcus* group, which might
explain why it was reclassified as *Streptococcus pneumoniae*. - Which
of your visuals above (1 through 5) is **most effective** at helping to
answer this question? - I would say the PCA plots were the most
helpful. - Why? - Because it directly shows the clustering between
bacteria of how similarly they reacted to different antibiotics.

# References

<!-- -------------------------------------------------- -->

\[1\] Neomycin in skin infections: A new topical antibiotic with wide
antibacterial range and rarely sensitizing. Scope. 1951;3(5):4-7.

\[2\] Wainer and Lysen, “That’s Funny…” *American Scientist* (2009)
[link](https://www.americanscientist.org/article/thats-funny)
