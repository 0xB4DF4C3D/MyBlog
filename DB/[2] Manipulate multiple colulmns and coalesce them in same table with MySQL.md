2017년 6월 27일 오전 3시 20분
# Manipulate multiple colulmns and coalesce them in same table with MySQL.
_ _ _
환경은 다음과 같다.
- MySQL 5.7.18-ndb-7.6.2
- x86_64
- Linux (Ubuntu 14.04.5)
_ _ _

하고자 하려는건 제목에 나와있듯이 같은 테이블 내의 컬럼들을 조작하고 합치는 것이다. 아직 MySQL을 배운지 일주일도 안된지라 더 우아한 방법을 찾게되면 수정할 것이다.

**1. Sort each columns**

sakila DB의 actor table을 보자. 

<pre>
+----------+-------------+--------------+---------------------+
| actor_id | first_name  | last_name    | last_update         |
+----------+-------------+--------------+---------------------+
|        1 | PENELOPE    | GUINESS      | 2006-02-15 04:34:33 |
|        2 | NICK        | WAHLBERG     | 2006-02-15 04:34:33 |
|        3 | ED          | CHASE        | 2006-02-15 04:34:33 |
|        4 | JENNIFER    | DAVIS        | 2006-02-15 04:34:33 |
|        5 | JOHNNY      | LOLLOBRIGIDA | 2006-02-15 04:34:33 |
|        6 | BETTE       | NICHOLSON    | 2006-02-15 04:34:33 |
|        7 | GRACE       | MOSTEL       | 2006-02-15 04:34:33 |
|        8 | MATTHEW     | JOHANSSON    | 2006-02-15 04:34:33 |
|        9 | JOE         | SWANK        | 2006-02-15 04:34:33 |
|       10 | CHRISTIAN   | GABLE        | 2006-02-15 04:34:33 |
|       11 | ZERO        | CAGE         | 2006-02-15 04:34:33 |
...
</pre>

여기서 first_name과 last_name을 각각 정렬하여 보고 싶다. 한번의 쿼리로! 그래서 나는 다음과 같은 쿼리로 해결하였다.

<pre>
<span style="font-size: 0.73em;">root@localhost at sakila said that.. SELECT first_name, last_name
    -> FROM (SELECT @i:=@i+1 idx, first_name FROM (SELECT @i:=0) i, (SELECT DISTINCT first_name FROM actor ORDER BY first_name) q1) t1
    -> LEFT OUTER JOIN
    -> (SELECT @j:=@j+1 idx, last_name FROM (SELECT @j:=0) j, (SELECT DISTINCT last_name FROM actor ORDER BY last_name) q2) t2
    -> ON t1.idx = t2.idx;</span>
</pre>

결과는 다음과 같다.

<pre>
+-------------+--------------+
| first_name  | last_name    |
+-------------+--------------+
| ADAM        | AKROYD       |
| AL          | ALLEN        |
| ALAN        | ASTAIRE      |
| ALBERT      | BACALL       |
| ALEC        | BAILEY       |
| ANGELA      | BALE         |
| ANGELINA    | BALL         |
| ANNE        | BARRYMORE    |
| AUDREY      | BASINGER     |
| BELA        | BENING       |
| BEN         | BERGEN       |
| BETTE       | BERGMAN      |
| BOB         | BERRY        |
| BURT        | BIRCH        |
| CAMERON     | BLOOM        |
| CARMEN      | BOLGER       |
| CARY        | BRIDGES      |
| CATE        | BRODY        |
| CHARLIZE    | BULLOCK      |
| CHRIS       | CAGE         |
| CHRISTIAN   | CARREY       |
| CHRISTOPHER | CHAPLIN      |
| CUBA        | CHASE        |
| DAN         | CLOSE        |
| DARYL       | COSTNER      |
| DEBBIE      | CRAWFORD     |
| DUSTIN      | CRONYN       |
| ED          | CROWE        |
...
| LAURA       | MCKELLEN     |
| LAURENCE    | MCQUEEN      |
| LISA        | MIRANDA      |
| LIZA        | MONROE       |
| LUCILLE     | MOSTEL       |
| MAE         | NEESON       |
| MARY        | NICHOLSON    |
| MATTHEW     | NOLTE        |
| MEG         | OLIVIER      |
| MENA        | PALTROW      |
| MERYL       | PECK         |
| MICHAEL     | PENN         |
| MICHELLE    | PESCI        |
| MILLA       | PFEIFFER     |
| MINNIE      | PHOENIX      |
| MORGAN      | PINKETT      |
| NATALIE     | PITT         |
| NICK        | POSEY        |
| OLYMPIA     | PRESLEY      |
| OPRAH       | REYNOLDS     |
| PARKER      | RYDER        |
| PENELOPE    | SILVERSTONE  |
| RALPH       | SINATRA      |
| RAY         | SOBIESKI     |
| REESE       | STALLONE     |
| RENEE       | STREEP       |
| RICHARD     | SUVARI       |
| RIP         | SWANK        |
| RITA        | TANDY        |
| RIVER       | TAUTOU       |
| ROCK        | TEMPLE       |
| RUSSELL     | TOMEI        |
| SALMA       | TORN         |
| SANDRA      | TRACY        |
| SCARLETT    | VOIGHT       |
| SEAN        | WAHLBERG     |
| SIDNEY      | WALKEN       |
| SISSY       | WAYNE        |
| SPENCER     | WEST         |
| SUSAN       | WILLIAMS     |
| SYLVESTER   | WILLIS       |
| THORA       | WILSON       |
| TIM         | WINSLET      |
| TOM         | WITHERSPOON  |
| UMA         | WOOD         |
| VAL         | WRAY         |
| VIVIEN      | ZELLWEGER    |
| WALTER      | NULL         |
| WARREN      | NULL         |
| WHOOPI      | NULL         |
| WILL        | NULL         |
| WILLIAM     | NULL         |
| WOODY       | NULL         |
| ZERO        | NULL         |
+-------------+--------------+
128 rows in set (0.00 sec)
</pre>

last_name은 중복이 많았나 보다. 그래서 저렇게 모자란 부분은 NULL이 나오게 된다.

2. **Updated someday** :D