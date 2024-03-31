## The Context

This project was created by me for an analytics course capstone. As part
of that capstone, I was given the task to use my knowledge to analyze
any topic that I wanted to. I decided that I wanted to analyze aspects
of entry level data science jobs in the U.S. such as job titles in data
science, salaries, number of opportunities, and more.

## The Data

The dataset used for this project was published on
[Kaggle](https://kaggle.com) by [Abhinav
Shaw](https://www.kaggle.com/abhinavshaw09), and was last updated 22
days before this project. His dataset on jobs and salaries in data
science can be found
[here](https://www.kaggle.com/datasets/abhinavshaw09/data-science-job-salaries-2024).
A glimpse of his data can be viewed there.

## Constraints

This project will use data from 2024 in the U.S., which might be
inaccurate for any foreigners reading this. In addition, this only
regards entry level jobs in data science.

## Setting Up The Project

I set the working directory as needed and installed the tidyverse
package to work with data manipulation (dplyr), visualization (ggplot2),
and reading files (readr). I also installed the scales package for
scaling axes in plots.

    library(tidyverse)

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.0     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

    library(scales)

    ## 
    ## Attaching package: 'scales'
    ## 
    ## The following object is masked from 'package:purrr':
    ## 
    ##     discard
    ## 
    ## The following object is masked from 'package:readr':
    ## 
    ##     col_factor

# The Number of Jobs Offered: Job Titles

## Purpose

Compares U.S. job titles by the number of entry-level opportunities in
those jobs in 2024 to find the most popular job titles.

## Setting Up The Dataset

I started off by reading and filtering the dataset for entry-level data
science jobs that are located within the United States in 2024.

    salaries <- read_csv("../Data Sources/salaries.csv")

    ## Rows: 13972 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (7): experience_level, employment_type, job_title, salary_currency, empl...
    ## dbl (4): work_year, salary, salary_in_usd, remote_ratio
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    entry_level_us_salaries_2024 <- salaries %>% filter(experience_level == "EN" & company_location == "US" & work_year == 2024)

## Analyzing The Dataset

After setting up the dataset, I manipulated the dataset by grouping
observations of data using job titles and counted the number of
observations by job titles. The results were saved into another data
frame.

    num_of_jobs_by_title <- entry_level_us_salaries_2024 %>% count(job_title) %>% rename(num_of_jobs = n) %>% arrange(desc(num_of_jobs))

## Plotting The Data Frame

    num_of_jobs_by_title %>% ggplot(mapping = aes(x = job_title, y = num_of_jobs)) +
      geom_bar(mapping = aes(fill = job_title), stat = "identity") +
      labs(title = "Job Titles and Offered Entry Level Jobs in 2024", subtitle = "By Raphael Manayon",
           x = "Job Title", y = "# of Entry Level Jobs Offered") +
      theme(axis.text.x = element_text(angle = 25), legend.position = "none")

![](../num_of_jobs_by_title.png)

Looking at the plot, you can see that the title of “Business
Intelligence Analyst” is the most offered amongst data science roles.
Either way, if you’re interested in some form of data analytics and want
to use business data and metrics to help your company make decision, you
can read [this
article](https://www.coursera.org/articles/business-intelligence-analysts-what-they-are-and-how-to-become-one)
created by Coursera.

# The Number of Jobs Offered: Company Size

## Purpose

Compares company sizes by the number of entry-level jobs inside of the
U.S. offered in each job to find the types of companies in the U.S. that
hire the most entry-level data scientists in 2024.

## Setting Up The Dataset

I set up the dataset for use again.

    salaries <- read_csv("../Data Sources/salaries.csv")

    ## Rows: 13972 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (7): experience_level, employment_type, job_title, salary_currency, empl...
    ## dbl (4): work_year, salary, salary_in_usd, remote_ratio
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    entry_level_us_salaries_2024 <- salaries %>% filter(experience_level == "EN" & company_location == "US" & work_year == 2024)

## Analyzing The Dataset

Following setup, I counted the number of observations by either a small,
medium, or large company size. This was also saved into another data
frame.

    entry_level_num_of_jobs_by_company_size <- entry_level_us_salaries_2024 %>% count(company_size) %>% rename(job_opportunities = n) %>% arrange(desc(job_opportunities))

## Plotting The Data Frame

    entry_level_num_of_jobs_by_company_size %>% ggplot(mapping = aes(x = company_size, y = job_opportunities)) +
      geom_bar(mapping = aes(fill = company_size), stat = "identity") +
      labs(title = "Entry Level Jobs Offered by Company Size in 2024", subtitle = "By Raphael Manayon",
           x = "Company Size (S/M/L)", y = "# of Entry Level Jobs Offered")

![](entry_level_ds_jobs_and_salaries_files/figure-markdown_strict/unnamed-chunk-7-1.png)

It seems that all entered U.S. companies offering entry-level jobs in
2024 have a medium size. If you’re looking for an entry-level data
science job at the moment, it’s likely that you won’t find it at a large
company or small company. If these limitations affect your dreams for
working at your dream company, don’t fret! You should start gaining some
experience in any way you can and use it to hopefully impress your dream
company. Mid-level+ jobs still exist!

# Median Salaries by Job Title

## Purpose

Compares U.S. job titles by median salary to determine the most
profitable U.S. job titles for entry-level data science in 2024.

## Setting Up The Dataset

In addition to setting up the dataset like usual, I also arranged the
dataset by salary in descending order and selected only the job title
and salary columns.

    salaries <- read_csv("../Data Sources/salaries.csv")

    ## Rows: 13972 Columns: 11
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (7): experience_level, employment_type, job_title, salary_currency, empl...
    ## dbl (4): work_year, salary, salary_in_usd, remote_ratio
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    entry_level_us_salaries_2024 <- salaries %>% filter(experience_level == "EN" & company_location == "US" & work_year == 2024)
    job_salaries <- entry_level_us_salaries_2024 %>% arrange(desc(salary_in_usd)) %>% select(job_title, salary_in_usd)

## Analyzing The Dataset

I grouped the observations by job title and summarized them by taking
the median of salaries where each job title is observed.

    summarized_salaries <- job_salaries %>% group_by(job_title) %>%
      summarize(median_salary = median(salary_in_usd)) %>%
        arrange(desc(median_salary))

## Plotting The Data Frame

    summarized_salaries %>% ggplot(mapping = aes(x = job_title, y = median_salary)) +
      geom_bar(mapping = aes(fill = job_title), stat = "identity") +
      labs(title = "Job Titles and Entry Level Median Salaries in 2024", subtitle = "By Raphael Manayon",
           x = "Job Title", y = "Entry Level Median Salary") +
      theme(axis.text.x = element_text(angle = 25), legend.position = "none") +
      scale_y_continuous(labels = label_comma())

![](entry_level_ds_jobs_and_salaries_files/figure-markdown_strict/unnamed-chunk-10-1.png)

Entry-level research scientists are the highest-paid entry-level data
scientists out there in the U.S. However, this title usually requires
intensive education. In addition, this job isn’t relatively popular,
making it hard to become a research scientist, even with plenty of
education.

# Which Title Should You Pursue?

The goal of this notebook is to help you align your interests with the
data science title you’d like. I’d like you to ignore pay, popularity,
or similar factors and listen to me for a bit.

All of these titles have varying responsibilities, knowledge, and skills
needed. For this, you may find that many of these roles are similar, yet
a lot different from each other. For example, data analysts and data
scientists are generalists who analyze data and make predictions to help
their company. However, data scientists usually have a more
math-intensive background and have knowledge regarding advanced concepts
in statistics, calculus, and linear algebra, whereas data analysts at
least know concepts from basic statistics.

I urge you to dive a little deeper into these titles; find out how each
one differs in the field of data science and see which one interests you
the most. [builtin](https://builtin.com) has an article that goes over
ten data science roles. If you’re interested, you can click the link
[here](https://builtin.com/data-science/data-science-jobs).
