## LeetCode Sql 50 solutions

## some information about Postgres

    Type	            Storage	            Precision	    Nature	            Use case
    integer	            4 bytes	            Exact	        Whole numbers	    Counts, IDs
    numeric(p,s)	    Variable	        Exact	        Fixed precision	    Financial data, exact decimals
    real (float4)	    4 bytes	            ~7 digits	    Approximate	        Lightweight float
    double precision	8 bytes	            ~15 digits	    Approximate	        Scientific, statistical, ratios

N.B. make one side non‑integer (1.0, ::numeric or ::decimal) so PostgreSQL does fractional division instead of truncating to zero, columns used in group by should be in select clause if it's not used in aggregation, aggregate functions must go in the HAVING clause after grouping.


#### Basic Aggregate Functions

1211. Queries Quality and Percentage

    -- code in Postgres
    -- This query handles duplicates naturally since averages and counts include them.
    -- ::numeric ensures you always get decimal math, at least one operand must be cast to a non‑integer type 

    SELECT 
        query_name,
        ROUND(AVG(rating::NUMERIC / position), 2) AS quality,
        ROUND(100.0 * SUM(CASE WHEN rating < 3 THEN 1 ELSE 0 END)::numeric/COUNT(*), 2) AS poor_query_percentage
    FROM queries
    GROUP BY query_name;

1193. Monthly Transactions I

    -- code in Postgres

    SELECT 
        TO_CHAR(trans_date::date, 'YYYY-MM') AS month,
        country,
        COUNT(*) AS trans_count,
        SUM(CASE WHEN state='approved' THEN 1 ELSE 0 END) AS approved_count,
        SUM(amount) AS trans_total_amount,
        SUM(CASE WHEN state='approved' THEN amount ELSE 0 END) AS approved_total_amount
    FROM Transactions
    GROUP BY TO_CHAR(trans_date::date, 'YYYY-MM'), country;

1174. Immediate Food Delivery II

    -- code in Postgres
    -- using CTE
    -- DISTINCT ON (customer_id) ensures one row per customer

    WITH first_orders AS (
        SELECT DISTINCT ON (customer_id)
            customer_id,
            order_date,
            customer_pref_delivery_date
        FROM Delivery
        ORDER BY customer_id, order_date
    )
    SELECT 
        ROUND(
            100.0 * SUM(CASE WHEN order_date = customer_pref_delivery_date THEN 1 ELSE 0 END) 
            / COUNT(*),
            2
        ) AS immediate_percentage
    FROM first_orders;    

550. Game Play Analysis IV

    -- code in Postgres

    WITH game_record AS (
        SELECT DISTINCT ON (player_id)
        player_id, event_date, LEAD(event_date, 1) OVER (PARTITION BY player_id ORDER BY event_date) AS next_play_day FROM Activity
    )
    SELECT
        ROUND(
            1.0 * SUM(CASE
                WHEN next_play_day IS NULL THEN 0
                WHEN ABS(event_date - next_play_day)=1 THEN 1 ELSE 0 END
                ) 
            / COUNT(DISTINCT player_id),
            2
            ) AS fraction
    FROM game_record;   

#### Sorting and Grouping

2356. Number of Unique Subjects Taught by Each Teacher

    -- code in Postgres

    SELECT teacher_id, COUNT(DISTINCT subject_id) AS cnt FROM Teacher GROUP BY teacher_id;

1141. User Activity for the Past 30 Days I

    -- code in Postgres

    SELECT activity_date AS day, COUNT(DISTINCT user_id) AS active_users FROM Activity WHERE activity_date BETWEEN '2019-06-28' AND '2019-07-27' GROUP BY activity_date;

1070. Product Sales Analysis III

    -- code in Postgres

    SELECT product_id, year AS first_year, quantity, price
    FROM (
        SELECT *,
            MIN(year) OVER (PARTITION BY product_id) AS min_year
        FROM Sales
    ) AS subquery
    WHERE year = min_year
    ORDER BY product_id, year;    

596. Classes With at Least 5 Students

    -- code in Postgres

    SELECT class FROM (
    SELECT class, COUNT(student) as student_count FROM Courses GROUP BY class) 
    AS subquery WHERE student_count>=5;

1729. Find Followers Count

    -- code in Postgres

    SELECT user_id, COUNT(follower_id) AS followers_count FROM Followers GROUP BY user_id ORDER BY user_id;

619. Biggest Single Number

    -- code in Postgres

    SELECT MAX(num) AS num FROM(
        SELECT DISTINCT num, COUNT(num) AS count_num from MyNumbers GROUP BY num ORDER BY count_num
        ) AS subquery WHERE count_num=1;

1045. Customers Who Bought All Products

    -- code in Postgres

    SELECT customer_id
    FROM Customer
    GROUP BY customer_id
    HAVING COUNT(DISTINCT product_key) = (SELECT COUNT(*) FROM Product);    

#### Advanced Select and Joins

1731. The Number of Employees Which Report to Each Employee

    -- code in Postgres
    -- self join is used 

    SELECT e.reports_to AS employee_id,
       m.name AS name,
       COUNT(e.employee_id) AS reports_count,
       ROUND(AVG(e.age)) AS average_age
    FROM Employees e
    JOIN Employees m
    ON e.reports_to = m.employee_id
    WHERE e.reports_to IS NOT NULL
    GROUP BY e.reports_to, m.name ORDER BY e.reports_to;

1789. Primary Department for Each Employee

    -- code in Postgres

    SELECT employee_id, department_id FROM Employee WHERE primary_flag='Y' 
    UNION
    SELECT employee_id, department_id
    FROM Employee
    WHERE primary_flag = 'N' AND employee_id NOT IN (
            SELECT employee_id
            FROM Employee
            WHERE primary_flag = 'Y'
        ) 
        ORDER BY employee_id;

610. Triangle Judgement

    -- code in Postgres

    SELECT
        x, y, z,
        CASE
            WHEN x + y > z AND y + z > x AND x + z > y
            THEN 'Yes'
            ELSE 'No'
        END AS triangle
    FROM Triangle;

180. Consecutive Numbers

    -- code in Postgres

    SELECT DISTINCT num AS ConsecutiveNums
    FROM (
        SELECT id, num,
            LAG(num, 1) OVER (ORDER BY id) AS prev1,
            LAG(num, 2) OVER (ORDER BY id) AS prev2,
            LEAD(num, 1) OVER (ORDER BY id) AS next1,
            LEAD(num, 2) OVER (ORDER BY id) AS next2
        FROM Logs
    ) AS sq
    WHERE (num = prev1 AND num = prev2)   -- current + 2 previous
    OR (num = prev1 AND num = next1)   -- current + prev + next
    OR (num = next1 AND num = next2);  -- current + 2 next

1164. Product Price at a Given Date

    -- code in Postgres