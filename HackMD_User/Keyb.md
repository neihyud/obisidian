---
title: Keyb

---

# Keyb

# Port
- lsof -i :name_port
- kill -9 name_id (từ lsof)

# Terminal
	Ctrl + W: xóa nhan theo từ
	Ctrl + L: clear teminal
	Ctrl + mũi tên: di chuyển nhanh
	Ctrl + Shift + T: tạo tab ms
	Ctrl + D: Đóng tab

# Git 
	git add .	git status     git commit -m""     git push origin master (main)
- git push --set-upstream origin main
- Sửa lỗi git khi clone 2 acc, vào Credential Manager, remove git

- 	(error ! [rejected] )
	* git pull --rebase

	git fetch origin master
	git merge  master
	git fetch origin master:tmp
	git rebase tmp
	git push origin HEAD:master
	git branch -D tmp
	
- Window + Shift + S: cắt ảnh màn hình
- Window + E: mở file explorer
- Window + V: bộ nhớ tạm
- Window + G: quay màn hình
- Window + H: nhập văn bản bằng giọng nói

# CSS
	<script
      src="https://kit.fontawesome.com/c909c23c5d.js"
      crossorigin="anonymous"
    ></script>

# Prettier
	congig ở extendsion
# Heroku
git subtree push --prefix name_folder_backend heroku master

# WSL2
kill $(lsof -t -i:8080)

Remove folder: rm -R folder_name
Remove file: rm name_file
Permission: sudo chown -R username path (sudo chown -R neihyud home/neihyud/Workspace 

cd + tab + tab == ls: hiển thị ds các folder bên trong

# PHP
php artisan serve
composer create-project laravel/laravel name_project

# FontWS
	<script src="https://kit.fontawesome.com/183e446a4f.js" crossorigin="anonymous"></script>

# MySQL
   *Window
	1. Go to Task manager
	2. Select Services tab
	3. Find MySql service
	4. Running
   *WSL
	show databases;
	sudo service mysql start
	sudo mysql

	sudo /etc/init.d/mysql start
	mysql -u root -p

# MongoDb
	DATABASE=mongodb+srv://double:<PASSWORD>@cluster0.vkmhm.mongodb.net/natours?retryWrites=true
	DATABASE_LOCAL=mongodb://localhost:27017/natours
	DATABASE_PASSWORD=Rscs3kHDfnfRxUqv
# Jenkins (có thể vào service để khởi chạy)
	D:\Programs\Jenkins
	java -jar jenkins.war --httpPort=8088



# Short Keyboard

Windows + ALT + R: quay màn hình
Ctrl+Alt+Shift+S: thay đổi SDK trong intelliJ
Ctrl + Enter: new line
Ctrl + Shift + F: format visual
Ctrl + Alt + L: format intelliJ
Alt + (PgUp hoặc PgDown) : bôi đen đa chữ và di chuyển lên xuống code
Ctrl + D: tạo con trỏ chuột lần lượt các từ có giống với phần bôi đen
Ctrl + Shift + L: tạo con trỏ chuột ở tất cả các từ giống với phần bôi đen
Ctrl + Shift: bôi đen một dòng
Shift + Alt + click left: select cho nhiều dòng
Alt + chuột: chèn con trỏ
bôi đen + ctrl + shift + L: để chọn đa con trỏ có nội dung giống nhau
Shift + click chuột để bôi đen
Shift + Tab: lùi tab
Shift + lăn chuột: thu nhỏ responsive chrome