select * from inventory--film_id, store_id,inventory_id
select * from payment--payment_id,customer_id, staff_id,rental_id
select * from film--film_id
select * from film_actor--actor_id,film_id
select * from film_category--film_id,category_id,
select * from rental--rental_id,inventory_id,customer_id,staff_id
select * from customer--customer_id,store_id,address_id
select * from address--address_id,city_id
select * from city--city_id,country_id
select * from staff--staff_id,address_id,store_id
select * from store--store_id,manager_staff_id
select * from country;


inventory film_id=1

1.select i.store_id,i.film_id,c.category_id  from store s join inventory i on i.store_id=s.store_id 
 join film_category c on i.film_id=c.film_id join film f on i.film_id=f.film_id where i.film_id <all (select f.film_id from film f where description like '%a' );
															   
															   
to find the amount according to row_number and find the 160th 

2.with E as
(
select payment_id,rental_id,staff_id,amount,row_number() over( order by amount asc) as rank from payment
) select * from E where rank =160;


3.select (select max(p.amount) from payment p) as max,
(select avg(p.amount) from payment p) as avg,p.payment_id,p.customer_id from payment p 

where p.customer_id=any(select c.customer_id from customer c where store_id=1);

4.select c.customer_id as "C_ID",c.first_name ||' '||c.last_name as "NAME",c.email as "MAIL" from 
customer c where c.customer_id = any (select c.customer_id from customer c 
union select p.customer_id from payment p union all select r.customer_id from rental r);
									
select cname as genre.count(distinct)
									
  
  
6.select c.category_id, c.film_id,i.last_update,f.title,f.description from film_category c join film f on 
c.film_id=f.film_id join film_actor a on a.film_id=f.film_id join inventory i on i.film_id=a.film_id where i.film_id
=all(select max(i.film_id) from inventory i )
The main query selects the following columns: category_id, film_id, last_update (from the inventory table), title, and description (from the film table).
The film_category table is joined with the film table using the common film_id column.
The film_actor table is joined with the film table using the common film_id column.
The inventory table is joined with the film table using the common film_id column.
The WHERE clause filters the results by comparing the film_id from the inventory table with the result of a subquery.
The subquery SELECT MAX(i.film_id) FROM inventory i retrieves the maximum film_id from the inventory table.
The ALL keyword ensures that the film_id from the main query matches all the values returned by the subquery.
In summary, the code fetches information about a film and its associated category, where the film has the maximum film_id in the inventory table.


7.select city_id,last_update, city,rank()over(partition by last_update order by country_id asc) 
from city group by city_id having city like 'B%'

8.SELECT P.AMOUNT AS "Product PRICE", 
       P.RENTAL_ID,
       R.RENTAL_DATE 
   FROM PAYMENT P, RENTAL R
   WHERE P.RENTAL_ID=R.RENTAL_ID
     AND R.INVENTORY_ID=(SELECT MAX(I.INVENTORY_ID) FROM INVENTORY I WHERE I.FILM_ID =(SELECT F.FILM_ID FROM FILM F WHERE F.TITLE='Academy Dinosaur') );

9.SELECT CITY_ID, CITY FROM CITY WHERE CITY_ID IN(SELECT CITY_ID FROM ADDRESS WHERE ADDRESS_ID<5)ORDER BY COUNTRY_ID ASC

10.SELECT f.film_id, f.title
FROM film f
JOIN inventory i ON f.film_id = i.film_id
JOIN rental r ON i.inventory_id = r.inventory_id
JOIN payment p ON r.rental_id = p.rental_id
group by f.film_id having count(16)<>length(title);

11.select c.city_id,c.city,c.country_id,a.district,a.postal_code from city c right outer join address a on c.city_id=a.city_id 
where city like 'C%' and country_id=any(select distinct c.country_id from city c);
	
