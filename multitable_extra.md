CASE clause
===========

[CASE](https://www.postgresql.org/docs/13/functions-conditional.html#FUNCTIONS-CASE)
permits conditionals in select statement outputs.

``` {.postgresql}
/* using case statement */ 
SELECT account_id, balance, limit,
CASE 
WHEN balance>limit then 'OVERDRAWN' 
WHEN balance=0 THEN 'ZERO' 
ELSE 'OK' 
END AS status
FROM accounts /* rest of query */
;
```


Domains {#sec:domains}
=======

[Domains](https://www.postgresql.org/docs/13/domains.html) are
user-defined types based on underlying type. Defaults to NULL allowed,
best to define any NOT NULL conditions on the underlying columns. CHECK
constraints can be defined.

``` {.postgresql}
/* Number to hold 1-10 user rating */
CREATE DOMAIN rating AS integer CHECK ( VALUE >= 1 AND VALUE <=10 );

/* Just use the domain as type when creating table */
CREATE TABLE restaurants (
    id bigserial primary key,
    /* creating two columns using our domain: */
    visitor_rating rating not null,
    reviewer_rating rating, 
    /* other columns */
);
```

Enumerated types {#sec:enumerated-types}
================

[Enumerated
types](https://www.postgresql.org/docs/13/datatype-enum.html) allow
columns to accept a small number of allowable values. Examples include
day of week, identified gender, north / south / east / west.

``` {.postgresql}
/* creating an enumerated type */
CREATE TYPE mood AS ENUM ( 'sad', 'ok', 'happy');

/* creating column using enumerated type  */
ALTER TABLE people ADD COLUMN reported_mood mood;
```