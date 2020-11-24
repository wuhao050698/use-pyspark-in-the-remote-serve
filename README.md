### use pyspark from the serve
you should execute the command in the /home/username, and do not use the root user.
1. get the spark
```
wget http://ftp.cuhk.edu.hk/pub/packages/apache.org/spark/spark-3.0.1/spark-3.0.1-bin-hadoop2.7.tgz
tar xf spark-3.0.1-bin-hadoop2.7.tgz
```
2. java8
```
java -version
```
if is not installed
```
sudo add-apt-repository ppa:openjdk-r/ppa
sudo apt-get update
sudo apt-get install openjdk-8-jdk
```
3. install annaconda
```
wget https://repo.anaconda.com/archive/Anaconda3-2020.07-Linux-x86_64.sh
bash Anaconda3-2020.07-Linux-x86_64.sh 
```
modify the bashrc
```
vim ~/.bashrc
```
set environment variables
add these line in the ~/.bashrc
```
export SPARK_HOME=/home/username/spark-3.0.1-bin-hadoop2.7
export PATH=/home/username/anaconda3/bin:$PATH
export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME
export PYSPARK_DRIVER_PYTHON="jupyter"
export PYSPARK_DRIVER_PYTHON_OPTS="notebook"
```
Let environment variables take effect
```
source ~/.bashrc
```
4. Setting up remote access of the jupyter
```
jupyter notebook --generate-config
```
you can see the information of the password in the 
```
vim .jupyter/jupyter_notebook_config.json
```
That is what you can see, then copy the password.
```
"NotebookApp": {
    "password": "argon2:$argon2id$v=19$m=10240,t=10,p=8$Hu5PKNkC2y66V12p61Pq/Q$17++4y4NKNAAAAAAvmDfzQ",
    "nbserver_extensions": {
      "jupyter_nbextensions_configurator": true
    }
  }
```
open and set the config
```
vim .jupyter/jupyter_notebook_config.py
```
add these in the jupyter_notebook_config.py
```
c.NotebookApp.ip='*'
c.NotebookApp.password = u'password copy from the json'
c.NotebookApp.open_browser = False
c.NotebookApp.port =7777 # any port you like
```
5. set the port in the azure
   虚拟机->网络->添加入站端口规则->目标端口范围:7777(port in the jupyter_notebook_config.py)->添加

   if you want to use the spark ui, do the same thing in the port 4040.
6. install the graphframe
```
wget http://dl.bintray.com/spark-packages/maven/graphframes/graphframes/0.8.0-spark3.0-s_2.12/graphframes-0.8.0-spark3.0-s_2.12.jar
sc.addPyFile('path_to_the_jar_file')
```
7. use the jupyter 
```
pyspark --packages graphframes:graphframes:0.8.0-spark3.0-s_2.12
```
If you want to keep it executing all the time, run the command following.
```
nohup pyspark --packages graphframes:graphframes:0.8.0-spark3.0-s_2.12 >pyspark_log.txt 2>&1 &
```