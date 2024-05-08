# Pyspark Connection MySQL Server

### <strong>1. Persiapan Lingkungan</strong>

#### 1.1. Update dan Instalasi Default JDK
```
sudo apt-get update
```
```
sudo apt-get upgrade
```
```
sudo apt-get install default-jdk
```

#### 1.2. Update dan Instalasi Python 3.8
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

#### 1.3. Periksa Versi Java dan Python
```
java --version
```
```
python3 --version
```

### <strong>2. Instalasi dan Konfigurasi Apache Spark</strong>

#### 2.1. Unduh dan Ekstraksi Apache Spark
```
wget https://archive.apache.org/dist/spark/spark-2.4.0/spark-2.4.0-bin-hadoop2.7.tgz
```
```
tar xvf spark-2.4.0-bin-hadoop2.7.tgz
```

#### 2.2. Pindahkan dan Konfigurasi Direktori Spark
```
sudo mkdir /usr/local/spark
```
```
sudo cp -a spark-2.4.0-bin-hadoop2.7/* /usr/local/spark/
```

#### 2.3. Konfigurasi Environment Variable
```
sudo nano ~/.bashrc
```

##### 2.4. Isikan Konfigurasi Dibawah ini Kedalam ~/.bashrc
```
export PATH=$PATH:/usr/local/spark/bin
export PYSPARK_PYTHON=/usr/bin/python3.8
export PYSPARK_DRIVER_PYTHON=/usr/bin/python3.8
```

#### 2.5. Perbarui Environment Variable
```
source ~/.bashrc
```

### <strong>3. Instalasi dan Konfigurasi MySQL</strong>

#### 3.1. Instalasi MySQL Server
```
sudo apt-get install mysql-server
```

#### 3.2. Memulai dan Mengakses MySQL
```
sudo systemctl status mysql
```
```
sudo systemctl start mysql
```
```
sudo mysql -u root -p
```

#### 3.3. Buat Pengguna dan Database MySQL
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

##### 3.4. Buat Tabel di MySQL
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

### <strong>4. Koneksi Spark dengan MySQL</strong>

#### 4.1. Instalasi Paket Python
```
pip3 install pyspark
```
```
pip3 install findspark
```

#### 4.2. Unduh dan Ekstraksi MySQL Connector-J
```
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j-8.3.0.tar.gz
```
```
tar -zxvf mysql-connector-j-8.3.0.tar.gz
```

#### 4.3. Kode Python untuk Koneksi MySQL-Spark
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
