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
