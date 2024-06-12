# spring-boot-demo
springboot demo project

# 开发过程记录
1.controller 代码需要添加在demo目录下，不让扫描不到，需要特殊指定
2.配置文件直接添加，会自动生效，main路径下 

# springboot 版本：2.6.13

# 增加flyway功能
1. 添加pom.xml依赖
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-jdbc</artifactId>
   </dependency>
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>8.0.28</version>
   </dependency>
    <dependency>
        <groupId>org.flywaydb</groupId>
        <artifactId>flyway-core</artifactId>
        <version>7.15.0</version>
    </dependency>
2. 添加flyway yml配置及数据库连接
server:
   port: 10001

spring:
    datasource:
        url: jdbc:mysql://127.0.0.1:3306/flyway?useUnicode=true&characterEncoding=UTF-8&allowMultiQueries=true&rewriteBatchedStatements=true&useSSL=false&serverTimezone=GMT%2B8
        username: root
        password: pass4Zentao
    flyway:
        enabled: true
        # 禁止清理数据库表
        clean-disabled: true
        # 如果数据库不是空表，需要设置成 true，否则启动报错
        baseline-on-migrate: true
        # 与 baseline-on-migrate: true 搭配使用
        baseline-version: 0
        locations:
            - classpath:db/migration/mysql（根据个人情况设置）

3.在resources目录下创建目录db.migration并添加迁移sql文件
V1__Create_person_table.sql
    create table PERSON (
    ID int not null,
    NAME varchar(100) not null
    );

4.启动springboot，自动进行flyway数据库迁移
Flyway Community Edition 7.15.0 by Redgate
2024-06-12 17:19:13.699  INFO 7322 --- [           main] o.f.c.i.database.base.BaseDatabaseType   : Database: jdbc:mysql://127.0.0.1:3307/flyway (MySQL 5.5)
2024-06-12 17:19:13.723  WARN 7322 --- [           main] o.f.c.internal.database.base.Database    : Flyway upgrade recommended: MariaDB 10.6 is newer than this version of Flyway and support has not been tested. The latest supported version of MariaDB is 10.5.
2024-06-12 17:19:13.734  INFO 7322 --- [           main] o.f.core.internal.command.DbValidate     : Successfully validated 1 migration (execution time 00:00.007s)
2024-06-12 17:19:13.755  INFO 7322 --- [           main] o.f.c.i.s.JdbcTableSchemaHistory         : Creating Schema History table `flyway`.`flyway_schema_history` ...
2024-06-12 17:19:13.785  INFO 7322 --- [           main] o.f.core.internal.command.DbMigrate      : Current version of schema `flyway`: << Empty Schema >>
2024-06-12 17:19:13.787  INFO 7322 --- [           main] o.f.core.internal.command.DbMigrate      : Migrating schema `flyway` to version "1 - Create person table"
2024-06-12 17:19:13.801  INFO 7322 --- [           main] o.f.core.internal.command.DbMigrate      : Successfully applied 1 migration to schema `flyway`, now at version v1 (execution time 00:00.018s)