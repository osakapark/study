# recursive

```
/* Formatted on 2020/06/06 오후 10:13:57 (QP5 v5.360) */
SELECT department_id,
       department_name,
       '0'                            AS parent_id,
       '1'                            AS levels,
       parent_id || department_id     AS sort
  FROM departments
 WHERE parent_id IS NULL
UNION ALL
SELECT t2.department_id,
       LPAD (' ', 3 * (2 - 1)) || t2.department_name     AS department_name,
       t2.parent_id,
       '2'                                               AS levels,
       t2.parent_id || t2.department_id                  AS sort
  FROM departments t1, departments t2
 WHERE t1.parent_id IS NULL AND t2.parent_id = t1.department_id
UNION ALL
SELECT t3.department_id,
       LPAD (' ', 3 * (3 - 1)) || t3.department_name        AS department_name,
       t3.parent_id,
       '3'                                                  AS levels,
       t2.parent_id || t3.parent_id || t3.department_id     AS sort
  FROM departments t1, departments t2, departments t3
 WHERE     t1.parent_id IS NULL
       AND t2.parent_id = t1.department_id
       AND t3.parent_id = t2.department_id
ORDER BY sort
```

```
/* Formatted on 2020/06/06 오후 10:13:50 (QP5 v5.360) */
    SELECT department_id, LPAD (' ', 3 * (LEVEL - 1)) || department_name, LEVEL
      FROM departments
START WITH parent_id IS NULL
CONNECT BY PRIOR department_id = parent_id;
```

```
/* Formatted on 2020/06/06 오후 10:13:54 (QP5 v5.360) */
WITH
    recur (department_id,
           parent_id,
           department_name,
           lvl)
    AS
        (SELECT department_id,
                parent_id,
                department_name,
                1     AS lvl
           FROM departments
          WHERE parent_id IS NULL
         UNION ALL
         SELECT a.department_id,
                a.parent_id,
                a.department_name,
                b.lvl + 1
           FROM departments a, recur b
          WHERE a.parent_id = b.department_id)
        SEARCH DEPTH FIRST BY department_id SET order_seq
SELECT department_id,
       LPAD (' ', 3 * (lvl - 1)) || department_name,
       lvl,
       order_seq
  FROM recur;
```
