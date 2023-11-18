# SQL-Database
# --  Create a database called Orangehrm
create database orangehrm;
use orangehrm;

# /*Create a table of employees containing the following fields: employee_id, employee_name, hire_date, job_title, email_address, telephone*/
create table employees (
employee_id INT PRIMARY KEY auto_increment NOT NULL,
employee_name VARCHAR(100) NOT NULL,
hire_date DATE,
job_title VARCHAR(100) NOT NULL,
email_address VARCHAR(50),
telephone VARCHAR(30)
);

# -- Insert values into the employees table
insert into employees 
values (1, "Scott Johnson", "2020-05-11", "manager", "scott.johnson@gmail.com", "0754545333"),
	     (2, "Tom Peterson", "2020-06-12", "hr specialist", "tom.peterson@gmail.com", "0752123123"),
       (3, "Jane Smith", "2020-06-19", "saleswoman", "jane.smith@yahoo.com", "0764252525"),
       (4, "Kate Wilson", "2021-03-03", "saleswoman", "wilson.kate@yahoo.com", "0712897883"),
       (5, "Bob Griffin", "2021-09-06", "salesman", "bob.griffin@gmail.com", "0723656578");

# -- Search for email addresses with "yahoo.com" domain
select * from employees
where email_address like "%yahoo.com%";

# /*Change email addresses from "yahoo.com" to "gmail.com" so that all employees have the address gmail.com domain*/
update employees
set email_address = "jane.smith@gmail.com"
where employee_id = 3;
update employees
set email_address = "kate.wilson@gmail.com"
where employee_name = "Kate Wilson";
SET SQL_SAFE_UPDATES = 0;

# /*Create table emergency_contacts containing the following fields: employee_id, emergency_contact_name, relationship, home_telephone, mobile, work_telephone. Link this table to the previous table by using a 
# foreign key (via employee_id)*/

create table emergency_contacts (
employee_id INT PRIMARY KEY auto_increment NOT NULL,
emergency_contact_name VARCHAR(100) NOT NULL,
relationship VARCHAR(100) NOT NULL,
home_telephone VARCHAR(30),
mobile VARCHAR(30),
work_telephone VARCHAR(30),
FOREIGN KEY(employee_id) REFERENCES employees(employee_id)
);

# -- Insert values into the emergency_contacts table
insert into emergency_contacts 
values (1, "Jane Johnson", "wife", "+4023115779", "0756999313", "+4023123456"),
	   (2, "John Peterson", "father", "+402555777", "0756999313", "-"),
       (3, "Maggie Robinson", "sister", "-", "0756784874", "+4023556789"),
       (4, "Chelsea Bing", "friend", "-", "0757443212", "-"),
       (5, "Jim Anderson", "flatmate", "+4023323214", "0753456456", "+4023199199");

# -- Drop foreign key
alter table emergency_contacts
drop FOREIGN KEY emergency_contacts_ibfk_1;

# -- Add a on delete cascade clause so, when a foreign key is deleted, the entire row is deleted
alter table emergency_contacts
add constraint fk_employee_id
FOREIGN KEY(employee_id) REFERENCES employees(employee_id)
on delete cascade;

# -- Delete an employee from the database
delete from employees
where employee_id = 5;

select * from employees;
select * from emergency_contacts;

# -- Employee has a new emergency contact and I want to update his emergency contact informations
update emergency_contacts
set emergency_contact_name = "Ema Watson",
	relationship = "girlfriend",
    home_telephone = "-",
    mobile = "0751999123",
    work_telephone = "+4026787856"
where employee_id = 1;

# /*I want to use a join clause to combine rows from the two tables, based on the related  employee_id column between them.*/
select *
from employees inner join emergency_contacts
on emergency_contacts.employee_id = employees.employee_id;
