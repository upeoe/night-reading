# Data Types

<!-- TOC -->

- [Data Types](#data-types)
    - [整型](#整型)
    - [浮点类型](#浮点类型)
    - [定点类型](#定点类型)
    - [日期类型](#日期类型)
        - [DATETIME vs TIMESTAMP](#datetime-vs-timestamp)
    - [字符串类型](#字符串类型)
        - [字符串类型转换](#字符串类型转换)

<!-- /TOC -->

## 整型

| 类型      | 字节数 | 有符号最小值 | 有符号最大值 | 无符号最小值 | 无符号最大值 |
|-----------|--------|--------------|--------------|--------------|--------------|
| TINYINT   | 1      | -2^7         | 2^7-1        | 0            | 2^8          |
| SMALLINT  | 2      | -2^15        | 2^15-1       | 0            | 2^16         |
| MEDIUMINT | 3      | -2^23        | 2^23-1       | 0            | 2^24         |
| INT       | 4      | -2^31        | 2^31-1       | 0            | 2^32         |
| BIGINT    | 8      | -2^63        | 2^63-1       | 0            | 2^64         |

## 浮点类型

| 类型   | 字节数 | 备注         |
|--------|--------|--------------|
| FLOAT  | 4      | 单精度浮点型 |
| DOUBLE | 8      | 双精度浮点   |

浮点型在MySQL中存储的是近似值。

## 定点类型

| 类型         | 字节数                              |
|--------------|-------------------------------------|
| DECIMAL(M,D) | 每4个字节存9个数字，小数点占1个字节 |

M: 小数点左右两侧总的最大数目，范围1~65，默认值10;
D: 小数点右侧数字数目，范围0~30，切不得超过M，默认值0;

## 日期类型

| 类型      | 字节数 | 格式                | 备注              |
|-----------|--------|---------------------|-------------------|
| DATE      | 3      | yyyy-MM-dd          | 日期值            |
| TIME      | 3      | HH:mm:ss            | 时分秒            |
| YEAR      | 1      | yyyy                | 年                |
| DATETIME  | 8      | yyyy-MM-dd HH:mm:ss | 日期+时间         |
| TIMESTAMP | 4      | yyyy-MM-dd HH:mm:ss | 日期+时间，时间戳 |

### DATETIME vs TIMESTAMP

| 类型     | DATETIME                                      | TIMESTAMP                                             |
|----------|-----------------------------------------------|-------------------------------------------------------|
| 字节数   | 8                                             | 4                                                     |
| 存储范围 | `1000-01-01 00:00:00` - `9999-12-31 23:59:59` | `1970-01-01 00:00:01 UTC` - `2038-01-19 03:14:07 UTC` |
| 默认值   | NULL                                          | CURRENT_TIMESTAMP                                     |
| 时区     | 存储时间与时区无关                            | 存储时间与显示时间依赖服务器时区                      |

## 字符串类型

M: 字段声明时的长度
L: 实际给定存储时的字符串长度

| 类型                          | 字节数     | 描述                                                                                                                  |
|-------------------------------|------------|-----------------------------------------------------------------------------------------------------------------------|
| CHAR(M)                       | M*w        | 0<=M<=255，w是字符编码所占字节数；每条数据占用等长字节空间                                                            |
| BINARY(M)                     | M          | 0<=M<=255                                                                                                             |
| VARCHAR(M), VARBINARY(M)      | L+1或L+2   | 头部会有1/2个字节用于保存总字符串长度，存储范围在0-255个字节时，占用L+1个字节；存储范围大于255个字节时，占用L+2个字节 |
| TINYBLOB, TINYTEXT            | L+1        | L<2^8                                                                                                                 |
| BLOB, TEXT                    | L+2        | L<2^16                                                                                                                |
| MEDIUMBLOB, MEDIUMTEXT        | L+3        | L<2^24                                                                                                                |
| LONGBLOB,LONGTEXT             | L+4        | L<2^32                                                                                                                |
| ENUM('value1', 'value2', ...) | 1或2       | 取决于枚举值中的value的个数，最大值65535                                                                              |
| SET('value1', 'value2', ...)  | 1,2,3,4或8 | 取决于集合中的成员个数，最大值64                                                                                      |

### 字符串类型转换

对于 `VARCHAR` 类型：

- M>255时转为`TINYTEXT`
- M>500时转为`TEXT`
- M>20000时转为`MEDIUMTEXT`