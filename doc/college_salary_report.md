
# Exploring attributes of US college education in relation to career salary

**By**: Alex Pak, Linyang Yu, Constantin Shuster  
**Date**: November 24, 2018

### Our question

We were interested in exploring whether there are any relationships
between certain attributes of US college education and graduates’
salaries. Specifically, we analyzed the relationships between college
region, type and degree on starting and mid-career salaries. Our
question is exploratory and hypothesis generating. If we observed a
relationship in the data, we created a hypothesis and tested it.

College region refers to one of the major regions of the US: Northeast,
Midwest, South, West and California. College type refers to the main
types of colleges in the US: Liberal Arts, Engineering, Ivy League,
Party and State colleges. College degree refers to the college major,
for example chemical engineering, accounting, etc.

Starting career salary was a median value. Mid-career salary was either
10th, 25, 50th (median), 75th or 90th percentile salary. Mid-career
salary was the salary at **10 years** from graduation.

### Our dataset

The dataset was downloaded from
[kaggle](https://www.kaggle.com/wsj/college-salaries) and is originally
from *The Wall Street Journal* but based on a study done by *Payscale,
Inc.*. The data is split into 3 tables. The first table contains College
degrees and the corresponding salary data. The second table lists
colleges, their region and corresponding salary data. The third table
lists colleges, their type and corresponding salary data.

To begin our analysis we cleaned up the data tables by converting
character data into numerical data so it can analyzed and visualized
appropriately. We removed any spaces in the heading or ‘$’. We also
joined the second two tables such that our joined table listed colleges,
their region, their type and corresponding salary data. Since this was
an [inner join](https://r4ds.had.co.nz/relational-data.html#inner-join)
we lost \~50 colleges that were listed in the third table, but not the
second. We wanted the comparison of college region and type with salary
to be similar in terms of the colleges listed and so we felt this type
of join was appropriate.

### Exploratory data analysis (EDA)

We were first interested in examining how starting and mid-career
salaries varied by college degree. Figure 1 and 2 below are [dumbbell
plots](http://r-statistics.co/Top50-Ggplot2-Visualizations-MasterList-R-Code.html#Dumbbell%20Plot)
where the **starting point** of the dumbbell is the **starting career
salary** and the **end point** is the **mid-career salary**. Both salary
values are median salaries.

**Figure 1**: Salary data is ordered by starting salary ![Figure
1](../results/degree_vs_salary_by_start.png)

**Figure 2**: Salary data is ordered by mid-career salary ![Figure
2](../results/degree_vs_salary_by_mid.png)

Figure 3 is also a dumbbell plot displaying the range of mid-career
salaries by college degree. The range is from 10th to 90th percentile
and the median is the point in red. The salary data was ordered by
median mid-career salary.

**Figure 3**: Salary data is ordered by *median* mid-career salary
![Figure 3](../results/degree_vs_mid_salary_range.png)

Next we examined how graduates’ salaries varied by college region.
Figure 4 shows start and mid-career salary data aggregated by college
region. The salary data was represented as a mean estimates with
standard errors at a 95% confidence interval. The mean estimates were of
median start and mid career salaries of colleges in each region that
were available in the *joined* table.

**Figure 4**: ![Figure 4](../results/salary_change_Region.png)

Figure 5 shows the mid-career salaries of the different US college
regions, split into panels of 10th, 50th and 90th percentiles. We did
this to look at whether the same pattern was seen across the full salary
range, and whether certain regions were associated with the potential to
achieve a higher “top” salary. Salary data was also represented as mean
estimates with standard errors at a 95% confidence interval.

**Figure 5** ![Figure 5](../results/salary_distribution_Region.png)

Lastly, we examined how graduates’ salary varied by college type. Figure
6 and 7 are similar analyses to figure 4 and 5 respectively, only the
variable is college type. Salary data was calculated in the exact same
manner as described in figure 4 and 5.

**Figure 6** ![Figure 6](../results/salary_change_SchoolType.png)

**Figure 7** ![Figure 7](../results/salary_distribution_SchoolType.png)

### Results

Since we had only 1 data point for each school degree, we could not do
any hypothesis testing and thus we report here only major trends we
observed in the data from the Figure 1, 2 and 3. From these figures we
can see that graduates from STEM majors (Science, Technology and
Mathematics) have higher salaries both at the start of their career and
in the middle of their career. Graduates with majors in education or the
arts/social sciences had the lowest salaries.

In regards to salary variation by school region, our hypothesis from the
EDA was that there is a difference in salary depending on which region
the college was in. We performed a one-way ANOVA shown in Table 1
    below.

    ## -- Attaching packages ----------------------------------------------------------------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 3.1.0     v purrr   0.2.5
    ## v tibble  1.4.2     v dplyr   0.7.7
    ## v tidyr   0.8.2     v stringr 1.3.1
    ## v readr   1.1.1     v forcats 0.3.0

    ## -- Conflicts -------------------------------------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

    ## Parsed with column specification:
    ## cols(
    ##   Salary_Type = col_character(),
    ##   F_Statistic = col_double(),
    ##   P_Value = col_double()
    ## )

| Salary Type                           | F Statistic |      p-value |
| :------------------------------------ | ----------: | -----------: |
| Starting\_Median\_Salary              |       13.37 | 6.234460e-10 |
| Mid\_Career\_Median\_Salary           |       19.87 | 2.600000e-14 |
| Mid\_Career\_10th\_Percentile\_Salary |       10.14 | 1.506101e-07 |
| Mid\_Career\_25th\_Percentile\_Salary |       12.53 | 2.433669e-09 |
| Mid\_Career\_50th\_Percentile\_Salary |       19.87 | 2.600000e-14 |
| Mid\_Career\_75th\_Percentile\_Salary |       21.79 | 2.000000e-15 |
| Mid\_Career\_90th\_Percentile\_Salary |       20.62 | 1.900000e-14 |

**Table 1** above shows there was a statistical difference in salary,
all salary types examined, amongst the different regions, with a p-value
\< 0.05. Next we performed a Tukey’s post-hoc test to see which region
was responsible for the difference in salary. This is summarized in
Table 2 below.

    ## Parsed with column specification:
    ## cols(
    ##   salary_type = col_character(),
    ##   term = col_character(),
    ##   comparison = col_character(),
    ##   adj.p.value = col_double(),
    ##   flag = col_character()
    ## )

| Salary Type                           | Region Comparison       | adjusted p-value | Interpretation                    |
| :------------------------------------ | :---------------------- | ---------------: | :-------------------------------- |
| Starting\_Median\_Salary              | Midwestern-California   |        0.0000408 | \*\*\*                            |
| Starting\_Median\_Salary              | Northeastern-California |        0.9634884 | Cannot Reject the NULL Hypothesis |
| Starting\_Median\_Salary              | Southern-California     |        0.0001661 | \*\*\*                            |
| Starting\_Median\_Salary              | Western-California      |        0.0005862 | \*\*\*                            |
| Starting\_Median\_Salary              | Northeastern-Midwestern |        0.0000018 | \*\*\*                            |
| Starting\_Median\_Salary              | Southern-Midwestern     |        0.9895888 | Cannot Reject the NULL Hypothesis |
| Starting\_Median\_Salary              | Western-Midwestern      |        0.9983989 | Cannot Reject the NULL Hypothesis |
| Starting\_Median\_Salary              | Southern-Northeastern   |        0.0000122 | \*\*\*                            |
| Starting\_Median\_Salary              | Western-Northeastern    |        0.0001904 | \*\*\*                            |
| Starting\_Median\_Salary              | Western-Southern        |        0.9999592 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_Median\_Salary           | Midwestern-California   |        0.0000250 | \*\*\*                            |
| Mid\_Career\_Median\_Salary           | Northeastern-California |        0.9652641 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_Median\_Salary           | Southern-California     |        0.0010309 | \*\*                              |
| Mid\_Career\_Median\_Salary           | Western-California      |        0.0002839 | \*\*\*                            |
| Mid\_Career\_Median\_Salary           | Northeastern-Midwestern |        0.0000000 | \*\*\*                            |
| Mid\_Career\_Median\_Salary           | Southern-Midwestern     |        0.7469916 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_Median\_Salary           | Western-Midwestern      |        0.9996818 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_Median\_Salary           | Southern-Northeastern   |        0.0000000 | \*\*\*                            |
| Mid\_Career\_Median\_Salary           | Western-Northeastern    |        0.0000000 | \*\*\*                            |
| Mid\_Career\_Median\_Salary           | Western-Southern        |        0.9142965 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_10th\_Percentile\_Salary | Midwestern-California   |        0.1283208 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_10th\_Percentile\_Salary | Northeastern-California |        0.4643881 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_10th\_Percentile\_Salary | Southern-California     |        0.1972554 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_10th\_Percentile\_Salary | Western-California      |        0.2778049 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_10th\_Percentile\_Salary | Northeastern-Midwestern |        0.0000019 | \*\*\*                            |
| Mid\_Career\_10th\_Percentile\_Salary | Southern-Midwestern     |        0.9976522 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_10th\_Percentile\_Salary | Western-Midwestern      |        0.9996644 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_10th\_Percentile\_Salary | Southern-Northeastern   |        0.0000046 | \*\*\*                            |
| Mid\_Career\_10th\_Percentile\_Salary | Western-Northeastern    |        0.0001972 | \*\*\*                            |
| Mid\_Career\_10th\_Percentile\_Salary | Western-Southern        |        0.9999950 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_25th\_Percentile\_Salary | Midwestern-California   |        0.0004422 | \*\*\*                            |
| Mid\_Career\_25th\_Percentile\_Salary | Northeastern-California |        1.0000000 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_25th\_Percentile\_Salary | Southern-California     |        0.0045416 | \*\*                              |
| Mid\_Career\_25th\_Percentile\_Salary | Western-California      |        0.0015425 | \*\*                              |
| Mid\_Career\_25th\_Percentile\_Salary | Northeastern-Midwestern |        0.0000009 | \*\*\*                            |
| Mid\_Career\_25th\_Percentile\_Salary | Southern-Midwestern     |        0.9120722 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_25th\_Percentile\_Salary | Western-Midwestern      |        0.9999998 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_25th\_Percentile\_Salary | Southern-Northeastern   |        0.0000408 | \*\*\*                            |
| Mid\_Career\_25th\_Percentile\_Salary | Western-Northeastern    |        0.0000279 | \*\*\*                            |
| Mid\_Career\_25th\_Percentile\_Salary | Western-Southern        |        0.9379583 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_50th\_Percentile\_Salary | Midwestern-California   |        0.0000250 | \*\*\*                            |
| Mid\_Career\_50th\_Percentile\_Salary | Northeastern-California |        0.9652641 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_50th\_Percentile\_Salary | Southern-California     |        0.0010309 | \*\*                              |
| Mid\_Career\_50th\_Percentile\_Salary | Western-California      |        0.0002839 | \*\*\*                            |
| Mid\_Career\_50th\_Percentile\_Salary | Northeastern-Midwestern |        0.0000000 | \*\*\*                            |
| Mid\_Career\_50th\_Percentile\_Salary | Southern-Midwestern     |        0.7469916 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_50th\_Percentile\_Salary | Western-Midwestern      |        0.9996818 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_50th\_Percentile\_Salary | Southern-Northeastern   |        0.0000000 | \*\*\*                            |
| Mid\_Career\_50th\_Percentile\_Salary | Western-Northeastern    |        0.0000000 | \*\*\*                            |
| Mid\_Career\_50th\_Percentile\_Salary | Western-Southern        |        0.9142965 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_75th\_Percentile\_Salary | Midwestern-California   |        0.0016209 | \*\*                              |
| Mid\_Career\_75th\_Percentile\_Salary | Northeastern-California |        0.2359886 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_75th\_Percentile\_Salary | Southern-California     |        0.0151179 | \*                                |
| Mid\_Career\_75th\_Percentile\_Salary | Western-California      |        0.0028741 | \*\*                              |
| Mid\_Career\_75th\_Percentile\_Salary | Northeastern-Midwestern |        0.0000000 | \*\*\*                            |
| Mid\_Career\_75th\_Percentile\_Salary | Southern-Midwestern     |        0.8940914 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_75th\_Percentile\_Salary | Western-Midwestern      |        0.9997165 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_75th\_Percentile\_Salary | Southern-Northeastern   |        0.0000000 | \*\*\*                            |
| Mid\_Career\_75th\_Percentile\_Salary | Western-Northeastern    |        0.0000000 | \*\*\*                            |
| Mid\_Career\_75th\_Percentile\_Salary | Western-Southern        |        0.8631754 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_90th\_Percentile\_Salary | Midwestern-California   |        0.2180698 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_90th\_Percentile\_Salary | Northeastern-California |        0.0027215 | \*\*                              |
| Mid\_Career\_90th\_Percentile\_Salary | Southern-California     |        0.4692042 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_90th\_Percentile\_Salary | Western-California      |        0.2184222 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_90th\_Percentile\_Salary | Northeastern-Midwestern |        0.0000000 | \*\*\*                            |
| Mid\_Career\_90th\_Percentile\_Salary | Southern-Midwestern     |        0.9603623 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_90th\_Percentile\_Salary | Western-Midwestern      |        0.9990184 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_90th\_Percentile\_Salary | Southern-Northeastern   |        0.0000000 | \*\*\*                            |
| Mid\_Career\_90th\_Percentile\_Salary | Western-Northeastern    |        0.0000000 | \*\*\*                            |
| Mid\_Career\_90th\_Percentile\_Salary | Western-Southern        |        0.9210864 | Cannot Reject the NULL Hypothesis |

**Table 2** above shows that graduates of colleges in the Northeast and
California had higher salaries than the other regions, however there was
no statistically significant difference at alpha = 0.05 between colleges
in the Northeast and California.

In regards to salary variation by school type, our hypothesis from the
EDA was that there is a difference in salary depending on which college
type a graduate attended. We performed a one-way ANOVA shown in Table 1
below.

    ## Parsed with column specification:
    ## cols(
    ##   Salary_Type = col_character(),
    ##   F_Statistic = col_double(),
    ##   P_Value = col_double()
    ## )

| Salary Type                           | F Statistic |    p-value |
| :------------------------------------ | ----------: | ---------: |
| Starting\_Median\_Salary              |       66.81 | 0.0000e+00 |
| Mid\_Career\_Median\_Salary           |       54.69 | 0.0000e+00 |
| Mid\_Career\_10th\_Percentile\_Salary |       42.43 | 2.5089e-26 |
| Mid\_Career\_25th\_Percentile\_Salary |       47.75 | 4.0000e-30 |
| Mid\_Career\_50th\_Percentile\_Salary |       54.69 | 0.0000e+00 |
| Mid\_Career\_75th\_Percentile\_Salary |       63.05 | 0.0000e+00 |
| Mid\_Career\_90th\_Percentile\_Salary |       57.32 | 0.0000e+00 |

**Table 3** above shows that there was a significant difference in
salary amongst different types of schools as the p-value in all
comparisons with different salary types was \<0.05.

    ## Parsed with column specification:
    ## cols(
    ##   salary_type = col_character(),
    ##   term = col_character(),
    ##   comparison = col_character(),
    ##   adj.p.value = col_double(),
    ##   flag = col_character()
    ## )

| Salary Type                           | Region Comparison        | adjusted p-value | Interpretation                    |
| :------------------------------------ | :----------------------- | ---------------: | :-------------------------------- |
| Starting\_Median\_Salary              | Ivy League-Engineering   |        0.9818323 | Cannot Reject the NULL Hypothesis |
| Starting\_Median\_Salary              | Liberal Arts-Engineering |        0.0000000 | \*\*\*                            |
| Starting\_Median\_Salary              | Party-Engineering        |        0.0000000 | \*\*\*                            |
| Starting\_Median\_Salary              | State-Engineering        |        0.0000000 | \*\*\*                            |
| Starting\_Median\_Salary              | Liberal Arts-Ivy League  |        0.0000000 | \*\*\*                            |
| Starting\_Median\_Salary              | Party-Ivy League         |        0.0000000 | \*\*\*                            |
| Starting\_Median\_Salary              | State-Ivy League         |        0.0000000 | \*\*\*                            |
| Starting\_Median\_Salary              | Party-Liberal Arts       |        0.9999999 | Cannot Reject the NULL Hypothesis |
| Starting\_Median\_Salary              | State-Liberal Arts       |        0.1951980 | Cannot Reject the NULL Hypothesis |
| Starting\_Median\_Salary              | State-Party              |        0.5768294 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_Median\_Salary           | Ivy League-Engineering   |        0.0095964 | \*\*                              |
| Mid\_Career\_Median\_Salary           | Liberal Arts-Engineering |        0.0000023 | \*\*\*                            |
| Mid\_Career\_Median\_Salary           | Party-Engineering        |        0.0000001 | \*\*\*                            |
| Mid\_Career\_Median\_Salary           | State-Engineering        |        0.0000000 | \*\*\*                            |
| Mid\_Career\_Median\_Salary           | Liberal Arts-Ivy League  |        0.0000000 | \*\*\*                            |
| Mid\_Career\_Median\_Salary           | Party-Ivy League         |        0.0000000 | \*\*\*                            |
| Mid\_Career\_Median\_Salary           | State-Ivy League         |        0.0000000 | \*\*\*                            |
| Mid\_Career\_Median\_Salary           | Party-Liberal Arts       |        0.4704613 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_Median\_Salary           | State-Liberal Arts       |        0.0000000 | \*\*\*                            |
| Mid\_Career\_Median\_Salary           | State-Party              |        0.1121139 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_10th\_Percentile\_Salary | Ivy League-Engineering   |        0.4521647 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_10th\_Percentile\_Salary | Liberal Arts-Engineering |        0.0000000 | \*\*\*                            |
| Mid\_Career\_10th\_Percentile\_Salary | Party-Engineering        |        0.0000000 | \*\*\*                            |
| Mid\_Career\_10th\_Percentile\_Salary | State-Engineering        |        0.0000000 | \*\*\*                            |
| Mid\_Career\_10th\_Percentile\_Salary | Liberal Arts-Ivy League  |        0.0027649 | \*\*                              |
| Mid\_Career\_10th\_Percentile\_Salary | Party-Ivy League         |        0.0000062 | \*\*\*                            |
| Mid\_Career\_10th\_Percentile\_Salary | State-Ivy League         |        0.0000000 | \*\*\*                            |
| Mid\_Career\_10th\_Percentile\_Salary | Party-Liberal Arts       |        0.5510676 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_10th\_Percentile\_Salary | State-Liberal Arts       |        0.0347582 | \*                                |
| Mid\_Career\_10th\_Percentile\_Salary | State-Party              |        0.8051782 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_25th\_Percentile\_Salary | Ivy League-Engineering   |        0.9841315 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_25th\_Percentile\_Salary | Liberal Arts-Engineering |        0.0000000 | \*\*\*                            |
| Mid\_Career\_25th\_Percentile\_Salary | Party-Engineering        |        0.0000000 | \*\*\*                            |
| Mid\_Career\_25th\_Percentile\_Salary | State-Engineering        |        0.0000000 | \*\*\*                            |
| Mid\_Career\_25th\_Percentile\_Salary | Liberal Arts-Ivy League  |        0.0000023 | \*\*\*                            |
| Mid\_Career\_25th\_Percentile\_Salary | Party-Ivy League         |        0.0000014 | \*\*\*                            |
| Mid\_Career\_25th\_Percentile\_Salary | State-Ivy League         |        0.0000000 | \*\*\*                            |
| Mid\_Career\_25th\_Percentile\_Salary | Party-Liberal Arts       |        0.9159326 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_25th\_Percentile\_Salary | State-Liberal Arts       |        0.0021356 | \*\*                              |
| Mid\_Career\_25th\_Percentile\_Salary | State-Party              |        0.4716570 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_50th\_Percentile\_Salary | Ivy League-Engineering   |        0.0095964 | \*\*                              |
| Mid\_Career\_50th\_Percentile\_Salary | Liberal Arts-Engineering |        0.0000023 | \*\*\*                            |
| Mid\_Career\_50th\_Percentile\_Salary | Party-Engineering        |        0.0000001 | \*\*\*                            |
| Mid\_Career\_50th\_Percentile\_Salary | State-Engineering        |        0.0000000 | \*\*\*                            |
| Mid\_Career\_50th\_Percentile\_Salary | Liberal Arts-Ivy League  |        0.0000000 | \*\*\*                            |
| Mid\_Career\_50th\_Percentile\_Salary | Party-Ivy League         |        0.0000000 | \*\*\*                            |
| Mid\_Career\_50th\_Percentile\_Salary | State-Ivy League         |        0.0000000 | \*\*\*                            |
| Mid\_Career\_50th\_Percentile\_Salary | Party-Liberal Arts       |        0.4704613 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_50th\_Percentile\_Salary | State-Liberal Arts       |        0.0000000 | \*\*\*                            |
| Mid\_Career\_50th\_Percentile\_Salary | State-Party              |        0.1121139 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_75th\_Percentile\_Salary | Ivy League-Engineering   |        0.0000000 | \*\*\*                            |
| Mid\_Career\_75th\_Percentile\_Salary | Liberal Arts-Engineering |        0.8432242 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_75th\_Percentile\_Salary | Party-Engineering        |        0.0065532 | \*\*                              |
| Mid\_Career\_75th\_Percentile\_Salary | State-Engineering        |        0.0000000 | \*\*\*                            |
| Mid\_Career\_75th\_Percentile\_Salary | Liberal Arts-Ivy League  |        0.0000000 | \*\*\*                            |
| Mid\_Career\_75th\_Percentile\_Salary | Party-Ivy League         |        0.0000000 | \*\*\*                            |
| Mid\_Career\_75th\_Percentile\_Salary | State-Ivy League         |        0.0000000 | \*\*\*                            |
| Mid\_Career\_75th\_Percentile\_Salary | Party-Liberal Arts       |        0.0192990 | \*                                |
| Mid\_Career\_75th\_Percentile\_Salary | State-Liberal Arts       |        0.0000000 | \*\*\*                            |
| Mid\_Career\_75th\_Percentile\_Salary | State-Party              |        0.0456821 | \*                                |
| Mid\_Career\_90th\_Percentile\_Salary | Ivy League-Engineering   |        0.0000000 | \*\*\*                            |
| Mid\_Career\_90th\_Percentile\_Salary | Liberal Arts-Engineering |        0.4387095 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_90th\_Percentile\_Salary | Party-Engineering        |        0.8516001 | Cannot Reject the NULL Hypothesis |
| Mid\_Career\_90th\_Percentile\_Salary | State-Engineering        |        0.0008946 | \*\*\*                            |
| Mid\_Career\_90th\_Percentile\_Salary | Liberal Arts-Ivy League  |        0.0000000 | \*\*\*                            |
| Mid\_Career\_90th\_Percentile\_Salary | Party-Ivy League         |        0.0000000 | \*\*\*                            |
| Mid\_Career\_90th\_Percentile\_Salary | State-Ivy League         |        0.0000000 | \*\*\*                            |
| Mid\_Career\_90th\_Percentile\_Salary | Party-Liberal Arts       |        0.0405936 | \*                                |
| Mid\_Career\_90th\_Percentile\_Salary | State-Liberal Arts       |        0.0000000 | \*\*\*                            |
| Mid\_Career\_90th\_Percentile\_Salary | State-Party              |        0.0182262 | \*                                |

The results of the Tukey post-hoc tests show that graduates from
colleges in Ivy Leagues school and Engineering schools had higher
salaries than the other school types, however there was no significant
difference between graduates of Ivy League and Engineering schools.

### Interpretation

When comparing Degrees to Salary, the Engineering fields consistently
appear as the top earners. Of the top 10 spots (when comparing starting
salary), Engineering degrees dominate, appearing in 7/10 spots. Although
this might imply that STEM fields are high earners, it seems to only
apply to the applied STEM field; for example, the more conventional
science fields such as psychology and biology fall much lower on the
list.

Looking at the Tukey procedure results for Region vs. Salary, the
Northeastern and California seem to have similar means, as the null
hypothesis consistently cannot be rejected across multiple salary types.
This seems to make sense, as California and the Northeastern region
contain highly rated schools (ex. Stanford, California Institute of
Technology, Harvard).

The Tukey procedure results for School Type vs Salary seems to follow
this interpretation. From the visualizations, Ivy League schools
consistently place highly on the salary scale. However, when using the
Tukey procedure, it seems as if there is no significant difference
between the Ivy League school earnings and Engineering school earnings,
suggesting once again that Engineering degrees are worth the most.

### Limitations

One of the major limitations of our analysis was that for the dataset we
chose, the individual datapoints for each salary were not given.
Instead, the medians for each salary type were pre-calculated and
tabulated into this dataset. This meant that: \* We had to take the
dataset as the absolute truth since we could not calculate the medians
ourselves, and \* We could not see the spread of the datapoints (ie.
summary statistics, or visualizing the data as a boxplot).

Although the median is a robust summary statistic (compared to the mean,
for example), it would have made our analysis more trustworthy if we
were able to view and manipulate the individual datapoints.

Another limitation of our analysis is that ANOVA tests assume an equal
variance between all levels of a factor. In our analysis, we did not
check this assumption before performing the ANOVA tests.

### Future directions

In the future, to check the credibility of our ANOVA test, we can
perform a Levene’s Test. A Levene’s Test assess the equality of
variances between different factors. If our dataset passes the Levene’s
Test, it would add extra credibility to our ANOVA results.

Another potential visual exploration for our dataset would be to import
a seperate dataset containing all US College names and the state they
are based in (one such example can be found
[here](https://trends.collegeboard.org/college-pricing/figures-tables/tuition-fees-flagship-universities-over-time)).
By joining the state names to the college names in our dataset, we would
be able to create a visual map of the United States, with each state
containing salary information on a colour scale.

To further enhance our analysis, we could compare the top performing
degrees in the States with the top performing degrees in Canada. It
would be interesting to see differences in top degrees and the
differences in starting/mid-career salaries between Canadian
universities and colleges in the States (or if there is even a
difference at all).

### References

1.  [Kaggle Data - College vs
    Salary](https://www.kaggle.com/wsj/college-salaries)  
2.  [Dumbbell plots
    explained](http://r-statistics.co/Top50-Ggplot2-Visualizations-MasterList-R-Code.html#Dumbbell%20Plot)