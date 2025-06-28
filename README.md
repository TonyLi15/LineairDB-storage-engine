# LineairDB MySQL Custom Storage Engine

Updated on Jun 28, 2025

# How to Build

```bash
git clone --recursive git@github.com:Tatzhiro/LineairDB-storage-engine.git
cd LineairDB-storage-engine
./build.sh
```

## Handle Errors while Building

### If `build.ninja` not found, do the following:
```bash
cd boost
wget https://sourceforge.net/projects/boost/files/boost/1.77.0/boost_1_77_0.tar.bz2/download -O boost_1_77_0.tar.bz2
tar -xjf boost_1_77_0.tar.bz2
```
### If `Could NOT find PkgConfig (missing: PKG_CONFIG_EXECUTABLE)`, do the following:
```bash
sudo apt-get update
sudo apt-get install pkg-config
```

### If `Cannot find /home/tonyli_15/LineairDB-storage-engine/third_party/mysql-server/sql/sql_yacc.h`, do the following:
```bash
sudo apt-get install bison
sudo apt-get install flex
```

### If `cc1plus: all warnings being treated as errors` while building, do the following:
Open `~/CMakeLists.txt` and add the following: 
```
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-error")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-error")
```
RIGHT AFTER
```
set(CMAKE_CXX_STANDARD 17)
```


# How to Launch SQL Server

## Rewrite `my.conf` according to your own environment and set default storage to `LINEAIRDB`
```
[mysqld]
basedir=/home/{YOUR_DIRECTORY_NAME}/LineairDB-storage-engine/build
datadir=/home/{YOUR_DIRECTORY_NAME}/LineairDB-storage-engine/build/data
...
default_storage_engine=LINEAIRDB
```

## Launch mysqld with proper initialization without temporary password
```bash
# initialize
mkdir -p build/data
build/bin/mysqld --defaults-file=my.cnf --initialize-insecure

# run mysql-server in background
build/bin/mysqld --defaults-file=my.cnf --daemonize
```

# How to login and test MySQL Server

```bash
build/bin/mysql -u root
```

Then, install the generated plugin of this repository into mysql-server:

```mysql
mysql> install plugin lineairdb soname 'ha_lineairdb_storage_engine.so';
```


For python tests, install mysql-connector-python. 
```bash
pip3 install -r testsrequirements.txt
```

At this time you can do testing.
To check all tests, execute the following file:
```bash
python3 tests/run_tests.py
```







# Benchmark

see [bench/README.md]

# How to Contribute
