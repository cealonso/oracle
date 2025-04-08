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
