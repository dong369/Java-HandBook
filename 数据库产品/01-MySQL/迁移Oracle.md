从oracle 11g迁移至mysql 5.7，其中的一项主要工作就是对代码中的sql进行改写。这里针对两个库的不同点做一下总结，以备后查。

 `nvl(xx, 0)` ==> `coalesce(xx, 0)`； 说明：返回第一个非空值。

`to_char(xx)` ==> `cast(xx as char)`； 说明：转换为char类型

`to_char(xx,'yyyymmdd')` ==> `date_format(xx, '%Y%m%d')`；说明：日期格式化，date_format具体参数查询文档

`to_char(xx,'yyyyq')` ==> `concat(date_format(xx, '%Y'), quarter(xx))`； 说明：mysql date_format无法格式化季度，需要借助quarter函数

`to_number(xx)` ==> `cast(xx as unsigned integer)`；说明：转换为数字类类型，unsigned integer为无符号整数

`sysdate` ==> `now()`；说明：获取当前时间

`decode(cond, val1, res1, default)` ==> `case when cond = val1 then res1 else default end`； 说明：根据cond的值返回不同结果

`trunc(xx, 2)` ==> `convert(xx, decimal(6,2))`；说明：保留2位小数

`wm_concat(xx)` ==> `group_concat(xx)`
说明：列转行

`over()` ==> `无`
说明：别想了，mysql没有开窗函数，代码实现吧

oracle与mysql之语法的区别：
`connect by…start with` ==> `无`；说明：别想了，mysql没有递归查询，代码实现吧

`rownum` ==> `limit`；说明：分页

`'a'||'b'` ==> `concat('a', 'b')`；说明：字符串拼接

`select xx from (select xx from a)` ==> `select xx from (select xx from a) t1`；说明：from后的子查询必须有别名

`nulls last` ==> `无`；说明：mysql排序时，认为null是最小值，升序时排在最前面，降序时排在末尾

`group by (a, b)` ==> `group by a, b`；说明：mysql group by 字段不能加括号

`begin end;` ==> `begin; commit;`；说明：mysql事务控制begin后需要加分号执行，提交使用commit。P.S.禁止在sql中进行事务控制

`select 1, 1 from dual`
 `union`
 `select 1, 1 from dual`
 ==>
 `select 1 as a, 1 as b from dual`
 `union`
 `select 1 as a, 1 as b from dual`

 说明：mysql的union查询的每个字段名必须不同

**mapper.xml:**
 `<selectKey resultType="java.lang.Integer" keyProperty="id" order="BEFORE" >`
 `select seq_VEM_ORG.nextval from dual`
 `</selectKey>`
 `insert into VEM_ORG (ID, NAME)`
 `values (#{id,jdbcType=DECIMAL}, #{name,jdbcType=VARCHAR})`
 ==>
 `<selectKey resultType="java.lang.Integer" keyProperty="id" order="AFTER" >`
 `SELECT LAST_INSERT_ID()`
 `</selectKey>`
 `insert into vem_org (NAME)`
 `values (#{name,jdbcType=VARCHAR})`

 说明：oracle使用sequence，在插入新值前查询下一个id。mysql使用自增主键，插入新值时无须显示为id赋值。在插入新值后通过SELECT LAST_INSERT_ID()可获取最新插入的值的id。

