# Note on sql #

## 重启与停止服务 ##
    > net stop mysql;
    > net start mysql;
## 创建用户 ##
    > mysql -uroor -proot
    > create user 'cgzhang'@'%' identified by 'cgzhang';
    > grant all privileges on *.* to 'cgzhang'@'%' with grant option;
    # or
    > use mysql;
    > update user set host='%' where user='root';
    > use mysql;
    > select host, user from user;

## 修改账号密码 ##
    # 忘记密码
	## 首先cmd进入mysql\bin文件夹
	> mysqld -- skip-grant-tables
	> mysql;
	> use mysql;
	> select user, host, password from user;
    > set password for 'cgzhang'@'%' = password('mypass')
    # or
    > set password = password('mypass')
## 基本用法 ##
	
### 创建表 ###
    > create table ecoindex
      (
      	 deliverydate text(11),
    	 external_employment float(11,3),
    	 external_unemployment float(7,3),
    	 external_rate float(7,3)
      ) engine = InnoDB default charset = utf8; 
    
    > load data infile 'C:/Users/Franc/ ?> Desktop/Dir/Competitions/# Google/> > data/economicsIndex.csv' into table ecoindex
    > fields
    > terminated by ','
    > enclosed by '\"'
    > escaped by '\''
    > lines
    > terminated by '\r\n';

如果出现Error 1290则需要在my.ini文件中添加：

    > [mysqld]
    > secure_file_priv='';

表修改操作

- 设置索引 
    alter table xxx add index index_name(columns\_list)

- 修改表名
	alter table xxx rename yyy

- 修改字段
	alter table xxx modify/change/add/drop




## sqlalchemy ##

### 连接 ###
    host, port, db = 'localhost','3306','data_competitions'
	user, pwd, encoding = 'root','root', 'utf8'
	engine = create_engine(
        f'mysql+pymysql://{user}:{pwd}@{host}:{port}/{db}?charset={encoding}',
        echo=False)
### 执行原生sql语句 ###
	db = engine.connect()
	sql = 'select count(*) from train_sample'
	db.execute(sql).fetchall()
	# or
	Session = sessionmaker(bind = engine)
	session = Session()
	a = session.execute(sql).fetchall()
### pandas ###
	import pandas as pd
	train = pd.read_sql_table('train_sample', engine)
	sql = 'select * from train_sample where device_browser=7;'
	train = pd.read_sql_query(sql,engine)
## Pymysql ##
### 连接 ###
    import pymysql
	conn = pymysql.connect(host, port, user, pwd, db, encoding)
	cur = conn.cursor()
	cur.execute(sql)
	train = cur.fetchall()
	cur.close()
	conn.close()
