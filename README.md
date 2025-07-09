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

```sql
select
    department_id,
    LISTAGG(first_name
            || ' '
            || last_name, ', ') WITHIN GROUP
   (
    ORDER BY
        hire_date
    ) employees
from
    employees
group by
    department_id
order by
    department_id;
```

```sql
create or replace view v_all_trainings as
SELECT
    c.course_name,
    e.first_name,
    e.last_name,
    ec.is_finished
FROM
         employees e
    INNER JOIN employees_courses ec ON e.employee_id = ec.employee_id
    INNER JOIN courses c ON c.course_id = ec.course_id;
```

```sql
select * from v_all_trainings order by last_name;
```

```sql
create or replace view v_employee_details as
select e.employee_id, e.first_name || ' ' || e.last_name AS full_name, e.hire_date, TRUNC(MONTHS_BETWEEN(SYSDATE, e.hire_date) / 12) as years_of_service, 
d.department_name, j.job_title, e.salary, m.first_name || ' ' || m.last_name as manager_name, l.city || ', ' || l.country_id as work_location 
from employees e inner join departments d 
on e.department_id = d.department_id inner join jobs j 
on e.job_id = j.job_id inner join employees m 
on e.manager_id = m.employee_id inner join locations l 
on d.location_id = l.location_id order by e.employee_id;
```

```sql
select * from v_employee_details where (department_name='IT')
```

```sql
drop view v_all_trainings;
```
## Clase 03/06

```sql
CREATE VIEW empleados_multiples_trabajos AS
SELECT  
    e.employee_id AS id_empleado,
    e.first_name AS nombre,
    e.last_name AS apellido,
    count(*) AS num_jobs
FROM employees e
INNER JOIN job_history jh ON e.employee_id = jh.employee_id
GROUP BY e.employee_id, e.first_name, e.last_name
HAVING num_jobs > 1;

Pruebo la vista:

select * from empleados_multiples_trabajos;
```

```sql
create or replace json relational duality view department_dv as
departments @insert @update @delete{
_id : department_id,
departmentName : department_name,
location : location_id,
employees : employees @insert @update @delete{
employeeNumber : employee_id,
employeeFirst : first_name,
employeeLast : last_name,
employeeEmail: email,
employeeDate: hire_date,
job : job_id,
salary : salary
}
}

select * from department_dv
```

```sql
select * from department_dv where json_value(data,'$.departmentName')='IT'
```

## Clase 05/06

```sql
BEGIN
    dbms_output.put_line('Hola mundo');
END;

```

```sql
DECLARE
    v_text VARCHAR2(50) := 'Estoy aprendiendo SQL';
BEGIN
    dbms_output.put_line(v_text);
END;
```


```sql
DECLARE
  c_text CONSTANT VARCHAR2(50):='Estoy aprendiendo SQL';
  v_new_text VARCHAR2(100);
BEGIN
  v_new_text := c_text || ' y tambien PL/SQL ';
  dbms_output.put_line(v_new_text);
END;

```

```sql
 
DECLARE
   v_name varchar2(25);
BEGIN
   select last_name into v_name from employees where employee_id=100;
   dbms_output.put_line('El apellido es : ' || v_name);
END;

```


```sql
DECLARE
   v_name varchar2(25);
   v_salary number(8,2);
   v_total_salary number(8,2);
BEGIN
   select last_name,salary,salary*12 into v_name,v_salary,v_total_salary from employees where employee_id=100;
   dbms_output.put_line('El apellido es ' || v_name || ' el salario es : ' || v_salary || ' y su salario total es : ' || v_total_salary);
END;
```


```sql
DECLARE
   v_name employees.last_name%type;
   v_salary employees.salary%type;
   v_total_salary employees.salary%type;
BEGIN
   select last_name,salary,salary*12 into v_name,v_salary,v_total_salary from employees where employee_id=100;
   dbms_output.put_line('El apellido es ' || v_name || ' el salario es : ' || v_salary || ' y su salario total es : ' || v_total_salary);
END;
```
## Clase 10/06

```sql
DECLARE 
    v_salary        employees.salary%TYPE;
    v_hire_date     employees.hire_date%TYPE;
    v_years_worked  NUMBER;
    v_eligible_promotion char(2);
    c_employee_id CONSTANT NUMBER := 115;
BEGIN

    -- Obtener datos del empleado
    SELECT salary, hire_date
    INTO v_salary, v_hire_date
    FROM employees
    WHERE employee_id = c_employee_id;
    
    -- Calcular años trabajados
    v_years_worked := TRUNC(MONTHS_BETWEEN(SYSDATE, v_hire_date) / 12);
    
    -- Evaluar elegibilidad para promoción usando múltiples condiciones IF
    IF v_years_worked >= 5 THEN
        v_eligible_promotion := 'SI';
    ELSE
        v_eligible_promotion := 'NO';
    END IF;
    
   DBMS_OUTPUT.PUT_LINE('=== EVALUACIÓN DE RENDIMIENTO ===');
   DBMS_OUTPUT.PUT_LINE('Empleado ID: ' || c_employee_id);
   DBMS_OUTPUT.PUT_LINE('Años trabajados: ' || v_years_worked);
   DBMS_OUTPUT.PUT_LINE('Promociona: ' || v_eligible_promotion);
END;
```

```sql
 

CREATE or REPLACE PROCEDURE get_eligible_promotion(p_employee_id IN NUMBER,p_eligible_promotion OUT CHAR) IS
/**
Determina si un empleado es apto para promocionar en un puesto laboral.

@param  p_employee_id  Numero del legajo del empleado a validar
@return p_eligible_promotion Un char por SI o por NO.

*/
v_salary        employees.salary%TYPE;
v_hire_date     employees.hire_date%TYPE;
v_years_worked  NUMBER;
BEGIN
    -- Obtener datos del empleado
    SELECT salary, hire_date
    INTO v_salary, v_hire_date
    FROM employees
    WHERE employee_id = p_employee_id;
    
    -- Calcular años trabajados
    v_years_worked := TRUNC(MONTHS_BETWEEN(SYSDATE, v_hire_date) / 12);
    
     -- Evaluar elegibilidad para promoción usando múltiples condiciones IF
    IF v_years_worked >= 5 THEN
        p_eligible_promotion := 'SI';
    ELSE
        p_eligible_promotion := 'NO';
    END IF;
END get_eligible_promotion;
```

Ejecutar el procedimiento desde SQL Developer:

```sql
DECLARE
  v_output_message CHAR(2);
BEGIN
  get_eligible_promotion(120,v_output_message);
  DBMS_OUTPUT.PUT_LINE(v_output_message);
END;

```

```sql
create or replace function get_eligible_candidate(p_employee_id IN NUMBER) return char is
/**

Dado un legajo de un empleado determina si promociona o no de puesto laboral.

@param p_employee_id es un legajo de empleado
@return Un char indicado SI o NO



*/
v_salary        employees.salary%TYPE;
v_hire_date     employees.hire_date%TYPE;
v_years_worked  NUMBER;
v_eligible_promotion char(2);
begin
 -- Obtener datos del empleado
SELECT salary, hire_date
INTO v_salary, v_hire_date
FROM employees
WHERE employee_id = p_employee_id;

-- Calcular años trabajados
v_years_worked := TRUNC(MONTHS_BETWEEN(SYSDATE, v_hire_date) / 12);

-- Evaluar elegibilidad para promoción usando múltiples condiciones IF
IF v_years_worked >= 5 THEN
   v_eligible_promotion := 'SI';
ELSE
   v_eligible_promotion := 'NO';
END IF;

return v_eligible_promotion;

end;

```

```sql

Version: Oracle 23 o Anterior
select get_eligible_candidate(101) from dual;
```


```sql

Version: Unicamente Oracle 23 
select get_eligible_candidate(101);
```

## Clase 12/06

```sql
CREATE OR REPLACE PROCEDURE get_hired_employees (
    p_start_year IN NUMBER,
    p_end_year   IN NUMBER
) IS
    v_count NUMBER;
BEGIN
    IF p_start_year < p_end_year THEN
        SELECT
            COUNT(*)
        INTO v_count
        FROM
            employees
        WHERE
               EXTRACT(YEAR FROM hire_date) BETWEEN p_start_year AND  p_end_year;
        dbms_output.put_line(v_count);
        ELSE
        dbms_output.put_line('El primer año debe ser menor que el segundo año');
    END IF;
END;
```

```sql
Test en SQL Developer:

BEGIN
 get_hired_employees(2024,2025);
END;

```

```sql
Version 1: Uso de Loop

DECLARE
v_salary employees.salary%TYPE;
v_employee_id NUMBER := 110;
BEGIN
LOOP
    SELECT salary
    INTO v_salary
    FROM employees
    WHERE employee_id = v_employee_id;
    DBMS_OUTPUT.PUT_LINE('id: ' || v_employee_id);
    DBMS_OUTPUT.PUT_LINE('salary: ' || v_salary);
    v_employee_id := v_employee_id + 1;
    IF v_employee_id > 115 THEN
         EXIT;
    END IF;
END LOOP;
   DBMS_OUTPUT.PUT_LINE('== Fin ==');
    
END;
```

```sql
Version 2: Uso de for loop

declare
v_salary  employees.salary%TYPE;
v_current_date VARCHAR(10);
v_list VARCHAR(255);
begin
select to_char(sysdate, 'YYYY-mm-dd') into v_current_date;
dbms_output.put_line('La fecha de hoy es: ' || v_current_date);
for i in 100..105 loop
        SELECT
            salary
        INTO v_salary
        FROM
            employees
        WHERE
            employee_id = i;
        DBMS_OUTPUT.PUT_LINE(v_salary);
        DBMS_OUTPUT.PUT_LINE(i);
end loop;
end;
```

```sql

Version 3: Mejorada

DECLARE
    v_salary       employees.salary%TYPE;
    v_current_date VARCHAR(10);
    v_list VARCHAR(255);
BEGIN
    SELECT
        to_char(sysdate, 'DD-MM-YYYY')
    INTO v_current_date;

    dbms_output.put_line('Informe del dia: ' || v_current_date);
    FOR i IN 100..105 LOOP
        SELECT
            salary
        INTO v_salary
        FROM
            employees
        WHERE
            employee_id = i;

        v_list := v_list
                  || to_char(v_salary)
                  || ',';
    END LOOP;
     -- Remove the trailing comma
    IF v_list IS NOT NULL THEN
        v_list := rtrim(v_list, ',');
    END IF;

    dbms_output.put_line(v_list);
END;
```

```sql

Version 4: Version sin loop y for
DECLARE
    v_current_date VARCHAR(10);
    v_list         VARCHAR(255);
BEGIN

SELECT to_char(sysdate, 'DD-MM-YYYY') INTO v_current_date;
dbms_output.put_line('Informe del dia: ' || v_current_date);

select listagg(salary,',') WITHIN GROUP(order by employee_id) into v_list from employees where employee_id in (100,101,102,103,104,105);

dbms_output.put_line(v_list);

END;
```

## Clase 17/06

```sql
create or replace procedure statistics_dept (p_dept_name IN departments.department_name%TYPE) is
r_dept departments%ROWTYPE;
v_count_total NUMBER;
v_salary_total NUMBER;
v_salary_max NUMBER;
v_salary_min NUMBER;
BEGIN
SELECT d.department_name,d.department_id, count(*) as cantidad, sum(e.salary) as total, min(e.salary) as minimo, max(e.salary) as maximo into
r_dept.department_name, r_dept.department_id,v_count_total,v_salary_total,v_salary_min,v_salary_max
FROM employees e INNER JOIN departments d ON e.department_id=d.department_id where UPPER(p_dept_name)=UPPER(d.department_name) group by d.department_name, d.department_id;
-- Mostrar las estadisticas
DBMS_OUTPUT.PUT_LINE('');
DBMS_OUTPUT.PUT_LINE('=== ESTADÍSTICAS DEL DEPARTAMENTO ===');
DBMS_OUTPUT.PUT_LINE('Nombre de Departamento: ' ||p_dept_name);
DBMS_OUTPUT.PUT_LINE('ID de Departamento: ' || r_dept.department_id);
DBMS_OUTPUT.PUT_LINE('Total de empleados: ' || v_count_total);
DBMS_OUTPUT.PUT_LINE('SALARIO TOTAL: ' || TO_CHAR(v_salary_total, '999,999,999.99'));
DBMS_OUTPUT.PUT_LINE('SALARIO MINIMO: ' || TO_CHAR(v_salary_min, '999,999,999.99'));
DBMS_OUTPUT.PUT_LINE('SALARIO MAXIMO: ' || TO_CHAR(v_salary_max, '999,999,999.99'));
EXCEPTION
WHEN NO_DATA_FOUND THEN
DBMS_OUTPUT.PUT_LINE('Departamento no encontrado: ' || p_dept_name);
WHEN OTHERS THEN
DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;

Ejecución:

begin
statistics_dept('IT');
end;
ó

begin
statistics_dept('ITA');
end;

ó

exec statistics_dept('IT');

ó
execute statistics_dept('IT');
```

```sql
create or replace procedure statistics_dept_2(p_dept_name IN departments.department_name%TYPE) is
TYPE r_dept IS RECORD(
department_id departments.department_id%TYPE,
department_name departments.department_name%TYPE,
employees_total     NUMBER,
salary_total        NUMBER,
salary_min          NUMBER,
salary_max          NUMBER
);
v_info_depto r_dept;
BEGIN
SELECT d.department_name,d.department_id, count(*) as cantidad, sum(e.salary) as total, min(e.salary) as minimo, max(e.salary) as maximo into
v_info_depto.department_name, v_info_depto.department_id, v_info_depto.employees_total, v_info_depto.salary_total,v_info_depto.salary_min, v_info_depto.salary_max
FROM employees e INNER JOIN departments d ON e.department_id=d.department_id where UPPER(p_dept_name)=UPPER(d.department_name) group by d.department_name, d.department_id;
-- Mostrar las estadisticas
DBMS_OUTPUT.PUT_LINE('');
DBMS_OUTPUT.PUT_LINE('=== ESTADÍSTICAS DEL DEPARTAMENTO ===');
DBMS_OUTPUT.PUT_LINE('Nombre de Departamento: ' ||p_dept_name);
DBMS_OUTPUT.PUT_LINE('ID de Departamento: ' || v_info_depto.department_id);
DBMS_OUTPUT.PUT_LINE('Total de empleados: ' || v_info_depto.employees_total);
DBMS_OUTPUT.PUT_LINE('SALARIO TOTAL: ' || TO_CHAR(v_info_depto.salary_total, '999,999,999.99'));
DBMS_OUTPUT.PUT_LINE('SALARIO MINIMO: ' || TO_CHAR(v_info_depto.salary_min, '999,999,999.99'));
DBMS_OUTPUT.PUT_LINE('SALARIO MAXIMO: ' || TO_CHAR(v_info_depto.salary_max, '999,999,999.99'));
EXCEPTION
WHEN NO_DATA_FOUND THEN
DBMS_OUTPUT.PUT_LINE('Departamento no encontrado: ' || p_dept_name);
WHEN OTHERS THEN
DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;

exec statistics_dept_2('IT');

```

```sql
create or replace procedure employee_data(
p_emp_id IN employees.employee_id%type)
IS
v_out VARCHAR2(100);
ex_invalid_id EXCEPTION;

BEGIN

if(p_emp_id <= 0) THEN
RAISE ex_invalid_id; 
ELSE
select e.first_name || ' ' || e.last_name into v_out from employees e where p_emp_id = e.employee_id;
dbms_output.put_line(v_out);
end if;

EXCEPTION
when ex_invalid_id then
dbms_output.put_line('el ID debe ser mayor a cero');
when no_data_found then
dbms_output.put_line('No se encontro el ID ingresado ' || p_emp_id);
when others then
dbms_output.put_line('Revisa bien los datos ingresados ');


END;

exec employee_data(100);

```

```sql
DECLARE
  v_last_name VARCHAR2(50);
BEGIN

FOR r_employee IN (SELECT last_name,hire_date FROM employees)
LOOP
dbms_output.put_line(r_employee.last_name || ' hire date: ' || r_employee.hire_date);
END LOOP;

END;
```

## Clase 19/06

```sql
DECLARE
CURSOR c_employees
   IS
      SELECT last_name FROM employees;
v_last_name VARCHAR2(50);
BEGIN

FOR r_employee IN c_employees
LOOP
dbms_output.put_line(r_employee.last_name);
END LOOP;

END;
```

```sql
DECLARE
  v_last_name VARCHAR2(50);
  CURSOR c_employee IS
  SELECT last_name FROM employees;
BEGIN
  OPEN c_employee;
  FETCH c_employee INTO v_last_name;
  DBMS_OUTPUT.PUT_LINE('El apellido del empleado es : ' || v_last_name);
  CLOSE c_employee;
END;
```

```sql
DECLARE
  v_last_name VARCHAR2(50);
  CURSOR c_employee IS
  SELECT last_name FROM employees;
BEGIN
--Cursor Explicito.
 OPEN c_employee;
 LOOP
 FETCH c_employee INTO v_last_name;
 EXIT WHEN c_employee%NOTFOUND;
 DBMS_OUTPUT.PUT_LINE('El apellido del empleado es : ' || v_last_name);
 
 END LOOP;
 CLOSE c_employee;
END;
```

```sql
DECLARE
  v_last_name VARCHAR2(50);
  CURSOR c_employee IS
  SELECT last_name FROM employees;
BEGIN
--Cursor Explicito.
 OPEN c_employee;
 LOOP
 FETCH c_employee INTO v_last_name;
 EXIT WHEN c_employee%NOTFOUND;
 DBMS_OUTPUT.PUT_LINE('El apellido del empleado es : ' || v_last_name);
 END LOOP;
 DBMS_OUTPUT.PUT_LINE('Cantidad de Filas: ' || c_employee%rowcount);
 CLOSE c_employee;
END;
```


```sql
create or replace procedure employes_department(departmen_num in employees.department_id%TYPE) IS 
  v_last_name employees.last_name%type;
  CURSOR c_employee IS
  SELECT last_name FROM employees where department_id= departmen_num;
  
BEGIN
--Cursor Explicito.
 OPEN c_employee;
 LOOP
 FETCH c_employee INTO v_last_name;
 EXIT WHEN c_employee%NOTFOUND;
 DBMS_OUTPUT.PUT_LINE('El apellido del empleado es : ' || v_last_name);
 END LOOP;
 DBMS_OUTPUT.PUT_LINE('Cantidad de Filas: ' || c_employee%rowcount);
 CLOSE c_employee;
 EXCEPTION
    WHEN OTHERS THEN
        IF c_employee%isopen THEN
            CLOSE c_employee;
        END IF;
END;

--Ejecución del Procedimiento employees_department
BEGIN 
employes_department (50); 
END;
```

```sql

-- Hacer un procedimiento denominado list_employees_department que dado un nombre de un departamento, despliega el nombre completo, cargo y salario 
-- de todos sus empleados ordenados por salario en forma ascendente.Si el departamento no existe o no tiene empleados cancelar (Usar Cursores Explicitos)


CREATE OR REPLACE PROCEDURE employees_average_salary (p_depto_name departments.department_name%type) IS
-- Mostrar los empleados de un departamento pasado por parametro cuyo salario sea mayor al promedio.
 CURSOR c_employees IS
        SELECT e.employee_id,
               e.first_name,
               e.last_name,
               e.email,
               e.salary,
               d.department_name,
               j.job_title
        FROM employees e
        INNER JOIN departments d ON e.department_id = d.department_id
        INNER JOIN jobs j ON e.job_id = j.job_id
        WHERE UPPER(d.department_name)=UPPER(p_depto_name) and e.salary > (SELECT AVG(salary) FROM employees)
        ORDER BY e.salary DESC;
        
    v_employee_id employees.employee_id%TYPE;
    v_first_name employees.first_name%TYPE;
    v_last_name employees.last_name%TYPE;
    v_email employees.email%TYPE;
    v_salary employees.salary%TYPE;
    v_department_name departments.department_name%TYPE;
    v_job_title jobs.job_title%TYPE;

    v_contador NUMBER := 0;
    v_salary_avg NUMBER;
    
BEGIN
  
   --Guardo en v_salary_avg el promedio salarial de los empleados de la empresa
   select avg(salary) into v_salary_avg from employees;
    
    DBMS_OUTPUT.PUT_LINE('=== EMPLEADOS CON SALARIO SUPERIOR AL PROMEDIO ===');
    DBMS_OUTPUT.PUT_LINE('Salario promedio: $' || ROUND(v_salary_avg, 2));
    DBMS_OUTPUT.PUT_LINE('================================================');
    DBMS_OUTPUT.PUT_LINE('');
    
    OPEN c_employees;
    LOOP
    FETCH c_employees INTO v_employee_id, v_first_name,v_last_name, v_email,v_salary,v_department_name,v_job_title;
    EXIT WHEN c_employees%NOTFOUND;
    v_contador:=v_contador+1;
    DBMS_OUTPUT.PUT_LINE('Empleado #' || v_contador);
    DBMS_OUTPUT.PUT_LINE('ID: ' || v_employee_id);
    DBMS_OUTPUT.PUT_LINE('Nombre: ' || v_first_name || ' ' || v_last_name);
    DBMS_OUTPUT.PUT_LINE('Email: ' || v_email);
    DBMS_OUTPUT.PUT_LINE('Salario: $' || v_salary);
    DBMS_OUTPUT.PUT_LINE('Departamento: ' || v_department_name);
    DBMS_OUTPUT.PUT_LINE('Puesto: ' || v_job_title);
    DBMS_OUTPUT.PUT_LINE('Diferencia con promedio: $' || ROUND(v_salary - v_salary_avg, 2));
    DBMS_OUTPUT.PUT_LINE('----------------------------------------');
    END LOOP;
    CLOSE c_employees;
    DBMS_OUTPUT.PUT_LINE('');
    DBMS_OUTPUT.PUT_LINE('Total de empleados con salario superior al promedio: ' || v_contador);
    EXCEPTION
    WHEN OTHERS THEN
        -- Cerrar el cursor en caso de error
        IF c_employees%ISOPEN THEN
            CLOSE c_employees;
        END IF;
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);

END;
```

## Clase 24/06 y 26/06

```sql

-- Hacer un procedimiento denominado list_employees_department que dado un nombre de un departamento, despliega el nombre completo, cargo y salario 
-- de todos sus empleados ordenados por salario en forma ascendente.Si el departamento no existe o no tiene empleados cancelar (Usar Cursores Explicitos)

CREATE OR REPLACE PROCEDURE list_employees_department (p_depto_name departments.department_name%type) IS
 CURSOR c_employees IS
        SELECT e.first_name || ' , ' || e.last_name,
               e.salary,
               j.job_title
        FROM employees e
        INNER JOIN departments d ON e.department_id = d.department_id
        INNER JOIN jobs j ON e.job_id = j.job_id
        WHERE UPPER(d.department_name)=UPPER(p_depto_name)
        ORDER BY e.salary asc;
        

    v_full_name VARCHAR2(50);
    v_salary employees.salary%TYPE;
    v_job_title jobs.job_title%TYPE;
    v_count NUMBER;
    v_emp_count NUMBER;
    ex_invalid_depto EXCEPTION;
    ex_no_employees EXCEPTION;
    
BEGIN
    
    -- Verificar si el departamento existe
    SELECT count(*) into v_count FROM departments WHERE UPPER(department_name)=UPPER(p_depto_name);
    IF(v_count = 0) THEN
    RAISE ex_invalid_depto; 
    END IF;
    -- Verificar si el departamento tiene empleados
    SELECT count(*) INTO v_emp_count FROM employees e INNER JOIN departments d ON e.department_id = d.department_id
    WHERE UPPER(d.department_name)=UPPER(p_depto_name);
    IF(v_emp_count = 0) THEN
    RAISE ex_no_employees;
    END IF;
    
    
    OPEN c_employees;
    LOOP
    FETCH c_employees INTO v_full_name,v_salary,v_job_title;
    EXIT WHEN c_employees%NOTFOUND;
    DBMS_OUTPUT.PUT_LINE('Nombre: ' || v_full_name);
    DBMS_OUTPUT.PUT_LINE('Salario: $' || v_salary);
    DBMS_OUTPUT.PUT_LINE('Puesto: ' || v_job_title);
    DBMS_OUTPUT.PUT_LINE('----------------------------------------');
    END LOOP;
    
    CLOSE c_employees;
    EXCEPTION
    WHEN ex_invalid_depto THEN
       DBMS_OUTPUT.PUT_LINE('EL Departamento no existe');
    WHEN ex_no_employees THEN
       DBMS_OUTPUT.PUT_LINE('ERROR: El departamento "' || p_depto_name || '" no tiene empleados asignados.');   
    WHEN OTHERS THEN
        -- Cerrar el cursor en caso de error
        IF c_employees%ISOPEN THEN
            CLOSE c_employees;
        END IF;
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;

-- Test

BEGIN
 list_employees_department('Shipping');
END;

```

```sql

-- Hacer un procedimiento denominado list_employees_department_2 que dado un nombre de un departamento, despliega el nombre completo, cargo y salario 
-- de todos sus empleados ordenados por salario en forma ascendente.Para esto cargar en una tabla en memoria los empleados ordenados por el salario.
-- Si el departamento no existe o no tiene empleados cancelar.Cargar la tabla con Bulk Collect

CREATE OR REPLACE PROCEDURE list_employees_department_2 (p_depto_name departments.department_name%type) IS
-- Definir el tipo de registro para los empleados
TYPE t_employee_record IS RECORD (
        full_name VARCHAR2(100),
        job_title jobs.job_title%TYPE,
        salary employees.salary%TYPE
);

-- Definir la tabla en memoria (array asociativo)
TYPE t_employees_table IS TABLE OF t_employee_record;

-- Declarar la variable de la tabla
v_employees_table t_employees_table;

ex_no_employees EXCEPTION;
ex_invalid_depto EXCEPTION;
v_count NUMBER;
v_emp_count NUMBER;

BEGIN

-- Verificar si el departamento existe
SELECT count(*) into v_count FROM departments WHERE UPPER(department_name)=UPPER(p_depto_name);
IF(v_count = 0) THEN
RAISE ex_invalid_depto; 
END IF;
-- Verificar si el departamento tiene empleados
SELECT count(*) INTO v_emp_count FROM employees e INNER JOIN departments d ON e.department_id = d.department_id
WHERE UPPER(d.department_name)=UPPER(p_depto_name);
IF(v_emp_count = 0) THEN
RAISE ex_no_employees;
END IF;
-- Cargar la tabla con Bulk Collect
 SELECT e.first_name || ', ' || e.last_name,
               j.job_title,
               e.salary
              
  BULK COLLECT INTO v_employees_table
  FROM employees e
  INNER JOIN departments d ON e.department_id = d.department_id
  INNER JOIN jobs j ON e.job_id = j.job_id
  WHERE UPPER(d.department_name)=UPPER(p_depto_name)
  ORDER BY e.salary asc;
  
   -- Recorrer la tabla en memoria y mostrar los resultados
    FOR i IN 1..v_employees_table.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE('Nombre Completo: ' || v_employees_table(i).full_name);
        DBMS_OUTPUT.PUT_LINE('Cargo: ' || v_employees_table(i).job_title);
        DBMS_OUTPUT.PUT_LINE('Salario: $' || TO_CHAR(v_employees_table(i).salary, '999,999.99'));
        DBMS_OUTPUT.PUT_LINE('--------------------------------------------');
    END LOOP;

EXCEPTION
   WHEN ex_invalid_depto THEN
    DBMS_OUTPUT.PUT_LINE('EL Departamento no existe');
    WHEN ex_no_employees THEN
        DBMS_OUTPUT.PUT_LINE('ERROR: El departamento "' || p_depto_name || '" no tiene empleados asignados.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('ERROR INESPERADO: ' || SQLERRM);

END;

--Test

BEGIN
 list_employees_department_2('Shipping');
END;

```

```sql
--Crear una función denominada search_employees_department que solicite el id del empleado y el nombre del departamento,
--verifique si el empleado trabaja actualmente en dicho departamento (Boolean).


CREATE OR REPLACE FUNCTION search_employees_department (p_id employees.employee_id%type,p_depto_name departments.department_name%type) RETURN BOOLEAN IS
v_count NUMBER;
BEGIN

SELECT count(e.employee_id) into v_count FROM EMPLOYEES e INNER JOIN DEPARTMENTS d ON e.department_id=d.department_id WHERE e.employee_id=p_id AND UPPER(d.department_name)=UPPER(p_depto_name);

if (v_count=0) THEN
return FALSE;
ELSE
return TRUE;
END IF;

END;

-- TEST : Verificar el employee_id 100 que trabaja en executive -->true
-- Pruebo esta QUERY para ver el si el empleado trabaja en dicho departamento: SELECT e.employee_id,d.department_name  FROM EMPLOYEES e INNER JOIN DEPARTMENTS d ON
--e.department_id=d.department_id ORDER BY employee_id;
 
 DECLARE
 v_out BOOLEAN;
 BEGIN
  v_out:=search_employees_department(100, 'executive');
  dbms_output.put_line(TO_CHAR(v_out));
 END;

```

```sql

-- Creación del Paquete pkg_department.

CREATE OR REPLACE PACKAGE pkg_department AS
FUNCTION search_employees_department (p_id employees.employee_id%type,p_depto_name departments.department_name%type) RETURN BOOLEAN;
PROCEDURE list_employees_department_2 (p_depto_name departments.department_name%type);
END;

-- Creación del cuerpo del Paquete

CREATE OR REPLACE PACKAGE BODY pkg_department AS

FUNCTION search_employees_department (p_id employees.employee_id%type,p_depto_name departments.department_name%type) RETURN BOOLEAN IS
v_count NUMBER;
BEGIN

SELECT count(e.employee_id) into v_count FROM EMPLOYEES e INNER JOIN DEPARTMENTS d ON e.department_id=d.department_id WHERE e.employee_id=p_id AND UPPER(d.department_name)=UPPER(p_depto_name);

if (v_count=0) THEN
return FALSE;
ELSE
return TRUE;
END IF;

END;

PROCEDURE list_employees_department_2 (p_depto_name departments.department_name%type) IS
-- Definir el tipo de registro para los empleados
TYPE t_employee_record IS RECORD (
        full_name VARCHAR2(100),
        job_title jobs.job_title%TYPE,
        salary employees.salary%TYPE
);

-- Definir la tabla en memoria (array asociativo)
TYPE t_employees_table IS TABLE OF t_employee_record;

-- Declarar la variable de la tabla
v_employees_table t_employees_table;

ex_no_employees EXCEPTION;
ex_invalid_depto EXCEPTION;
v_count NUMBER;
v_emp_count NUMBER;

BEGIN

-- Verificar si el departamento existe
SELECT count(*) into v_count FROM departments WHERE UPPER(department_name)=UPPER(p_depto_name);
IF(v_count = 0) THEN
RAISE ex_invalid_depto; 
END IF;
-- Verificar si el departamento tiene empleados
SELECT count(*) INTO v_emp_count FROM employees e INNER JOIN departments d ON e.department_id = d.department_id
WHERE UPPER(d.department_name)=UPPER(p_depto_name);
IF(v_emp_count = 0) THEN
RAISE ex_no_employees;
END IF;
-- Cargar la tabla con Bulk Collect
 SELECT e.first_name || ', ' || e.last_name,
               j.job_title,
               e.salary
              
  BULK COLLECT INTO v_employees_table
  FROM employees e
  INNER JOIN departments d ON e.department_id = d.department_id
  INNER JOIN jobs j ON e.job_id = j.job_id
  WHERE UPPER(d.department_name)=UPPER(p_depto_name)
  ORDER BY e.salary asc;
  
   -- Recorrer la tabla en memoria y mostrar los resultados
    FOR i IN 1..v_employees_table.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE('Nombre Completo: ' || v_employees_table(i).full_name);
        DBMS_OUTPUT.PUT_LINE('Cargo: ' || v_employees_table(i).job_title);
        DBMS_OUTPUT.PUT_LINE('Salario: $' || TO_CHAR(v_employees_table(i).salary, '999,999.99'));
        DBMS_OUTPUT.PUT_LINE('--------------------------------------------');
    END LOOP;

EXCEPTION
   WHEN ex_invalid_depto THEN
    DBMS_OUTPUT.PUT_LINE('EL Departamento no existe');
    WHEN ex_no_employees THEN
        DBMS_OUTPUT.PUT_LINE('ERROR: El departamento "' || p_depto_name || '" no tiene empleados asignados.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('ERROR INESPERADO: ' || SQLERRM);

END;
END;

-- Probar la función search_employees_department() del package pkg_department

DECLARE
v_out Boolean;
BEGIN

v_out:=pkg_department.search_employees_department (100,'Executive'); 
dbms_output.put_line(TO_CHAR(v_out));

END;

-- Otra forma de testear la misma función

select pkg_department.search_employees_department(100,'Executive') from dual;

-- Ahora llamo al procedimiento list_employees_department_2()

execute pkg_department.list_employees_department_2('Executive');



```

```sql

-- Obtener los tres departamentos que poseen el mayor promedio salarial, incluyendo el nombre del departamento y su valor promedio

SELECT
    d.department_name AS "primeros_3_departamentos",
    AVG(e.salary)     AS "mayor_promedio_salarial"
FROM
         departments d
    INNER JOIN employees e ON d.department_id = e.department_id
GROUP BY
    d.department_name
ORDER BY
    AVG(e.salary) DESC
FETCH FIRST 3 ROW ONLY;


```

## Clase 01/07

```sql
--Cabecera del Package pkg_salary

CREATE OR REPLACE PACKAGE pkg_salary AS
TYPE t_employee_analysis IS RECORD (
    employee_id    employees.employee_id%TYPE,
    full_name      VARCHAR2(100),
    job_title      jobs.job_title%TYPE,
    current_salary employees.salary%TYPE,
    proposed_salary NUMBER,
    bonus          NUMBER
  );
TYPE t_salary IS TABLE OF t_employee_analysis;

PROCEDURE department_review_salary (p_department_id IN departments.department_id%TYPE);
END;

--Cuerpo del Package pkg_salary

CREATE OR REPLACE PACKAGE BODY pkg_salary AS
PROCEDURE department_review_salary (p_department_id IN departments.department_id%TYPE) AS
    -- El procedimiento department_review_salary analiza todos los empleados de un departamento específico y 
    -- genera un informe con propuestas de aumento salarial y bonos basados en su puesto de trabajo.

    -- Colección para cargar eficientemente a todos los empleados del departamento
    TYPE t_employee_table IS TABLE OF employees%ROWTYPE;
    v_employees t_employee_table;

    -- Varray para almacenar los resultados de nuestro análisis
    v_review_list t_salary := t_salary(); -- Inicializamos la colección vacía

    v_temp_record t_employee_analysis;
    v_increase_rate NUMBER;
    c_it_prog_rate     CONSTANT NUMBER := 0.07;
    c_fi_account_rate  CONSTANT NUMBER := 0.05;
    c_default_rate     CONSTANT NUMBER := 0.04;
    
     CURSOR c_employees IS
     SELECT *
     FROM employees
     WHERE department_id = p_department_id
     AND employees.salary IS NOT NULL;


  BEGIN
  -- Validación de entrada
    IF p_department_id IS NULL THEN
      RAISE_APPLICATION_ERROR(-20001, 'El ID del departamento no puede ser nulo');
    END IF;
    
    -- Paso A: Usar BULK COLLECT para una carga masiva y eficiente de datos.
    -- Esto ejecuta UNA sola consulta para traer TODAS las filas necesarias.
    OPEN c_employees;
    FETCH c_employees BULK COLLECT INTO v_employees;
    CLOSE c_employees;


    -- Si no se encontraron empleados, salir.
    IF v_employees.COUNT = 0 THEN
      DBMS_OUTPUT.PUT_LINE('No se encontraron empleados para el departamento ' || p_department_id);
      RETURN;
    END IF;

    -- Paso B: Iterar sobre la colección en memoria para aplicar la lógica de negocio.
    FOR i IN v_employees.FIRST .. v_employees.LAST LOOP
      -- Lógica de negocio para determinar aumento y bono
      CASE v_employees(i).job_id
        WHEN 'IT_PROG' THEN
          v_increase_rate := c_it_prog_rate; -- 7% de aumento
          v_temp_record.bonus := 1500;
        WHEN 'FI_ACCOUNT' THEN
          v_increase_rate := c_fi_account_rate; -- 5%
          v_temp_record.bonus := 750;
        ELSE
          v_increase_rate := c_default_rate; -- 4% para el resto
          v_temp_record.bonus := 500;
      END CASE;

      -- Poblar nuestro record personalizado con los resultados
      v_temp_record.employee_id    := v_employees(i).employee_id;
      v_temp_record.full_name      := v_employees(i).first_name || ' ' || v_employees(i).last_name;
      v_temp_record.job_title      := v_employees(i).job_id;
      v_temp_record.current_salary := v_employees(i).salary;
      v_temp_record.proposed_salary := ROUND(v_employees(i).salary * (1 + v_increase_rate));
      
      -- Añadir el record con los resultados a nuestra lista de revisión
      v_review_list.EXTEND;
      v_review_list(v_review_list.LAST) := v_temp_record;
      
    END LOOP;

    -- Paso C: Generar el informe final desde la colección de resultados.
    -- Encabezado
    DBMS_OUTPUT.PUT_LINE('--- Revisión Salarial para Departamento ' || p_department_id || ' ---');
    DBMS_OUTPUT.PUT_LINE(RPAD('Empleado', 25) || RPAD('Puesto', 15) || RPAD('Salario Actual', 20) || RPAD('Salario Propuesto', 20) || 'Bono por Capacitación');
    DBMS_OUTPUT.PUT_LINE(RPAD('-', 103, '-'));

    FOR i IN v_review_list.FIRST .. v_review_list.LAST LOOP
      DBMS_OUTPUT.PUT_LINE(
        RPAD(v_review_list(i).full_name, 25) ||
        RPAD(v_review_list(i).job_title, 15) ||
        RPAD(TO_CHAR(v_review_list(i).current_salary, '$99,999.00'), 20) ||
        RPAD(TO_CHAR(v_review_list(i).proposed_salary, '$99,999.00'), 20) ||
        TO_CHAR(v_review_list(i).bonus, '$9,999.00')
      );
    END LOOP;
  EXCEPTION
    WHEN OTHERS THEN
      DBMS_OUTPUT.PUT_LINE('Otro Error -> '||SQLERRM);
  END;


END;

-- Test (Recorremos todos los departamentos para visualizar)

DECLARE

CURSOR c_departments IS
 SELECT DISTINCT department_id
    FROM departments 
    WHERE department_id IS NOT NULL
    ORDER BY department_id;
BEGIN

 DBMS_OUTPUT.PUT_LINE('=== TESTING COMPLETO DE TODOS LOS DEPARTAMENTOS ===');
 
 FOR dept IN c_departments LOOP --Cursor Implicito
 
    DBMS_OUTPUT.PUT_LINE(CHR(10) || 'Procesando: ' || dept.department_id);
     pkg_salary.department_review_salary(dept.department_id);
 
 
 END LOOP;

END;

```

## Clase 03/07

```sql
SELECT e.employee_id, e.first_name, e.last_name, e.email, e.salary,d.department_name
FROM employees e INNER JOIN departments d ON e.department_id = d.department_id ORDER BY e.last_name ASC;
```

```sql
EXPLAIN PLAN FOR
SELECT e.employee_id, e.first_name, e.last_name, e.email, e.salary,d.department_name
FROM employees e INNER JOIN departments d ON e.department_id = d.department_id ORDER BY e.last_name ASC;
SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);
```

```sql
CREATE INDEX idx_emp_covering ON employees(department_id, last_name, employee_id, first_name, email, salary);
```

```sql
SELECT index_name, status, visibility, last_analyzed FROM user_indexes WHERE table_name = 'EMPLOYEES';
```


## Clase 08/07

```sql
CREATE GLOBAL TEMPORARY TABLE gtt_employees_salary (
    employee_id    NUMBER,
    full_name      VARCHAR2(100),
    current_salary NUMBER,
    proposed_salary NUMBER
)ON COMMIT DELETE ROWS;

```

```sql
SELECT object_type, temporary 
FROM user_objects 
WHERE object_name = 'GTT_EMPLOYEES_SALARY';
```


```sql
INSERT INTO gtt_employees_salary (employee_id, full_name, current_salary,proposed_salary)
VALUES (101, 'Martin Bach', 8000,8500);

INSERT INTO gtt_employees_salary (employee_id, full_name, current_salary,proposed_salary)
VALUES (102, 'Anders Donner', 11000,12500);
```

```sql
SELECT * FROM gtt_employees_salary;
```

```sql
commit;
```


```sql
SELECT count(*) FROM gtt_employees_salary;
```


```sql
CREATE GLOBAL TEMPORARY TABLE gtt_employees_salary (
    employee_id    NUMBER,
    full_name      VARCHAR2(100),
    current_salary NUMBER,
    proposed_salary NUMBER
)ON COMMIT PRESERVE ROWS;

```

```sql
INSERT INTO gtt_employees_salary (employee_id, full_name, current_salary,proposed_salary)
VALUES (101, 'Martin Bach', 8000,8500);

INSERT INTO gtt_employees_salary (employee_id, full_name, current_salary,proposed_salary)
VALUES (102, 'Anders Donner', 11000,12500);
```

```sql
SELECT * FROM gtt_employees_salary_data;
```

```sql
commit;
```

```sql
SELECT count(*) FROM gtt_employees_salary;
```

```sql
SELECT * FROM gtt_employees_salary;
```

```sql
CREATE GLOBAL TEMPORARY TABLE gtt_high_salary_employees
ON COMMIT PRESERVE ROWS
AS SELECT employee_id, first_name, last_name, department_id, salary
FROM employees 
WHERE salary > 5000;
```

```sql
select * from gtt_high_salary_employees;
```

```sql
WITH cte_salary_total AS (
    SELECT employee_id, SUM(salary) AS salary_anual
    FROM employees
    WHERE EXTRACT(YEAR FROM hire_date) = 2013
    GROUP BY employee_id
)
select * from cte_salary_total;
```
