# Pyspark Connection MySQL Server

### 1. Persiapan Lingkungan

#### Update dan Instalasi Default JDK
```
sudo apt-get update
```
```
sudo apt-get upgrade
```
```
sudo apt-get install default-jdk
```

#### Update dan Instalasi Python 3.8
```
sudo apt-get update
```
```
sudo apt-get upgrade
```
```
sudo add-apt-repository ppa:deadsnakes/ppa
```
```
sudo apt install python3.8
```

#### Periksa Versi Java
```
java --version
```

#### Periksa Versi Python
```
python3 --version
```

### 2. Instalasi dan Konfigurasi Apache Spark

#### Unduh dan Ekstraksi Apache Spark
```
wget https://archive.apache.org/dist/spark/spark-2.4.0/spark-2.4.0-bin-hadoop2.7.tgz
```
```
tar xvf spark-2.4.0-bin-hadoop2.7.tgz
```

#### Pindahkan dan Konfigurasi Direktori Spark
```
sudo mkdir /usr/local/spark
```
```
sudo cp -a spark-2.4.0-bin-hadoop2.7/* /usr/local/spark/
```

#### Konfigurasi Environment Variable
```
sudo nano ~/.bashrc
```

##### Isikan Konfigurasi Dibawah ini Kedalam ~/.bashrc
```
export PATH=$PATH:/usr/local/spark/bin
export PYSPARK_PYTHON=/usr/bin/python3.8
export PYSPARK_DRIVER_PYTHON=/usr/bin/python3.8
```

#### Perbarui Environment Variable
```
source ~/.bashrc
```

### 3. Instalasi dan Konfigurasi MySQL

#### Instalasi MySQL Server
```
sudo apt-get install mysql-server
```

#### Memulai dan Mengakses MySQL
```
sudo systemctl status mysql
```
```
sudo systemctl start mysql
```
```
sudo mysql -u root -p
```

#### Buat Pengguna dan Database MySQL
```
create user '{your_name}'@'localhost' identified with mysql_native_password by '{your_pw}';
```
```
grant all privileges on *.* to '{your_name}'@'localhost' with grant option;
```
```
exit
```
```
sudo mysql -u {your_name} -p;
```

##### Buat Tabel di MySQL
```
create database {your_prefered_db};
```
```
use {your_prefered_db};
```
```
create table {your_prefered_table} (
    {var_name_1} {data_type_1},
    {var_name_2} {data_type_2},
    ....
);
```
```
insert into {your_prefered_table} ({var_name_1}, {var_name_2}, ...) values 
    ('{values_1.1}', '{values_1.2}', ...),
    ('{values_2.1}', '{values_2.2}', ...),
    ....
```
```
exit
```

### 4. Koneksi Spark dengan MySQL

#### Instalasi Paket Python
```
pip3 install pyspark
```
```
pip3 install findspark
```

#### Unduh dan Ekstraksi MySQL Connector-J
```
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j-8.3.0.tar.gz
```
```
tar -zxvf mysql-connector-j-8.3.0.tar.gz
```

#### Kode Python untuk Koneksi MySQL-Spark
```
python3
```
```
import findspark
from pyspark.sql import SparkSession

findspark.init()

spark = SparkSession.builder.appName("PySpark MySQL Connection").config("spark.jars", "/path/to/mysql-connector-j-8.3.0.jar").getOrCreate()

df = spark.read.jdbc("jdbc:mysql://localhost:3306/{your_db}", "{your_table}", properties={
    "user": "{your_name}",
    "password": "{your_pw}",
    "driver": "com.mysql.cj.jdbc.Driver"
}).show()
```
