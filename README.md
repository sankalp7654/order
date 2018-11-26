create table Salesman(
salesman_id integer primary key,
Name varchar(20),
city varchar(20),
commission varchar(20)
);
create table Customer(
customer_id integer primary key,
customer_name varchar(20),
city varchar(20),
grade integer,
salesman_id integer references salesman(salesman_id) on delete set null
);
create table orders(
ord_no integer primary key,
purchase_amt numeric(10,2),
ord_date date,
customer_id integer references customer(customer_id) on delete cascade,
salesman_id integer references salesman(salesman_id) on delete cascade
);
INSERT INTO SALESMAN VALUES (1000, 'JOHN','BANGALORE','25 %'); 
INSERT INTO SALESMAN VALUES (2000, 'RAVI','BANGALORE','20 %'); 
INSERT INTO SALESMAN VALUES (3000, 'KUMAR','MYSORE','15 %'); 
INSERT INTO SALESMAN VALUES (4000, 'SMITH','DELHI','30 %'); 
INSERT INTO SALESMAN VALUES (5000, 'HARSHA','HYDRABAD','15 %'); 
 
INSERT INTO CUSTOMER VALUES (10,'PREETHI','BANGALORE',100,1000); 
INSERT INTO CUSTOMER VALUES (11, 'VIVEK','MANGALORE', 300, 1000); 
INSERT INTO CUSTOMER VALUES (12, 'BHASKAR','CHENNAI', 400, 2000); 
INSERT INTO CUSTOMER VALUES (13, 'CHETHAN','BANGALORE', 200, 2000); 
INSERT INTO CUSTOMER VALUES (14, 'MAMATHA','BANGALORE', 400, 3000);
INSERT INTO ORDERS VALUES (50,5000,'04-MAY-17',10,1000)
INSERT INTO ORDERS VALUES (51, 450, '20-JAN-17', 10, 2000); 
INSERT INTO ORDERS VALUES (52, 1000, '24-FEB-17', 13, 2000); 
INSERT INTO ORDERS VALUES (53, 3500, '13-APR-17', 14, 3000); 
INSERT INTO ORDERS VALUES (54, 550, '09-MAR-17', 12, 2000);
select * from Customer;
select * from Salesman;
select * from orders;
--1
select grade,count(customer_id) from Customer where grade>(select avg(grade) from customer where city='BANGALORE') group by grade;
SELECT GRADE, COUNT (DISTINCT CUSTOMER_ID) FROM CUSTOMER GROUP BY GRADE HAVING GRADE > (SELECT AVG(GRADE) FROM CUSTOMER WHERE CITY='BANGALORE');
--2
select s.salesman_id,s.name from salesman s where 1<(select count(*) from Customer where salesman_id=s.salesman_id);
(select s.salesman_id, count(*) from Customer c,salesman s where c.salesman_id=s.salesman_id group by s.salesman_id having count(*)>1);
select * from Customer,Salesman;
select s.salesman_id,s.name from customer c,salesman s  where s.salesman_id=c.salesman_id group by s.salesman_id,s.Name having count(*)>1;

select s.salesman_id,s.Name,c.customer_name,s.commission from Customer c,Salesman s where s.salesman_id=c.salesman_id group by s.salesman_id,s.Name,c.customer_name,s.commission 
--3
SELECT SALESMAN.SALESMAN_ID, NAME, customer_name, COMMISSION FROM SALESMAN, CUSTOMER WHERE SALESMAN.CITY = CUSTOMER.CITY UNION SELECT SALESMAN_ID, NAME, 'NO MATCH', COMMISSION FROM SALESMAN WHERE NOT CITY = ANY  (SELECT CITY   FROM CUSTOMER) ORDER BY 2 DESC;
SELECT SALESMAN.SALESMAN_ID, NAME, customer_name, COMMISSION FROM SALESMAN, CUSTOMER WHERE SALESMAN.CITY = CUSTOMER.CITY
SELECT SALESMAN_ID, NAME, 'NO MATCH', COMMISSION FROM SALESMAN WHERE NOT CITY = ANY  (SELECT CITY   FROM CUSTOMER) ORDER BY 2 DESC;

--4
create view highest_salesman as
select salesman_id,

CREATE VIEW ELITSALESMAN AS
SELECT B.ORD_DATE, A.SALESMAN_ID, A.NAME FROM SALESMAN A, ORDERS B 
WHERE A.SALESMAN_ID = B.SALESMAN_ID AND 
B.PURCHASE_AMT=(SELECT MAX (PURCHASE_AMT) FROM ORDERS C WHERE C.ORD_DATE = B.ORD_DATE);
select * from ELITSALESMAN

SELECT B.ORD_DATE, A.SALESMAN_ID, A.NAME FROM SALESMAN A, ORDERS B 
WHERE A.SALESMAN_ID = B.SALESMAN_ID
SELECT a.salesman_id,b.ord_date,b.purchase_amt,max(b.purchase_amt) FROM SALESMAN A, ORDERS B where a.salesman_id=b.salesman_id group by a.salesman_id,b.ord_date,b.purchase_amt
--practise
--2
select s.name,s.salesman_id from salesman s,orders o where s.salesman_id=o.salesman_id and customer_id in(select customer_id from orders group by customer_id having count(*)>1 );
select customer_id from orders group by customer_id having count(*)>1
--4
select * from orders o,Salesman s where s.salesman_id=o.salesman_id and customer_id in(select customer_id from orders group by customer_id having count(*)>0
)
 --5
 SELECT SALESMAN.SALESMAN_ID, NAME, customer_name, COMMISSION FROM SALESMAN, CUSTOMER
  WHERE SALESMAN.CITY = CUSTOMER.CITY UNION
  SELECT SALESMAN_ID, NAME, 'NO MATCH', COMMISSION FROM SALESMAN 
  WHERE NOT CITY = ANY  (SELECT CITY   FROM CUSTOMER) ORDER BY 2 DESC; 
 
 
 select s.salesman_id,s.Name,c.customer_name,s.commission from Salesman s ,Customer c where c.city=s.city union
 select s.salesman_id,s.Name,'No Match',s.commission from Salesman s where not s.city=any (select city from Customer)
 order by 2 desc;
