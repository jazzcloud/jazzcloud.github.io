# Dealing with legacy database and Rails

Well, I had some experience in the past, which I think might be useful for ruby developers, who have to deal with legacy database.

There are some potential problems, when you had to deal with existing database and you can't change its structure and naming.

Let's enumerate some of the potential problems:
1. Your database doesn't follow rails conventions: plural form for tables, columns, timestamp columns, foreign keys, primary key column names
2. Constraints + no defaults
3. Columns named with rails reserved names.



## ActiveRecord related problems



## Dealing with legacy database and Sequel

