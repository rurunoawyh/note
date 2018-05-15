### excel公式
 ####场景
     给定excel表格，取表格某几列的值对应数据库表的字段。
     ="insert into biz_component_relation values("&INDIRECT(ADDRESS(ROW(),15))&",200009,'"&INDIRECT(ADDRESS(ROW(),13))&"','space','space','"&INDIRECT(ADDRESS(ROW(),14))&"','device','Xi_JACE_Meter_Water','topology','wyh','"&INDIRECT(ADDRESS(ROW(),4))&"→"&INDIRECT(ADDRESS(ROW(),11))&"',NOW(),NULL,'n')"
     
  #### 表达式
   * NOW() , COLUMN() 函数
         
        | 公式 | 结果 | 说明 |
        |------|:----:|:----:|
        |=ROW()|  1   | 去当前行号|
        |=ROW(A1)| 1  | 取A1所在的行号 |
        |=ROW(B3:B14)|3|以垂直数组形式返回B3:B14的行号|
        |=ROWS(B3:B14)|12|返回B3:B14单元格的行数|
        
   * ADDRESS() 函数
        
        | 公式 | 结果 | 说明 |
        |------|:----:|:----:|
        |=ADDRESS(1,1)|  $A$1| 返回1行1列单元格的地址|
       
   * INDIRECT() 函数
        
        =INDIRECT("A2") 返回A2单元格的内容