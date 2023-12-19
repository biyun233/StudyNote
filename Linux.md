

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

## Listing Files and Understanding ls output

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

## Permissions

![linux17](img/linux17.jpeg)![linux18](img/linux18.jpeg)  

### files vs directories

![linux19](img/linux19.jpeg)

### Permission categories

![linux20](img/linux20.jpeg)

### Groups

![linux21](img/linux21.jpeg)

### Secret Decoder Ring

![linux22](img/linux22.jpeg)

### Changing Permissions

![linux23](img/linux23.jpeg)

### Numeric based permissions

![linux24](img/linux24.jpeg)

![linux25](img/linux25.jpeg)

### Order has meaning

![linux26](img/linux26.jpeg)



### Working with Groups

![linux27](img/linux27.jpeg)

### Displaying the content of files

![linux28](img/linux28.jpeg)

### Head and tail

![linux29](img/linux29.jpeg)

## Editing files in Vi

![linux30](img/linux30.jpeg)

![linux31](img/linux31.jpeg)

![linux32](img/linux32.jpeg)

![linux33](img/linux33.jpeg)

![linux34](img/linux34.jpeg)

![linux35](img/linux35.jpeg)

![linux36](img/linux36.jpeg)

### Finding files

![linux37](img/linux37.jpeg)

![linux38](img/linux38.jpeg)

![linux39](img/linux39.jpeg)

```sh
find . -size +1M
```

## Actions to files

### Removing files

![linux40](img/linux40.jpeg)

### Copying files

![linux41](img/linux41.jpeg)![linux42](img/linux42.jpeg)

```sh
cp -i # prompt before overwrite
```

### Moving and Renaming files

![linux43](img/linux43.jpeg)

### Sorting files

![linux44](img/linux44.jpeg)

### Disk usage

![linux45](img/linux45.jpeg)

### Input/Output types

![linux46](img/linux46.jpeg)

### Redirection![linux47](img/linux47.jpeg)

![linux48](img/linux48.jpeg)

### Comparing files![linux49](img/linux49.jpeg)

![linux50](img/linux50.jpeg)

### Searching in files and using pipes

- ### grep![linux51](img/linux51.jpeg)![linux52](img/linux52.jpeg)

- ### The file commande![linux53](img/linux53.jpeg)

- ### Pipes![linux54](img/linux54.jpeg)

- ### Cut

  ![linux57](img/linux57.jpeg)

  ![linux55](img/linux55.jpeg)

- ### Strings

  ![linux56](img/linux56.jpeg)

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

![linux58](img/linux58.jpeg)![linux59](img/linux59.jpeg)



### Customizing the shell prompt

![linux60](img/linux60.jpeg)

- ### Customizing the Prompt with PS1

![linux61](img/linux61.jpeg)

![linux62](img/linux62.jpeg)

![linux63](img/linux63.jpeg)

### Shell aliases

![linux64](img/linux64.jpeg)

![linux65](img/linux65.jpeg)

## Environment variables

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

## Processes and job control

- ### Listing processes and information

  ![linux66](img/linux66.jpeg)

   ![linux67](img/linux67.jpeg)

- ### Other ways to view processes

![linux68](img/linux68.jpeg)

- ### Background and foreground processes

  ![linux69](img/linux69.jpeg)
  
  ![linux70](img/linux70.jpeg)



- ### Killing processes

  ![linux71](img/linux71.jpeg)



### Scheduling repeated jobs with cron![linux73](img/linux73.jpeg)

![linux74](img/linux74.jpeg)

![linux75](img/linux75.jpeg)

### Crontab command

![linux76](img/linux76.jpeg)

## Installing software

### APT - Advanced packaging tool

![linux77](img/linux77.jpeg)![linux78](img/linux78.jpeg)



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

