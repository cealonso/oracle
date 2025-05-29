# Curso de Oracle 2025

Este repositorio forma parte del curso de Oracle del año 2025 dictado en el C.F.P N° 401 de San Martín.

## Clase 03/04

```sql
select * from employees where salary=9000;
```
## Clase 08/04

```sql
select * from employees where salary>9000;
```

```sql
select * from employees where salary>=9000 order by salary;
```

```sql
select * from employees where salary>=9000 order by salary desc;
```

```sql
select * from employees where salary>=9000 and salary<=12000 order by salary;
```


```sql
select * from employees where not salary=9000;
```

```sql
select * from employees where salary<>9000 order by salary;
```

```sql
select * from employees where manager_id=101 or manager_id=103 order by last_name;
```

```sql
select * from employees where manager_id in (100,101,103) order by last_name;
```

```sql
select count(employee_id) from employees;
```

```sql
select count(manager_id) as "cantidad de empleados" from employees;
```

```sql
select count(*) as "Cantidad de Taylor" from employees where last_name='Taylor';
```

```sql
select count(*) as "Cantidad" from employees where last_name in ('Grant','Williams','Smith','Jeffs');
```

```sql
A resolver (Pensarlo mas adelante), ERROR: ORA-00937: la función de grupo no es de grupo único
select last_name, max(salary) from employees;
```

```sql
select min(salary) from employees;
```

```sql
select round(avg(salary),2) from employees;
```

```sql
select sum(salary) from employees where job_id='IT_PROG';
```

```sql
A resolver (Pensarlo mas adelante), ERROR: ORA-00937: la función de grupo no es de grupo único
select job_id,sum(salary) from employees where job_id='IT_PROG';
```

## Clase 15/04

```sql
select to_char(hire_date,'YYYY') from employees;
```

```sql
Ejercicio: Devolver los nombres, apellidos, id del puesto laboral, año de contratación y salario de los empleados que cumplan estas condiciones 1°) El año de contratación sea el año 2015, 2°) El salario se encuentre entre los 6000 y 15000 dólares 3°) Los salarios esten ordenados en forma ascendente.

select first_name,last_name,job_id,to_char(hire_date,'YYYY'),salary from employees where to_char(hire_date,'YYYY')='2015' and salary between 6000 and 15000 order by salary;
```

```sql
Ejercicio: Devolver el nombre del empleado en minúscula (usar la función lower()) concatenado con el apellido del mismo en mayúscula  (usar la función upper()). El título de la columna debe ser Nombre y Apellido. En otra columna mostrar la fecha de contratación del empleado. Listar toda esa información ordenada por fecha de contratación.

select lower(first_name)|| ',' || upper(last_name) as "Nombre y Apellido",hire_date from employees order by hire_date;

```

```sql
select rpad(last_name, 25, '.')  || lpad(salary, 10, '.') as "Listado de Salarios" from employees;
```

```sql
select first_name,last_name from employees where (first_name like 'S%') or (last_name like'S%');
```

```sql

Clase: 08/04. A resolver (Pensarlo mas adelante), ERROR: ORA-00937: la función de grupo no es de grupo único
select job_id,sum(salary) from employees where job_id='IT_PROG';

Solución:

select job_id,sum(salary) from employees group by job_id having job_id='IT_PROG';
```

```sql
select job_id,count(*), avg(salary), max(salary)-min(salary) "diferencia de salarios" from employees group by job_id;
```

```sql
select job_id as "Puesto laboral",count(*) as "cantidad de empleados", avg(salary) as promedio, min(salary) as "salario mas chico", max(salary) as "salario mas grande", max(salary)-min(salary) "diferencia de salarios" from employees group by job_id;
```

## Clase 22/04

```sql
select manager_id,COUNT(*) from employees group by manager_id order by manager_id;
```

```sql
Nota: Oracle23Free
select manager_id,count(employee_id) as "cantidad de empleados" from employees group by manager_id order by "cantidad de empleados" desc;
select manager_id,count(employee_id) as "cantidad de empleados" from employees group by manager_id order by 2 desc;
```

```sql
select manager_id,count(employee_id) as "cantidad de empleados" from employees group by manager_id order by 2 desc fetch first 1 rows only;
```

```sql
select count(location_id) "localidad", country_id from locations group by country_id;
```

```sql
select count(location_id) "localidad", country_id from locations where country_id='US' group by country_id;
```

```sql
select employee_id from job_history group by employee_id having count(*) > 1;
```

```sql
select department_id, to_char(hire_date, 'YYYY'), count(employee_id) from employees group by department_id, to_char(hire_date, 'YYYY') order by department_id;
```

```sql
select distinct department_id from employees order by department_id;
```

```sql
A resolver (Pensarlo mas adelante), ERROR: ORA-00937: la función de grupo no es de grupo único
select last_name, max(salary) from employees;

Solución:
select initcap(last_name),salary from employees where salary=(select max(salary) from employees);
```

## Clase 24/04

```sql
select initcap(last_name) as Apellido, salary  as salario from employees where salary>(select avg(salary) from employees) order by last_name
```

```sql
select last_name, job_id, employees.department_id, department_name from employees,departments where departments.department_id=employees.department_id;
```

```sql
select last_name, job_id, employees.department_id, department_name from employees,departments where departments.department_id=employees.department_id order by last_name;
```

```sql
Ejercicio: Inner Join

select last_name, job_id, employees.department_id, department_name from employees,departments where departments.department_id=employees.department_id and job_id = 'SA_MAN' order by last_name;
```

```sql

Ejercicio: Self Join

select employee_id,last_name,manager_id from employees where last_name like 'R%';-- Rajs tiene como jefe a 124 y Rogers tiene como jefe a 122
select employee_id,last_name from employees where employee_id=124 or employee_id=122; -- 122 es Kaufling y 124 Mourgos
select e1.last_name,e2.last_name from employees e1,employees e2 where e1.manager_id = e2.employee_id and e1.last_name LIKE 'R%'; 
select e1.last_name||' trabaja para el gerente '||e2.last_name "Empleados y Gerentes" from employees e1,employees e2 where e1.manager_id = e2.employee_id;
select e1.last_name||' trabaja para el gerente '||e2.last_name "Empleados y Gerentes", e1.salary " Salario del empleado", e2.salary "Salario del Gerente" from employees e1, employees e2 where e1.manager_id = e2.employee_id order by e1.last_name;

Atención: Modificar los salarios de los empleados Abel y  Ozer.
```

## Clase 29/04

```sql
(NO ANSI-SQL/ORACLE)
select e.first_name,j.job_title,d.department_name from employees e,jobs j,departments d where e.job_id=j.job_id and e.department_id=d.department_id;
(ANSI-SQL)
select e.first_name,j.job_title,d.department_name from employees e inner join jobs j  on e.job_id=j.job_id inner join departments d on e.department_id=d.department_id;
```

```sql
select e.first_name,j.job_title,d.department_name from employees e,jobs j,departments d where e.job_id=j.job_id and e.department_id=d.department_id and to_char(e.hire_date,'dd') = '10' and to_char(e.hire_date,'mm') = '03';
```

```sql
  Left Join (NO ANSI SQL/ORACLE)
  select emps.employee_id,
         emps.first_name,
         jh.department_id,
         jh.start_date,
         jh.end_date
  from   employees emps, job_history jh
  where  emps.employee_id = jh.employee_id(+);
```

```sql
Right Join (NO ANSI SQL/ORACLE)
 select emps.employee_id,
         emps.first_name,
         jh.department_id,
         jh.start_date,
         jh.end_date
  from   employees emps, job_history jh
  where  emps.employee_id(+) = jh.employee_id;
```

```sql
Left Join (Ansi SQL)
select e.employee_id, e.last_name, d.department_name
from employees e
left outer join departments d on e.department_id = d.department_id;

Right Join (Ansi SQL)

select e.employee_id, e.last_name, d.department_name
from employees e
right outer join departments d on e.department_id = d.department_id;
``` 

## Clase 08/05


```sql
select e.employee_id, e.last_name, d.department_name
from employees e
full outer join departments d on e.department_id = d.department_id;

```

```sql

Uso de IA con claude.ai

Quiero que me muestres en un artefacto los job_title de la tabla jobs, el first_name y el last_name de la tabla employees y el department_name de la tabla departments 
en oracle sql usando el esquema human resources
```

```sql

De la clase 24/04 modifico el salario de Zlotkey asi no tiene menos salario que su empleado.

update employees set salary=18000 where employee_id=149;

select salary from employees where employee_id=149;


update employees set salary=19000 where employee_id=148;

select salary from employees where employee_id=148;
```

```sql
insert into jobs (job_id,job_title,min_salary,max_salary) values ('DBA','Admin Databases',20000,30000);
insert into jobs (job_id,job_title,min_salary,max_salary) values ('BOGA','ABOGADO',15000,25000);
```

## Clase 13/05

```sql
insert into jobs(job_id,job_title,min_salary,max_salary) values ('PROG_JR','Developer Junior',6500,9000);
commit;
```
  
```sql
insert into jobs values ('TEST','Testing Developer',5000,7000);  
commit;
```

```sql
insert into countries values ('SK','South Korea',30),('SA','South Africa',50),('CL','Chile',20);
commit;
```

```sql
select * from employees where upper(last_name)=upper('James');
insert into employees values(300,'Erik','Spencer','ESPENCER','1.650.555.0999',SYSDATE,'PROG_JR',7500,NULL,103,60);
commit;
```

## Clase 20/05

```sql
create table instructors (
    instructor_id NUMBER GENERATED BY DEFAULT ON NULL AS IDENTITY PRIMARY KEY,
    data JSON
);

```

```sql
insert into instructors (data) values ('{
    "first_name": "David",
    "last_name": "Lawson",
    "email": "david.lawson@myemail.com",
    "skills": [
        "Inteligencia Artificial",
        "Machine Learning",
        "Big Data",
        "Cloud Computing"
    ]
}'),
('{
    "first_name": "Diana",
    "last_name": "Gray",
    "email": "diana.gray@myemail.com",
    "skills": [
        "Economía",
        "Estadística",
        "Derecho"
    ]
}');


```

```sql
select JSON_SERIALIZE(i.data PRETTY) from instructors i;

```

```sql
select i.data.last_name,i.data.email from instructors i;
```

```sql
select json_serialize(i.data PRETTY) FROM instructors i where json_value(i.data, '$.last_name') = 'Gray';
```

```sql
create table courses (
course_id number GENERATED BY DEFAULT ON NULL AS IDENTITY,
course_name varchar2(100) not null,
start_date date not null,
end_date date,
instructor_id number,
CONSTRAINT fk_instructor_id FOREIGN KEY (instructor_id) REFERENCES instructors (instructor_id),
CONSTRAINT pk_course_id PRIMARY KEY (course_id),
CONSTRAINT chk_dates CHECK (start_date <= end_date)
);
```
## Clase 22/05

```sql
alter table courses MODIFY course_name varchar2(50); 
```

```sql
alter table courses MODIFY start_date date default sysdate; 
```

```sql
alter table courses ADD difficultylevel varchar2(20);
```

```sql
alter table courses ADD CONSTRAINT chk_difficultylevel CHECK (difficultylevel in ('low','medium','high')); 
```
## Clase 27/05

```sql
ERROR: ORA-02291: restricción de integridad (HR_ADMIN.FK_COURSE_ID) violada - clave principal no encontrada
insert into employees_courses values('',102,999,FALSE);
```

```sql
OK.
insert into employees_courses values('',102,1,FALSE);
```

```sql

select  first_name,last_name,salary,
case 
when salary<6000 then 'Salario Bajo' 
when salary>=6000 and salary<10000 then 'Salario Medio'
else 'Salario Alto'
end as "Tipo de Salario"
from employees order by salary asc;

```

```sql
select course_name,difficultylevel, 
case 
when difficultylevel='low' then 'Sin requisitos' 
else 'Con requisitos' 
end "Requisitos" 
from courses;
```

```sql
 select
    e.employee_id,
    e.first_name || ' ' || e.last_name AS empleado,
    TO_CHAR(e.hire_date, 'YYYY') AS año_contratación,
    case
        WHEN EXTRACT(YEAR FROM SYSDATE) - EXTRACT(YEAR FROM e.hire_date) < 5 THEN 'Novato'
        WHEN EXTRACT(YEAR FROM SYSDATE) - EXTRACT(YEAR FROM e.hire_date) < 10 THEN 'Semi senior'
        WHEN EXTRACT(YEAR FROM SYSDATE) - EXTRACT(YEAR FROM e.hire_date) < 15 THEN 'Senior'
        ELSE 'Experto' -- Added a category for those with 15+ years
    end as nivel,
    d.department_name
from
    employees e
inner join
    departments d ON e.department_id = d.department_id
order by
    empleado; 

```

```sql
SELECT 
    last_name,
    department_id,
    salary,
    ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) as ranking_salary
FROM employees
ORDER BY department_id, ranking_salary;
```

## Clase 29/05

```sql
SELECT 
    department_id,
    department_name,
    manager_id,
    CASE 
        WHEN manager_id IS NULL THEN 'Sin Manager'
        ELSE 'Con Manager'
    END AS estado_manager
FROM departments 
ORDER BY department_id;
```

```sql
select
    employee_id,
    department_id,
    last_name,
    salary,
    rank()
    over(PARTITION BY department_id order by salary asc) posición
from
    employees
```


```sql
select
    employee_id,
    department_id,
    last_name,
    salary,
    dense_rank()
    over(PARTITION BY department_id order by salary asc) posición
from
    employees
```

```sql
select 
    employee_id,
    first_name,
    last_name,
    department_id,
    salary,
    SUM(salary) OVER (PARTITION BY department_id) AS "Total de salario del  departamento",
    ROUND(salary / SUM(salary) OVER (PARTITION BY department_id) * 100, 2) AS  "Porcentaje de participación en el presupuesto departamental"
from employees
order by department_id, salary desc;
```

```sql
select
    employee_id,
    first_name,
    department_id,
    hire_date,
    LAG(hire_date)
    OVER(PARTITION BY department_id
         ORDER BY
             hire_date
    ) AS prev_hiredate
from
employees;
```

```sql
select
    employee_id,
    first_name,
    last_name,
    hire_date,
    department_id,
    lead(first_name || ' ' || last_name) OVER (PARTITION BY department_id ORDER BY hire_date) AS "Siguiente Contratado"
from
    employees;
```

```sql
select employee_id,
    first_name,
    last_name,
    department_id,
    salary,
    hire_date,
    FIRST_VALUE(first_name || ' ' || last_name) 
        OVER (PARTITION BY department_id 
              ORDER BY salary DESC) AS "Salario más Alto"
from employees
order by department_id, salary DESC;
```

```sql
select employee_id,
    first_name,
    last_name,
    department_id,
    salary,
    hire_date,
    LAST_VALUE(first_name || ' ' || last_name) 
        OVER (PARTITION BY department_id 
              ORDER BY salary DESC) AS "Salario más Alto"
from employees
order by department_id, salary DESC;
```
