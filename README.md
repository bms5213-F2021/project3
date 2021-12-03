# Projects 3 & 4

The previous projects and midterm focused on earlier steps of a data analytic project such as forming questions and sampling data. The next two projects will focus on the latter steps, directly working with data and communicating findings.

The goal of Project 3 is to perform an analysis with real-life published data. Even though you have access to the paper which originally analyzed this data, please do not consult it until Project 4. For Project 3, please only use the descriptions and instructions which I have provided to make analytic decisions.

Project 3 focuses on analysis of data and communication of the analysis for 3 data files/study questions. For each of these 3 files, there are at least 3 broad steps (2 analysis, 1 communication).

The goal of project 4 is to compare and contrast the analysis choices that you made and the conclusions that you reached, specifically in regards to the original, published analysis: *"Prenatal stress increases IgA coating of offspring microbiota and exacerbates necrotizing enterocolitis-like injury in a sex-dependent manner"*.  

## Logistics

**Due: Dec 14, 6 PM CDT**

### Project 3
#### Individual or Group Work

You may work individually or in groups of 2-3 people. Please email the instructor with your decision as soon as possible after Thanksgiving Break so repositories can be made on GitHub that reflect the group structure.

#### Submission

* All work for Project 3 should be done in `.Rmd` files which are knit to `.html` files prior to submission.
  * Utilize code blocks for all code.
  * Comments and all specified communication/writing steps of the project should also be in the `.Rmd` file(s).
  * Turn in both the `.Rmd` and the knit `.html` files to the repository.
  * If working in a group, a single `.Rmd` and knit `.html` file submission to the group repository is acceptable.
* Please also provide a *brief* breakdown of how each group member contributed to the submission.
  * This may be a `.pdf`, `.docx`, `.pages`, (etc.), or text section in the `.Rmd`/`.html` itself.
  * Additionally, if a group member has any comments or concerns which they wish to remain anonymous, they may email the professor.

#### Grading & Assessment Strategy

* Project 3 will be assessed only by the professor and based solely upon the `.Rmd` and `.html` uploads to GitHub.
* Project 3 is worth 20 points. The total number of available points is greater than 20, but the project will be graded out of 20 points.
* Groups members will receive the same grade as long as contributions are fair or nearly equal or group members worked together.

#### Resources and analysis choices

I will be providing links to resources relevant to the analysis steps I'm asking you to perform. Make informed decisions using these resources. Again, please do not consult the source paper to decide what you want to do.

### Project 4

Project 4 must be individual work.

### Submission

* Project 4 submissions may be uploaded to Canvas or the Project 3 GitHub repositories. If you upload to GitHub, feel free to enter the GitHub repository URL as your submission to Canvas
* Project 4 should not exceed 3 pages.
* Submissions may be a `.pdf`, `.docx`, `.pages`, or an `.Rmd`/`.html` file (etc.)

### Grading and assessment strategy

* Project 4 will be assessed only by the professor and based solely on the uploads to GitHub or Canvas
* Project 4 is worth 10 points. The total number of available points is greater than 10, but the project will be graded out of 10.   

## Project 3 Questions

### Analysis 1 (8 Possible points)

#### File with data

The file with the data is `data_conc_prop.csv`.

You can read this file directly into your `.Rmd` using a code block with the following code:

```
conc_prop_data <- read.csv("https://raw.githubusercontent.com/bms5213-F2021/project3/main/rawdata/data_conc_prop.csv")
```

#### Description of data

The data has 5 columns (variables) and 32 rows (probands).

```
 X  Sex Condition conc_fecal_iga prop_bound_bac
1 1 Male   Control           16.7          11.00
2 2 Male   Control           52.8          19.70
3 3 Male   Control            6.7          12.40
4 4 Male   Control            0.0           8.52
5 5 Male   Control            1.0          12.10
6 6 Male   Control           14.0           7.96
...
15 15   Male    Stress           10.0           6.09
16 16   Male    Stress           16.0          15.00
17 17 Female   Control           19.0          11.50
18 18 Female   Control           49.7          13.90
19 19 Female   Control           60.8          15.60
20 20 Female   Control            0.0           5.47
...
27 27 Female    Stress           38.1           18.5
28 28 Female    Stress           41.7           22.6
29 29 Female    Stress           19.0           39.0
30 30 Female    Stress           23.5           26.0
31 31 Female    Stress           20.8           31.7
32 32 Female    Stress           17.7           20.9
```

You have two overall sets or conditions of data (Control and Stress/experimental). Within each set, there are two further subsets of data (male or female). Then for each proband or individual (which is either control or stress and then either male or female), you have two measurements specific to that proband. The first is the fecal supernatant concentration of IgA as measured by ELISA. The second is the percentage of IgA+ fecal bacteria as measured following antibody staining and flow cytometry.

#### Question

For each set of conditions and sexes, is there a linear positive correlation between the concentration of fecal IgA and the proportion of IgA+ bacteria?

#### What we want from the plot

We want a plot that shows the correlation between IgA concentrations and proportion of IgA+ bacteria, separating the conditions (control and stress) and sexes (male and female) into different subpanels (e.g., 4 subpanels: (1) female control, (2) female stress, (3) male control, (4) male stress).

Aesthetics
* "Subpanels" for the conditions and sexes
  * You can either manually make different plots and just keep them separate
  * You can manually make different plots but combine them as subpanels in the same plot (packages like `patchwork` are great for this)
  * You can use `+ facet_wrap(Sex~Condition)` in order for ggplot to automatically make subpanels for you.
* The plot should have relevant axis labels and title.
* The plot *does not need* to have asterisks, but by using the `stat_cor()` function from the `ggpubr` package, it will communicate the results of correlation tests.

#### General outline of how you'll want to tidy the data

In order to check the assumptions of approximate normality, you may need to check the approximate normality of a variable after a natural logarithm (ln) transformation. Therefore, for the two variables `conc_fecal_iga` and `prop_bound_bac`, you should make new columns that are the ln(value + 1) (the +1 is to avoid taking a natural log of 0. This is allowed if you add it to all the values). You can either do this with the `log()` function, and adding a constant value like 1 *before* taking the ln. You could alternatively use the `log1p()` function which will add 1 before taking the ln.

#### Steps of analysis

1. Plot *(3 points)*
  * In `aes` for `ggplot`, include `x` and `y` arguments.
  * `facet_wrap()` would be helpful to separate the conditions and sexes into subpanels
  * `geom_point()` will scatter the points
  * `geom_smooth()` can be used to draw a straight line (but this isn't necessary); If used, consider setting the `method` and `se` arguments to have a linear model, and to turn off the shading respectively
2. Find the correlation between the variables  *(3 points)*
  * Check the assumptions for Pearson R correlation for each subplot. (Primarily, are both the variables being considered together approximately bivariate normal?)
    * If you believe this assumption is satisfied, proceed with the correlation using Pearson's R, and be sure to justify your choices in your write up in step 3
    * If you don't think that the evidence supports the assumption of approximate bivariate normality, then proceed with the correlation using Kendall's Tau or Spearman's Rho (your choice).  
  * You can run the correlation manually using the `cor()` and `cor.test()` functions for each subplot and add annotations to plots (not recommended) or you can use `+ stat_cor()` (a function from the `ggpubr` package) (recommended)
3. Write 1-3 paragraphs *(2 points)*
  * stating the analysis question (original question posed by the professor for this data file/analysis)
  * explaining the analysis that was done
  * explaining the results you observe
  * explaining how the analysis and results answer the original question posed by the professor (for this data file/analysis)

#### Helpful Resources

##### R Packages you'll need
* `ggplot2`
* `ggpubr`
* `mvnormtest`

##### R packages you may want
* `patchwork`

##### Tidying data and plotting
* [ggpubr and stat_cor](https://rpkgs.datanovia.com/ggpubr/reference/stat_cor.html)
* [facet_wrap](http://zevross.com/blog/2019/04/02/easy-multi-panel-plots-in-r-using-facet_wrap-and-facet_grid-from-ggplot2/)
* [facet_wrap 2](http://www.cookbook-r.com/Graphs/Facets_(ggplot2)/)
* [plot labels](http://www.sthda.com/english/wiki/ggplot2-title-main-axis-and-legend-titles)
* [patchwork package](https://patchwork.data-imaginist.com/index.html)

##### Assessing Normality
* [bivariate normal](https://universeofdatascience.com/shapiro-wilk-test-for-univariate-and-multivariate-normality-in-r/)

#### Types of Correlation metrics
* [R, tau, and rho](http://www.sthda.com/english/wiki/wiki.php?id_contents=7307)

##### Communication
* [Jeff Leek -- Advanced Data Science](http://jtleek.com/ads2020/week-9.html#week-9-learning-objectives)

--------------------------------------------------------------------------------

### Analysis 2 (8 possible points)

#### File with data

The file with the data is `data_score.csv`.

You can read this file directly into your `.Rmd` using a code block with the following code:

```
score_data <- read.csv("https://raw.githubusercontent.com/bms5213-F2021/project3/main/rawdata/data_score.csv")
```

#### Description of data

The data has 4 columns (variables) and 70 rows (probands).

```
  X Condition    Sex Score
1 1   Control Female     2
2 2   Control Female     0
3 3   Control Female     0
4 4   Control Female     1
5 5   Control Female     3
6 6   Control Female     0
...
25 25    Stress Female     2
26 26    Stress Female     3
27 27    Stress Female     1
28 28    Stress Female     2
29 29    Stress Female     2
30 30    Stress Female     2
...
40 40   Control Male     4
41 41   Control Male     4
42 42   Control Male     1
43 43   Control Male     1
44 44   Control Male     0
45 45   Control Male     3
...
65 65    Stress Male     3
66 66    Stress Male     2
67 67    Stress Male     2
68 68    Stress Male     3
69 69    Stress Male     2
70 70    Stress Male     2
```

You have two overall sets or conditions of data (Control and Stress/experimental). Within each set, there are two further subsets of data (male or female). Then for each proband or individual (which is either control or stress and then either male or female), you have an Injury Score. For each row, the proband ID is specified in the `X` column, the condition set is specified in the `Condition` column, the sex subset is specified in the `Sex` column, and the injury score is recorded in the `Score` column.

The possible scores are 0, 1, 2, 3, & 4. And the scoring system is described as follows:
> 0-no injury, 1-mild separation of the lamina propria from the muscularis mucosa, 2-moderate separation, 3-severe separation and/or edema in submucosa, 4-transmural injury.

#### Question

For each sex of pup, is there a difference in the distribution of injury scores between pups born from the mothers in control conditions and pups born from mothers in the stress conditions?

#### What we want from the plot

We want a plot that shows the injury scores of pups born from mothers from each condition (control and sex), separating the pup sexes (female and male) into different subpanels (e.g., 2 subpanels).

Aesthetics
* "Subpanels" for the sexes
  * You can either manually make different plots and just keep them separate
  * You can manually make different plots but combine them as subpanels in the same plot (packages like `patchwork` are great for this)
  * You can use `+ facet_wrap(~Sex)` in order for ggplot to automatically make subpanels for you.
* The plot should have relevant axis labels and title.
* The plot *does not need* to have asterisks or any other means of reporting the result of hypothesis tests.
* You can find and report medians and quantiles, but no need to add them to the plot

#### General outline of how you'll want to tidy the data

You won't need to do any tidying for this analysis.

#### Steps of analysis

1. Plot *(3 points)*
  * In `aes` for `ggplot`, include `x` and `y` arguments.
  * the `geom_point()` function will add points for each score
  * the `geom_jitter(width=0.1)` function can be used to separate the points out within each condition
  * `facet_wrap` would be helpful to separate the conditions (Control and Stress) into subpanels
2. Hypothesis Tests *(3 points)*
  * Given what you know about the type of data these injury scores are, and the resources provided below, decide on an appropriate hypothesis test and apply it to the data for each sex.
3. Write 1-3 paragraphs *(2 points)*
  * stating the analysis question (original question posed by the professor for this data file/analysis)
  * explaining the analysis that was done
  * explaining the results you observe
  * explaining how the analysis and results answer the original question posed by the professor (for this data file/analysis)

#### Helpful Resources

##### R packages you'll need
* `ggplot2`

##### R packages you may want
* `patchwork`

##### Plotting
* [facet_wrap](http://zevross.com/blog/2019/04/02/easy-multi-panel-plots-in-r-using-facet_wrap-and-facet_grid-from-ggplot2/)
* [facet_wrap 2](http://www.cookbook-r.com/Graphs/Facets_(ggplot2)/)
* [plot labels](http://www.sthda.com/english/wiki/ggplot2-title-main-axis-and-legend-titles)
* [patchwork package](https://patchwork.data-imaginist.com/index.html)

##### Hypothesis Testing
* [Ordinal data](https://www.sheffield.ac.uk/polopoly_fs/1.885207!/file/99_Mann_Whitney_U_Test.pdf)
* [t-test and Wilcoxon-rank-sum](https://data.library.virginia.edu/the-wilcoxon-rank-sum-test/)
* [mann whitney and wilcoxon-rank sum 1](http://www.r-tutor.com/elementary-statistics/non-parametric-methods/mann-whitney-wilcoxon-test)
* [mann whitney and wilcoxon-rank sum 2](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.mannwhitneyu.html)

##### Communication
* [Jeff Leek -- Advanced Data Science](http://jtleek.com/ads2020/week-9.html#week-9-learning-objectives)


--------------------------------------------------------------------------------

### Analysis 3 (9 Possible points)

#### File with data

The file with the data is `data_pre_post.csv`.

You can read this file directly into your `.Rmd` using a code block with the following code:

```
pre_post_data <- read.csv("https://raw.githubusercontent.com/bms5213-F2021/project3/main/rawdata/data_pre_post.csv")
```

#### Description of data

The data has 4 columns (variables) and 27 rows (probands).

```
  X Condition  Pre Post
1 1   Control 45.0 50.4
2 2   Control 30.4 69.6
3 3   Control 49.8 72.4
4 4   Control 64.6 77.6
5 5   Control 38.6 46.5
6 6   Control 44.1 80.2
...
22 22    Stress 78.40 61.8
23 23    Stress 30.91 41.9
24 24    Stress 53.30 31.5
25 25    Stress 44.30 60.2
26 26    Stress 35.50 51.2
27 27    Stress 38.80 33.2
```

The `X` column is an ID for the proband. The `Condition` column specifies which condition the proband belongs to. The `Pre` column specifies the IgA concentration (in ug/mL) measured from ELISA of the fecal supernatant from the stool sample for the given proband on Embryonic Day 7 (E7). The `Post` column specifies the IgA concentration (in ug/mL) measured from ELISA of the fecal supernatant from the stool sample fro the given proband on Embryonic Day 20 (E20).

In general, you have two overall sets or conditions of data (Control and Stress/experimental). Within each set, there is **paired** data where the observations you have are two-dimensional. In one dimension you have individuals (who are independent of each other). In this specific case, the second dimension of the data corresponds to the observations for each individual. These observations are taken pre- and post- __ ; therefore, the pre- and post- observations are not independent of each other because they stem from the same individual. With paired data we want to treat each pair as a group, keeping the related info connected.

#### Question

For each condition of data (control and stress), does the concentration of IgA in fecal supernatants from stool samples increase from Embryonic Day 7 to Embryonic Day 20?  

[e.g. Do we observe a statistically significant increase in the mean IgA concentration of post-stress data (E20) compared to pre-stress data (E7) within each condition?]

#### What we want from the plot

We want a plot that shows the paired Pre and Post IgA concentrations, separating the conditions (control and stress) into different subpanels.

Aesthetics
* "Subpanels" for the conditions
  * You can either manually make different plots and just keep them separate
  * You can manually make different plots but combine them as subpanels in the same plot (packages like `patchwork` are great for this)
  * You can use `+ facet_wrap(~Condition)` in order for ggplot to automatically make subpanels for you.
* The plot should have relevant axis labels and title.
* The plot *does not need* to have asterisks or any other means of reporting the result of hypothesis tests.  


#### General outline of how you'll want to tidy the data for original plotting with ggplot2

* Make a longer dataframe with the `tidyr` `pivot_longer` function such that the values in the `Pre` and `Post` columns are stacked together in a column named `value`. So there's one column (`value`) with the concentrations (or Y values for the plots) and a new column `Timepoint` which specifies if the row is from the `Pre` or `Post` timepoint (and then you can use the new `Timepoint` column for the X values for the plot).
  * You can tell the `pivot_longer` function to stack the values in the `Pre` and `Post` columns using the `cols` argument
  * `pivot_longer` will automatically name the new column stacking these values `value`
  * You can tell the `pivot_longer` function the name of the new column, specifiying timpepoint using the `names_to` argument
* Because `ggplot` will plot the X values alphabetically (plotting `Post` data before `Pre` data), you'll have to set the "factor levels" of the new `Timepoint` column


#### Steps of analysis

1. Make a plot of the data using the new longer dataframe *(3 points)*
  * In `aes` for `ggplot`, include `x`, `y`, and `group` arguments. The `group` argument should point to the `X` or proband ID column so that `ggplot` knows which Y values go together.
  * `+ geom_line()` will draw the lines
  * `+ geom_point()` will draw the points (if you want; this isn't necessary)
  * `facet_wrap` would be helpful to separate the conditions (Control and Stress) into subpanels
2. Decide on a hypothesis test, check its assumptions for both conditions of data, and perform hypothesis test(s) *(4 points)*
  * For each condition....
    * if the assumptions are met
      * perform hypothesis test
    * if the assumptions are not met
      * perform an outlier test
      * remove outliers if appropriate
      * re-plot/repeat step 1 with data without outlier
      * re-check assumptions on data without outlier
      * perform hypothesis test
3. Write 1-3 paragraphs *(2 points)*
  * stating the analysis question (original question posed by the professor for this data file/analysis)
  * explaining the analysis that was done
  * explaining the results you observe
  * explaining how the analysis and results answer the original question posed by the professor (for this data file/analysis)

#### Helpful Resources

##### R Packages you'll need
* `tidyr`
* `ggplot2`
* `outliers`

##### R packages you may want
* `patchwork`

##### Tidying data and plotting
* [pivot_longer](https://tidyr.tidyverse.org/reference/pivot_longer.html)
* [factor setting](https://stackoverflow.com/questions/2375587/reorder-levels-of-a-factor-without-changing-order-of-values)
* [facet_wrap](http://zevross.com/blog/2019/04/02/easy-multi-panel-plots-in-r-using-facet_wrap-and-facet_grid-from-ggplot2/)
* [facet_wrap 2](http://www.cookbook-r.com/Graphs/Facets_(ggplot2)/)
* [plot labels](http://www.sthda.com/english/wiki/ggplot2-title-main-axis-and-legend-titles)
* [patchwork package](https://patchwork.data-imaginist.com/index.html)

##### Assessing Normality
* [general info](https://www.sheffield.ac.uk/polopoly_fs/1.885202!/file/95_Normality_Check.pdf)
* [Normality when there's paired data](https://www.graphpad.com/guides/prism/latest/statistics/stat_checklist_pairedt.htm)
* [ggplot Q-Q plots](https://ggplot2.tidyverse.org/reference/geom_qq.html)
* [base R Q-Q plots](https://www.dummies.com/programming/r/how-to-use-quantile-plots-to-check-data-normality-in-r/)
* [Shapiro-Wilk tests](https://stat.ethz.ch/R-manual/R-devel/library/stats/html/shapiro.test.html)
* [F-test](https://www.itl.nist.gov/div898/handbook/eda/section3/eda359.htm)
* [F-test in R](http://www.sthda.com/english/wiki/f-test-compare-two-variances-in-r)
* [overview of tests for variance -- minitab](https://support.minitab.com/en-us/minitab-express/1/help-and-how-to/modeling-statistics/anova/supporting-topics/basics/understanding-test-for-equal-variances/)

##### Outliers
* [Outliers in R](https://statsandr.com/blog/outliers-detection-in-r/)
* [Outliers in GraphPad](https://www.graphpad.com/guides/prism/latest/statistics/stat_how_it_works_rout_method.htm)

##### Communication
* [Jeff Leek -- Advanced Data Science](http://jtleek.com/ads2020/week-9.html#week-9-learning-objectives)

--------------------------------------------------------------------------------------------------------------------------------

## Project 4

For each of the 3 analyses from Project 3, you will compare and contrast your question, analysis, & results to those in the original study. Please comment on each of the following prompts for each of the analyses:
* How do the study questions compare? (e.g., are they the same, are they different?) *(1.25 pt for each analysis)*
  * If different
    * how are they different, specifically?
    * how might the difference impact comparing results from your analysis and the original analysis? (will it impact comparison at all?)
* Are there differences in my analysis compared to the original analysis? *(1.25 pt for each analysis)*  
  * If yes: what (about the study questions or data) motivated the differences? (e.g., a difference in the posed study question, a difference in hypothesis test used due to __ (fill in the blank))
  * If no: what (about the study question or data) motivated you to use the analysis that you did and does this seem to be the same motivation the original authors had?
* Compare the results, interpretation and answers to study questions (keeping in mind that the study questions or analysis steps may have differed). *(1.25 pt for each analysis)*
  * If the study question and analysis match between your analysis and the original analysis...
    * How do the results, interpretation, or the answers to study questions compare? (e.g., are they same, are they similar, are they different? **did you replicate their findings?**)
    * If you didn't replicate their findings, why not?
  * If the study question, but not the analysis, match between your analysis and the original analysis...
    * How are the results, interpretations, answers similar? How are they different?
  * If the study question did not, but the analysis did, match between your analysis and the original analysis ....
    * How did the differences in the study question not lead to differences in the analysis?
    * Are there differences in the results or interpretation or answers to the question?
  * If neither the study question nor the analysis matched...
    * Are there any similarities at all in the results, interpreation, or answers? If so, what?


| Analysis `#` from Project 3 | File from Project 3 | Corresponding Figure | Corresponding Results section |
|:---------------------------:|:-------------------:|:--------------------:|:-----------------------------:|
| Analysis 1                  | `data_conc_prop.csv`| Figure 3             | 3.3                           |
| Analysis 2                  | `data_score.csv`    | Figure 5             | 3.5                           |
| Analysis 3                  | `data_pre_post.csv` | Figure 1             | 3.1                           |
