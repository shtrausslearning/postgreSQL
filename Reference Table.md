
### Reference Table:

- Here’s an example of a database model using a reference table for available colors available for some cars brands and models:

```SQL
create table color(id serial primary key, name text);

create table cars
 (
   brand   text,
   model   text,
   color   integer references color(id)
 );

insert into color(name)
     values ('blue'), ('red'),
            ('gray'), ('black');

insert into cars(brand, model, color)
     select brand, model, color.id
      from (
            values('ferari', 'testarosa', 'red'),
                  ('aston martin', 'db2', 'blue'),
                  ('bentley', 'mulsanne', 'gray'),
                  ('ford', 'T', 'black')
           )
             as data(brand, model, color)
           join color on color.name = data.color;
```

```
    brand     │   model   │ color 
══════════════╪═══════════╪═══════
 aston martin │ db2       │     1
 ferari       │ testarosa │     2
 bentley      │ mulsanne  │     3
 ford         │ T         │     4
(4 rows)
```

### Enum Types

```SQL
create type color_t as enum('blue', 'red', 'gray', 'black');

drop table if exists cars;
create table cars
 (
   brand   text,
   model   text,
   color   color_t
 );

insert into cars(brand, model, color)
     values ('ferari', 'testarosa', 'red'),
            ('aston martin', 'db2', 'blue'),
            ('bentley', 'mulsanne', 'gray'),
            ('ford', 'T', 'black');
```
