

# Linux



### Common Directories

![linux1](img/linux1.jpeg)

### Comprehensive Directory Listing

![linux2](img/linux2.jpeg)

![linux3](img/linux3.jpeg)

![linux4](img/linux4.jpeg)

![linux5](img/linux5.jpeg)

![linux6](img/linux6.jpeg)

### Basic Linux Commands

![linux7](img/linux7.jpeg)

### Navigating Man pages

```shell
ENTER # move down one line
SPACE # move down one page
g # move to the top of the page
G # move to the bottom of the page
q # quit
```

### Environmental Variables

- PATH

  ![linux8](img/linux8.jpeg)

### Directory Shortcuts

![linux9](img/linux9.jpeg)



### Creating and removing directories

![linux10](img/linux10.jpeg)

```sh
# [-p]: create the parent directories
mkdir -p dir1/dir2
#--dir1		
#		|--dir2

# rmdir only removes directories that are empty
```



# Listing Files and Understanding ls output

### Decoding ls -l output

![linux11](img/linux11.jpeg)

### Listing All files, including hidden files

![linux12](img/linux12.jpeg)

### Listing files by type

![linux13](img/linux13.jpeg)

![linux14](img/linux14.jpeg)

### Listing files by time and in reverse

![linux15](img/linux15.jpeg)

### Listing files recursively

ls -R



### The tree command

![linux16](img/linux16.jpeg)



# Permissions

![linux17](img/linux17.jpeg)![linux18](img/linux18.jpeg)  

### files vs directories

![linux19](img/linux19.jpeg)

### Permission categories

![linux20](img/linux20.jpeg)

### Groups

![linux21](img/linux21.jpeg)

### Secret Decoder Ring

![截屏2021-12-06 11.37.10](https://tva1.sinaimg.cn/large/008i3skNly1gx3ylomkcrj30q007w74m.jpg)

### Changing Permissions

![截屏2021-12-06 11.38.03](https://tva1.sinaimg.cn/large/008i3skNly1gx3ymbo7tvj30ni0bdaan.jpg)

### Numeric based permissions

![截屏2021-12-06 11.43.28](https://tva1.sinaimg.cn/large/008i3skNly1gx3ytded6ij30nx08z0sx.jpg)

![截屏2021-12-06 11.45.26](https://tva1.sinaimg.cn/large/008i3skNly1gx3yujyt66j30r40dagnc.jpg)



### Order has meaning

![截屏2021-12-06 11.45.44](https://tva1.sinaimg.cn/large/008i3skNly1gx3yvhlp6xj30of09bweo.jpg)



### Working with Groups

![截屏2021-12-06 11.52.30](https://tva1.sinaimg.cn/large/008i3skNly1gx3z1dk3fnj30p203ywer.jpg)



### Displaying the content of files

![截屏2021-12-06 13.09.59](https://tva1.sinaimg.cn/large/008i3skNly1gx419yztn5j30pz0bdgn6.jpg)



### Head and tail

![截屏2021-12-06 13.10.16](https://tva1.sinaimg.cn/large/008i3skNly1gx41ai5uhmj30k706t3z0.jpg)



# Editing files in Vi

![截屏2021-12-06 13.18.35](https://tva1.sinaimg.cn/large/008i3skNly1gx41jinwfkj30hh0bigmc.jpg)![截屏2021-12-06 13.19.25](https://tva1.sinaimg.cn/large/008i3skNly1gx41juoj8oj30md07vq3q.jpg)![截屏2021-12-06 13.19.45](https://tva1.sinaimg.cn/large/008i3skNly1gx41kac19nj30kq0aiwfc.jpg)

![截屏2021-12-06 13.20.12](https://tva1.sinaimg.cn/large/008i3skNly1gx41kmgk3hj30my09faaw.jpg)

![截屏2021-12-06 15.47.31](https://tva1.sinaimg.cn/large/008i3skNly1gx45u39utyj30no07sgm4.jpg)![截屏2021-12-06 15.47.52](https://tva1.sinaimg.cn/large/008i3skNly1gx45u8cl2oj30pw0af3zn.jpg)

![截屏2021-12-06 15.48.29](https://tva1.sinaimg.cn/large/008i3skNly1gx45uv9ts1j30ns043wes.jpg)



### Finding files

![截屏2021-12-07 11.04.33](https://tva1.sinaimg.cn/large/008i3skNly1gx539siq8cj30pd08s0tp.jpg)

![截屏2021-12-07 11.04.49](https://tva1.sinaimg.cn/large/008i3skNly1gx53a0olu1j30p00br75a.jpg)

![截屏2021-12-07 11.05.05](https://tva1.sinaimg.cn/large/008i3skNly1gx53abyy5pj30pv0ahq48.jpg)

```sh
find . -size +1M
```

#  Actions to files

### Removing files

![截屏2021-12-07 11.37.05](https://tva1.sinaimg.cn/large/008i3skNly1gx548hekunj30px0br759.jpg)

### Copying files

![截屏2021-12-07 11.38.49](https://tva1.sinaimg.cn/large/008i3skNly1gx549vqzoej30p6093js9.jpg)

![截屏2021-12-07 11.39.42](https://tva1.sinaimg.cn/large/008i3skNly1gx54ab6zrqj30pl092wf5.jpg)

```sh
cp -i # prompt before overwrite
```

### Moving and Renaming files

![截屏2021-12-07 11.44.54](https://tva1.sinaimg.cn/large/008i3skNly1gx54g3w5c9j30pw09aaam.jpg)



### Sorting files

![截屏2021-12-07 11.47.33](https://tva1.sinaimg.cn/large/008i3skNly1gx54igr9epj30o809v3yy.jpg)



### Disk usage

![截屏2021-12-07 11.52.41](https://tva1.sinaimg.cn/large/008i3skNly1gx54ocz9xuj30pe0940t9.jpg)



### Input/Output types

![截屏2021-12-07 13.47.46](https://tva1.sinaimg.cn/large/008i3skNly1gx5806od1lj30qn08vgm3.jpg)



### Redirection

![截屏2021-12-07 13.48.37](https://tva1.sinaimg.cn/large/008i3skNly1gx580g4accj30oj0bb0tr.jpg)

![截屏2021-12-07 13.49.02](https://tva1.sinaimg.cn/large/008i3skNly1gx580vzfqmj30qc09h3za.jpg)



### Comparing files

![截屏2021-12-07 14.00.33](https://tva1.sinaimg.cn/large/008i3skNly1gx58cwpjopj30qe08pt9d.jpg)

![截屏2021-12-07 14.04.50](https://tva1.sinaimg.cn/large/008i3skNly1gx58hl58qmj30q8076dgm.jpg)



### Searching in files and using pipes

- ### grep

![截屏2021-12-07 14.40.15](https://tva1.sinaimg.cn/large/008i3skNly1gx59i7v69wj30p807jmxg.jpg)

![截屏2021-12-07 14.40.31](https://tva1.sinaimg.cn/large/008i3skNly1gx59ig4ui9j30p207iab4.jpg)

- ### The file commande

![截屏2021-12-07 14.46.13](https://tva1.sinaimg.cn/large/008i3skNly1gx59oe9mooj30p90bkgmn.jpg)

- ### Pipes

  ![截屏2021-12-07 14.48.56](https://tva1.sinaimg.cn/large/008i3skNly1gx59r75pfqj30nc07dmxc.jpg)

- ### Cut

  ![截屏2021-12-07 14.52.35](https://tva1.sinaimg.cn/large/008i3skNly1gx59vhvxh4j30ol05wjrr.jpg)

  ![截屏2021-12-07 14.52.40](https://tva1.sinaimg.cn/large/008i3skNly1gx59vikr0pj30ow0770t5.jpg)

- ### Strings

  ![截屏2021-12-07 14.53.34](https://tva1.sinaimg.cn/large/008i3skNly1gx59wbsz4oj30mm02wt8r.jpg)

```sh
cat file2 | grep -i tot | head -1
# total 0
cat file2 | grep -i tot | head -1 | cut -d ' ' -f 1
# total
```

- ### tr

```sh
cat file2 | grep -i tot | head -1
# total 0
cat file2 | grep -i tot | head -1 | tr "0" "1"
# total 1
```

### Transferring and copying files over the network

- ### copying files over the network

![截屏2021-12-08 09.00.55](https://tva1.sinaimg.cn/large/008i3skNly1gx65bios8zj30l704qgls.jpg)

![截屏2021-12-08 09.08.34](https://tva1.sinaimg.cn/large/008i3skNly1gx65jgjggij30pl08ydgh.jpg)



### Customizing the shell prompt

![截屏2021-12-08 09.24.00](https://tva1.sinaimg.cn/large/008i3skNly1gx663cju02j30og05et93.jpg)

- ### Customizing the Prompt with PS1

![截屏2021-12-08 09.19.59](https://tva1.sinaimg.cn/large/008i3skNly1gx65vdmsysj30pe0b90to.jpg)

![截屏2021-12-08 09.21.37](https://tva1.sinaimg.cn/large/008i3skNly1gx65x17tjpj30ot0b3aba.jpg)

![截屏2021-12-08 09.23.34](https://tva1.sinaimg.cn/large/008i3skNly1gx65z20dgzj30cr03jwes.jpg)

https://camo.githubusercontent.com/47b6ae8d151556713dfd88efb9134b602918af66ab7ebcbd8d86b03919e9452b/68747470733a2f2f747661312e73696e61696d672e636e2f6c617267652f30303876787667476c79316838706a3877386a377a6a333066783063797461692e6a7067

### Shell aliases



![截屏2021-12-08 09.28.44](https://tva1.sinaimg.cn/large/008i3skNly1gx664ffxi7j30kt094t99.jpg)

![截屏2021-12-08 09.28.56](https://tva1.sinaimg.cn/large/008i3skNly1gx664n8mamj30p807jmxj.jpg)



# Environment variables

### Viewing

```sh
printenv
printenv $HOME
echo $HOME
```

### Creating

```sh
export EDITOR="vi"
```

### Removing

```sh
unset EDITOR
```

# Processes and job control

- ### Listing processes and information

  ![截屏2021-12-08 09.50.58](https://tva1.sinaimg.cn/large/008i3skNly1gx66rjfyqej30ha027t8n.jpg)

![截屏2021-12-08 09.51.11](https://tva1.sinaimg.cn/large/008i3skNly1gx66rqr3znj30pq07naat.jpg)

![截屏2021-12-08 09.53.06](https://tva1.sinaimg.cn/large/008i3skNly1gx66tr6cq6j30q5096ab7.jpg)

- ### Other ways to view processes

![截屏2021-12-08 09.53.34](https://tva1.sinaimg.cn/large/008i3skNly1gx66u9uwbaj30pi054js0.jpg)

- ### Background and foreground processes

  ![截屏2021-12-08 10.02.33](https://tva1.sinaimg.cn/large/008i3skNly1gx673khmsqj30pl050dgk.jpg)

![截屏2021-12-08 10.03.21](https://tva1.sinaimg.cn/large/008i3skNly1gx674emq69j30nw0b6jsd.jpg)

- ### Killing processes

  ![截屏2021-12-08 10.04.22](https://tva1.sinaimg.cn/large/008i3skNly1gx675hdeg2j30pz0b4wff.jpg)



### Scheduling repeated jobs with cron

![截屏2021-12-08 10.29.32](https://tva1.sinaimg.cn/large/008i3skNly1gx67vymxcjj30oj0b4jsd.jpg)

![截屏2021-12-08 10.31.09](https://tva1.sinaimg.cn/large/008i3skNly1gx67xbyik2j30qn0a3my8.jpg)

![截屏2021-12-08 10.35.51](https://tva1.sinaimg.cn/large/008i3skNly1gx682e23j9j30hi0bf3yu.jpg)

### Crontab command

![截屏2021-12-08 10.36.21](https://tva1.sinaimg.cn/large/008i3skNly1gx682r7bq8j30pa06xdgr.jpg)



# Installing software

### APT - Advanced packaging tool

![截屏2021-12-08 11.50.44](/Users/Biyun/Library/Application Support/typora-user-images/截屏2021-12-08 11.51.44.png)

![截屏2021-12-08 11.52.27](https://tva1.sinaimg.cn/large/008i3skNly1gx6a9y9z1mj30p708t0tg.jpg)

![截屏2021-12-08 11.54.04](https://tva1.sinaimg.cn/large/008i3skNly1gx6abm7c06j30ot08874w.jpg)



## Commands

### RSYNC

- 常用选项
  - -a 包含 -rtplgoD
  - -r 同步目录时要加上
  - -t 保持文件的时间属性
  - -p 保持文件的权限属性
  - -l 保留软链接
  - -g 保持文件的属组
  - -o 保持文件的属主
  - -D 保持设备文件信息
  - -L 加上该选项后，同步软链接时会把源文件一起同步
  - -v 同步时显示信息
  - --delete 删除DEST中SRC没有的文件
  - --exclude 过滤指定文件，如 --exclude “logs” 会把文件名包含logs的文件或目录过滤掉，不同步
  - -P 显示同步过程，比如速率，比 -v 更加详细
  - -u 如果DEST中的文件比SRC新，则不同步
  - -z 传输时压缩

​		

```sh
rsync SRC/ DEST/

# 模拟同步过程
rsync -av --dry-run SRC/ DEST/
```

