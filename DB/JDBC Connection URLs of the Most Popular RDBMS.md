---
created date: 2023-12-11
---

> [!note]
> RDBMS 의 URL 정보 모음입니다.
> 
> 아래 글의 단순 번역본 입니다.
> [JDBC Connection URLs of the Most Popular RDBMS](https://blog.jooq.org/jdbc-connection-urls-of-the-most-popular-rdbms/)

# JDBC Connection URLs of the Most Popular RDBMS

Posted on [December 1, 2023](https://blog.jooq.org/jdbc-connection-urls-of-the-most-popular-rdbms/) by [lukaseder](https://blog.jooq.org/author/lukaseder/)

JDBC로 RDBMS에 연결해야 하는데 JDBC 연결 URL이나 드라이버 이름이 없으신가요? 문제없습니다. 아래에서 사용 중인 RDBMS를 찾아보세요:

## BigQuery

```properties
driver = "com.simba.googlebigquery.jdbc42.Driver";

url = "jdbc:bigquery://https://www.googleapis.com/bigquery/v2:443;ProjectId=<project-id>;OAuthType=0;OAuthServiceAcctEmail=<service-account>;OAuthPvtKeyPath=path-to-key.json";
```

## CockroachDB

```properties
driver = "org.postgresql.Driver";

url = "jdbc:postgresql://<host>/<database>";
```

## Db2

```properties
driver = "com.ibm.db2.jcc.DB2Driver";

url = "jdbc:db2://<host>:50000/<database>";
```

## Derby

```properties
driver = "org.apache.derby.jdbc.EmbeddedDriver";

url = "jdbc:derby:<path-to-database>;create=true";
```

## DuckDB

```properties
driver = "org.duckdb.DuckDBDriver";

url = "jdbc:duckdb:<path-to-database>";
```

## EXASOL

```properties
driver = "com.exasol.jdbc.EXADriver";

url = "jdbc:exa:<host>:9563;schema=<default-schema>";
```

## Firebird

```properties
driver = "org.firebirdsql.jdbc.FBDriver";

url = "jdbc:firebirdsql:<host>:<path-to-database>";
```

## H2

```properties
driver = "org.h2.Driver";

url = "jdbc:h2:<path-to-database>";
```

## HANA

```properties
driver = "com.sap.db.jdbc.Driver";

url = "jdbc:sap://<host>:39017";
```

## HSQLDB

```properties
driver = "org.hsqldb.jdbcDriver";

url = "jdbc:hsqldb:file:<path-to-database>";
```

## Ignite

```properties
driver = "org.apache.ignite.IgniteJdbcThinDriver";

url = "jdbc:ignite:thin://<host>";
```

## Informix

```properties
driver = "com.informix.jdbc.IfxDriver";

url = "jdbc:informix-sqli://<host>:9088/<database>:INFORMIXSERVER=informix";
```

## Ingres

```properties
driver = "com.ingres.jdbc.IngresDriver";

url = "jdbc:ingres://<host>:II7/<database>";
```

## MariaDB

```properties
driver = "org.mariadb.jdbc.Driver";

url = "jdbc:mariadb://<host>/<default-schema>";
```

## MemSQL

```properties
driver = "com.mysql.cj.jdbc.Driver";

url = "jdbc:mysql://127.0.0.1/<default-schema>";
```

## MySQL

```properties
driver = "com.mysql.cj.jdbc.Driver";

url = "jdbc:mysql://127.0.0.1/<default-schema>";
```

## Oracle

```properties
driver = "oracle.jdbc.OracleDriver";

url = "jdbc:oracle:thin:@<database>:1521/<SID>"
```

## PostgreSQL

```properties
driver = "org.postgresql.Driver";

url = "jdbc:postgresql://<host>/<database>";
```

## Redshift

```properties
driver = "com.amazon.redshift.jdbc.Driver";

url = "jdbc:redshift://<cluster-id>.<region>.redshift.amazonaws.com:5439/<database>"
```

## Snowflake

```properties
driver = "net.snowflake.client.jdbc.SnowflakeDriver";

url = "jdbc:snowflake://<server>.<region>.snowflakecomputing.com:443/?db=<database>&schema=<schema>";
```

## SQLite

```properties
driver = "org.sqlite.JDBC";

url = "jdbc:sqlite:<path-to-database>";
```

## SQL Server

```properties
driver = "com.microsoft.sqlserver.jdbc.SQLServerDriver";

url = "jdbc:sqlserver://<host>:1433;databaseName=<database>";
```

## Sybase ASE

```properties
driver = "com.sybase.jdbc4.jdbc.SybDriver";

url = "jdbc:sybase:Tds:<host>:5000/<database>";
```

## Sybase SQL Anywhere

```properties
driver = "com.sybase.jdbc4.jdbc.SybDriver";

url = "jdbc:sybase:Tds:<host>:2638";
```

## Teradata

```properties
driver = "com.teradata.jdbc.TeraDriver";

url = "jdbc:teradata://<host>/DATABASE=<database>,DBS_PORT=1025"
```

## Trino

```properties
driver = "io.trino.jdbc.TrinoDriver";

url = "jdbc:trino://<host>:8080/memory/default";
```

## Vertica

```properties
driver = "com.vertica.jdbc.Driver";

url = "jdbc:vertica://<host>:5433/<database>";
```

## YugabyteDB

```properties
driver = "com.yugabyte.Driver";

url = "jdbc:yugabytedb://<host>/<database>";
```
