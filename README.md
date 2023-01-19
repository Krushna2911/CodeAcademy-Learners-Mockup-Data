# CodeAcademy Learners Mockup Data

**2.** First, use SELECT * from both tables and use your knowledge of queries and aggregate functions to get to know the data:

- 	What are the Top 25 schools (.edu domains)?

**Query:**

             SELECT DISTINCT email_domain
              FROM users
              WHERE email_domain LIKE "%.edu"
             LIMIT 25;
- How many .edu learners are located in New York?

**Query:**

                     SELECT COUNT(DISTINCT user_id)
                      FROM users
                    WHERE city = "New York";

-	The mobile_app column contains either mobile-user or NULL. How many of these Codecademy learners are using the mobile app?

**Query:**

               SELECT mobile_app, COUNT (user_id) AS 
               'Mobile Users Count' FROM users 
               WHERE mobile_app !='' GROUP BY 1;
**3.** The data type of the sign_up_at column is DATETIME. It is for storing a date/time value in the database.
Notice that the values are formatted like:
[2015-01-01 18:33:52]
So the format is:
[YYYY-MM-DD HH:MM:SS]
SQLite comes with a strftime() function - a very powerful function that allows you to return a formatted date.
It takes two arguments:
strftime(format, column)
Let’s test this function out:
[SELECT sign_up_at,
strftime('%S', sign_up_at)
FROM users
GROUP BY 1
LIMIT 20;]
Now, using this function, query for the sign up counts for each hour.
Open in the hints for more info.

**Query:**
     
                 SELECT strftime('%S',sign_up_at) AS 
                 'Hour of the day' ,count(*) AS 'Count by Hour' FROM 
                 users GROUP BY 1 LIMIT 20;

**4.** Join the two tables using JOIN and then see what you can dig out of the data!
-	Do different schools (.edu domains) prefer different courses?

**Query:**

    WITH combine AS (SELECT * FROM users JOIN progress ON users.user_id = progress. user_id),
    classes AS(SELECT COUNT(learn_cpp), COUNT(learn_sql), COUNT(learn_html),
    COUNT(learn_javascript), COUNT(learn_java)FROM combine
          WHERE email_domain LIKE "%.edu"
       AND (learn_cpp = "completed" OR learn_cpp = "started"))
       SELECT *FROM classes;

-	What courses are the New Yorkers students taking?

**Query:**

           WITH combine AS (SELECT * FROM users JOIN progress ON users.user_id = progress. user_id),
           classes AS(SELECT COUNT(learn_cpp), COUNT(learn_sql), 
           COUNT(learn_html), COUNT(learn_javascript), COUNT(learn_java)FROM combine
           WHERE email_domain LIKE "%.edu"
            AND city = "New York"
           AND (learn_cpp = "completed" OR learn_cpp = "started"))
            SELECT *
           FROM classes;
       
-	What courses are the Chicago students taking?

**Query:**

       WITH combine AS (SELECT * FROM users JOIN progress ON users.user_id = progress. user_id), classes AS
      (SELECT COUNT(learn_cpp), COUNT(learn_sql), COUNT(learn_html), COUNT(learn_javascript), COUNT(learn_java)
        FROM combine
        WHERE email_domain LIKE "%.edu"
      AND city = "Chicago"
      AND (learn_cpp = "completed" OR learn_cpp = "started"))
       SELECT *
      FROM classes;
