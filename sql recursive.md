---
created: 2023-11-24
tags:
  - prog/sql
share: "true"
---

[Create a new MySQL program - myCompiler - myCompiler](https://www.mycompiler.io/new/mysql)
[What Is a Recursive CTE in SQL? | LearnSQL.com](https://learnsql.com/blog/sql-recursive-cte/)

# 例子
```sql 
with recursive t(n) as (
    select 1 -- 初始化
    union all
    select n+1 from t where n<10 -- 递归
)

select * from t
```

## fibonacci
n: 序列号
n1:  $f_{n-2}$
n2: $f_{n-1}$
$$f_n = f_{n-2} +f_{n-1}$$
```sql
WITH RECURSIVE Fibonacci(n1, n2) AS (
    SELECT 0, 1 -- 初始化序列的前两个值
    UNION ALL
    SELECT n2, n1 + n2
    FROM Fibonacci
    WHERE n2 < 1000 -- 您可以设置这个值来限定序列的长度
)
SELECT n1 FROM Fibonacci;
```

## 公司层级
```sql
WITH RECURSIVE company_hierarchy AS (
  SELECT    id,
            first_name,
            last_name,
            boss_id,
        0 AS hierarchy_level
  FROM employees
  WHERE boss_id IS NULL
 
  UNION ALL
   
  SELECT    e.id,
            e.first_name,
            e.last_name,
            e.boss_id,
        hierarchy_level + 1
  FROM employees e, company_hierarchy ch
  WHERE e.boss_id = ch.id
)
 
SELECT   ch.first_name AS employee_first_name,
       ch.last_name AS employee_last_name,
       e.first_name AS boss_first_name,
       e.last_name AS boss_last_name,
       hierarchy_level
FROM company_hierarchy ch
LEFT JOIN employees e
ON ch.boss_id = e.id
ORDER BY ch.hierarchy_level, ch.boss_id;
```