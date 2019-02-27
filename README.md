# testodbcpp11

This example shows how to use ODB with C++11. In particular, this example
examines ODB support for the new std::unique_ptr and std::shared_ptr smart
pointers and their lazy variants as well as the unordered containers. It
also shows how to use new C++11 features such as the range-based for-loop
when working with persistent objects and handling query results.

The example consists of the following files:

employee.hxx
  Header file defining the 'employee', 'employer', and 'pension_fund'
  persistent classes. We use the standard std::shared_ptr/weak_ptr smart
  pointers as well as their lazy versions provided by ODB to establish a
  bidirectional employee-employer relationship. We also enable session
  support for these two classes using the 'db session' pragma. Because
  we don't share the 'pension_fund' objects, we use std::unique_ptr
  as an object pointer for this persistent class. We also use the
  std::unordered_set container to keep track of the employee's email
  addresses.

employee-odb.hxx
employee-odb.ixx
employee-odb.cxx
employee.sql
  The first three files contain the database support code and the last file
  contains the database schema for the employee.hxx header.

  These files are generated by the ODB compiler from employee.hxx using the
  following command line:

  odb -d <database> --std c++11 --generate-schema --generate-query employee.hxx

  Where <database> stands for the database system we are using, for example,
  'mysql'.

  The --std c++11 option is used to instruct the ODB compiler to compile in
  the C++11 mode.

database.hxx
  Contains the create_database() function which instantiates the concrete
  database class corresponding to the database system we are using. Unlike
  other examples, here we use std::unique_ptr instead of std::auto_ptr to
  return the created database.

driver.cxx
  Driver for the example. It includes the employee.hxx and employee-odb.hxx
  headers to gain access to the persistent classes and their database support
  code. It also includes database.hxx for the create_database() function
  declaration.

  In main() the driver first calls create_database() to obtain the database
  instance. It then creates a number of 'employee', 'employer', and
  'pension_fund' objects and persists them in the database. Then the driver
  loads and prints some information about various objects and their
  relationships. Finally, the driver performs a database query and iterates
  over the result printing basic information about the returned objects.

To compile and link the example manually from the command line we can use
the following commands (using  MySQL as an example; replace 'c++ -std=c++11'
with your C++ compiler in C++11 mode):

c++ -std=c++11 -c employee-odb.cxx
c++ -std=c++11 -DDATABASE_MYSQL -c driver.cxx
c++ -std=c++11 -o driver driver.o employee-odb.o -lodb-mysql -lodb

To run the example we may first need to create the database schema (for some
database systems, such as SQLite, the schema is embedded into the generated
code which makes this step unnecessary). Using MySQL as an example, this
can be achieved with the following command:

mysql --user=odb_test --database=odb_test < employee.sql

Here we use 'odb_test' as the database login and also 'odb_test' as the
database name.

Once the database schema is ready, we can run the example (using MySQL as
the database):

./driver --user odb_test --database odb_test

