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