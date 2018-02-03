# Introduction


使用备注：


创建 Python 容器：docker run -it --rm --name my-python -v e:/python:/python  python /bin/bash

docker run -it --name my-python -v e:/python:/python  python /bin/bash


docker run -it --name web-python -p 8082:8082 -v e:/awesome-python3-webapp:/python  python /bin/bash


## 创建 mysql 

docker run -p 3307:3306 --name mysql -v e:/docker-mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -d mysql 

## 命令详解
http://blog.csdn.net/kunloz520/article/details/53839237
