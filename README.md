# bug-fixing-analysis

In this project, I have analysed the opened and closed bugs of Pytorch and Tensorflow in the last 2 years(2021-2022). 

I analyzed 5 properties of every issue in both the projects. The properties are below:
* **lifetime(days)**: Duration (in days) between issue create and close time.
* **first_response_duration(days)**: Duration (in days) between issue create and first comment time. Though there are 
  many forms of response like reactions, commit etc, I have considered only comments for simplicity.
* **number_of_comments**: Number of human(not *bots*) comments in issue
* **number_of_labels**: Number of labels in issue
* **number_of_assignees**: Number of assignees in issue
The analyzed data are in *pytorch_issues_analysis.csv* and *tensorflow_issues_analysis.csv*.

In addition, *repository_analysis.csv* contains the following self-explanatory data for every project.
* **median_lifetime(days)**
* **median_first_response(days)**
* **avg_number_comments_per_issue**
* **percentage_of_issues_with_no_assignees**
* **avg_number_of_labels_per_issue**

#### Summary of analysis:

