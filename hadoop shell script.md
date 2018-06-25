# hadoop
learning hadoop

--------------------------------------------------------------------------------
# shell basics | shell 基础
--------------------------------------------------------------------------------
ls -la #list all files
cd /home/user/..../ #enter a directory
cd ../ #enter parent directory
cd ./ #enter current directory
pwd # current directory
cp ../file.txt ./ #copy file.txt from parent dir to current directory

--------------------------------------------------------------------------------
# hadoop shell script | hadoop shell 语言
--------------------------------------------------------------------------------
# I am using amazon aws EMR cluster for hadoop mapreduce.
# I am using github classroom, my result files will be commit to github repo.
# Please correct me if you find mistakes. Thanks!

---------------
# basics | 须知
---------------
pwd # it tells you your "local" directory on hdfs, usually is:
/home/hadoop # Local path: this is usually where you put files.
# this is also where your github repo will be clone to.
# but your remote dir is the following:
/user/hadoop # HDFS DEST path: this is where hdfs clusters nodes store the large
# files that was copy from S3 to your clusters for mapreduce operations. You have
# to copy/move the file from here to your local in order to commit to github repo,
# if applicable.

#create dir | 造一个目录
hdfs dfs -mkdir /user/hadoop/dir1 # create a directory on hdfs
hadoop fs -mkdir /user/hadoop/dir1 # same

#list the content/files in dir1 | 列出目录内文件
hadoop fs -ls /user/hadoop/dir1

--------------------------
# file operation | 操作文件
--------------------------
#create files | 新建文件
hdfs dfs -touchz /user/hadoop/dir1/test.txt # create a new file in the directory
nano test.txt # edit the files
control + x -> (Y)# save it and exit

# read contents of a file | 查看文件内容
hdfs dfs -cat /user/hadoop/dir1/test.txt
hadoop fs -cat /user/hadoop/dir1/test.txt

#upload a file | 从“本地”上传文件到hdfs，这里的“本地”不是desktop，是hdfs的本地。
hadoop fs -put /home/hadoop/test.txt /user/hadoop/dir3 #upload test.txt file
# from <local file system path> to <HDFS destination path>
hadoop fs -copyFromLocal /home/hadoop/test.txt /user/hadoop/dir3
# Similar to put command, except that the source is restricted to a local file reference.

#download a file | 从hdfs下载文件到“本地”
hadoop fs -get /user/hadoop/dir3/test.txt /home/hadoop #download test.txt file
# from <HDFS destination path> to <local file system path>, local is not your
# desktop, but your hadoop local file system (master node).
hadoop fs -copyToLocal /user/hadoop/dir3/test.txt /home/hadoop
# Similar to get command, except that the destination is restricted to a local file reference.

#copy a file from source to destination | 拷贝文件到其他文档
hadoop fs -cp /user/hadoop/dir1/test.txt /user/hadoop/dir3
# this only work when both source and destination are on local or both on hdfs.

# move file from source to destination | 移动文件
hadoop fs -mv /user/hadoop/dir1/test.txt /user/hadoop/dir3

#rename file using -mv | 重命名文件
hadoop fs -mv /user/hadoop/dir1/test.txt /user/hadoop/dir1/renamed.txt
-----------------------------------------
#remove dir or delete file | 删除目录或文件
-----------------------------------------
hadoop fs -rm /home/user/folder1 #remove the dir, only success when the folder is empty
hadoop fs -rm -R /home/user/folder1 # recursively remove the dir with all files in it
hdfs dfs -rm -r /home/user/folder1/* # delete all files in folder1
hadoop fs -rm /user/hadoop/dir1/test.txt # delete a single file
-------------------------------
# read large files | 读取文件内容
-------------------------------
hadoop fs -cat test.txt | less # use arrow keys to navigate the file. (q) quit.
hadoop fs -cat test.txt | head n # look at the first n line of the file
# there is no "hadoop fs -head" command.
hadoop fs -tail test.txt | less # look at the last few line using arrow key to navigate
# however, it will be slow/inefficienct to use on a large file.
---------------------------
# file management | 文件管理
---------------------------
hadoop fs -du /user/hadoop/dir1/test.txt # see the length/size of the file
hadoop fs -count /user/hadoop/dir1/ # count the number of file under dir1
hadoop fs -df </user/root> (optional) # see the storage & available storage of the file system
------------------------------
# file permission | 文件权限管理
------------------------------
# when you type: "hdfs dfs -ls /", it will show the following result:

# -rwxrw-r-x    - hdfs   hdfs   0 2013-12-11 09:09 /data

#syntax:
# read(r), write(w), execute(x)
# read only: 4 (100), read + write: 6 (110), read + write + execute: 7 (111)
# - (r w -) (r w -) (r - -) >>>>>> - (1 1 0) (1 1 0) (1 0 0): what are they
# - (owner) (group) (other) >>>>>> - (r + w) (r + w) (r only):what they mean

#command:
hadoop fs -chmod 664 test.txt # change test.txt to -rw-rw-r
--------------------------------------------------------------------------------

--------------------------------------------------------------------------------
