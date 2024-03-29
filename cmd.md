<!--NOTE HEAD START-->
<link rel="icon" type="image/png" href="./imgs/favicon_db.png" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/mermaid/8.0.0/mermaid.min.js"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {skipTags: ['script', 'noscript','style', 'textarea', 'pre'],inlineMath: [['$','$']]}});</script>
<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
<script>document.body.style.background = "#f2f2f2";</script>
<!--NOTE HEAD END-->
# Syntax reminders
A big personal mess, nobody should never ever look at it :D

## Big Data related
### Spark / scala
```bash
//Spark-submit
C:\...\wordcountsparkscala\target> spark-submit --class <package.main_class_name> <jar_name.jar> args_0 arg_1 arg_2 ....
```
```scala
//spark2-shell
val textFile = sc.textFile("hdfs:///user/dlff1193/WORK/TESTHDFS/heyJude.txt")
textFile.first
val counts = textFile.flatMap(line => line.split(" ")).map(word => (word, 1)).reduceByKey(_ + _)
counts.saveAsTextFile("hdfs:///userr/dlff1193/WORK/TESTHDFS/example")
counts.collect
val countsFiltered=counts.filter(_._2>10)
countsFiltered.collect
//Compte nb 'e'
val v2=tokens.map(word=>{var x=0;for(l <- word){if(l=='e'){x=x+1;println(x)}};(1,x)})
val r1=v2.reduceByKey(_+_)
//fin compte nb 'e'
//remplacement 'e' par 'a'
val v2=v1.map(word =>{var wordModif:String="";for(l<-word){if(l=='e'){wordModif+='a'}else{wordModif+=l}};(1,wordModif);})
//fin remplacement 'e' par 'a'
```

### Redis
```bash
# list databases
redis-cli INFO | grep 'db'
# dump all keys
redis-cli KEYS '*' > /tmp/redis_keys_dump.txt
```

### PYSPARK
```bash
pyspark --master local[2] (lance jupyter avec sc accessible)
```
### HDFS
```bash
hadoop fs -text /user/dlff1193/WORK/TESTHDFS/resOnOneDayTestTestTestIuEmptyIMEICounter/*00*
hdfs dfs -ls WORK/TESTHDFS
hadoop fs -get WORK/TESTHDFS/*py
```
### MR
```bash
//MapReduce
yarn jar .\target\wordcountmr-1.0-SNAPSHOT.jar com.bonnalenzo.wordcountmr.WordCount .\pom.xml ./output
hdfs dfs -text ./output/part-r-00000
```
### Kafka:
```
./zookeeper-server-start.bat ..\..\config\zookeeper.properties
./kafka-server-start.bat ../../config/server.properties
//production:
./kafka-console-producer.bat --broker-list localhost:9092 --property "parse.key=true" --property "key.separator=:" --topic topicwithkey
//consumer:
./kafka-console-consumer.bat --bootstrap-server localhost:9092 --property "parse.key=true" --property "key.separator=:" --topic mytopicout
```

### GCP
```bash
# install SDK
curl https://dl.google.com/dl/cloudsdk/release/install_google_cloud_sdk.bash | bash
# submit spark job
gcloud dataproc jobs submit spark --cluster=dataproc-xxxxxxx-dev --region=global --class com.xxxxxxx.analysis.testparquetjoin.Main --jars gs://xxxxxxx-dataproc/testparquetjoin/testparquetjoin-0.1-jar-with-dependencies.jar
# list jobs
gcloud dataproc jobs list --cluster=dataproc-xxxxxxx-dev --region=global
# kill job by id
gcloud dataproc jobs kill --region=global
# rename file on Cloud Storage
gsutil mv gs://old_name gs://new_name
# switch google cloud project
gcloud config set project <project-id>
# get disk usage of a folder in human readable unit
gsutil du -sh gs://bucket/some/folder
# verbose listing (with generation) while listing
gsutil ls -L <path>
```


## Terminals
### vim basics

|||
|--|--|
|exit a mode (=enter command mode)|escape|
|enter edit mode|`i`|
|quit|`:q`|
|force quit without saving|`:q!`|
|save|`:w`|
|save and quit|`:wq`|
|copy|`yy`|
|cut|`dd`|
|paste|`p`|

### Terraform
```
terraform state rm google_bigquery_table.dataset_name_table_name
terraform import google_bigquery_table.dataset_name_table_name project_name/dataset_name/table_name
terraform apply
```

### Container technos

```bash
# docker run jupyter notebook
docker run -p 8888:8888 -v notebooks:/home/jovyan/work -v /tmp:/home/jovyan/data jupyter/all-spark-notebook
# docker delete dangling volumes
docker volume rm $(docker volume ls -qf dangling=true)
# docker remove all images with none tag
docker rmi $(docker images -f "dangling=true" -q) --force
# docker delete all images
docker rmi $(docker images -a -q) # -f
```
### Elastic search
Je pose là les urls qui peuvent servir:  

-   lister les indices  es  dispos:  [https://esweb-01.adm.dc3.dailymotion.com:9200/_cat/indices](https://esweb-01.adm.dc3.dailymotion.com:9200/_cat/indices)
-   get le document d’une video avec son id entier en clé  [https://esweb-01.adm.dc3.dailymotion.com:9200/video_v4_idx/_doc/492342220](https://esweb-01.adm.dc3.dailymotion.com:9200/video_v4_idx/_doc/492342220)
-   Le ticket  [https://jira.dailymotion.com/browse/GLUE-1147](https://jira.dailymotion.com/browse/GLUE-1147) (la question stackoverflow date du 28 sept  [@Achille Lacoin](https://dailymotion.slack.com/team/UK0SVEF3P))
-   [https://developer.dailymotion.com/tools/](https://developer.dailymotion.com/tools/)
### Tensorflow
```bash
pip3 install tensorflow
saved_model_cli show --dir /path/to/model --all
```

### unix

```bash
# for loops
for json_file_name in foo bar; do touch /tmp/$json_file_name.json; done;
for i in `seq 1 10`; do echo $i; done;
# print gpu info
sudo lshw -C display
# (find location of a symlink:
ls -al <symlink>
# extended ls
ls -alL .
# count lines
wc -l filename
# split large file in files of X lines, smallfileaa, smallfileaa, smallfileab
split -l X largefile.txt smallfile
# print current terminal pid
echo $$
# pass env key-value pairs to a process, i.e bash here
A=B B=C bash
# from where the following will output C
echo ${!A}
# xargs hello world
echo world | xargs -I % echo hello %
# search and kill a process foo by pid
ps -e
#     PID TTY          TIME CMD
#   9947 pts/6    00:00:00 foo
kill -9 9947

# set up ultra wide on HDMI-1-2 output for example
# - get modeline 3440p x 1440p, 44Hz
cvt 3440 1440 44
# - apply it
xrandr --newmode wide44 299.75  3440 3664 4024 4608  1440 1443 1453 1479 -hsync +vsync
xrandr --addmode HDMI-1-2 wide44
xrandr --output HDMI-1-2 --mode wide44
# ubuntu show date and time
gsettings set org.gnome.desktop.interface clock-show-date true
gsettings set org.gnome.desktop.interface clock-show-seconds true
gsettings set org.gnome.desktop.interface clock-show-weekday true
# switch between virtual consoles
Ctrl + Alt + F[0-9]
# keyboard's layout to azerty
loadkeys fr
# purge nvidia drivers
sudo apt-get purge nvidia*
# check which GPU is in use
glxheads
# or monitor valatile usage and power consumption every 250ms
watch -n 0.25 nvidia-smi
# duplicate command's output into a file
ping google.com | tee -a /tmp/t.txt
# jq pretty print / multiple select
echo '{"foo":{"bar": 0}}' | jq '.,.foo'
# gron + grep
echo '{"root": {"a": "foo", "b": ["foo","foo","bar"]}}' | gron | grep --color foo
# get unique whitespace separated words (using sed to Substitute spaces by newlines Globally)
echo foo bar foo | sed 's/ /\n/g' | sort | uniq
# skip first 10 lines of a stream, using tail
... | tail -n +11
# grep example
grep 'alias' /home/foo/.bashrc
# grep regexp
echo 'valid_pattern' | grep -E 'valid_.*'
# inverted grep: -v or --invert-match option
grep -v <except_word>
# find all __pycache__ folders and rm them
find . | grep -E "(__pycache__|\.pyc|\.pyo$)" | xargs rm -rf
# find/xargs/cat/grep/wc to count the number of lines containing 'abc' in all python files contained in cwd
find . -type f -name '*.py' | xargs cat | grep 'abc' | wc -l
# or using `subcommand` (that executes subcommand and replaces new lines by spaces in its stddout before inlining it in wrapping command)
cat `find . -type f -name '*.py'` | grep 'abc' | wc -l
# change default jdk: in .bashrc:
export JAVA_HOME=/opt/java/<jdk>
export PATH=$JAVA_HOME/bin:$PATH
# create a new session
sudo adduser <user_name>
# Disk Usage Analyzer
sudo baobab
# du: list folder sizes with a given depth, at bytes level
du -h --max-depth=1 --block-size=1

# pipe stderr also
some_command |& grep in_stdout_or_stderr
# show running processes
ps -A
//NEBOJSA PIPE EXAMPLE : LISTE toute la 1ere colonne
 cat OS_IU_sample | cut -f1 |more 
 hadoop dfs -cat /Data/O19593/SECURE/RAW/TXT/OS_IU_1/2017/05/16/2017_05_16_OSO_3G_37.csv | head -1000 > OS_IU_sample
//SELECT 1000 1eres lignes
hadoop dfs -cat /Data/O19593/SECURE/RAW/TXT/OS_IU_1/2017/05/16/2017_05_16_OSO_3G_37.csv | head -1000 > O
S_IU_sample
hadoop dfs -put OS_IU_sample WORK/TESTHDFS/IU_TEST

// bash helloworld (implementation of sh POSIX norm)
#!/bin/bash
# declare STRING variable
STRING="Hello World"
#print variable on a screen
echo $STRING
// make file executable
$ chmod +x hello_world.sh 
// chmod recursively on folder
chmod u=rwx,g=rx,o=r /.../folder/ --recursive
# compression de tous les fichiers gzip du répertoire, 8 à la fois, en faible prio avec nice:  
nice parallel -j 8 gzip -- *.json
// tar extract tgz
tar -xvzf /path/to/yourfile.tgz
// archive and compress with gzip
tar -cz filename -f filename.tar.gz
// unzip
unzip file.zip -d destination_folder
# decompressed .gz file
gunzip file.gz
// env var tmp
export ABC=/bla/blouche
// env var perm for all users
nano /etc/environment
// show permissions
ls -la fileOrDir
// mount read-only
sudo mount -o ro /dev/nvme0n1p5 dest
sudo unmount dest
// Fix broken installs
sudo apt-get --fix-broken install
// install specific version
`apt-get install «pkg»=«version»`
// list available versions on apt
apt-cache policy <package name>
// fix broken installations
sudo apt-get --fix-broken install
// list files installed by a package
dpkg -L <package_name>
// install a .deb package
dpkg -i package_name.deb
// update paths
source ~/.bashrc
// You can make a file executable as follows:
chmod a+x exampleName.AppImage
// or
chmod 755 <script or binary name>
// get historic back after reset and clear
[shortcut] Ctrl + l
// search terminal history, same shortcut to navigate matching lines
[shortcut] Ctrl + r
# output last 100 commands run in current terminal, with execution timestamp
history -E 100
// create a symlink (symbolic link)
sudo ln -s origin target
// generate ssh pub/private keys
// 1. generate the keys
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
// 2. run ssh agent in background and 3. add it to the agent
eval "$(ssh-agent -s)" && ssh-add ~/.ssh/id_rsa
// -> public key is in ~/.ssh/id_rsa.pub

// ssh bridge: maps port 9000 of 10.240.0.44 accessed through <host_url> to localhost:9000
ssh -L 9000:10.240.0.44:9000 <host_url>
// put a directories's jars in classpath var, in ~/.bashrc
export CLASSPATH="/home/enzo/Prog/lib/jar/*"
// log as another user in terminal
sudo su - username
# make host resolution made by a server
ssh -D 1080 <server>
# chromium with port redirection
chromium-browser --proxy-server="socks5://127.0.0.1:1080" --user-data-dir="/home/enzo/chrome-proxy-profile"
# desactivate CORS for dev between file:// and http protocols
chromium-browser --disable-web-security --user-data-dir
# quit ssh connection
exit
// shebang commentary
#!/usr/bin/python
#!/bin/bash

//grant current user with privilegess on all the content of folderx
sudo chown $USER folderx -R

// empty root trash
sudo rm -r /root/.local/share/Trash/files

# Null device in UNIX systems
/dev/null

# scalastyle jar usage example
java -jar ~/Apps/scalastyle_2.12-1.0.0-batch.jar --config ~/Prog/spark/scalastyle-config.xml ./PageRank.scala
```


### PowerShell
```
//display env:
$env:HADOOP_HOME
// remove file
del <file>
//remove directory :
rd /s /q <directory> 
```

## Java
### Java
```bash
// JAVAC RECURSIVELY
//Linux
$ find -name "*.java" > sources.txt
$ javac @sources.txt

//CMD
> dir /s /B *.java > sources.txt
> javac @sources.txt

//Java outside bin directory (like eclipse does)
java -classpath bin <className>

//java inline opti
javac -o ...
```
## Build/IDEs/VCS
### Gradle
```
# build without test
gradle build -x <test task>
# run specific test classes
gradle test --tests SomeClassTest
# in build.gradle, to have an auto generated 'run' task:
plugins {  
  [...]
  id "application"  
}  
  
apply plugin : "java"  
ext {  
  javaMainClass = "com.bla.MyMainClass"  
}  
  
application {  
  mainClassName = javaMainClass  
}
```

### Maven:
```
//one java/scala test:
mvn test -Dtest=com....   [-f pom.xml]
mvn test -Dsuites=com....   [-f pom.xml]
mvn package -DskipTests=true

// build specific modules (. is parent pom)
mvn -pl .,library1,library2 clean install ...

sudo mvn install:install-file -U -Dfile=/path/bla.jar    -DgroupId=com.bla    -DartifactId=bla    -Dversion=X.x    -Dpackaging=jar   -DgeneratePom=true -DlocalRepositoryPath=/root/.m2/repository
```
### pypi //pip
```bash
# specific version
pip install 'scimple==1.2.5' --force-reinstall

pip install twine
# local install :
pip install .
# pypi install:
python setup.py sdist (bdist_wininst)
cd dist
twine upload <package_name>-<version>.tar.gz
# use:
pip install <package_name>
pip install <package_name> --upgrade

# Deep Learning
pip install tensorflow-gpu

# use pip as module
python2 -m pip install ...
```
### Eclipse :
```
//REMOVE BLANCK LINE
regular ^\s*\n
```

### VSCode
```bash
# duplicate current line
Shift + Alt + Down/Up Arrow Key
# reformat document
option + shift + f
```


### IntelliJ
```
Doc : Select item and then Ctrl + Q
```
### atom
```bash
apm install hydrogen
```

### vim
```bash
# quit mode
esc

# save and quit
:wq
enter

# move fast right
e
# and left
b

# batch remove and write 
Ctrl + V
select characters to remove rectangle with arrows
c
*type stuff on one line*
esc # applies to every lines

# copy paster a line
esc
yy
*go to the line below which you want to paste*
p

# delete a line
esc
ddine
```
### git
[How to write good commit messages](https://chris.beams.io/posts/git-commit/#imperative)

```bash 
# restore a bar file's state as it was in commit foo
git restore -s foo -- ./bar
# show common ancestor commit between branch A and B
git merge-base A B # result -> C
# with previous example, these two 3-dots diff are equivalent:
git diff A...B # diff between common ancestor of A and B (=C) and B
git diff C...B # diff between common ancestor of C and B (=C) and B
# true branch to branch diff uses 2-dots
git diff A..B
# git diff name of changed files only
git diff A..B --name-only
# git merge B in A, without merge commit (if ff not possible then error):
git switch A
git merge B --ff-only
# Instruct git to always access remote repositories using ssh instead of https (useful for vscode golang)
git config --global url.git@github.com:.insteadOf https://github.com/
# cherry pick a commit's changes
git cherry-pick <commit hash>
# git delete tag locally and remotely
git tag -d tagname
git push --delete origin tagname
# git force push last local committed state to remote branch foo
git push origin --force HEAD:foo
# generic force push syntax
git push origin --force localbranch:remotebranch
# git revert a erroneous force push
git checkout master
git reset --hard origin/master@{1} # reset to the previous state of origin/master, as recorded by reflog
git push -f origin master

# use ssh auth o github
- generate ssh pairs and add them to agent
- copy entire content (including algo name and email)
- paste it as a new ssh key in section Setting -> SSH/GPG
- git remote set-url origin git@github.com:ORGA_OR_USER/REPO.git
// rename branch
git branch -m <oldname> <newname>
//tag
$ git tag -a v1.4 -m "my version 1.4"
//Fusionner i last commits from current branch
git rebase -i <onto-this-commit>
# rebase from begining
git rebase -i --root
//lines in repo:
git ls-files | wc -l
//reset to origin branch state
git reset --hard origin/foo
# `git reset --hard` is equivalent to `git reset --hard origin/<current branch>`
//init :
git init 
git remote add origin http/...
//si on importe : 
git pull origin master
//1st commit, idem for others:
git add .
git commit -m "Initial Commit"
git push origin master
# get short commit hash
git rev-parse --short <my_long_commit_hash>
//branch
# set upstream remote branch
git branch --set-upstream-to=origin/<branch>
git checkout (-b for creation only) <name>
git push --set-upstream origin <name>
# checkout a remote branch
git checkout -b branch-name origin/branch-name
// chackout a remote tag
git checkout tags/<name>
//sometime :
git push origin heads/<name>
//show refs :
git show-ref (to know if heads/... or sm is needed)
# show last sates of HEAD
git reflog
#show last commit log:
git log -1 --pretty=fuller
# show commit in branch b that are not in a:
git log a..b
# list branches containing commit foo123
git branch --contains foo123
# show commits history graph
git log --graph --oneline
//update change in .gitignore
git rm -r --cached .
AND:
git add .
git commit ...etc
//rename last commit and add current changes to it (--no-edit to skip message edition)
git commit --amend -m "New commit message"
# merge stashed changes with pulled (or just current) state
git stash apply
# git stash a specific path or filename
git stash push <path>
# co author commit
git commit -m "Refactor usability tests.
>
Co-authored-by: foo <foo@foobar.com>
Co-authored-by: bar <bar@foobar.com>"
// roll back to commit -X
$ git commit -m "Something terribly misguided"             # (1) reset to n-th last commit
# git reset HEAD^ is equivalent to git reset HEAD~1
$ git reset HEAD~X n                                        # (2)
<< edit files as necessary >>                              # (3)
$ git add .                                              # (4)
$ git commit -c CommitHashThatYouWantTheMessageAndAuthorship                               # (5)
# after rebasing, force git pull with stop if someone else pushed on the branch:

$ sudo git push origin branch-x --force-with-lease
# override local branch with remote

$ sudo git fetch --all
$ sudo git reset --hard origin/master

# Pull unitary commits onto current branch
git cherry-pick <commit-hash>

# git list files impacted by previous commit
git diff --name-only HEAD HEAD~1

# find a pattern of path in folder
find <path_to_folder> -type <d or t> -name '*middleisthis*'

# dump man output to file
man somecommand | col -b > ksh.txt

```

## Text edition
### Pandoc
```
c:/applications/anaconda2/scripts/pandoc --latex-engine=xelatex -H preamble.tex -V fontsize=12pt -V documentclass:book -V papersize:a4paper  -V classoption:openright --chapters --bibliography=papers.bib --csl="csl/nature.csl" title.md summary.md zusammenfassung.md acknowledgements.md toc.md "introduction/intro1.md" "introduction/intro2.md" chapter2_paper.md chapter3_extra_results.md chapter4_generaldiscussion.md appendix.md references.md -o "phdthesis.pdf"
c:/applications/anaconda2/scripts/pandoc .\plan.md -o plan.pdf  --read=markdown --latex-engine=xelatex
```

## airflow
### local airflow setup test
```bash
pip3 install apache-airflow
export AIRFLOW_HOME="~/airflow"
cd ~/airflow
mkdir dags
gedit ./dags/hello.py
```

`hello.py` content:
```python
from datetime import datetime
from airflow import DAG
from airflow.operators.python_operator import PythonOperator


dag = DAG('hello_world', description='Hello world example', schedule_interval='* * * * *', start_date=datetime(1970, 1, 1), catchup=False)

hello_operator1 = PythonOperator(task_id='hello_task1', python_callable=lambda: "hello 1", dag=dag)
hello_operator2 = PythonOperator(task_id='hello_task2', python_callable=lambda: "hello 2", dag=dag)
hello_operator1 >> hello_operator2
```

```bash
airflow db init
airflow webserver --port 8080 -D --pid
airflow scheduler
airflow dags trigger hello_world
airflow dags pause hello_world

AIRFLOW_HOME=~/airflow python -m airflow dags report -v 
```

## Kubernetes
```bash
# config GKE context
gcloud container clusters get-credentials <cluster_name> --region <region> --project <project>
# if former command failed with "get-credentials requires edit permissions on", run (cf https://stackoverflow.com/a/62957050/6580080)
gcloud config set container/use_client_certificate False
# get a shell to a running container (can replace --stdin --tty by -it)
kubectl exec --stdin --tty <pod_name> [-c <container_name>] -- /bin/bash
# list available contexts
kubectl config get-contexts
# Access Airflow's Kube executor webserver locally
kubectl port-forward svc/airflow-webserver 8080:8080 --namespace airflow
# deploy and expose (create service) externally a 1-container deployment
kubectl create deployment --image eu.gcr.io/playground-314008/example:0.0.1 example
kubectl expose deploy example --port=5000 --target-port=8080 --name=example-http-ext --type=LoadBalancer
# get logs from a pod (streamed real-time with -f)
kubectl --context ... --namespace ... logs -f <pod_name>
# get logs from all pods sharing a foo=bar label
kubectl --context ... -n ... logs -l foo=bar --tail 10000
# get all logs from deployment's pods
kubectl --context ... -n ... logs deployment/<deploy name>
# scale to 0 pods: noOp deployment
kubectl scale --replicas=0 deployment <deployment_name>
# edit and redeploy a deployment (vim-like edition)
kubectl edit deployment <deployment_name>
# redeploy a deployment without any change
kubectl rollout restart deployment <deployment_name>
# get the image of each pod of a deployment foo
kubectl get pods -o name | grep foo | xargs -I% kubectl describe % | grep Image
```

### macos
Shortcuts
```
# exit full screen
cmd + ctl + F
```

## helm

```bash
# list helm release 
helm -n <namespace> ls
# get values from a helm release
helm -n <namespace> get values re.lease
# get manifest 
helm -n <namespace> get manifest re.lease
```

## macos
```
# edit mouse sensitivity
defaults write -g com.apple.mouse.scaling 20
```