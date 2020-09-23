---
layout: post
title:  "MariaDB encoding"
date:   2020-09-16 18:00:13 +0800
categories: setting
tags: mariadb encoding
---

# mariadb(마리아디비) 인코딩!

mariadb를 사용하면 설정하기 전에는 한글이 보이지 않는다!

*  client, connection, results 부분이** latin1**로 설정되어있어 한글이 깨진다.
>MariaDB [none]> show variables like 'c%';
+----------------------------------+----------------------------+
| Variable_name                    | Value                      |
+----------------------------------+----------------------------+
| character_set_client             | latin1                     |
| character_set_connection         | latin1                     |
| character_set_database           | utf8mb4                    |
| character_set_filesystem         | binary                     |
| character_set_results            | latin1                     |
| character_set_server             | utf8mb4                    |
| character_set_system             | utf8                       |
| character_sets_dir               | /usr/share/mysql/charsets/ |
| check_constraint_checks          | ON                         |
| collation_connection             | latin1_swedish_ci          |
| collation_database               | utf8mb4_general_ci         |
| collation_server                 | utf8mb4_general_ci         |
| column_compression_threshold     | 100                        |
| column_compression_zlib_level    | 6                          |
| column_compression_zlib_strategy | DEFAULT_STRATEGY           |
| column_compression_zlib_wrap     | OFF                        |
| completion_type                  | NO_CHAIN                   |
| concurrent_insert                | AUTO                       |
| connect_timeout                  | 10                         |
| core_file                        | OFF                        |
+----------------------------------+----------------------------+


encoding 설정을 하기 위해선
다음 명령어를 입력해서 설정을 하자

> docker exec -it maria-db /bin/bash

인코딩 설정은 **/etc/mysql/my.cnf**파일에서 할 수 있다.

1. 파일을 연다.
	> vi my.cnf
2. 다음 내용을 추가한다.
	> [client]
	default-character-set = utf8mb4

	> [mysql]
	default-character-set = utf8mb4

설정을 바꾼 후 저장을 한다.

다시 확인을 하면 인코딩이 **utf8mb4**로 바뀐것을 알 수 있다.
(utf8mb4는 utf8로는 emoji를 표현할 수 없는 문제를 해결하기 위해 사용한다고 한다.)
> MariaDB[none]> show variables like 'c%';
+----------------------------------+----------------------------+
| Variable_name                    | Value                      |
+----------------------------------+----------------------------+
| character_set_client             | utf8mb4                    |
| character_set_connection         | utf8mb4                    |
| character_set_database           | utf8mb4                    |
| character_set_filesystem         | binary                     |
| character_set_results            | utf8mb4                    |
| character_set_server             | utf8mb4                    |
| character_set_system             | utf8                       |
| character_sets_dir               | /usr/share/mysql/charsets/ |
| check_constraint_checks          | ON                         |
| collation_connection             | utf8mb4_general_ci         |
| collation_database               | utf8mb4_general_ci         |
| collation_server                 | utf8mb4_general_ci         |
| column_compression_threshold     | 100                        |
| column_compression_zlib_level    | 6                          |
| column_compression_zlib_strategy | DEFAULT_STRATEGY           |
| column_compression_zlib_wrap     | OFF                        |
| completion_type                  | NO_CHAIN                   |
| concurrent_insert                | AUTO                       |
| connect_timeout                  | 10                         |
| core_file                        | OFF                        |
+----------------------------------+----------------------------+


----
이건 맞는 건지 모르겠지만 mariadb /bin/bash로 들어가면 vim이 설치가 안되어 있어서
>apt-get update
>apt-get install vim

으로 설치를 한 후 vi를 사용하여 파일을 고칠 수 있었다.

