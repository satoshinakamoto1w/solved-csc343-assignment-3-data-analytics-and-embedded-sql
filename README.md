Download Link: https://assignmentchef.com/product/solved-csc343-assignment-3-data-analytics-and-embedded-sql
<br>
The goal of this program is to use data about courses, topics and skills – collected in the CEA database – to produce course recommendations, similar to movie and music recommendations in such applications as Netflix or Pandora.

The idea of an automatic recommender is quite simple: find a group of users most similar to an active user and ask what did they rank highly. Use these highly ranked items as recommendations for an active user. If you want to explore the topic of recommenders in more details, you can read a book chapter excerpt provided on the assignment page.

When a new active user logs in, your application should collect data about this new user in order to find his/her nearest neighbors – students most similar to him/her. In addition, you should verify what courses this active user has already taken – in order to exclude them from your recommendations.

Once you collect all the required information, you present the user with the list of recommended courses, ranked according to the user-specified criteria.

<h1>1.      Database</h1>

The database instance represents a snapshot of the CEA database – so familiar to you: all the included entities, attributes and relationships have the same meaning as in Assignment 1. SQL scripts for recreating an SQLite instance of this database are provided in cea_db.zip.

The database structure is summarized in the E/R diagram in Figure 1, and in the database schema below.

Note that artificial integer IDs have been added to the week entities such as Course and Course Edition, to make joins more efficient. Also the ternary relationships between student, course edition and skill (topic) contain an additional attribute course id, to ensure that any combination of (skill, course) is valid: foreign key constraints.




<em>Figure 1. E/R diagram of the CEA database </em>

<em> </em>

<em>Relational schema: </em>

<em>Departments</em> (<em><u>dept_code</u></em>, <em>dept_name</em>)

<em>Courses</em> (<em><u>course_id</u></em>, <em>dept_code</em>, <em>course_number</em>, <em>course_name</em>)

<em>Course_editions</em> (<em><u>edition_id</u></em>, <em>course_id</em>, <em>year</em>,  <em>semester</em>, <em>total_students</em>, <em>time_day</em>)

<em>Students</em> (<em><u>username</u></em>, <em>permission</em>, <em>age</em>, <em>gender</em>, <em>native_country</em>)

<em>Skills</em> (<em><u>skill_id</u></em>, <em>skill</em>)

<em>Topics</em> (<em><u>topic_id</u></em>, <em>topic</em>)

<em>Course_topics</em> (<em><u>topic_id</u></em>, <em><u>course_id</u></em>)

<em>Course_skills</em> (<em><u>skill_id</u></em>, <em><u>course_id</u></em>)

<em>Enrollments</em> (<em><u>username</u></em>, <em><u>edition_id</u></em>, <em>letter_grade</em>, <em>course_ranking</em>, <em>instr_ranking</em>)

<em>Skill_rankings</em> (<em><u>username</u></em>, <em><u>edition_id</u></em>, <em>course_id</em>, <em><u>skill_id</u></em>, <em>rank_before</em>, <em>rank_after</em>)

<em>Topic_interests</em> (<em>username</em>, <em>edition_id</em>, <em>course_id</em>, <em>topic_id</em>, <em>interest_before</em>, <em>interest_after</em>) <em>Letter_grades</em> (<em>letter_grade</em>, <em>min_grade</em>, <em>max_grade</em>, <em>gpv</em>)




<h1>2.      Functionality</h1>

<em>3.1.</em><em> Initial user information </em>

An active user logs in and if his<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> user name is not found in the database, the initial demographic information is collected and recorded into the Students table. Otherwise the existing information is used.

<em>3.2.</em><em> Collecting user courses </em>

The first step is to identify which courses this student has taken. This applies to both a new and a returning user. The courses are not added to the existing database yet, but are recorded into an appropriate data structure for the future use.

<em>3.3.</em><em> Collecting user interests </em>

Then the user needs to express his interest in the available topics. The topics are presented grouped by departments, to make navigation and selection easier. The user does not have to rank all the available topics.

<em>3.4.</em><em> Collecting user skills </em>

Next the user accesses his level of different skills, which are also presented grouped by the departments. Again, the user has the right to omit some skills.

<em>3.5.</em><em> Computing potential recommendations </em>

The program then finds 15 users which are most similar to an active user by their demographic characteristics, level of skills before the course, and topic interests before the course. It extracts all courses which these top 15 users have ranked, except the ones which have been already taken by the active user. Do not forget to exclude an active user himself from the set of his nearest neighbors.




<em>3.6.</em><em> Presenting recommendations based on the user-specified criteria </em>

At this point the user is presented with the choice of how he wants his recommendations to be ranked.

<ul>

 <li>“Recommend courses with the best predicted grade”</li>

</ul>

The first choice is by the expected grade. For each course that program has identified as possible recommendation, it computes an average grade, based on the numeric max_grade corresponding to the letter grade, and converts it to the letter grade again. Then all the recommendations are sorted by this average grade in descending order and 5 top are presented to the active user.

<ul>

 <li>“Recommend courses which promote my interests”</li>

</ul>

The second choice is based on the development of student interests in different topics. The program will rank the potential candidate courses by an average increase in topic interest after the course, taking into account only those topics that the user expressed some interest in.

<ul>

 <li>“Recommend courses which improve my skills”</li>

</ul>

The third choice is based on an average skill improvement after each course. Again, the courses with the best skill improvement are presented as course recommendations. For both option 2 and option 3, the relevant improvements are summed up for each course before averaging them among all 15 nearest neighbors.

<ul>

 <li>“Recommend courses which make me happy” (with the best predicted evaluation score) Finally, the user may select to see the recommended courses based on the best average course satisfaction.</li>

</ul>

You do not have to apply additional weights to all the values computed above, however if you want you may weigh each value by the inverse of the distance from an active user (see book examples for details)

<h1>3.      Data collection</h1>

After the user explored all his recommendations and before he exits, he is prompted to add his own data to the CEA database. For each course he has already taken (recorded in step 3.2), the regular questions such as grade, course rank and instructor rank are asked. The user may want to add new topics and skills, and record skill improvements and topic interest dynamics. You may reuse your code from Assignment 1 for this part. This step of the data collection should be optional for the user.

<a href="#_ftnref1" name="_ftn1">[1]</a> <em>His, he, him</em> is used in a gender-neutral sense throughout the text.