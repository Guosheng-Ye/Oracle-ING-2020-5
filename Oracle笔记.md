##  第3章 初识Oracle 
#### 3.3 SQL Plus 基本命令介绍
-   >连接命令
    -   **CONNECT**
        -   作用：连接到数据库
        -   语法：```connect + 用户名[/口令] + @SID(数据库实例名)  + [as 何种身份]```
    -   **DISCONNECT** 
        -   作用：断开连接
        -   语法：```disconnect ```
-   >文件操作命令
    -   **START**
        -   作用：读取文件中的内容到缓冲区，然后再SQL-PLUS中跑
        -   语法：```strat {url |  file_name}```
        -   例：```strat d:\xx\xxx\test.sql``` 
        -   >结果：执行test.sql
    -   **GET**
        -   作用：读取文件内容到缓冲区
        -   语法：```get [file] file name [list | nolist] ```
        -   说明：
            -   file_name :表示一个指定文件，将该文件的内容读入SQL-PLUS缓冲区
            -   list\nolist：列出\不列出缓冲区的语句
        -   例：```get d:\db\test.sql```
        -   >结果：SQL-PLUS显示文件中的内容（默认 list 显示），不执行语句
    -   **SPOOL**
        -   作用：将SQL-PLUS中的输出结果复制到指定的文件中，或将查询结果发送到打印机中，直到命令：spool off
        -   语法：```spool [ file_name | create | replace | append | off | out ]```
        -   说明
            -   create：创建一个指定的file_name文件
            -   replace：如果指定文件存在，换掉
            -   append：内容追加形式
            -   off：停止输出结果，并关闭该文件
            -   out：启动该功能，将SQL-PLUS中的输出结果复制到file_name文件中
        -   例：```spool d:\db\test.txt create```
        -   >结果：对应位置create名为test的文本文件，遇到spool off 后，存储了两条命令间的所有命令与结果
    -   **SAVE**
        -   作用：将当前缓冲区的内容保存到文件中，即使缓冲区的文件被覆盖，也保留前面的执行语句
        -   语法：```save | file | file_name |create |replace| append ```
        -   说明：同spool
        -   例：```save d:\db\test2.txt append```
        -   >结果:对应位置append名为test的文本文件
        -   **spool与save的比较**：edit在对上述两个TXT文件打开编辑时，因为SQL缓冲区中保存最后一条执行的sql语句，save命令就是将这一条命令存入文件。spool 则存储到spool off前的所有
    -   **EDIT**
        -   作用：将指定文件调入编辑器进行编辑
        -   语法：```edit file_name ```
        -   说明：将file_name 指定文件的内容读入缓冲区
-   >快速编辑SQL语句命令
    -   **LIST**
        -   作用：显示缓冲区内容
        -   语法：```list | n|*last|```
        -   说明
            -   n：缓冲区的第n行
            -   *：缓冲区的所有行
            -   last：缓冲区最后一行
        -   延伸：```list m n || list * n || list n *```
    -   **RUN**
        -   作用：执行当前缓冲区的命令
        -   语法：```run <=> /```
        -   说明：使用run会显示执行的语句，而 / 不会
    -   **N(行号)**
        -   作用：指定缓冲区的第N行为当前行
        -   语法：```N```
    -   **APPEND**
        -   作用：添加文本到缓冲区当前行尾
        -   语法：```append [text]```
    -   **CHANGE**
        -   作用：对行内容进行替换
        -   语法 ```change /old/new``` 
    -   **DEL**
        -   作用：删除指定行
        -   语法：```del | n | * | last |```
        -   说明：同list
    -   **INPUT**
        -   作用：在当前行后插入一行
        -   语法：```input [text]```
-   >格式化命令
-   >变量的使用
-   >其他命令
  
++++++++++++++++++++++++++++++++++这里是分割线++++++++++++++++++++++++++++++++++++++++
##  第4章 Oracle11g 体系结构
#### 4.1 物理存储结构
>存储在磁盘中的操作系统文件所组成其他文件
-   >**控制文件：Contraol Filse（*.ctl）**
    -   作用：
        -   维护数据库的全局物理结构
        -   通过scn(同步数据)操持数据库的一致性，用以支持数据库成功启动和运行
    -   内容：
        -   数据库名字和标识符(DBID)
        -   数据库创建的时间戳
        -   表空间的名字
        -   检查点信息
        -   回滚段的开始和结束
        -   联机重做日志的归档信息
        -   数据文件和联机重做日志文件的位置和名字
        -   当前联机重做日志文件的sequence号码
        -   备份信息
    -   特点：
        -   二进制文件
        -   创建数据库时，同时就提供了与之对应的控制文件
        -   当数据库发生任何物理变化都将被自动更新，换言之控制文件的内容只能够由Oracle本身来修改
        -   因其重要性有多个镜像，任何时候都将保持内容一致
-   >**数据文件：Data Files（*.dbf）**
    -   作用：
        -  用来存储数据库中的全部信息
    -   内容：
        -   表中的数据
        -   索引数据
        -   数据字典定义
        -   回滚事务所需要信息
        -   存储过程、函数和数据包的代码
        -   用来排序的临时数据
    -   特点：
        -   每个oracle数据库都有一个或多个物理数据文件
        -   一个数据文件只能与一个数据库相关联
        -   可以对数据文件设置一些特性，在数据库空间用完的情况下可以自动扩展
        -   一个或多个数据文件构成了一个数据库存储的逻辑单元--表空间(table space)
-   >**参数文件：Parameter File（*.ora）**
    -   作用：用来记录关于数据库和实例的参数配置，包括数据库名称和控制文件所在位置
    -   内容：
        -   设置内存大小
        -   设置要使用的数据库和控制文件
        -   设置检查点
        -   设置数据库的控制结构
        -   非强制性后台进程的初始化
    -   分类：
        -   Server ParameterFile、spfile 
            -   二进制的服务器参数文件
            -   不能直接修改，可以通过alter语句进行修改，根据修改时的参数(scope=spfile | memory | both)确定修改的参数新值是否立即生效
            -   通过名为spfile<SID>.ora(优先级最高)，或全局的spfile.ora
        -   Paramater File、pfile
            -   文本类型的参数文件
            -   可以直接修改
            -   通常名为init<SID>.ora 或 全局的 init.ora
        -   选用优先级：spfile<SID>.ora > spfile.ora > init<SID>.ora > init.ora
-   >**重做日志文件：RedoLog Files（*.log）**
    -   作用：记录数据库所做的全部变更（如增加、删除、修改），以便在系统发生故障时，用他对数据进行恢复
    -   特点：
        -   保证数据库安全，是进行数据库备份与恢复的重要收手段
        -   日志文件受损，数据库无法正常启动
        -   为确保日志文件的安全，对日志文件进行镜像
        -   日志文件和镜像文件构成日志文件组
        -   日志文件组循环使用
-   >**关系**  
    -   参数文件指向控制文件
    -   控制文件存储数据文件和日志文件信息
-   >**其他文件**
    -   **跟踪文件(Trace file)**：存放着后台进程的警告和错误信息，每个后台进程有相应的跟踪文件
    -   **警告文件(Alert file)**：由连续的消息和错误组成，可以看到Oracle内部错误，块损错误等
    -   **备份文件(Baackup file)**：包含恢复数据库结构和数据文件所需要的副本
    -   **口令文件(Password file)**：存放用户口令的加密文件
-   >**借助数据字典查看数据库物理存储结构**
    -   ```desc V$controlfile```：描述controlfile的内部定义  
    -   ```selcet * from V$controlfile;```//查看控制文件基本信息
```
         STATUS//为空代表控制文件正常
         -------     
         NAME 
         --------------------------------------------------------------------------------
         IS_ BLOCK_SIZE FILE_SIZE_BLKS      
         ---------------------------         
         D:\ORACLE_HOME\ORADATA\ORCL\CONTROL01.CTL    
         NO       16384            594                 
         D:\ORACLE_HOME\FLASH_RECOVERY_AREA\ORCL\CONTROL02 CTL  
         NO       16384            594             
         STATUS   
         -------     
         NAME 
         --------------------------------------------------------------------------------
         IS_ BLOCK_SIZE FILE_SIZE_BLKS  
         --------------------------- 
```
-   
    -   ```alter database backup controlfile to trace;```查看控制文件脚本   
        -   ```show parameter user_dump_dest``` 显示
```
NAME                                 TYPE        VALUE   
------------------------------------ ----------- ------------------------------ 
user_dump_dest                       string      d:\oracle_home\diag\rdbms\orcl 
                                                 \orcl\trace    
```
-   
    -   ```desc dba_data_files;``` 常见数据文件字典：静态数据字典
    -   ```select * from dba_data_files;```查看数据文件基本信息
    -   ```select * form V$datafile```查看数据文件动态信息
```
NAME                                 TYPE        VALUE                          
------------------------------------ ----------- ------------------------------ 
user_dump_dest                       string      d:\oracle_home\diag\rdbms\orcl 
                                                 \orcl\trace                    
SQL> spool off
SQL> desc dba_data_files;
 名称                                      是否为空? 类型
 ----------------------------------------- -------- ----------------------------
 FILE_NAME                                          VARCHAR2(513)
 FILE_ID                                            NUMBER
 TABLESPACE_NAME//所在表空间名                       VARCHAR2(30)
 BYTES//大小                                        NUMBER
 BLOCKS                                             NUMBER
 STATUS                                             VARCHAR2(9)
 RELATIVE_FNO//相对文件名                            NUMBER
 AUTOEXTENSIBLE//扩展性                             VARCHAR2(3)
 MAXBYTES                                           NUMBER
 MAXBLOCKS                                          NUMBER
 INCREMENT_BY                                       NUMBER
 USER_BYTES//实际可用空间                            NUMBER
 USER_BLOCKS                                        NUMBER
 ONLINE_STATUS                                      VARCHAR2(7)
```

```
SQL> select file_Name,file_id,tablespace_name,status,online_status from dba_data_files;//查看基本数据
//表空间各不相同
FILE_NAME                                                                       
--------------------------------------------------------------------------------
   FILE_ID TABLESPACE_NAME                STATUS    ONLINE_                     
---------- ------------------------------ --------- -------                     
D:\ORACLE_HOME\ORADATA\ORCL\USERS01.DBF                                         
         4 USERS                          AVAILABLE ONLINE                      
                                                                                
D:\ORACLE_HOME\ORADATA\ORCL\UNDOTBS01.DBF                                       
         3 UNDOTBS1                       AVAILABLE ONLINE                      
                                                                                
D:\ORACLE_HOME\ORADATA\ORCL\SYSAUX01.DBF                                        
         2 SYSAUX                         AVAILABLE ONLINE                      
                                                                                

FILE_NAME                                                                       
--------------------------------------------------------------------------------
   FILE_ID TABLESPACE_NAME                STATUS    ONLINE_                     
---------- ------------------------------ --------- -------                     
D:\ORACLE_HOME\ORADATA\ORCL\SYSTEM01.DBF                                        
         1 SYSTEM                         AVAILABLE SYSTEM                      
                                                                                
D:\ORACLE_HOME\ORADATA\ORCL\EXAMPLE01.DBF                                       
         5 EXAMPLE                        AVAILABLE ONLINE                      
                                                                                
D:\DB\ORACLEDATA\USER_DATA.DBF                                                  
         6 USER_DATA                      AVAILABLE ONLINE                      
                                                                                

FILE_NAME                                                                       
--------------------------------------------------------------------------------
   FILE_ID TABLESPACE_NAME                STATUS    ONLINE_                     
---------- ------------------------------ --------- -------                     
D:\DB\ER\ER.DBF                                                                 
         7 ER                             AVAILABLE ONLINE                      
                                                                                
```
```
SQL> desc v$datafile;//动态数据字典
 名称                                      是否为空? 类型
 ----------------------------------------- -------- ----------------------------
 FILE#                                              NUMBER
 CREATION_CHANGE#                                   NUMBER
 CREATION_TIME                                      DATE
 TS#                                                NUMBER
 RFILE#                                             NUMBER
 STATUS                                             VARCHAR2(7)
 ENABLED                                            VARCHAR2(10)
 CHECKPOINT_CHANGE#//最近一次检查点修改号             NUMBER
 CHECKPOINT_TIME                                    DATE
 UNRECOVERABLE_CHANGE#                              NUMBER
 UNRECOVERABLE_TIME                                 DATE
 LAST_CHANGE#                                       NUMBER
 LAST_TIME                                          DATE
 OFFLINE_CHANGE#                                    NUMBER
 ONLINE_CHANGE#                                     NUMBER
 ONLINE_TIME                                        DATE
 BYTES                                              NUMBER
 BLOCKS                                             NUMBER
 CREATE_BYTES                                       NUMBER
 BLOCK_SIZE                                         NUMBER
 NAME                                               VARCHAR2(513)
 PLUGGED_IN                                         NUMBER
 BLOCK1_OFFSET                                      NUMBER
 AUX_NAME                                           VARCHAR2(513)
 FIRST_NONLOGGED_SCN                                NUMBER
 FIRST_NONLOGGED_TIME                               DATE
 FOREIGN_DBID                                       NUMBER
 FOREIGN_CREATION_CHANGE#                           NUMBER
 FOREIGN_CREATION_TIME                              DATE
 PLUGGED_READONLY                                   VARCHAR2(3)
 PLUGIN_CHANGE#                                     NUMBER
 PLUGIN_RESETLOGS_CHANGE#                           NUMBER
 PLUGIN_RESETLOGS_TIME                              DATE
```
```
SQL> show parameter spfile[pfile];//参数文件

NAME                                 TYPE        VALUE                          
------------------------------------ ----------- ------------------------------ 
spfile                               string      D:\ORACLE_HOME\PRODUCT\11.2.0\ 
                                                 DBHOME_1\DATABASE\SPFILEORCL.O 
                                                 RA   
```
```
SQL> desc v$log;//显示重做日志文件
 名称                                      是否为空? 类型
 ----------------------------------------- -------- ----------------------------
 GROUP#                                             NUMBER
 THREAD#//所在线程号                                 NUMBER
 SEQUENCE#//编号                                    NUMBER
 BYTES                                              NUMBER
 BLOCKSIZE                                          NUMBER
 MEMBERS//当前组的几个成员文件                        NUMBER
 ARCHIVED//是否归档        已写满的数据是否保存       VARCHAR2(3)
 STATUS                                             VARCHAR2(16)
 FIRST_CHANGE#                                      NUMBER
 FIRST_TIME                                         DATE
 NEXT_CHANGE#                                       NUMBER
 NEXT_TIME                                          DATE
```

-   
    -   ```show parameter spfile```查看spfile位置
    -   ```show parameter pfile```查看pfile位置
    -   ```create spfile from pfile```Spfile与pfile可以互相转换(root)权限
    -   ```create pfile from spfile```

-   
    - ```select * from v$logfile```查看日志文件基本信息
    - ```select * from v$log```   
++++++++++++++++++++++++++++++++++这里又是一条分割线++++++++++++++++++++++++++++++++++++

#### 4.2 逻辑存储结构
>是数据库利用逻辑概念描述数据库内部数据组织和管理的概念
-   >**TABLESPACE**
    -   作用：是oracle数据库中数据的逻辑组织单位，数据库逻辑上由一个或多个表空间组成
    -   特点：
        -   某一时刻只能属于一个数据库
        -   由一个或多个数据文件组成
        -   可进一步划分成逻辑存储单元
    -   查看表空间信息：
        -   ```select * from dba_tablespace```
-   >**SEGMENT**
    -   作用：是用户建立的数据库对象的存储单元
    -   内容：
        -   表
        -   表分区
        -   被索引组织的表
        -   索引分区
        -   簇
        -   索引
        -   还原段
        -   临时段
-   >**EXTENT**
    -   作用：用来为段存储数据的逻辑上**连续**的数据块的集合
    -   特点：
        -   磁盘空间分配的最小单位  
-   >**BLOCK**
    -   作用：oracle以数据块整数倍读取内容
    -   特点：
        -   数据库中最小、最基本的逻辑存储单元
        -   oracle从磁盘读写的最小单元
        -   oracle的块常见大小可以是2KB,8KB,16KB等
        -   快的标准大小由初始参数```db_block_size```指定，具有标准大小的块成为标准块
  
|         |            |        |     |      |
|  ----   |  ----      |  ----- |---  | ---  |
|-------->|--->        | SEGMENT-->|  EXTENTS -->|BLOCKS|
|   DB--->|   TBS-->   | SEGMENT-->|  EXTENTS -->|BLOCKS|
|-------->|---->       | SEGMENT-->|  EXTENTS -->|BLOCKS|          
  
-   >**代码操作**
    -   **显示数据文件对应表空间的信息**
        -   ```select file_id,tablespace_name,file_name from dba_data_files;```
    -   **创建表并分配表空间**
        -   ```create table t1(id number(3),tname varchar2(20)) tablespace users;```
        -   该数据对象在表空间对应的数据文件(*.dbf)上创建
    -   **通过段查看上述操作信息**
        -   ```desc dba_segments;```通过数据字典获取段的有关信息
        -   ```select segment_type,tablespace_name,header_file,header_block,extents,initial_extent from dba_segments where segment_name='T1';```(数据字典中对象名默认大写)
        -   结果：
            -   segment_type:table
            -   tablespace_name:users
            -   header_file:4（file_id）
            -   header_block:522
            -   extents:1
            -   initial_extent:65536(B)
    -   **查看区的相关内容**
        -   ```desc dba_extents;```
        -   查看t1段的区的有关内容
            -   ```select extent_id,file_id,block_id,blocks from dba_extents where segment_name='T1';```
            -   结果：
                -   extent_id:0
                -   file_id:4
                -   block_id:520
                -   blocks:8
        -   查看orcl下标准数据库的大小
            -   ```show parameter db_block_size;```
                -   结果：
                    -   name：db_block_size
                    -   type:integer
                    -   value:8192(B)===>8192 * 8 = 65536
        -   查看是否建立的是连续的数据块
            -   ```create table t2(id number(3)) tablespace users;```创建新表，放在user01.dbf上
            -   查看t2段的区的有关内容
                -   ```select extent_id,file_id,block_id,blocks from dba_extents where segment_name='T2';```
                -   结果：
                    -   extent_id:0
                    -   file_id:4
                    -   block_id:528(证实区是连续的数据块集合)
                    -   blocks:8 
#### 4.3 内存结构
>I/O瓶颈：数据库的数据操作需要借助CPU完成，而On chip cache：10sec，但是数据库读取这10s的数据需要好几年
为了解决这一问题，二者间加入内存，内存读取10s的数据需要1.5h。工作时先把磁盘中中的数据读入(预先读入按block读取)到内存中，
内存再读入到cpu中(按字节读取)、
-   >**系统全局区(System Global Area,SGA)**
    -   **Database Buffer Cache** <数据库数据文件读入数据到内存时，数据缓存在这里>
        -   >SGA的主要成员，用来存放读取自数据文件的数据块副本，或是使用者曾经处理过的数据。
        -   >数据库缓冲区又称用户数据高速缓冲区，为所有与该实例连接的用户进程所共享，采用最少使用算法(LRU)管理
        -   >数据库缓冲区的容量受物理容量限制
        -   >数据库缓存的大小可以由服务器文件spfile.ora文件中的DB_cacahe_size参数指定，该参数可以直接以K字节或M字节为单位设置数据库缓存的大小
        -   Default Block Size <针对标准数据块，从数据文件中以8kb为单位读入这里>
        -   Other Block Size <读入非标准大小的数据>
    -   **Redo Log Buffer** <缓存联机重做日志的操作>
        -   >联机重做日志文件用于记录数据库的更改，读数据库进行修改的事务(Transaction)在记录到重做日志文件前都必须放在Redo Log Buffer中
        -   >重做日志缓冲区是专为此开辟的一块内存区域，RLB的内容将被LGWR后台进程写入重做日志文件
        -   >大小由LOG_BUFFER确定
    -   Large Pool <数据库实例恢复与备份的区域>
        -   提供一个大的缓冲区供数据库的备份与恢复操作使用
        -   为共享服务器模式与多个数据库间的联机事务分配内存等作用
        -   共享服务器模式下的会话信息(可选)
        -   大小由参数large_pool_size决定，是SGA的可选区域
    -   Java Pool <支持Java应用>
        -   保存jvm中特定会话的java code和数据，用于在数据库中支持java的运行，大小由java_pool_size决定
    -   Streams Pool <针对IOstraam>
    -   **Shared Pool**
        -   **Data Dictionary Cache** <无数据字典，orcl实例将无法正常运行>
            -   >包含表、字段和其他对象的定义与权限
        -   Control Structure <控制数据状态>
        -   Reserved Pool <查询结果，解析结果的缓存>
        -   **Libary Cache** <提供语法解析,并缓存解析结果>
            -   >系统解析SQL命令和PL/SQL程序块，保存解析后的结果以备用
            -   PL/SQL Procedure Part
            -   Shared SQL Area
        -   >大小取决于参数SHARED_POOL_SIZE，它是以字节为单位，用户必须将这个值设置足够大，以确保有足够的可用空间来装载和存储PL/SQL语句块和SQL语句
    -   Fixed SGA
    -   **总结**
        -   SGA区是由oracle分配的共享内存结构，包含一个数据库实例共享的数据和控制信息
        -   当多个用户同时连接到同一个实例时，SGA提供多个用户共享，所以SGA又称为共享全局区
        -   实例启动时分配，关闭时释放
-   >**程序全局区(Program(Process) Global Area,PGA)**
    -   **Dedicated connections(专用服务器模式)**
        -   DB <--> Server Process <--> PGA
            -   PGA:
                -   SQL Work Areas
                    -   >由PGA分配，用于一些基于内存的操作，列如排序，哈希，和位图合并，如果基于内存的操作(sort,hash,merge,group by)的数据超出了work area的空间，oracle自动会将数据分为若干个小的数据片，然后在work area中分别处理每个数据片，其他暂时还未处理的数据片则放在temp表空间中，以待处理
                    -   >一般而言，work area越大，需要消耗大量内存的操作性能越好
                    -   Sort Area(排序操作，越大排序越快)
                    -   Hash Area(多表查询，hash joined)
                    -   Bitmap Merge Area(位图处理)
                -   Session Memory(UGA)(用户会话数据)
                    -   用于保存会话变量(如登录信息)和其他与会话相关信息的内容
                    -   在共享服务器模式下，在SGA的shared pool 或 large pool中分配
                -   Private SQL Area
                    -   >保存一些解析语句的信息和会话相关的信息，当一个服务器进程执行sql或者pl/sql语句时，服务器进程使用private sql area存储绑定变量，查询执行状态等信息
                    -   >Private sql area 可分为以下部分
                        -   Persistent Area(涉及游标变量绑定、类型转换)
                            -   包含查询执行的状态信息，如全表扫描在当前时间返回的行数
                        -   Runtime Area(涉及动态执行数据)
                            -   包含了绑定变量，当sql语句在执行的时候将绑定变量的值赋给语句，只有在游标关闭时persisten area才会释放
                            -   在共享服务器模式下，部分Private sql area在SGA中分配
    -   **Shared Server connections(共享服务器模式)**
        -   DB <--> Server Process <--> Memery
            -   Memory
                -   Shared pool or Large pool
                    -   Session Memory
                    -   Persistent Area
                -   PGA 
                    -   SQL Work Areas
                    -   Runtime Area
    -   **总结**
        -   PGA是单个oracle进程使用的内存区域，不属于实例的内存结构。它含有单个进程工作时需要的数据和控制信息
        -   PGA是非共享的，只有服务器进程本身才能访问它自己的PGA区
        -   创建进程时分配，终止进程时回收
        -   ```show parameter target;```模糊查询，其中包括显示可分配内存实例，参数0：可自动管理的默认分配而非未分配
        -   ```show parameter area_size;```模糊查询，显示*.area_size信息
-   >**用户全局区(User Global Area,UGA)**(具体依附在哪个GA中，看服务器进程模式)
#### 4.4 进程结构
-   >**连接与会话**
    -   Connnection：连接是客户端进程和实例间的一种物理通讯链路
    -   Session:会话是数据库实例内存中描述登录到数据库当前用户状态的逻辑实体
    -   一个用户可以拥有多个会话，某个会话中的提交不会影响其他会话的事务
-   >**专用服务器进程**
    -   >一个客户端能且仅能连接一个服务器进程，服务器进程也只能在客户端连接持续期间为其专一的客户端进程服务
-   >**共享服务器进程**
    -   >客户端应用通过网络连接到服务器进程适配器而不是服务器进程，适配器从连接客户端处收到请求后将其放在大对象池中的请求队列，当第一个可用的共享服务器进程从请求队列处获得请求并进程处理，然后将处理完的结果放到适配器的响应队列中，接着适配器进程监视该队列并将结果转发到客户端
-   >**必备后台进程**
    -   **数据库写入程序**
        -   DBwn(DBw1、DBw2....)   
            -   负责将数据块缓冲区内容变动过的数据块写回磁盘内的数据文件
            -   DBwn可有多个
            -   启动时间(万不得已)
                -   检查点发生
                -   脏数据块达到极限
                -   没有空间
                -   对表空间的操作
                -   表的删除(释放也是写的一种)
    -   **日志写入程序**
        -   LGWR
            -   负责将重做日志文件缓冲区内变动记录循环写回磁盘内的重做日志文件，该进程会将所有数据从重做日志文件缓存中写入到现行的在线重做日志文件中
            -   启动时间(能懒就懒)
                -   事务处理进行提交
                -   重做日志文件缓存达到一定的量
                -   一定时间间隔内
                -   在DBWn写之前
    -   **检查点进程**
        -   CKPT
            -   通过checkpoint事件确保缓冲区内的数据定期被写入数据文件
            -   设置恢复时间点，缩短数据库的重新激活时间
            -   更新数据文件头和控制文件同步数据
            -   启动时间
                -   在检查点发信号给DBWn
                -   使用检查点信息更新数据文件的标头
                -   使用检查点信息更新控制文件
    -   **系统监控进程**
        -   SMON
            -   主要职责是重新启动系统。在出现故障实例的情况下，SMON负责将重新启动系统，执行崩溃恢复，实例恢复，未完成的事务回滚
            -   例程恢复
                -   回滚重做日志文件中的更改
                -   打开数据库供用户访问
                -   回退未提交的事务处理
            -   合并空闲空间
            -   回收临时段
    -   **进程监控器**
        -   PMON
            -   主要职责是监控服务器进程和注册数据库服务
            -   监控服务器进程
            -   服务器进程失败、释放这些进程的资源
            -   使用oracle监听器注册数据库事务
-   >**可选后台进程**
    -   **归档程序**
        -   ARCn   
            -   可选的后台进程
            -   设置ARCHIVELOG模式对自动归档联机重做日志
            -   保留数据库的全部变更记录
-   >**操作**
    -   ```show parameter sessions;```
    -   ```show parameter processes;```
    -   上述中sessions = 1.5 proceses + 后台进程数
    -   查看后台进程信息
        -   ```desc V$bgprocess;```
            -   查看正在运行的实例进程
                -   ```select paddr,name,descripttion from v$bgprocess where paddr<>'00';```
#### 4.5 数据库与实例
-   >**概念**
    -   数据库 
        -   >物理操作系统文件或磁盘的集合 
        -   =Redolog Files  +
        -   Control Files  +
        -   Data Files     +
        -   Temporary Files
    -   实例
        -   >数据库启动时，系统首先在服务器内存中分配系统全局区(SGA),构成了oracle的内存结构，然后启动若干个常驻内存的操作系统进程，即组成了oracle的进程结构，内存区域和后台进程合称为一个oracle实例
    -   数据库名
        -   >指一个数据库的标识，如人的身份证号一样，用参数DB_NAME表示，如果一台机器上装了多个数据库，那么每一个数据库都有一个数据库名，数据库名是在安装数据库、创建新的数据库、创建新的数据库控制文件、修改数据库结构、备份与恢复数据库时都需要使用的、
    -   数据库域名
        -   >在分布式数据库系统中，不同版本的数据库服务器之间，不论运行的操作系统是unix还是windows，各服务器之间都可以通过数据库链路进行进程复制，数据库域名主要用于oracle分布式环境中的复制，数据库域名存在于参数文件中，参数为db_domain
        -   >全局数据库名 = 数据库名 + 数据库域名
    -   数据库服务名
        -   >从oracle9i版本开始，引入新参数，即数据库服务名，参数名为service_name。如果数据库有域名，则数据库服务名就是全局数据库名，否则，数据库服务名与数据库名相同
    -   实例名(SID or Instance_name)
        -   >oracle System IDentification 使用于和操作系统进行联系的标识，数据库和操作系统间的交互作用用的是数据库实例名，属于操作系统的环境变量，写入注册表。
        -   >在oracle内部的参数文件中实例名该参数为instance_name
        -   oracle_sid必须与instance_name的值一致
-   >**实例与数据库的关系**
    -   在一般情况下， 数据库名和实例名是一对一的关系，但如果在oracle并行服务器架构(oracle RAC,Real Application Clusters)中，数据库名和实例名是一对多的关系
    -   实例可以在任何时间点装载和打开一个数据库
    -   访问数据库的时候是通用实例访问到数据库中的数据
    -   Oracle Server  =  Oracle Instance  +  Oracle Database
#### 4.6 数据字典
>数据字典是Oracle存放有关数据库信息的地方，其用途是用来描述数据
>数据字典表的所欲这是sys用户，其数据字典表和数据字典视图都被保存在system表空间中。除了sys用户可以修改AUD$表外，任何用户(包括DBA)都不能直接修改数据字典，但可以查询
>当用户执行DDL语言(如create table)或DML语言(如alter table、drop table)由oracle负责对相应的数据字典更新
-   >**数据字典主要保存的信息**
    -   各种方案定义的信息，如表、视图、索引、存储过程、序列、触发器等各种对象
    -   存储空间的分配信息
    -   安全信息，如账户、权限、角色、完整性约束
    -   例程(实例)运行时的性能和统计信息
    -   其他数据库本身的基本信息
-   >**分类**
    -   **静态数据字典**
        -   >静态数据字典是指在用户访问数据字典时内容不发生改变，这类数据字典主要是由表和视图组成，应该注意的是，数据字典中的表是不能直接被访问的，但是可以访问数据字典视图
            -   user_*(该用户对象的信息)
            -   all_*(记录用户对象的信息及被授权访问的对象信息，该用户可以访问的所有对象的信息)
            -   dba_*(数据库实例的全部数据库对象信息)
    -   **动态数据字典**
        -   >oracle包含了一些潜在的由系统管理员如sys维护的表和视图，由于当前数据库运行的时候他们会不断进行更新，所以称为动态数据字典
            -   V$(记录与数据库活动相关的性能统计动态信息)
            -   GV$(记录分布式环境下的所有实例的动态信息)
            -   
++++++++++++++++++++++++++++++++++这还是一条分割线++++++++++++++++++++++++++++++++++++
##  第五章 用户与权限
#### 5.0 Oracle安全控制机制概述
-   >**数据库安全性的定义**
    -   保护数据以防止不合法的使用所造成的数据泄露、更改或破坏
        -   防止**非法用户**对数据库的访问
        -   防止用户的**非法操作**
-   **安全控制机制**
    -   角色管理
    -   用户权限
    -   资源限定
    -   表空间管理
    -   临时表空间
    -   缺省表空间
    -   账号的锁定
    -   授权机制
-   **用户操作的安全性条件**
    -   登录oracle必须通过身份验证
    -   数据库的具体操作执行必须有相应的权限
    -   必须是该数据库的用户或者是某一数据库角色的成员
-   >**用户身份验证**
    -   数据库验证
        -   数据库验证是指用户的账户、口令及身份鉴定都是由oracle数据库执行的验证方式
    -   外部验证
        -   用户的账户由oracle数据库管理，但是口令管理和身份鉴定由外部服务完成
    -   企业验证
        -   用户的账户由oralce数据库管理，口令管理和用户鉴定有Oracle Security Service(OSS)完成
-   >**用户权限**
    -   >用户如果要执行某项操作，必须具备相应的权限，即用户权限。可分为
    -   系统权限
        -   指对数据库系统及数据结构的操作权限，例如创建/删除用户、表、同义词、索引等
    -   对象权限
        -   指用户对数据库的操作权，如查询、更新、插入、删除、完整性约束
    -   用户权限可以被授权和收回
-   >**角色**
    -   一组用户权限和角色的集合
    -   使用角色可以简化权限管理，因为通过角色可以一次把**一组权限**授予用户，或者从**用户收回**
    -   角色分为**系统预定义角色**和**用户自定义角色**
    -   角色可以被授权和收回
#### 5.1 用户
-   >**分类**
    -   数据库管理员(DBA)
        -   安装和升级oracle服务器和应用工具
        -   数据库系统存储空间的管理
        -   数据库结构的管理。如创建、修改和删除表空间
        -   控制、监视用户对oracle数据库的访问
        -   调整和优化数据库性能
        -   制定和实施数据库的备份和恢复计划
    -   安全管理员
        -   数据库安全有关的管理工作应由安全管理员负责，小型系统中，一般由DBA兼任
    -   应用开发者
        -   设计和开发数据库应用
        -   为应用程序设计和修改数据库结构，估计存储空间的需求
        -   在应用开发期间，调整应用，指定应用的安全机制
    -   应用管理员
        -   在数据库应用交付使用后，通常需要有专人负责应用程序的日常维护工作
        -   应用管理员负责发现和解决应用程序运行中出现的错误，或者把错误信息报告给数据库管理员
    -   普通数据库用户
        -   录入、修改和删除数据
        -   生成数据报表
    -   网络管理员
        -   负责oracle网路产品的管理工作，如SQL*Net的管理
-   >**初始管理用户**
    -   SYS
        -   具有最高权限的SYADBA，拥有所有权限，拥由数据字典
    -   SYSTEM
        -   具有SYSOPER的权限，不能创建、删除数据库，但可以运行其他管理工作
    -   SYS用户可以给其他用户授予去拿先和角色，并且可授予他们转授这些权限和角色的权力  
  
-   >**创建用户**
    -   ```CREATE USER user_name(用户名)``` 
    -   ```IDENTIFIED [BY password |EXTERNALLY | GLOBALLY  AS 'external_name'](验证方式)``` 
    -   ```[DEFAULT TABLESPACE tablespace_name](默认表空间)``` 
    -   ```[TEMPORARY TABLESPACE temp_poraryspace_name](表空间配额)```
    -   ```[PROFILE profile)_name](配置文件)``` 
    -   ```[PASSWORD EXPIRE](指定口令到期)//要求用户在第一次登录时，必须修改密码``` 
    -   ```[ACCOUNT LOCK | UNLOCK](锁定 | 解锁用户)```
    -   >**新创建的用户并不能连接数据库，因为不具备```create session```权限，因此需要创建时授予，通常使用**GRANT**语句**
    -   例：使用system连接数据库，并创建用户xiaoqi
    -   >```SQL>connect system/admin```
    -   >```已连接```
    -   >```SQL>CREATE USER xiaoqi```
    -   >```IDENTIFIED BY xiaoqi0101```
    -   >```DEFAULT  TABLESPACE users```
    -   >```TEMPORARY TABLESPACE temp```
    -   >```QUOTA 20M ON users;//表空间最大使用配额```
    -   >```用户已创建```

-   >**修改用户**
    -   对创建好的用户可以使用```ALTER USER```进行修改
        -   修改密码：```ALTER USER tea IDENTIFIED BY m123;```
        -   修改用户的表空间配额：```ALTER USER test QUOTA 15M ON user_data;```

-   >**删除用户**
    -   >DROP USER user_name[cascade];
    -   若用户有对象，则需加入**cascade**,以便在删除用户前，先级联删除用户所创建的对象
    -   删除用户后，相应的表空间被释放，其他引用被删除用户对象的地方变为无效
    -   正在连接服务器的用户不能被删除
    -   具有DROP USER系统权限的用户，可以删除数据库中的其他用户
-   >**与用户相关的数据字典**
    -   **DBA_USERS**:描述数据库中的所有用户信息
    -   ALL_USERS:描述可以被当前用户看见的用户信息
    -   **USER_USERS**:描述当前用户信息
    -   DBA_TS_QUOTAS:描述用户的空间限额信息
    -   USER_TS_QUOTA,USER_PASSWORD_LIMITS:描述分配给该用户的口令配置文件参数信息
    -   USER_RESOURCE_LIMITS:描述当前用户的资源限制信息
-   >**用户锁定与解锁**
    -   锁定账户后，用户就不能再使用该账户连接到数据库。可借助此方法控制对数据库的访问，而不必删除重建用户
    -   锁定账户的原因有：某个用户暂时离开工作，某个用户永久离开工作，登录失败次数超限，DBA创建的特殊用户账户等
    -   默认新建的用户是借解锁的
    -  语法
       -  锁定用户：```ALTER USER user_name ACCOUNT LOCK;```
       -  解锁用户：```ALTER USER user_name ACCOUNT UNLOCK;```
-   >**用户口令过期**
    -   用一个临时口令更改用户的口令，并使其过期，这将强制用户第一次登录时使用该临时口令，随后需要更改才能继续使用
    -   语法： 
        -   >```ALTER USER user_name```
        -   >```IDENTIFIED BY password```
        -   >```PASSWORD EXPIRE```
-   >**管理用户会话**
    -   oracle提供了动态视图```v$session```,通过该视图可以了解当前数据库中的用户会话信息
    -   查询部分信息
        -   >```COLUMN username FORMAT A20```
        -   >```CLOIMN machine FORMAT A1```
        -   >```SQL>select sid,seria#,username,```
        -   >```status,logon_time,machine from```
        -   >```v$session where username IS NOT NULL;```
    -   终止用户会话
        -   DBA可以在需要时用```ALTER SYSTEM```终止用户会话
        -   ```ALTER SYSTEM KILL SESSION 'sid,serial#';```
        -   sid与serial#的值可以通过查询动态地图v$session得到
        -   例：终止sid为136，serial#为341的会话
            -   >```ALTER SYSTEM KILL SESSION '136,341';```
#### 5.2 权限
-   >**授予系统权限**
    -   ```GRANTE system_privilege [,...] TO {user_name[,...] | role_name[,...]| PUBLIC}[WITH ADMIN OPTION]```
    -   
        -   WITH ADMIN OPTION：用户可将获得的系统权限转授给他人，即系统权限的传递性
        -   可以将系统权限授予指定的用户或角色，执行授权的用户必须本身具有这样的权限
        -   授予了相关权限后，立即生效，不需重启或重新登录
        -   如果有了GRANT ANY PRIVILEGE权限，则代表可以授予任意的系统权限(不包括对象权限)，只有DBA才应当拥有ALTER DATABASE权限
    -   例：
        -   ```|GRANT CREATE SESSION,CREATE TABLE TO user1,user2 WITH ADMIN OPTION;```
        -   
-   >**查看用户系统权限的数据字典**
    -   ```v$pwfile_users```: 查询被授予sysdba或sysoper系统权限的用户信息
    -   ```user_sys_privs```：查询某个用户所具有的系统权限
-   >**消除系统权限**
    -   ```REMOKE system_privilege[,...] FROM {user1_name[,...] | role_name[,...] | PUBLIC;}```
    -   
    -   说明：
        -   多个管理员授予用户同一个权限后，其中一个管理员回收授予该用户的系统权限时，该用户不在拥有该权限
        -   为了回收用户系统权限的传递性(授权时使用了WITH ADMIN OPTION)必须先回收其系统权限，然后再不带WITH ADMIN OPTION授予其相应的系统权限
        -   回收系统权限不会级联，即如果一个用户获得的系统权限具有传递性(使用了WITH ADMIN OPTION)并且已经给其他用户授权，那么该用户权限回收后，其授予权限的用户的权限不受影响
-   >**授予对象权限**
    -   oracle数据库方案的对象主要是指：表、索引、视图、序列、同义词、过程、函数、包、触发器等
    -   创建对象的用户拥有该对象的所有对象权限，不需要授予。所以，对象权限的设置实际上是为对象的所有者给其他用户提供的操作该对象的某种权力的一种方法
    -   有些对象(簇、索引、触发器和数据库连接)没有对应的对象权限，他们通过系统权限控制。例如：修改簇用户必须拥有ALTER ANY CLUSER系统权限
    -   **9种对象权限**
        -   **SELECT**  读取表、视图、序列中的行
        -   **UPDATE**  更新表、视图、序列中的行
        -   **DELETE**  删除表、视图中行的数据
        -   **INSERT**  向表和视图中插入数据
        -   **EXECUTE** 执行类型、函数、包和过程
        -   **READ**    读取数据字典中的数据
        -   **INDEX**   生成索引
        -   **REFERENCES**  生成外键
        -   **ALTER**   修改表、序列、同义词中的结构
    -   ```GRANT object_privilege[,...] |ALL [PRIVILEGES] ON <schema.>object_name TO {user_name[,...] | role_name[,...] | PUBLIC} [WITH GRANT OPTION];```
    -   例：
        -   ```GRANT alter,update,select ON temp TO user1,user2 WITH GRANT OPTION;```
        -   ```GRANT UPDATE(ename,sal) ON emp ON user1 WITH GRANT OPTION;```
    -   **授予**列**对象权限**
        -   直接授予对象权限时，用户可以访问对象的所有列。**如果希望只访问个别的列**，则必须加上列的权限限制
        -   INSERT、UPDATE、REFERENCES这3种对象权限可以授予表、视图中的列
        -   授予SELECT对象权限时不能指定列名称。为了授予列级的SELECT权限，应创建一个包含所需要的列的视图，然后授予该视图的SELECT权限
    -   **对象授权注意事项**
        -   如果是自己创建的对象，则具有该对象的所有对象权限
        -   如果要授予对象权限，则该对象必须是自己方案中的对象，或得到该对象权限时也得到了WITH GRANT OPTION
        -   WITH GRANT OPTION 选项不能被授予角色
        -   在将对象权限授予其他用户时要考虑安全问题
    -   **与对象权限相关的数据字典**
        -   **DBA_TAB_PRIVS**：   DBA视图包含了数据库所有用户或角色的对象权限
        -   **ALL_TAB_PRIVS**：   ALL视图包含了当前用户或PUBLIC的对象权限
        -   **USER_TAB_PRIVS**：  USER视图列出当前用户的对象权限
        -   **DAB_COL_PRIVS**：   DBA视图描述了数据库中所有用户角色的对象权限
        -   **ALL_COL_PRIVS**：   ALL视图描述了当前用户或PUBLIC是其所有者、授予者或被授予者，所有的列对象权限
        -   **USER_COL_PRIVS**：  USER视图描述了当前用户是其所有者、授予者或被授予者的，所有列对象的权限
        -   **......**
    -   **撤销对象权限**
        -   ```REVOLE object_privilege[,...] | ALL [PRIVILEGES] ON <schema.>object_name FROM {username[,..] | role_name[,...] | PUBLIC};```
        -   
        -   例：
            -   ```|REVOKE execute,select ON scott.emp FROM user1,user2;```
            -   
    -   **回收对象权限注意事项**
        -   授权者只能从自己授权的用户那里回收对象权限
        -   用户不能有选择的回收列权限，必须首先回收该对象权限，然后再将合适的列权限授予用户
        -   假如基于一个对象创建了过程、视图、那么回收该对象权限后，这些过程、视图将变为无效
        -   回收对象权限会级联。如果用户A将对象权限a授予了用户B，用户B又将对象权限a授予了用户C，那么当删除用户B后或从用户B回收了对象权限a后，用户C将不再具有对象权限a，并且用户B和用户C中与该对象权限有关的对象都变为无效
#### 5.3 角色
-   >**概念**:oracle将一组权限授予某个角色，然后将这个角色授予需要的用户，拥有该角色的用户拥有该角色的所有权限
-   >**功能**
    -   一个角色可授予系统权限或对象权限
    -   一个角色可授权给其他角色，但不能循环授权
    -   任何角色可授权给任何数据库用户
    -   授权给用户的每一角色可以是可用的或者不可用的，一个用户的安全域包含当前对该用户可用的全部角色的权限
    -   一个间接授权角色对用户可显示地使其可用或不可用
    -   在一个数据库中，每一个角色名必须唯一。角色名与用户不同，角色不包含在任何模式中，所以建立角色的用户被删除时不影响该角色
-   >**创建角色**
    -   ```|CREATE ROLE role_name [NOT IDENTIFIED | IDENTIFIED BY password];```
-   >**为角色授予权限**
    -   ```GRANT SELECT,UPDATE,INSERT,DELETE ON emp TO emp_manager;///向角色emp_manager授予scott.emp表上的select、update、insert、delete对象权限```
    -   ```CREATE USER emp_user INDENTIFIED BY user_password;```
    -   ```GRANT emp_manager,CREATE SESSION TO emp_user;///创建用户emp_user，并为用户授予emp_manager，以及连接数据库所必须的create session权限```
    -   ```CONNECT emp_user/user_paaasword;```
    -   ```SELECT * FROM scott.emp where empno=7900;//使用emp.user用户连接数据库，用使用select语句查询数据```
-   >**修改用户的默认角色**
    -   ```ALTER USER user_name DEFAULT ROLE```
    -   ```{```
    -   ```role_name[,...]```
    -   ```| ALL [EXCEPT role_name[,.]]```
    -   ```| NONE ```
    -   ```};```
    -   说明
        -   role_name：角色名
        -   ALL：将用户的所有角色设置为默认橘色
        -   EXCEPT：将用户的除某些角色外的所有角色设置为默认橘色
        -   NONE：将用户的所有角色都设置成非默认角色
    -   例：将emp_user用户的emp_manager角色设置成非默认角色
        -   ```|ALTER USER emp_manager DEFAULT ROLE ALL EXCEPT emp_manager;```
        -   
        -   此时，emp_user用户将无法对scott.emp表进行相关的操作
-   >**管理角色**
    -   对角色的管理主要包括设置用户的口令、为角色添加或减少权限、禁用与启用角色、删除角色
    -   设置角色的口令
        -   ```ALTER ROLE role_name NOT IDENTIFIED | IDENTIFIED BT new_password;```
        -   
    -   禁用与启用角色
        -   ```SET ROLE { role_name [IDENTIFIED BY password][,....] | ALL [EXCEPT role_name[,...]] | NONE };```
        -   
    -   删除角色
        -   ```|DROP ROLE role_name;```
        -   
-   >**与角色有关的数据字典**
    -   **user_role_privs**
        -   >username
            -   该角色所授予的用户名
        -   >granted_role
            -   授予该用户的角色名
        -   >admin_option
            -   该用户是否可以将该角色授予其他用户或角色，值为YES | NO
        -   >default_role
            -   该角色是否为默认角色，值为YES | NO
        -   >os_granted
            -   该角色是否是否由操作系统授予，值为YES | NO
    -   **role_sys_privs**
        -   >role
            -   角色名
        -   >privilege
            -   授予该角色的系统权限
        -   >admin_option
            -   授予该系统权限是否使用了WITH ADMIN OPTION选项，值为YES | NO
    -   **role_tab_privs**   
        -   >role
            -   对象权限所授予的角色
        -   >owner
            -   对象的拥有者
        -   >table_name
            -   对象权限所针对的对象
        -   >column_name
            -   对象权限所针对的列
        -   >privilege
            -   对象权限
        -   >grantalbe
            -   权限该对象是否使用了WITH GRANT OPTION选项，值为 YES | NO 
-   **用户、权限与角色的关系**
    -   每个用户可以拥有0个或多个权限或角色
    -   每个权限可以分配给多个用户或角色
    -   每个角色可以拥有0个或多个权限或角色
    -   每个角色可以分配给多个用户或角色
    -   角色不能与用户同名
#### 5.4 用户配置文件(User Profile)
-   >**用户配置文件的概念**
    -   一个参数的集合，其功能是限制用户可使用的系统和数据库资源并管理口令
    -   为了限制数据库用户对数据库系统资源的使用，在安装数据库时，oracle自动创建了名为**DEFAULT**的资源配置文件，如果在创建用户时没有为用户指定配置文件，则oracle会为该用户指定配置文件为**DEFAULT**，默认用户配置文件指定对于所有用户资源没有限制
-   >**创建用户配置文件**
    -   -   ```|CREATE PROFILE prifile_name LIMIT [......];```
        -   
        -   例：使用DBA的身份创建用户配置文件user_profile,说明如下
        -   限制用户允许拥有的会话数位1 ，对应的参数为**SESSIONS_PER_USER**
        -   限制该用户执行的每条SQL语句可以占用的CPU总时间为5s%，对应的参数为**CPU_PER_CAL**
        -   限制该用户的空间时间为10min，对应的参数为**IDLE_TIME**
        -   限制用户登录数据库时可以失败的次数为3次，对应的参数为**FAILED_LOGIN_ATTEMPTS**
        -   限制口令的有效时间为10天，对应的参数为**PASSWORD_LIFE_TIME**
        -   设置用户登录失败次数达到限制要求时，用户被锁定3天，对应的参数为**PASSWORD_LOCK_TIME**
        -   设置口令使用时间达到有效时间后，口令依然可以使用的“宽限时间”为3天，对应的参数为**PASSWORD_GRACE_TIME**
        -   ```SQL>CREATE PROFILE user_profile LIMIT```
        -   ```SESSIONS_PER_USER 1```
        -   ```CPU_PER_CALL 5```
        -   ```FAILED_LOGIN_ATTEMPTS 3```
        -   ```PASSWORD_LIFE_TIME 10```
        -   ```PASSWORD_LOCK_TIME 3```
        -   ```PASSWORD_GRACE_TIME 3;```
-   >**使用配置文件**
    -   修改用户xiaoqi的配置文件为user_profile
        -   ```|SQL>ALTER USER xiaoqi PROFILE user_profile;```
        -   
        -   使用**SHOW PARAMETER**查看参数**resource_limit**的默认值
            -   ```|SHOW PARAMETER resource_limit;```
            -   
            -   使用**ALTER SYSTEM**语句修改该参数值为TRUE
                -   ```|ALTER SYSTEM SET resource_limit = TRUE;```
                -   
-   >**查看配置文件的信息**
    -   **dba_profiles**
        -   查看系统默认配置文件DEFAULT的参设置
            -   ```|SELECT profile,resource_name,limit FROM dba_profiles WHERE profile='DEFAULT';```
            -     
-   >**修改与删除配置文件**
    -   例：修改配置文件user_profile的IDLE_TIME参数，将用户允许的空间时间修改为20分
        -   ```|ALTER PROFILE user_profile LIMIT IDLE_TIME 20;```
        -   
    -   删除配置文件
        -   ```|DROP PROFILE profile_name;```
        -   
++++++++++++++++++++++++++++++++++没看错，还是一条分割线++++++++++++++++++++++++++++++++
## 第6章 表空间
####    6.1 表空间的概念、特点和分类
-   >**概念**
    -   oracle数据库在逻辑上可以划分一系列的逻辑空间，每一个逻辑空间简称为一个表空间
    -   oracle数据库中的数据逻辑存储在表空间并物理的存储在数据文件中
-   >**特点**
    -   一个数据库由一个或多个表空间构成，不同表空间用于存放不同应用的数据，表空间大小决定了数据库的大小
    -   一个表空间对应一个或多个数据文件，数据文件大小决定了表空间的大小，一个数据文件只能从属于一个表空间
    -   表空间是存储模式对象的容器，一个数据库对象只能存储在一个表空间中(分区表和分区索引除外)，但可以存储在该表空间所对应的一个或多个数据文件中
-   >**分类**
    -   >**根据表空间存储的主要用途可分为**
        -   系统表空间
            -   **SYSTEM表空间**
                -   数据库的数据字典
                -   PL/SQL程序的源代码和解释代码，包括存储过程，函数，包，触发器等
                -   数据库对象的定义，如表、视图、序列、同义词等
            -   **SYSAUX表空间**
                -   SYSAUX表空间是oracle 10g新增的辅助系统表空间，主要用于存储数据库组件等信息，以减小SYSTEM表空间的负荷
                -   在通常情况下，不允许删除、重命名及传输SYSAUX表空间
        -   非系统表空间
            -   撤销表空间
                -   专门进行回滚信息的自动管理，由**UNDO_TABLESPACE**参数设置
            -   临时表空间
                -   专门进行临时数据管理的表空间，在数据库实例运行过程中，执行排序等SQL语句会产生大量的临时数据，这些数据将保存在临时表空间中
            -   用户表空间
                -   保存用户数据
    -   **根据表空间数据文件的特点可分为**
        -   大文件表空间(Bigfile Tablespace)
            -   值一个表空间只包含一个大数据文件，该文件的最大尺寸(数据大小的4G值)为128TB(如果数据块大小为32kb)或32TB(如果数据块大小为8KB)
        -   小文件表空间(Smilefiel Tablespace)
            -   系统默认创建的表空间成为小文件空间，如SYSTEM表空间，SYSAUX表空间等，小文件表空间可以包含多达1022个数据文件，小文件表空间的总容量与大文件表空间的容量基本相似
    -   **根据表空间存储方式可分为**
        -   字典管理方式(DICTIONARY)
            -   表空间使用数据字典来管理存储空间的分配，当进行区的分配与回收时，oracle将对数据字典中的相关基础信息进行更新，同时会产生回滚信息和重做信息，字典管理方式将渐渐被淘汰
        -   本地管理方式(LOCAL)
            -   xxxxx
            -   xxxxxx
            -   xxxxx
            -   xxxx
            -   asdas
            -   asda
####    6.2 管理表空间(用户表空间)
#####   6.2.1 表空间的规划
#####   6.2.2 创建表空间
#####   6.2.3 表空间的状态管理
#####   6.2.4 重命名表空间
#####   6.2.5 默认表空间
#####   6.2.6 删除表空间  
####    6.3 管理表空间文件(用户表空间)
#####   6.3.1 改变表空间的容量
#####   6.3.2 数据文件的状态管理
#####   6.3.3 移动数据文件
#####   6.3.4 删除数据文件  
####    6.4 其他表空间
#####   6.4.1 临时表空间
#####   6.4.2 大文件表空间
#####   6.4.3 非标准数据库表空间
#####   6.4.4 撤销表空间
