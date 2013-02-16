file_fdw (Hadoop)
=================

Hadoop file_fdw is a slightly modified version of PostgreSQL 9.2's [file_fdw
module](http://www.postgresql.org/docs/9.2/static/file-fdw.html). This module
extends on the original file_fdw to make it compatible with CitusDB's native
Hadoop integration. The changes are still in Beta form.

To get started on using file_fdw on Hadoop, please see our documentation page on
http://citusdata.com/docs/sql-on-hadoop! If you'd like to simply use file_fdw on
distributed files instead, please check out
http://citusdata.com/docs/foreign-data. Finally, you can find a performance
evaluation of foreign data wrappers on our [blog
post](http://citusdata.com/blog/50-postgresql-foreign-file-performance).

Building
--------

The easiest way to get started with using file_fdw on Hadoop is going over the
[documentation pages](http://citusdata.com/docs/sql-on-hadoop). To build from
scratch on POSIX compliant systems (Linux and OS X), you need to include the
pg_config directory path in your make command. This path is typically the same
as your CitusDB installation's bin/ directory path. For example:

    PATH=/opt/citusdb/2.0/bin/:$PATH make USE_PGXS=1
    sudo PATH=/opt/citusdb/2.0/bin/:$PATH make USE_PGXS=1 install

Changes
-------

This module makes three changes to the original file_fdw that are still in Beta
form. First, we add another option to file_fdw to accept hdfs_directory_path as
an argument. Users creating the distributed foreign table on the master node use
this option to associate the table with an HDFS directory path.

Second, file_fdw skips a malformed line and emits a debug message instead of
erroring out on it. This is similar to Apache Hive's behavior, but isn't safe.
In the future, we may error out if the number of malformed lines exceeds a
certain threshold.

Third, CitusDB currently associates one HDFS block with one foreign table, and
executes the entire SQL query locally on that block. If bytes for the last
record in an HDFS block spill over to the next one, we currently don't fetch
those bytes and instead skip the last record. This is a limitation we intend to
fix in CitusDB 2.1.

For all types of questions and comments about CitusDB's Hadoop integration or
our changes to this wrapper, please contact us at http://citusdata.com/contact
