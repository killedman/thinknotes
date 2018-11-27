title: SQL命名约定
date: 2018-05-12


## 为什么命名约定很重要
### 名字长期存在
数据结构意味着比应用程序代码持续更长的时间。任何在长时间运行的系统上工作的人都可以证明这一点。

定义良好的数据结构和表格布局将超越任何应用程序代码。在没有对其数据库模式进行任何更改的情况下，完全重写应用程序并不罕见。

### 名称是合同
数据库对象由其名称引用，因此对象名称是对象合同的一部分。通过某种方式，您可以将数据库表和列名称视为数据模型的API。

一旦设置完成，更改它们可能会破坏相关的应用程序。这是在第一次使用之前正确命名事物的更多理由。

### 开发者上下文切换
在数据模型中统一命名约定意味着开发人员需要花更少的时间查找表，视图和列的名称。当您知道person_id必须是人员表的id字段的外键时，编写和调试SQL更容易。

## 命名约定
1. **避免引号**，同时避免标识符中存在空格，Ex: Avoid using names like "FirstName" or "All Employees".
2. **Lowercase** Identifiers should be written entirely in lower case. Ex: Use first_name, not "First_Name".
3.  Database object names, particularly column names, should be a noun describing the field or object. Avoid using words that are just data types such as text or timestamp. The latter is particularly bad as it provides zero context.
4. 使用下划线分割单词 Object name that are comprised of multiple words should be separated by underscores (ie. snake case).Ex: Use word_count or team_member_id, not wordcount or wordCount.
5. 使用完整的单词，避免缩写  Object names should be full English words. In general avoid abbreviations, especially if they're just the type that removes vowels. Most SQL databases support at least 30-character names which should be more than enough for a couple English words. PostgreSQL supports up to 63-character for identifiers.Ex: Use middle_name, not mid_nm.
6. 对于少数很长的词，同时缩写比原始词更通用的，使用缩写。Ex:i18n and l10n 
7. 避免使用保留词

## Singular Relations单数关系
1. 数据库表、视图等应该是单数，而不是复数 This means our tables and views would be named team, not teams.

## Key Fields

### Primary Keys主键
1. 单列的主键应该命名为id. 它短小、简单、一目了然 。Single column primary key fields should be named id. It's short, simple, and unambiguous. This means that when you're writing SQL you don't have to remember the names of the fields to join on.

### Foreign Keys 外键
1. 外键应该由指向表格的名称和指向字段组成。 Foreign key fields should be a combination of the name of the referenced table and the name of the referenced fields. For single column foreign keys (by far the most common case) this will be something like foo_id.

## Prefixes and Suffixes (are bad)前缀和后缀都是不好的做法

## Explicit Naming 显示命名

一些创建数据库对象的数据库命令不要求您指定名称。 对象名称将随机生成（例如：fk239nxvknvsdvi）或通过公式（例如：foobar_ix_1）生成。 除非您确切知道如何生成名称，并且您对此感到满意，否则应该明确指定名称。

这还包括由ORM生成的名称。 许多ORM默认使用冗长的乱码生成名称来创建索引和约束。 在短期内节省几分钟的时间并不值得记住fkas9dfnksdfnks从长远来看所指的头痛。
### 索引
索引应明确命名，并包含索引的表名和列名。 包括列名使得通过SQL解释计划更容易阅读。 如果一个索引被命名为foobar_ix1，那么你需要查找该索引涵盖哪些列以了解它是否被正确使用。
我们可以清楚地看到名字和姓氏的索引，即。 正在使用person_ix_first_name_last_name：
    CREATE INDEX person_ix_first_name_last_name ON person (first_name, last_name);
### Constraints 约束
1. Similar to indexes, constraints should given descriptive names. This is especially true for check constraints. It's much easier to diagnose an errant insert if the check constraint that was violated lets you know the cause.类似于索引，约束应该给予描述性名称。 检查约束尤其如此。 如果违反的检查约束可以让你知道原因，那么诊断错误的插入就容易得多。

     CONSTRAINT team_member_pkey PRIMARY KEY (team_id, person_id));
2. If we try inserting a row that violates one of these constraints we immediately know the cause just based on the constraint name:如果我们尝试插入违反这些约束条件之一的行，我们立即知道基于约束名称的原因：

