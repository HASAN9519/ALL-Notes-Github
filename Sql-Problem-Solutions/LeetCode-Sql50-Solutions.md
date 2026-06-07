## LeetCode Sql 50 solutions

## some information about Postgres

    Type	            Storage	            Precision	    Nature	            Use case
    integer	            4 bytes	            Exact	        Whole numbers	    Counts, IDs
    numeric(p,s)	    Variable	        Exact	        Fixed precision	    Financial data, exact decimals
    real (float4)	    4 bytes	            ~7 digits	    Approximate	        Lightweight float
    double precision	8 bytes	            ~15 digits	    Approximate	        Scientific, statistical, ratios

N.B. make one side non‑integer (1.0, ::numeric or ::decimal) so PostgreSQL does fractional division instead of truncating to zero, columns used in group by should be in select clause if it's not used in aggregation, aggregate functions must go in the HAVING clause after grouping.

#### Basic Joins

570. Managers with at Least 5 Direct Reports

    -- code in Postgres

    SELECT m.name
    FROM Employee AS m
    JOIN (
        SELECT managerId
        FROM Employee
        WHERE managerId IS NOT NULL
        GROUP BY managerId
        HAVING COUNT(managerId) >= 5
    ) AS sub ON m.id = sub.managerId;

    -- view count of direct reports alongside the name
    SELECT m.name, sub.report_count
    FROM Employee AS m
    JOIN (
        SELECT managerId, COUNT(*) AS report_count
        FROM Employee
        WHERE managerId IS NOT NULL
        GROUP BY managerId
        HAVING COUNT(*) >= 5
    ) AS sub ON m.id = sub.managerId;    

1934. Confirmation Rate

    -- code in Postgres

    WITH join_table AS(
        SELECT s.user_id, c.action FROM Signups s 
        LEFT JOIN Confirmations c ON s.user_id=c.user_id
    ) 
    SELECT user_id, 
        1.0 * ROUND(SUM(CASE
        WHEN action='confirmed' THEN 1 ELSE 0 END)::numeric / COUNT(*), 2) 
        AS confirmation_rate
    FROM join_table GROUP BY user_id ORDER BY user_id;


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
    -- execution order: where, case, DISTINCT

    SELECT DISTINCT ON (product_id) 
            product_id,
            CASE
                WHEN prev_price >= 10 AND change_date <= '2019-08-16'
                    THEN new_price  -- show new_price for this case
                WHEN prev_price = 10 AND change_date > '2019-08-16'
                    THEN prev_price   -- show prev_price for this case
            END AS price FROM
            (SELECT
                product_id, 
                LAG(new_price, 1, 10) OVER (PARTITION BY product_id ORDER BY change_date) AS prev_price,
                new_price,
                LAG(new_price, 2, 0) OVER (PARTITION BY product_id ORDER BY change_date) AS prev_prev_price,
                change_date
            FROM Products ORDER BY product_id, change_date DESC) AS sq
        WHERE 
        (prev_price = 10 AND change_date > '2019-08-16' AND prev_prev_price=0)
        OR 
        (prev_price >= 10 AND change_date <= '2019-08-16');

1204. Last Person to Fit in the Bus

    -- code in Postgres

    SELECT
        person_name
    FROM (
        SELECT Turn, person_name, Weight, SUM(Weight) OVER (ORDER BY Turn) AS running_weight FROM Queue) AS sq
        WHERE 
        running_weight<=1000 ORDER BY running_weight DESC LIMIT 1;

1907. Count Salary Categories

    -- code in Postgres
    -- categories defines fixed set of labels
    -- inner query (counts) calculates actual counts
    -- LEFT JOIN ensures all categories appear, even if missing
    -- COALESCE(..., 0) replaces NULL with 0

    WITH categories AS (
        SELECT 'Low Salary' AS category
        UNION ALL
        SELECT 'Average Salary'
        UNION ALL
        SELECT 'High Salary' 
    )
    SELECT c.category,
        COALESCE(counts.accounts_count, 0) AS accounts_count
    FROM categories c
    LEFT JOIN (
        SELECT CASE
                WHEN income < 20000 THEN 'Low Salary'
                WHEN income BETWEEN 20000 AND 50000 THEN 'Average Salary'
                WHEN income > 50000 THEN 'High Salary'
            END AS category,
            COUNT(account_id) AS accounts_count
        FROM Accounts
        GROUP BY category
    ) AS counts
    ON c.category = counts.category;

#### Subqueries

1978. Employees Whose Manager Left the Company

    -- code in Postgres

    SELECT employee_id
        FROM Employees e
    WHERE e.salary < 30000
        AND e.manager_id IS NOT NULL
        AND e.manager_id NOT IN (
            SELECT employee_id
            FROM Employees
        )
    ORDER BY e.employee_id;

626. Exchange Seats

    -- code in Postgres

    SELECT 
        CASE
            WHEN id % 2 = 1 AND id + 1 <= (SELECT MAX(id) FROM Seat)
                THEN id + 1   -- odd seat, swap with next
            WHEN id % 2 = 0
                THEN id - 1   -- even seat, swap with previous
            ELSE id           -- last odd seat stays
        END AS id,
        student
    FROM Seat
    ORDER BY id;

1341. Movie Rating

    -- code in Postgres

    SELECT name AS results
    FROM (
        SELECT mr.user_id, u.name, COUNT(mr.rating) AS rating_count 
        FROM MovieRating mr INNER JOIN Users u ON mr.user_id=u.user_id 
        GROUP BY mr.user_id, u.name 
        ORDER BY rating_count DESC, u.name LIMIT 1
        ) AS q1
    UNION ALL
    SELECT title AS results
    FROM (
        SELECT m.title, AVG(mr.rating) AS avg_rating
        FROM MovieRating mr INNER JOIN Movies m ON mr.movie_id=m.movie_id
        WHERE mr.created_at BETWEEN '2020-02-01' AND '2020-02-29'
        GROUP BY m.title 
        ORDER BY avg_rating DESC, m.title LIMIT 1
        ) AS q2;

1321. Restaurant Growth

    -- code in Postgres
    -- rolling total and average

    SELECT 
        visited_on, amount, average_amount FROM
        (
        SELECT 
        visited_on,
        ROW_NUMBER() OVER (ORDER BY visited_on) AS rn,  
        SUM(daily_amount) OVER (
                ORDER BY visited_on
                RANGE BETWEEN INTERVAL '6 days' PRECEDING AND CURRENT ROW
        ) AS amount,
        ROUND(AVG(daily_amount) OVER (
                ORDER BY visited_on
                RANGE BETWEEN INTERVAL '6 days' PRECEDING AND CURRENT ROW
        ), 2) AS average_amount       
        FROM 
            (
            -- adding multiple entries for the same date into one row
            SELECT visited_on, SUM(amount) AS daily_amount FROM Customer GROUP BY visited_on
            )
        )
    WHERE rn>=7
    ORDER BY visited_on;    

602. Friend Requests II: Who Has the Most Friends

    -- code in Postgres
    -- using CTE
    -- duplicates are discarded because UNION by default removes duplicates, use UNION ALL to keep all values

    -- first counting accepter_id and requester_id using group by 
    WITH accepter AS (
        SELECT accepter_id AS user_id, COUNT(requester_id) AS req_count
        FROM RequestAccepted
        GROUP BY accepter_id
    ),
    requester AS (
        SELECT requester_id AS user_id, COUNT(accepter_id) AS acc_count
        FROM RequestAccepted
        GROUP BY requester_id
    )

    SELECT u.user_id AS id,
        COALESCE(a.req_count, 0) + COALESCE(r.acc_count, 0) AS num
    FROM (
        SELECT accepter_id AS user_id FROM RequestAccepted
        UNION
        SELECT requester_id AS user_id FROM RequestAccepted
    ) AS u
    LEFT JOIN accepter a ON u.user_id = a.user_id
    LEFT JOIN requester r ON u.user_id = r.user_id
    ORDER BY num DESC LIMIT 1;

585. Investments in 2016

    -- code in Postgres
    -- using CTE, will try to use CTE more 

    WITH base AS (
        SELECT pid,
            tiv_2015,
            tiv_2016,
            lat,
            lon,
            COUNT(*) OVER (PARTITION BY tiv_2015) AS t_count
        FROM Insurance
    ),
    coord_unique AS (
        SELECT lat, lon
        FROM Insurance
        GROUP BY lat, lon
        HAVING COUNT(*) = 1
    )
    SELECT ROUND(SUM(b.tiv_2016)::numeric, 2) AS tiv_2016
    FROM base b
    JOIN coord_unique c
    ON b.lat = c.lat AND b.lon = c.lon
    WHERE b.t_count > 1;

185. Department Top Three Salaries

    -- code in Postgres
    -- first do group by then think about ranking

    SELECT Department, Employee, Salary
    FROM (
        SELECT e.name AS Employee, e.salary AS Salary, d.name AS Department,
        DENSE_RANK() OVER (PARTITION BY d.name ORDER BY e.salary DESC) AS rn
        FROM Employee e JOIN Department d 
        ON e.departmentId=d.id ORDER BY d.name, e.salary DESC) AS subq
    WHERE rn<=3;

#### Advanced String Functions / Regex / Clause

1667. Fix Names in a Table

    -- code in Postgres
    -- || symbol means Concatenation 

    SELECT user_id, UPPER(LEFT(name, 1)) || LOWER(SUBSTRING(name FROM 2)) AS name FROM Users ORDER BY user_id;

1527. Patients With a Condition

    -- code in Postgres

    SELECT patient_id, patient_name, conditions
    FROM Patients
    WHERE (conditions LIKE 'DIAB1%'
        OR conditions LIKE '% DIAB1%')
    AND conditions NOT LIKE '%DIAB1';

196. Delete Duplicate Emails

    -- code in Postgres

    DELETE FROM Person p
    WHERE p.id NOT IN (
        SELECT MIN(id)
        FROM Person
        GROUP BY email
    );

176. Second Highest Salary

    -- code in Postgres

    -- OFFSET 1 LIMIT 1 skips highest and takes next one
    -- If there’s no second highest, subquery returns NULL
    SELECT (
        SELECT DISTINCT salary
        FROM Employee
        ORDER BY salary DESC
        OFFSET 1 LIMIT 1
    ) AS SecondHighestSalary;

    -- with window function
    SELECT salary AS SecondHighestSalary
    FROM (
        SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rn
        FROM Employee
    ) AS subq
    WHERE rn = 2 LIMIT 1; 

1484. Group Sold Products By The Date

    -- code in Postgres
    -- Groups rows by sell_date
    -- Concatenates all product values for that date into one string, separated by commas

    SELECT sell_date, COUNT(*) AS num_sold, STRING_AGG(product,',') AS products
    FROM
    (SELECT DISTINCT ON (sell_date, product) * FROM Activities) AS subq
    GROUP BY sell_date;

1327. List the Products Ordered in a Period

    -- code in Postgres

    SELECT p.product_name, s.unit FROM Products p JOIN 
    (SELECT product_id, SUM(unit) AS unit FROM Orders WHERE order_date BETWEEN '2020-02-01' AND '2020-02-29' GROUP BY product_id HAVING SUM(unit)>=100) AS s 
    ON p.product_id=s.product_id;

1517. Find Users With Valid E-Mails

    -- code in Postgres
    -- using Regex
    -- ^ start of string
    -- [A-Za-z] first character must be a letter (uppercase or lowercase)
    -- [A-Za-z0-9_.-]* zero or more valid characters (letters, digits, underscore, period, dash)
    -- @leetcode\.com literal domain (\. escapes the dot)
    -- $ end of string

    SELECT * FROM Users
    WHERE mail ~ '^[A-Za-z][A-Za-z0-9_.-]*@leetcode\.com$';

