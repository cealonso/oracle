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

