# Notes for Expense tracker project

# Database Design: Creating the `expenses` db

createdb expenses

psql -d expenses < schema.sql

# 1.
9999.99

expenses=# INSERT INTO expenses (amount, memo, created_on) VALUES (10000.00, 'blah', '2023-01-01');
ERROR:  numeric field overflow
DETAIL:  A field with precision 6, scale 2 must round to an absolute value less than 10^4.
expenses=# INSERT INTO expenses (amount, memo, created_on) VALUES (9999.99, 'blah', '2023-01-01');
INSERT 0 1

# 2.
-9999.99

# 3.
ALTER TABLE expenses ADD CONSTRAINT positive_amount CHECK (amount >= 0.01);

# Listing Expenses

Insert data into table:
INSERT INTO expenses (amount, memo, created_on) VALUES (14.56, 'Pencils', NOW());
INSERT INTO expenses (amount, memo, created_on) VALUES (3.29, 'Coffee', NOW());
INSERT INTO expenses (amount, memo, created_on) VALUES (49.99, 'Text Editor', NOW());

# Displaying Help

# 1.
We're created a HEREDOC block, which is a way to create a multine String in Ruby. `<<~` syntax strips leading whitespace from the beginning of each line of the string to allow for natural indenting in the code.

# Adding Expenses

Because our solution only uses string interpolation to create our sql statement, this can allow values that would alter the meaning of our statement, `'` for example in passed in memo. We'll need to parse user inputs and insure we don't accidently alter any of our sql statements.

# Handling Parameters Safely

# 1.
A `PG::ProtocolViolation` error will be raised. 

>> connection.exec_params("SELECT position($1 in $2)", ["t"]).values
PG::ProtocolViolation: ERROR:  bind message supplies 1 parameters, but prepared statement "" requires 2

        from (irb):9:in `exec_params'
        from (irb):9
        from /Users/jimb/.rubies/ruby-2.3.1/bin/irb:11:in `<main>'

# 2

From:
def add_expense(amount, memo)
  date = Date.today
  sql = "INSERT INTO expenses (amount, memo, created_on) VALUES (#{amount}, '#{memo}', '#{date}')"
  CONNECTION.exec(sql)
end

to:
def add_expense(amount, memo)
  date = Date.today
  sql = ("INSERT INTO expenses (amount, memo, created_on) VALUES ($1, $2, $3);")
  CONNECTION.exec_params(sql, [amount, memo, date])
end

# 3.

The memo including "drop table" will be added, and not executed.
./expense add 0.01 "', '2015-01-01'); DROP TABLE expenses; --"

$ ./expense list
=>   8 | 2023-07-07 |         0.01 | ', '2015-01-01'); DROP TABLE expenses; --
$

# Code Structure
