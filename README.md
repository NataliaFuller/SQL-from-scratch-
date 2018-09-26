# SQL-from-scratch-
-- Quiz Funnel
SELECT * 
FROM survey 
LIMIT 10;

SELECT question,
COUNT(DISTINCT user_id)
FROM survey
GROUP BY 1;

-- Home Try-On Funnel

SELECT *
FROM quiz
LIMIT 20;

SELECT *
FROM home_try_on
LIMIT 20;

SELECT *
FROM purchase
LIMIT 20;

SELECT DISTINCT q.user_id,
   h.user_id IS NOT NULL AS 'is_home_try_on',
   h.number_of_pairs,
   p.user_id IS NOT NULL AS 'is_purchase'
FROM quiz q
LEFT JOIN home_try_on h
   ON q.user_id = h.user_id
LEFT JOIN purchase p
   ON p.user_id = q.user_id
LIMIT 100;

WITH funnels AS(SELECT DISTINCT q.user_id,
   h.user_id IS NOT NULL AS 'is_home_try_on',
   h.number_of_pairs,
   p.user_id IS NOT NULL AS 'is_purchase'
FROM quiz q
LEFT JOIN home_try_on h
   ON q.user_id = h.user_id
LEFT JOIN purchase p
   ON p.user_id = q.user_id)
   SELECT COUNT(*) AS 'quiz', 
SUM(is_home_try_on) AS 'num_try_on',
SUM(is_purchase) AS 'purchased',
1.0 * SUM(is_home_try_on) / COUNT(user_id) AS 'browse_to_home_try_on',
1.0 * SUM(is_purchase) / SUM(is_home_try_on) AS 'try_on_to_purchase'
FROM funnels;

-- 2.1 at home try on A/B

SELECT DISTINCT q.user_id,
   h.user_id IS NOT NULL AS 'is_home_try_on',
   h.number_of_pairs,
   p.user_id IS NOT NULL AS 'is_purchase'
FROM quiz q
LEFT JOIN home_try_on h
   ON q.user_id = h.user_id
LEFT JOIN purchase p
   ON p.user_id = q.user_id
 WHERE number_of_pairs = '3 pairs'
 limit 100;

SELECT DISTINCT q.user_id,
   h.user_id IS NOT NULL AS 'is_home_try_on',
   h.number_of_pairs,
   p.user_id IS NOT NULL AS 'is_purchase'
FROM quiz q
LEFT JOIN home_try_on h
   ON q.user_id = h.user_id
LEFT JOIN purchase p
   ON p.user_id = q.user_id
 WHERE number_of_pairs = '5 pairs'
 limit 100;
 
SELECT *
FROM purchase
WHERE price < 100;

-- 1.2 Popular shapes
SELECT quiz.user_id, fit, shape, COUNT(*)
FROM quiz
JOIN purchase p
ON quiz.user_id = p.user_id
WHERE fit in ('Narrow')
GROUP BY shape
LIMIT 100;

SELECT quiz.user_id, fit, shape, COUNT(*)
FROM quiz
JOIN purchase p
ON quiz.user_id = p.user_id
WHERE fit in ('Medium')
GROUP BY shape
LIMIT 100;

SELECT quiz.user_id, fit, shape, COUNT(*)
FROM quiz
JOIN purchase p
ON quiz.user_id = p.user_id
WHERE fit in ('Wide')
GROUP BY shape
LIMIT 100;

-- 1.3 Colors
SELECT style, color, COUNT(*) orders
FROM purchase
WHERE style in ("Women's Styles")
GROUP BY color
ORDER BY 3 DESC
LIMIT 100;

SELECT style, color, COUNT(*) orders
FROM purchase
WHERE style in ("Men's Styles")
GROUP BY color
ORDER BY 3 DESC
LIMIT 100;
 
  
