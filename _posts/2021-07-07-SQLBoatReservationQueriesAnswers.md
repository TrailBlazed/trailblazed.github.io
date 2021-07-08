---
layout: post
title: Answers to Boat Reservation Database Table
---
### Basic Quieres
1) Find the names and ages of all sailors.	
``` sql
select s.sname ,s.age from sailors s;
```
2) Find all sailors with a rating above 7.
```sql 
select s.sid,s.sname,s.rating,s.age from sailors s where s.rating>7;
```
7)	select distinct s.sid,s.sname from sailors s,reserves r where s.sid=r.sid and r.bid=103
8)	select distinct s.sid,s.sname,b.color from sailors s,reserves r,boats b where s.sid=r.sid and r.bid=b.bid and b.color='red';
9)	select distinct b.color from sailors s,boats b , reserves r where s.sid=r.sid and r.bid=b.bid and s.sname='Lubber'
10)	select distinct s.sname from sailors s,reserves r where s.sid=r.sid
11)	select distinct s.sname,s.rating+1 as inc from sailors s,reserves r1,reserves r2,boats b1,boats b2 where s.sid=r1.sid and r1.reserveDate=r2.reserveDate and r1.bid=b1.bid and b1.color<>b2.color;
12)	select distinct s.sname,s.age from sailors s where s.sname like 'B_%B';
13)	select distinct s.sname from sailors s,boats b, reserves r where s.sid=r.sid and r.bid=b.bid and (b.color='red' or b.color='green');
best way of querying:
select s1.sname from sailors s1,reserves r1,boats b1 where s1.sid=r1.sid and r1.bid=b1.bid and b1.color='red'
union 
select s2.sname from sailors s2,reserves r2,boats b2 where s2.sid=r2.sid and r2.bid=b2.bid and b2.color='green';

10)	select  distinct s.sname from sailors s,boats b1,boats b2, reserves r1,reserves r2 where s.sid=r1.sid and s.sid=r2.sid and r1.bid=b1.bid and r2.bid=b2.bid and r2.bid=b2.bid and (b1.color='red' AND b2.color='green');
best way of querying:
select s1.sname from sailors s1,reserves r1,boats b1 where s1.sid=r1.sid and r1.bid=b1.bid and b1.color='red'
intersect 
select s2.sname from sailors s2,reserves r2,boats b2 where s2.sid=r2.sid and r2.bid=b2.bid and b2.color='green';
11)	 select s1.sname from sailors s1,reserves r1,boats b1 where s1.sid=r1.sid and r1.bid=b1.bid and b1.color='red'
except 
select s2.sname from sailors s2,reserves r2,boats b2 where s2.sid=r2.sid and r2.bid=b2.bid and b2.color='green';

12)	select s1.sid from sailors s1,reserves r1,boats b1 where s1.rating=10
union
select s2.sid from sailors s2,reserves r2,boats b2 where
       s2.sid=r2.sid and r2.bid=104
13)	select distinct s1.sname from sailors s1 where
s1.sid in(select r1.sid from reserves r1 where r1.bid=103)
14)	select s1.sname from sailors s1 where
s1.sid in (select r1.sid from reserves r1 where r1.bid in(select b1.bid from boats b1 where b1.color='red')) 
15)	select s1.sname from sailors s1 where
s1.sid not in (select r1.sid from reserves r1 where r1.bid in(select b1.bid from boats b1 where b1.color='red')) 
16)	select s1.sname from sailors s1 where exists (select * from reserves r1 where r1.bid=103 and s1.sid=r1.sid)
17)	select s1.sname from sailors s1 where s1.rating>any (select s2.rating from sailors s2 where s2.sname='Horatio')
18)	select s1.sname,s1.rating from sailors s1 where s1.rating>=all (select s2.rating from sailors s2 where s2.sname='Horatio')
19)	select s1.sname,s1.rating from sailors s1 where s1.rating>=all (select s2.rating from sailors s2)
20)	select distinct s1.sname,s1.sid from sailors s1,reserves r1,boats b1 where s1.sid=r1.sid and r1.bid=b1.bid and b1.color='red'
and s1.sid in (select s2.sid from sailors s2,reserves r2,boats b2 where s2.sid=r2.sid and r2.bid=b2.bid and b2.color='green')
21)	SELECT S.sname
FROM Sailors S
WHERE NOT EXISTS (( SELECT B.bid
FROM Boats B )
EXCEPT
(SELECT R. bid
FROM Reserves R
WHERE R.sid = S.sid ))
22)	select avg(s.age) from sailors s

23)	select avg(s.age) from sailors s where s.rating=10;

24)	select s.sname,s.age from sailors s where s.age=(select max(s1.age) from sailors s1);

25)	select count(*) from sailors s;

26)	select distinct count(*) from sailors s;

27)	select s1.sname,s1.age from sailors s1 where s1.age>(select max(s2.age) from sailors s2 where s2.rating=10);

28)	select s1.rating,min(s1.age) from sailors s1 group by s1.rating ;
29)	select s1.rating,min(s1.age) from sailors s1 where s1.age>18 group by s1.rating having count(*)>=2 ;
30)	select b.bid,count(*) as NoOfReservations from reserves r,boats b where r.bid=b.bid and b.color='red' group by b.bid
31)	select s.rating,avg(s.age) as AvgAge from sailors s group by s.rating having count(*)>1;
32)	select s.rating,avg(s.age) as AvgAge from sailors s where s.age>=18 group by s.rating having count(*)>1;
33)	 CREATE TABLE Sailors ( sid INTEGER, sname CHAR(10), rating INTEGER, age REAL, PRIMARY KEY (sid),
CHECK (rating >= 1 AND rating <= 10 ))	