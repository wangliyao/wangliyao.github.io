## 使用postgre导出csv文件
* 使用命令 COPY table_name TO '/Users/user/table_name.csv' WITH csv; 
* COPY ads TO '/Users/wangliyao/Downloads/ads.csv'(DELIMITER E'\t', FORMAT CSV, NULL '', ENCODING 'UTF8');
* mac excel 打开乱码问题 iconv -f UTF8 -t GB18030 a.csv >b.csv
