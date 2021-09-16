# interview-mariadb

面試題2
以下為使用command line的順序

#### 打開terminal，準備安裝需要的apt。首先進入root權限，方便後面輸入指令不需要再寫sudo
    sudo su

#### 然後移除舊版Docker，以便後面安裝Docker
    apt-get remove docker docker-engine docker.io

#### 安裝curl套件
    apt-get curl

#### 然後套件庫更新
    apt-get update

#### 接下來就是安裝Docker
    apt-get install docker.io

#### 再來去下載安裝Docker Compose
    curl -L https://github.com/docker/compose/releases/download/1.29.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

#### 開啟Docker Compose使用權限
    chmod +x /usr/bin/docker-compose

#### 建立一個文件夾
    mkdir interview-mariadb
    
#### 跳轉路徑到interview-mariadb
    cd interview-mariadb
    
#### 安裝vim套件
    apt install vim
    
#### 在目錄底下建立yml文件，待會要給docker-compose使用
    vim docker-compose.yml
    
#### 輸入以下需要安裝和設定的mariadb和phpmyadmin，另外mariadb資料用volume掛出
    version: "3.8"
    services:
        mariadb:
            image: mariadb:latest
            container_name: mariadb
            command: --default-authentication-plugin=mysql_native_password
            restart: unless-stopped
            environment:
                MYSQL_ROOT_PASSWORD: admin
            volumes:
                - dbdata:/var/lib/mysql
            ports:
                - '3306:3306'

        phpmyadmin:
            image: phpmyadmin/phpmyadmin:latest
            container_name: phpmyadmin
            ports:
                - "8082:80"
            environment:
                MYSQL_ROOT_PASSWORD: admin
                PMA_HOST: mariadb
                PMA_USER: root
                PMA_PASSWORD: admin
       
    volumes:
        dbdata:

#### 然後輸入以下指令掛上docker-compose，指令完成後會分別看到mariadb和phpmyadmin都顯示done的字眼
    docker-compose up -d

#### 接下來就可以操作mariadb了，輸入以下指令登入
    mysql -h 127.0.0.1 -u root -p

#### 剛yml文件裡mariadb的密碼設定是admin，輸入密碼
    admin

#### 之後就成功進入mariadb，再建立資料庫:nantou
    create database nantou;

#### 然後選擇資料庫:nantou
    use nantou

#### 然後就可以建立表格:puli
    create table puli(id int auto_increment,name varchar(20) NOT NULL,age int NOT NULL,gender enum('M','F','T'),PRIMARY KEY(id));

#### 建好表格後就可以分別對表格:puli輸入各欄位的值，id是auto increment所以放null,系統會自動生成欄位值
`insert into puli values(null,'Sky Yu',28,'M');`

`insert into puli values(null,'Emily',30,'F');`
 
 #### 最後附上在phpmyadmin DB Gui裡連線成功的畫面
![image](https://github.com/joerenyu93/interview-mariadb/blob/master/phpmyadmin%E9%80%A3%E7%B7%9A%E6%88%90%E5%8A%9F.jpg)
