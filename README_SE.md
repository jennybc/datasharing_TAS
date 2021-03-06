---
title: "How to share data for collaboration"
author: "Shannon E. Ellis and Jeffrey T. Leek"
bibliography: DataSharing.bib
output: github_document
---


1. Introduction
===========

A set of general principles for sharing data have emerged within the statistics community [@_data_org; @_tidy_data; @wilson_good_2016]. But these principles are not always clear to researchers, scientists, or collaborators generating the data. This has led to a disconnect between those generating data and those analyzing it about the best way for data to be shared. To bridge this divide, we have developed general guidelines for anyone generating data who anticipates their data will be shared with a statistician, data scientist, or analyst at some point during their project. The goals of this guide are to provide some instruction on the best way to share data to avoid the most common pitfalls and sources of delay in the transition from data collection to data analysis [@leek2015opinion]. 

When it comes to collaborations between data collectors and statisticians, it is a reasonable expectation that the statistician should be able to handle and analyze the data in whatever state they arrive. For this to be possible, the statistician must be provided the raw data, information on any steps taken to preprocess the data, and enough information about the experimental conditions to allow the statistician to identify and incorporate hidden sources of variability into his or her analysis [@baggerly2010disclose]. On the data generator's end, it can be expected that he or she will receive results from a statistician in a reasonable amount of time. From our experience in the Leek group [@_jtleek] (where we work with a large number of collaborators to analyze data) and from conversations with other statisticians, the number one source of delay in the speed of returning results to collaborators is the condition of the data when they arrive. 

To help meet the expectations of both the data generator and the statistician, all of the necessary information must be provided and provided in a consistent and well-organized manner to the statistician.  Consistent data sharing reduces the likelihood of errors during analysis and also decreases analysis turnaround time. 

We provide these guidelines on data sharing and explain the reasoning behind them from the analyst's perspective. We envision these will be useful to the following individuals: 

* Collaborators who need statisticians or data scientists to analyze data with/for them
* Students or postdocs in various disciplines looking for consulting advice
* Junior statistics students whose job it is to collate/clean/wrangle data sets
* Statisticians or data scientists seeking a concise guide to share with collaborators to clarify best practices for data sharing



2. What you should deliver to the statistician
====================

To facilitate the most efficient and timely analysis this is the information you should pass to a statistician:

1. The raw data.
2. A tidy data set [@_tidy_data]. 
3. A code book describing each variable and its values in the tidy data set.
4. An explicit and exact recipe you used to go from 1 -> 2,3. 

For clarity, we will further define each part of the data package transferred. 

### 2.1 The raw data

It is critical that you include the rawest form of the data to which you have access. This ensures that data provenance can be maintained throughout the workflow.  Here are some examples of the raw form of data:

* The strange binary file [@_binary_2017] your measurement machine spits out
* The unformatted Excel file with 10 worksheets the company you contracted with sent you
* The complicated JSON [@_json_2017] data you got from scraping the Twitter API [@_twitter]
* The hand-entered numbers you collected looking through a microscope

You know the raw data are in the right format if you: 

1. Ran no software on the data
2. Did not modify any of the data values
3. Did not remove any data from the data set
4. Did not summarize the data in any way

If you made any modifications to the raw data, it is not the raw form of the data. It can often help to consider what would happen if new data for a study were to arrive to the statistician. If this new data requires no modification before being combined with the first set of raw data provided, it is likely raw data. Reporting modified data as raw data is a very common way to slow down the analysis process, since the analyst will often have to do a forensic study of your data to figure out why the raw data looks weird.

### 2.2 The tidy data set

The general principles of tidy data are laid out by Hadley Wickham [@_hadley,@leek2015elements] in this paper [@_tidy_data]
and this video [@_tidy_video]. While both the paper and the video describe tidy data using R [@_r], the principles
are more generally applicable:

1. Each variable you measure should be in one column.
2. Each different observation of that variable should be in a different row.
3. There should be one table for each "kind" of data.
4. If you have multiple tables, they should include a column in the table (with the same column label!) that allows them to be joined or merged (see **Figure 1A-B**).

While these are the most critical decisions, there are a number of additional things that will make your data set much easier to handle [@_data_org] (summarized in Box 1). Briefly, it is best to include a row at the top of each data table/spreadsheet that contains informative column names. And, each cell should include only one value or unit of information. Sentences should generally be avoided here; any lengthy explanations should instead be included in the "Code book."

To provide an experimental example, suppose you want to know if individuals with diabetes have altered hormonal profiles. To answer this question, you carry out an experiment in which blood is drawn from 20 individuals with diabetes as well as 20 healthy controls. This blood was used to measure blood levels for 50 different hormones. These measurements would comprise your first 'kind' of data (see #3 above). You have also collected demographic information from the patients including their age, sex, BMI, treatment, and diagnosis (your second "kind" of data). For this example, your first table/spreadsheet would include the measurements for the 20 different hormones. This would have 41 rows (a row for the name of the measured hormones at top and then one row for each of the 40 individuals in your study) and 51 columns (one for `PatientID` and then one for each measurement) (**Figure 1A**). You would have another table/spreadsheet that contains the demographic information. It would have five columns (`PatientID`, `Age`, `Sex`, `BMI`, `Diagnosis`) and 41 rows (a row with variable names, then one row for each patient) (**Figure 1B**).

With regards to the formatting of these data, if you are sharing your data with the collaborator in Excel, the tidy data should be in one Excel file per table. They should not have multiple worksheets, no macros should be applied to the data, and no columns/cells should be highlighted. Alternatively, the data could be shared in either a CSV [@_comma-separated_2017] or TAB-delimited [@_tab-separated_2017] text file. Caution should always be taken when reading CSV files into Excel as it can sometimes lead to non-reproducible handling of date variables, time variables, and variables that Excel incorrectly assumes are date and/or time variables [@zeeberg_mistaken_2004]. For example, Excel incorrectly assumes the gene `SEPT9` is the date `Sept-9` due to  default date format conversions. Floating-point format conversions cause similar problems. To avoid these issues, use ISO 8601 [@_iso_2017] guidelines when coding date and/or time variables. (See Box 1).

### 2.3 The code book

For almost any data set, the measurements you calculate will need to be described in more detail than you can or should sneak into the spreadsheet. The code book (also referred to as a 'data dictionary') contains this information. At minimum it should contain:

1. Information about the variables (including units!) in the data set not contained in the tidy data.
1. Information about the summary choices you made.
1. Information about the experimental study design you used.

In our blood example, the statistician would want to know what the unit of measurement for each demographic variable is (age in years, treatment by name/dose, level of diagnosis and how heterogeneous). They would also want to know any other information about how you did the data collection/study design. For example, are these the first 20 patients that walked into the clinic? Are they 20 highly selected patients by some characteristic like age? Are they randomized to treatments? This is the place for any detail about either the experimental design or the data itself that may be informative to the statistician.

A common format for this document is a Word file. There should be a section called "Study design" that has a thorough description of the question being asked by the study as well as how you collected the data. An additional section called "Code book" should be provided to describe each variable and its units. This information is frequently conveyed most simply in tabular form. In this case, the columns of the table would contain columns including `VariableName`, `Description`, `Units`, `CodingNotes`, and `OtherNotes`. Further columns that provide additional information to the statistician should be included. (**Figure 1C**)

#### How to code variables

When you put variables into a spreadsheet there are several main categories you will run into depending on their data type [@_statistical_2016]:

1. Continuous
1. Ordinal
1. Categorical
1. Missing 
1. Censored

Continuous variables are anything measured on a quantitative scale that could be any fractional number. An example would be something like weight measured in kg. Ordinal data [@_ordinal_2017] are data that have a fixed, small (< 100) number of levels but are ordered. This could be for example survey responses where the choices are: poor, fair, good. Categorical data [@_categorical_2017] are data where there are multiple categories, but they aren't ordered. One example would be sex: male or female.  Missing data [@_missing_2016] are data that are unobserved and you don't know the mechanism. Missing values should be coded as `NA`. If, however, missingness is coded in an alternative manner, this should be explicitly noted in the code book. Censored data [@_censoring_2016] are data where you know the missingness mechanism on some level. Common examples are a measurement being below a detection limit or a patient being lost to follow-up. They should also be coded as `NA` when you don't have the data. But you should also add a new column to your tidy data called, "VariableNameCensored" which should have values of `TRUE` if censored and `FALSE` if not. In the code book you should explain why those values are missing. It is absolutely critical to report to the analyst if there is a reason you know about that some of the data are missing. You should also not impute [@_imputation_2017]/make up/throw away missing observations. 

Explanations for the reasoning behind variable coding guidelines can be found in Box 1. Generally, try to avoid coding categorical or ordinal variables as numbers. When you enter the value for sex in the tidy data, it should be "male" or "female". The ordinal values in the data set should be "poor", "fair", and "good" not 1, 2, 3. This coding is attractive because it is self-documenting; any ambiguity or need for interpretation by the analyst is removed. This will ultimately avoid potential mix-ups about which direction effects go and will help identify coding errors. 

Always encode every piece of information about your observations using text. For example, if you are storing data in Excel and use a form of colored text or cell background formatting to indicate information about an observation ("red variable entries were observed in experiment 1.") then this information will not be exported (and will be lost!) when the data is exported as raw text.  Every piece of data should be encoded as actual text that can be exported.  

### 2.4 The instruction list/script

You may have heard this before, but reproducibility is a big deal in computational science [@peng_reproducible_2011]. To accomplish this goal, best practices have been discussed in detail previously [@wilson_good_2016]. However, for simplicity here, this means that when you submit your paper, the reviewers and the rest of the world should be able to exactly replicate the analyses from raw data all the way to final results.  If you are trying to be efficient, you will likely perform some summarization/data analysis steps before the data can be considered tidy and passed off to your statistician. 

The ideal thing for you to do when performing summarization is to create a computer script (in `R`, `Python`, or something else) that takes the raw data as input and produces the tidy data you are sharing as output. Ideally, this script would be run a couple of times to ensure the code produces the same output. 

Alternatively, in many cases, the person who collected the data may not know how to code in a scripting language. However, he or she still has incentive to make the data tidy for a statistician to speed the process of collaboration. In this case, the statistician should be provided something called pseudocode [@_pseudocode_2017] (**Figure 1D**). It should look something like:

1. Step 1 - take the raw file, run version 3.1.2 of summarize software with parameters a=1, b=2, c=3
1. Step 2 - run the software separately for each sample
1. Step 3 - take column three of outputfile.out for each sample and that is the corresponding row in the output data set

You should also include information about which system (Mac/Windows/Linux) you used the software on, the specific version of any software used, and whether you tried it more than once to confirm it gave the same results. Ideally, you will run this by a fellow student/labmate to confirm that they can obtain the same output file you did. 

3. What you should expect from the analyst
====================

When you turn over a properly tidied data set it dramatically decreases the workload on the statistician and minimizes the likelihood of errors during analysis. By taking the time to tidy the data, the data generator, who knows the details of the data generated better than anyone else, can expect to get the analysis back sooner and can be more confident in its accuracy. Careful statisticians will check your recipe, ask questions about steps you performed, and try to confirm that they can obtain the same tidy data that you did with, at minimum, spot checks.

You should then expect from the statistician:

1. An analysis script that performs each of the analyses (not just instructions).
2. The exact computer code they used to run the analysis.
3. All output files/figures they generated. 

This is the information you will use in the supplement to establish reproducibility and precision of your results. Each of the steps in the analysis should be clearly explained and you should ask questions when you don't understand what the analyst did. It is the responsibility of both the statistician and the scientist to understand the statistical analysis. You may not be able to perform the exact analyses without the statistician's code, but you should be able to explain why the statistician performed each step to a labmate/your principal investigator. 

4. Discussion
====================
These guidelines aim to provide guidelines for effective and efficient data sharing between those generating data and those analyzing it. We highlight the need for data generators to (1) provide data in a tidy and consistently coded format, (2) include all the necessary experimental information regarding data generation, and (3) to explain any steps taken to pre-process the data. If followed, these guidelines will both speed up analysis turnaround time and minimize the likelihood of errors during analysis. 


Acknowledgments
====================
We want to thank Jenny Bryan and the referees, Nicholas Horton and Stephen Turner, for their helpful contributions and edits. We would additionally like to thank Foram Ashar, Pat Carlson, Claude Chaunier, Leonardo Collado-Torres, Dan Fowler, David Jankoski, Sean Kross, Gene Miller, Leslie Myint, and Nick Reich for their suggestions and edits.

Funding
====================
This work was supported by NIH R01 GM105705.


Figure Legends
====================
**Figure 1** -- **A. Raw (and tidy) data.** The raw measurements taken during the experiment are included here for the 40 patients (rows) and 20 hormones (columns) measured. Indicating that these are raw values conveys that no manipulation or computation has been done on the included values.  **B. Tidy data.** These data convey the important clinical information to your statistician for each sample (rows) across a number of variables (columns).  With a single value included in each cell and consistently and informatively named column headers and values, these tidy data can be easily understood and used by your statistician. Importantly, `PatientID` is coded in the exact same way between Hormone_Data_raw.csv and Demographic_Data.csv enabling easy joining/merging by the statistician. **C. Code Book.** Here, all pertinent and detailed information about both the experiment and the data are conveyed to the statistician. This is the place for any extra detail that does not fit into the data generator’s tidy data tables/spreadsheets. **D. Pseudocode.** This includes information about any processing steps taken to get the data into the form in which the statistician is receiving it. 

# References


