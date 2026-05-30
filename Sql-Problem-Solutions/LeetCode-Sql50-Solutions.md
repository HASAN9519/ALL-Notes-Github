## LeetCode Sql 50 solutions

## some information about Postgres

    Type	            Storage	            Precision	    Nature	            Use case
    integer	            4 bytes	            Exact	        Whole numbers	    Counts, IDs
    numeric(p,s)	    Variable	        Exact	        Fixed precision	    Financial data, exact decimals
    real (float4)	    4 bytes	            ~7 digits	    Approximate	        Lightweight float
    double precision	8 bytes	            ~15 digits	    Approximate	        Scientific, statistical, ratios

#### Basic Aggregate Functions

1211. Queries Quality and Percentage

    -- code in Postgres
    -- This query handles duplicates naturally since averages and counts include them.
    -- ::numeric ensures you always get decimal math, need to make numeric one of the Variable

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
