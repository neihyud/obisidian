---
title: Hệ điều hành
tags: [Devops]

---
	
###### tags: `Devops`
# Hệ điều hành
# 1. Điều hướng hệ thống
## Giao diện đồ họa
![](https://hackmd.io/_uploads/HJXzsmwE3.png)

































- `/etc/environment`

- rm -rf ~/.config/dconf/user && sudo reboot: reset all UI về ban đầu

## File
### MIME Types
- tiêu chuẩn để phân loại file trên Internet
- mime types chứa hai phần: 
    - type
    - subtype
### Xác định loại MIME của File
- By File Extension
- By File Content: chậm hơn file extension nhưng đáng tin cậy hơn https://en.wikipedia.org/wiki/List_of_file_signatures
- Kết hợp hai cách

```xml=
<?xml version="1.0" encoding="UTF-8"?>
<mime-info xmlns="http://www.freedesktop.org/standards/shared-mime-info">
    <mime-type type="image/png">
        <comment xml:lang="en">PNG image</comment>
        <comment xml:lang="af">png beeld</comment>
        ...
        <magic priority="50">
            <match type="string" value="\x89PNG" offset="0"/> ==> xác định nội dung file
        </magic>
        <glob pattern="*.png"/> => check extension
    </mime-type>
</mime-info>
```

# Linux Filesystem Guide
## /opt
- dành riêng cho việc cài đặt các gói phần mềm ứng dụng bổ trợ (k p là một phần của hệ thống)

## /usr/local
- Dành cho quản trị viên hệ thống sử dụng khi cài đặt phần mềm cục bộ

# Command Linux
## sudo
-  The Linux system will log a timestamp as a tracker, By default, every root user can run sudo commands for 15 minutes/session.
-  Option:
    -  **-k** or **–reset-timestamp** invalidates the timestamp file.
    -  **-g** or **–group**=group runs commands as a specified group name or ID
    -  **-h** or **–host**=host runs commands on the host.
## pwd
- find path of current working directory
- Option
    - **-L** or **–logical** prints environment variable content, including symbolic links.
    - **-P** or **–physical** prints the actual path of the current directory.
## cd
- navigate 
- shortcut
    - **cd ~[username]**: go to home directory
    - **cd ..**: move on directory up
    - **cd -**: move to your previos directory
## ls
- lists file and directories, running it without a flag or parameter will show the current working directory’s content.
- options
    - **ls -R** lists all the files in the subdirectories.
    - **ls -a** shows hidden files in addition to the visible ones.
    - **ls -lh** shows the file sizes in easily readable formats, such as MB, GB, and TB.

## cat
- lists, combines, and writes file content to the standard output. 
- use
    - **cat > [path_file]**: create a new file
    - **cat [file1] [file2] > [file3]**: merge file1 and file2 => file3
    - **tac filename.txt** displays content in reverse order.
## cp
- copy files or directories and their content
- **cp [file_name ...] [path_folder]**: copy file_name sang destination directory
- **cp -R [path_folder1] [path_folder2]**: copy entire directory
## mv
- move, rename file and directories
- **mv [file_name] [path_folder]**: move
- **mv [old_file_name] [new_file_name]**: rename
## mkdir
- create 1 or many dirctories and set permission for each of them
- **mkdir folder/subfolder/...**: folder exist
- option: 
    - **-p** or -parents: create a directory between two existing folders.
        - mkdir -p a/b/c => tạo 3 folder a, b, c
    - **-m**: sets the file permissions.
    - **-v**: prints a message for each created directory.
## rmdir
- remove **empty directory**, should have **sudo** privaties in the parents directory
- **rmdir -p mydir/personal1**: remove subdirectory, **-p**
    - **-p**
## rm
- delete file
- **rm [file_name ...]**: can remove multipe file
- options: 
    - **-i** prompts system confirmation before deleting a file.
    - **-f** allows the system to remove without a confirmation.
    - **-r** deletes files and directories recursively.
## touch
- create new file or generate and modify a timestamp in the Linux command line
## locate
- find file in the database system
## find
- tìm kiếm trong thư mục cụ thê
- **- find [option] [path] [expression]**
- **find /home -name notes.text**:  you want to look for a file called notes.txt within the home directory and its subfolders:
- **find -name filename.txt** to find files in the current directory.
- **find ./ -type d -name directoryname** to look for directories.
## grep
- tìm kiếm một từ bằng cách tìm kiếm trong tất cả các văn bảng trong một tệp cụ thể
## df 
- báo cáo mức sử dụng dung lượng của ổ đĩa
- **df [options] [file]**
- **df -h**: mức sử dụng dung lượng ổ đĩa hê thống của thư mục hiện tại
- **df -m** displays information on the file system usage in MBs.
- **df -k** displays file system usage in KBs.
- **df -T** shows the file system type in a new column.
## du
- check 1 file hoặc một folder sử dụng bao nhiêu dung lượng
- option:
    - -s: total size of a specified folder
    - -m: provides folder and file infomation in MB
    - -h: thông báo ngày sửa đổi cuối cùng folder và file
## head
- cho phép xem 10 dòng đầu tiên của văn bản
## tail
- cho phép xem 10 dòng cuối cùng của văn bản
## diff
- so sánh nội dung 2 tệp theo từng dòng
- **diff [option] file1 file2**
- options:
    - **-c** displays the difference between two files in a context form.
    - **-u** displays the output without redundant information.
    - **-i** makes the diff command case insensitive.
## tar
- lưu trũ nhiều tệp vào một file, giống với nén file
- **tar [options] [archive_file] [file or directory to be archived]
- **tar [options] [archive_file] [file or directory to be archived]
- **tar -cvf newarchive.tar /home/user/Documents**: tạo một TAR archive mới trong thư mục hiện tại, chứa tất cả file trong home/user/Documents
- options:
    - **-x** extracts a file.
    - **-t** lists the content of a file.
    - **-u** archives and adds to an existing archive file.
## chmod
- sửa đổi quyền đọc, ghi và thực thi của tệp hoặc thư mục.
- **chmod [option] [permission] [file_name]**
- option:
    **-c** or **–changes** displays information when a change is made.
    **-f** or **–silent** suppresses the error messages.
    **-v** or **–verbose** displays a diagnostic for each processed file.
- example:
    - chmod u=rwx, g=rx, o=r name_file
        - u: user
        - g: group
        - other: r
        - rwx: read(4), write(2), execute(1), no permission(0)
## chown
- sửa đổi quyền sở hữu của file, directory thành username chỉ định
- **chown [option] owner[:group] file(s)**
## jobs
- hiển thị tiến trình và các trạng thái của chúng
## kill
- chấm dứt chương trình không phản hồi theo hướng thủ công
- **ps ux**: list PID
- **kill [signal_option] pid**
    - signal_option:
        - **SIGTERM** yêu cầu một chương trình ngừng chạy và cho nó một khoảng thời gian để lưu tất cả tiến trình của nó. Hệ thống sẽ sử dụng điều này theo mặc định nếu bạn không chỉ định tín hiệu khi nhập lệnh tiêu diệt.
        - **SIGKILL** buộc các chương trình dừng lại và bạn sẽ mất tiến trình chưa được lưu.
## ping
- kiểm tra mạng hoặc server có truy cập được hay không
- **ping [option] [hostname_or_IP_address]**
## wget
- cho phép install file từ internet, tg tự curl 
- **wget [options] [url]**
## uname
- in ra thông tin chi tiết về system and hardware
- **uname [option]**
    - **-a** prints all the system information.
    - **-s** prints the kernel name.
    - -n**** prints the system’s node hostname.
## top
- hiển thị tất cả process đang chạy và dynamic real-time view of the current system, It sums up the resource utilization, from CPU to memory usage.
## history
- list history 500 previously executed command
- **history [option]**: 
    - **-c**: clear the complete history list
    - **-d offset**: deletes the history entry at the OFFSET position.
    - **-a**: appends history lines
## man
- cung cấp hướng dẫn về sử dụng command hoặc tiện ích sử dụng trên 
- **man [command_name]**
    - command_name: node, ls, cd, ...
## echo
- hiển thị đầu ra tiêu chuẩn
## zip, unzip
- nén các tệp thành tệp ZIP
- **zip [options] {zipfile} file1 file2….**: nén file1, file2 thành tệp zipfile (name_file_zip)
    - -r nén folder
- **unzip [option] file_name.zip**: 
## hostname
- biết về tên hệ thống
- **hostname -i**: displays the machine’s IP address.
## apt-get
- công cụ xử lý các thư viện Advanced Package Tool (APT)
- yêu cầu sử dung **sudo** hoặc quyền root
- **apt-get [options] (command)**
    - **update** synchronizes the package files from their sources.
    - **upgrade** installs the latest version of all installed packages.
    - **check** updates the package cache and checks broken dependencies.
## nano, vi
- The process status or ps command produces a snapshot of all running processes in your system, The static results are taken from the virtual files in the /proc file system.