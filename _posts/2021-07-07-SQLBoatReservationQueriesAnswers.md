---
layout: post
title: Answers to Boat Reservation Database Queries
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
3) Find the names of sailors who have reserved boat number 103.
``` sql
select distinct s.sid,s.sname from sailors s,reserves r where s.sid=r.sid and r.bid=103
```
4) Find the names of sailors who have reserved a Red boat.
```sql
select distinct s.sid,s.sname,b.color from sailors s,reserves r,boats b where s.sid=r.sid and r.bid=b.bid and b.color='red';
```
5) Find the colors of boats reserved by Lubber.
```sql
select distinct b.color from sailors s,boats b , reserves r where s.sid=r.sid and r.bid=b.bid and s.sname='Lubber'
```
6) Find the names of sailors who have reserved at least one boat.
```sql 
select distinct s.sname from sailors s,reserves r where s.sid=r.sid
```
7) Compute increments for the ratings of persons who have sailed two different boats on the same day.	
``` sql 
select distinct s.sname,s.rating+1 as inc from sailors s,reserves r1,reserves r2,boats b1,boats b2
where s.sid=r1.sid and r1.reserveDate=r2.reserveDate and r1.bid=b1.bid and b1.color<>b2.color;
```
8) Find the ages of sailors whose name begins and ends with B and has at least three characters.
```sql
select distinct s.sname,s.age from sailors s where s.sname like 'B_%B';
```
9)  Find the names of sailors who have reserved a red or a green boat.
```sql
select distinct s.sname from sailors s,boats b, reserves r where s.sid=r.sid 
and r.bid=b.bid and (b.color='red' or b.color='green');
```
Best way of querying:
```sql
select s1.sname from sailors s1,reserves r1,boats b1 where s1.sid=r1.sid and r1.bid=b1.bid and b1.color='red'
union 
select s2.sname from sailors s2,reserves r2,boats b2 where s2.sid=r2.sid and r2.bid=b2.bid and b2.color='green';
```

10) Find the names of sailor's who have reserved both a red and a green boat.
```sql 
select  distinct s.sname from sailors s,boats b1,boats b2, reserves r1,reserves r2 where s.sid=r1.sid 
and s.sid=r2.sid and r1.bid=b1.bid and r2.bid=b2.bid and r2.bid=b2.bid 
and (b1.color='red' AND b2.color='green'); 
```
Best way of querying:
```sql 
select s1.sname from sailors s1,reserves r1,boats b1 where s1.sid=r1.sid and r1.bid=b1.bid and b1.color='red'
intersect 
select s2.sname from sailors s2,reserves r2,boats b2 where s2.sid=r2.sid and r2.bid=b2.bid and b2.color='green';
```
11) Find the sids of all sailor's who have reserved red boats but not green boats.
```sql
select s1.sname from sailors s1,reserves r1,boats b1 where s1.sid=r1.sid and r1.bid=b1.bid and b1.color='red'
except 
select s2.sname from sailors s2,reserves r2,boats b2 where s2.sid=r2.sid and r2.bid=b2.bid and b2.color='green';
```

12) Find all sids of sailors who have a rating of 10 or reserved boat 104.
```sql
select s1.sid from sailors s1,reserves r1,boats b1 where s1.rating=10
union
select s2.sid from sailors s2,reserves r2,boats b2 where s2.sid=r2.sid and r2.bid=104
```
### Nested Queries
13) Find the names of sailors who have reserved boat 103.
```sql
select distinct s1.sname from sailors s1 where s1.sid in(select r1.sid from reserves r1 where r1.bid=103) 
```
14) Find the names of sailors who have reserved a red boat.
```sql
select s1.sname from sailors s1 where s1.sid 
in (select r1.sid from reserves r1 where r1.bid in(select b1.bid from boats b1 where b1.color='red')) 
```
15) Find the names of sailors who have not reserved a red boat.
```sql 
select s1.sname from sailors s1 where s1.sid 
not in (select r1.sid from reserves r1 where r1.bid in(select b1.bid from boats b1 where b1.color='red')) 
```
### Correlational Queries
16)	Find the names of sailors who have reserved boat number 103.
```sql
select s1.sname from sailors s1 where exists (select * from reserves r1 where r1.bid=103 and s1.sid=r1.sid)
```
17) Find sailors whose rating is better than some sailor called Horatio.
```sql 
select s1.sname from sailors s1 where s1.rating>any (select s2.rating from sailors s2 where s2.sname='Horatio')
```
18)	Find sailors whose rating is better than every sailor' called Horatito.
```sql
select s1.sname,s1.rating from sailors s1 where s1.rating>=all (select s2.rating from sailors s2 where s2.sname='Horatio')
```
19)	Find the sailor's with the highest rating.
```sql 
select s1.sname,s1.rating from sailors s1 where s1.rating>=all (select s2.rating from sailors s2)
```
### More Nested
20) Find the names of sailors who have reserved both a red and a green boat.
```sql
select distinct s1.sname,s1.sid from sailors s1,reserves r1,boats b1 where s1.sid=r1.sid and r1.bid=b1.bid and b1.color='red'
and s1.sid in (select s2.sid from sailors s2,reserves r2,boats b2 where s2.sid=r2.sid and r2.bid=b2.bid and b2.color='green')
```
21)  Find the names of sailors who have reserved all boats.
```sql
SELECT S.sname
FROM Sailors S
WHERE NOT EXISTS (( SELECT B.bid
FROM Boats B )
EXCEPT
(SELECT R. bid
FROM Reserves R
WHERE R.sid = S.sid ))
```
### AGGREGATE OPERATORS
22)	Find the average age of all sailors.
```sql
select avg(s.age) from sailors s
```

23) Find the average age of sailors with a rating of 10.
```sql
select avg(s.age) from sailors s where s.rating=10;
```

24) Find the name and age of the oldest sailor.
 ```sql   
  select s.sname,s.age from sailors s where s.age=(select max(s1.age) from sailors s1);
  ```
 

25) Count the number of sailor.
```sql
select count(*) from sailors s;
```

26)	Count the number of different sailor names.
```sql 
select distinct count(*) from sailors s;
```

27)	Find the names of sailors who are older than the oldest sailor with a rating of 10.
```sql 
select s1.sname,s1.age from sailors s1 where s1.age>(select max(s2.age) from sailors s2 where s2.rating=10);
```
### The GROUP BY and HAVING Clauses
28)	Find the age of the youngest sailor for each rating level.
```sql 
select s1.rating,min(s1.age) from sailors s1 group by s1.rating ;
```
29) Find the age of the youngest sailor who is eligible to vote (i.e., is at least 18 years old) for each rating level with at least two such sailors.
```sql
select s1.rating,min(s1.age) from sailors s1 where s1.age>18 group by s1.rating having count(*)>=2 ;
```
### More Examples of Aggregate Queries
30)	 For each red boat, find the number of reservations for this boat.
```sql
select b.bid,count(*) as NoOfReservations from reserves r,boats b where r.bid=b.bid and b.color='red' group by b.bid
```
31) Find the average age of sailors for each rating level that has at least two sailors.
```sql
select s.rating,avg(s.age) as AvgAge from sailors s group by s.rating having count(*)>1;
```
32)
* Find the average age of sailors who are of voting age (i.e., at least 18 years old) for each rating level that has at least two sailors.
```sql
select s.rating,avg(s.age) as AvgAge from sailors s where s.age>=18 group by s.rating having count(*)>1;
```
### COMPLEX INTEGRITY CONSTRAINTS IN SQL

34) Ensure that rating must be an integer in the range 1 to 10
```sql
CREATE TABLE Sailors ( sid INTEGER, sname CHAR(10), rating INTEGER, age REAL, PRIMARY KEY (sid),
CHECK (rating >= 1 AND rating <= 10 ))	
```
