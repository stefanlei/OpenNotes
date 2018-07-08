- 常见的数据类型

> MySQL支持多种类型，大致可以分为四类：**数值型、浮点型、日期/时间、字符串(字符)类型**。 

- 数值

> MySQL支持所有标准SQL数值数据类型。 

> **这些数值类型包括** 严格数值数据类型(INTEGER、SMALLINT、DECIMAL和NUMERIC)，以及近似数值数据类型(FLOAT、REAL和DOUBLE PRECISION)。关键字INT是INTEGER的同义词，关键字DEC是DECIMAL的同义词。作为SQL标准的扩展，MySQL也支持整数类型TINYINT、MEDIUMINT和BIGINT。下面的表显示了需要的每个整数类型的存储和范围：

![](http://ke.dajiangtai.com/content/411/1.png)

- 浮点型

![](http://ke.dajiangtai.com/content/411/2.png)



- 时间和日期

**在生产里，日期时间型，往往用的比较少，而是用数字类型来取代日期类型** 

> 表示时间值的日期和时间类型为DATETIME、DATE、TIMESTAMP、TIME和YEAR。每个时间类型有一个有效值范围和一个"零"值，当指定不合法的MySQL不能表示的值时使用"零"值。TIMESTAMP类型有专有的自动更新特性，将在后面描述。

![](http://ke.dajiangtai.com/content/411/3.png)



- 字符串类型

> 字符串类型指CHAR、VARCHAR、BINARY、VARBINARY、BLOB、TEXT、ENUM和SET。该节描述了这些类型如何工作以及如何在查询中使用这些类型。 

![](http://ke.dajiangtai.com/content/411/4.png)

> CHAR和VARCHAR类型类似，但它们保存和检索的方式不同。它们的最大长度和是否尾部空格被保留等方面也不同。在存储或检索过程中不进行大小写转换。
>
> BINARY和VARBINARY类类似于CHAR和VARCHAR，不同的是它们包含二进制字符串而不要非二进制字符串。也就是说，它们包含字节字符串而不是字符字符串。这说明它们没有字符集，并且排序和比较基于列值字节的数值值。
>
> 有4种TEXT类型：TINYTEXT、TEXT、MEDIUMTEXT和LONGTEXT。这些对应4种BLOB类型，有相同的最大长度和存储需求。