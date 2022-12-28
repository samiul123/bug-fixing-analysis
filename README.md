# bug-fixing-analysis

In this project, I have analysed the opened and closed bugs of Pytorch and Tensorflow in the last 2 years(2021-2022). 

I analyzed 5 properties of every issue in both the projects. The properties are below:
* **lifetime(days)**: Duration (in days) between issue create and close time. This metric helps to 
  understand whether it takes much time to close the issue. If this duration is higher, the corresponding issues
  may need to be broken down more.
* **first_response_duration(days)**: Duration (in days) between issue create and first comment time. Though there are 
  many forms of response like reactions, commit etc, I have considered only comments for simplicity. This metric
  indicates how responsive the developer community is or are the issues ignored for long time after being posted. 
* **number_of_comments**: Number of human(not *bots*) comments in issue. It helps to understand how much discussion 
  is needed to close the issue. Overall, it gives hints about community engagement.
* **number_of_labels**: Number of labels in issue. It gives an idea on using the number of labels per issue.
  Labels are important to track the issue in project. 
* **number_of_assignees**: Number of assignees in issue. This metric will help on understanding the issue scope.
  If it needs more assignees than expected, the issue might be critical or has larger scope than expected.
The analyzed data are in *analysis/pytorch_issues_analysis.csv* and *analysis/tensorflow_issues_analysis.csv*.

In addition, *analysis/repository_analysis.csv* contains the following self-explanatory data for every project.
* **median_lifetime(days)**
* **median_first_response(days)**
* **avg_number_comments_per_issue**
* **percentage_of_issues_with_no_assignees**
* **avg_number_of_labels_per_issue**

### Analysis Methodology
* **Data fetch**
  * Originally data are fetched through [github issue API](https://docs.github.com/en/rest/issues/issues?apiVersion=2022-11-28#list-repository-issues)
    with the query parameter *state=closed*.
  * Data are fetched page by page
  * Original data are stored in [pytorch_issues.json](pytorch_issues.json) and [tensorflow_issues.json](tensorflow_issues.json) files.
* **Data filter**
  * Github issue API returns both bugs and pull requests. To retrieve only bugs, data, whose *pull_request* is null, are filtered.
  * Data are also filtered based on *created_at* and *closed_at* datetime fields being after January 1, 2021.
* **Construct initial working dataframe**
  * Necessary columns *('number', 'created_at', 'closed_at', 'comments', 'assignees', 'labels')* are fetched from the above filtered dataframe.
  * Used multi-threading for comments data retrieval as they are huge. It took lots of time in single thread.
  * Created a new column *first_comment_created_at* as the first comment time of every issue.
  * Comments data are stored in [pytorch_comments.csv](pytorch_comments.csv) and [tensorflow_comments.csv](tensorflow_comments.csv) files.
  * Removed rows having null value
* **Calculating lifetime**
  * Difference between issue created and closed time.
* **Calculating first response**
  * Difference between issue created and first comment time.
* **Calculating number of comments**
  * Number of comments for every issue.
* **Calculating number of assignees**
  * Length of assignees list per issue
* **Calculating number of labels**
  * Length of labels list per issue
* **Top Labels**
  * Construct a dataframe of labels data
  * Calculated the number of times each label is used.
  * Label summary is stored in [tensorflow_labels_summary.csv](analysis/tensorflow_labels_summary.csv) and
    [pytorch_labels_summary.csv](analysis/pytorch_labels_summary.csv)
  * Top 10 labels with the most counts are retrieved.
* **Top Commentator**
  * Count of commentator id (*login* field of [pytorch_comments.csv](pytorch_comments.csv) or 
    [tensorflow_comments.csv](tensorflow_comments.csv))
  * Top 10 commentator are retrieved based on their counts in comments data.

### Summary of analysis:
* **Distribution of lifetime(days)**:<br>
  ![Distribution of lifetime(days)](images/Distribution%20of%20issue%20lifetime(days).png "Distribution of lifetime(days)")
  
  **Tensorflow**
  * Distribution is right skewed, as in most of the cases, issues do not take much time to close.

  **Pytorch**
  * Distribution is right skewed, as in most of the cases, issues do not take much time to close.

  There are some outliers in both distributions


* **Distribution of first response(days)**
  ![Distribution of first response(days)](images/Distribution%20of%20first%20responses(days).png "Distribution of first responses(days)")
  
  **Tensorflow**
  * Distribution of first responses in Tensorflow is bimodal.
  * Most of the first responses are posted either early or lately

  **Pytorch**
  * Distribution of first responses in Pytorch is right skewed.
  * Most of the first responses does not take much time to be posted.


* **Distribution of number of comments**
  ![Distribution of number of comments](images/Distribution%20of%20number%20of%20comments.png "Distribution of number of comments")
  
  **Tensorflow**
  * Distribution is right skewed, as in most of the cases, issues have 3 to 5 comments.

  **Pytorch**
  * Distribution is right skewed, as in most of the cases, issues have 1 to 2 comments.

  There are some outliers in both distributions


* **Distribution of number of assignees**
  ![Distribution of number of assignees](images/Distribution%20of%20number%20of%20assignees.png "Distribution of number of assignees")
  
  **Tensorflow**
  * Most of the issues have 1 assignee.
  * Distribution is right skewed, as in most of the cases, 1 issue is assigned to 1 assignee

  **Pytorch**
  * Most of the issues do not have any assignee.
  * Distribution is right skewed


* **Distribution of number of labels**
  ![Distribution of number of labels](images/Distribution%20of%20number%20of%20labels.png "Distribution of number of labels")
  
  **Tensorflow**
  * Distribution is left skewed, as most of the issues have 4 to 5 labels

  **Pytorch**
  * Distribution is slightly right skewed, as most of the issues have 2 to 3 labels


* **Top Commenter**

  ![Top Commenters](images/Top%20commenter.png "Top Commenters")


* **Top 10 labels**

  ![Top 10 Labels](images/Top%2010%20labels.png "Top 10 Labels")
  * Labels for the tensorflow project supports the long time of first response of the issues. The top most
    used label is *stat: awaiting response* which is described as *Status - Awaiting response from author*
    in [tensorflow_labels_summary.csv](analysis/tensorflow_labels_summary.csv)


* **Default Labels**<br><br>

  Default labels are provided by github itself.
  ![Default Labels](images/Default%20Labels.png "Default Labels")
 
  * Both the projects use custom labels mostly.





