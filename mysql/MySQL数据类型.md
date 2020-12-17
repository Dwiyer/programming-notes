#### 概述

------

**UNSIGNED**: 在MySQL数据库中，对于UNSIGNED 数的操作，其返回值都是UNSIGNED 的。尽量不要使用UNSIGNED，因为可能带来一些意想不到的效果。 （SQL_MODE='NO_UNSIGNED_SUBTRACTION' 减法运算不返回UNSIGNED结果）

**SQL_MODE**: 默认为空，在某些模式下，可以允许一些非法操作。生产环境强烈建议设置为严格模式（SQL_MODE='STRICT_TRANS_TABLES'|'STRICT_ALL_TABLES'）

**ZEROFILL**: 如果字段值的宽度小于设定的宽度，则查询结果自动填充0。



#### 日期和时间类型

------

| 类型      | 所占空间（字节） |
| --------- | ---------------- |
| DATETIME  | 8                |
| DATE      | 3                |
| TIMESTAMP | 4                |
| YEAR      | 1                |
| TIME      | 3                |

标准日期格式：YYYY-MM-DD HH:MM:SS































