# spring-boot-demo
springboot demo project

# 开发过程记录
1.controller 代码需要添加在demo目录下，不让扫描不到，需要特殊指定
2.配置文件直接添加，会自动生效，main路径下 

# springboot 版本：2.6.13

# 增加flyway功能
1. 添加pom.xml依赖
server:
   port: 10001

spring:
    datasource:
        url: jdbc:mysql://127.0.0.1:3307/flyway?useUnicode=true&characterEncoding=UTF-8&allowMultiQueries=true&rewriteBatchedStatements=true&useSSL=false&serverTimezone=GMT%2B8
        username: root
        password: pass4Zentao
    flyway:
        # 启用或禁用 flyway
        enabled: true
        # flyway 的 clean 命令会删除指定 schema 下的所有 table, 生产务必禁掉。这个默认值是 false 理论上作为默认配置是不科学的。
        clean-disabled: true
        # SQL 脚本的目录,多个路径使用逗号分隔 默认值 classpath:db/migration
        locations: classpath:db/migration
        #  metadata 版本控制信息表 默认 flyway_schema_history
        table: flyway_schema_history
        # 如果没有 flyway_schema_history 这个 metadata 表， 在执行 flyway migrate 命令之前, 必须先执行 flyway baseline 命令
        # 设置为 true 后 flyway 将在需要 baseline 的时候, 自动执行一次 baseline。
        baseline-on-migrate: true
        # 指定 baseline 的版本号,默认值为 1, 低于该版本号的 SQL 文件, migrate 时会被忽略
        baseline-version: 1
        # 字符编码 默认 UTF-8
        encoding: UTF-8
        # 是否允许不按顺序迁移 开发建议 true  生产建议 false
        out-of-order: false
        # 需要 flyway 管控的 schema list,这里我们配置为flyway  缺省的话, 使用spring.datasource.url 配置的那个 schema,
        # 可以指定多个schema, 但仅会在第一个schema下建立 metadata 表, 也仅在第一个schema应用migration sql 脚本.
        # 但flyway Clean 命令会依次在这些schema下都执行一遍. 所以 确保生产 spring.flyway.clean-disabled 为 true
        schemas: flyway
        # 执行迁移时是否自动调用验证   当你的 版本不符合逻辑 比如 你先执行了 DML 而没有 对应的DDL 会抛出异常
        validate-on-migrate: true

2.添加flyway yml配置及数据库连接

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