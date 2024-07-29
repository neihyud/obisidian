---
title: Git

---
## Setup
- 1. `ssh-keygen`: để tạo một key mới
- 2. `eval "$(ssh-agent -s)"`
- 3. khi thêm một ssh key mới thì sử dụng:` ssh-add ~/.ssh/{name_file_ssh}` để add vào máy
- 4. add ssh public key vào github
## Overview
![[Pasted image 20240729211238.png]]

- Có 2 phần
    - index coi như **staging area**: nơi đặt các thay đổi tạm thời
    - object database

## **`Status file`** 
![[Pasted image 20240729211245.png]]


## **`Git log work`**
![[Pasted image 20240729211252.png]]

## **`Git restore`**
![[Pasted image 20240729211301.png]]

## **`Git remove`**: Object db no change
![[Pasted image 20240729211312.png]]

## **`Role HEAD`**
![[Pasted image 20240729173419.png]]
![[Pasted image 20240729173358.png]]

## **`Referencing commits using HEAD`**
    - **~**: parent specific
    
![[Pasted image 20240729211418.png]]


    - **^**: many parent

![[Pasted image 20240729211445.png]]
## **`Git reset`**: undo commit
- **--soft** => chuyển sang index nên chỉ cần một commit nữa để add vào object database (code => index, working = index)

![[Pasted image 20240729211456.png]]

- **--mixed** (~ **git reset**): index = object db (code => working directory, index = object db)

![[Pasted image 20240729211507.png]]
- **--hard**: (remove code => working = index = object db)

![[Pasted image 20240729211514.png]]

## **`Git revert`**: no change history commit
![[Pasted image 20240729211521.png]]

## Other
- **`--set-upstream (or -u)`**: telling Git that the branch it should be trackingin the remote (origin in this case)
- **REJECT**

![[Pasted image 20240729211537.png]]

- **git config pull.ff (fast-forward)**: code ở remote master branch giống với branch local (pull master => create new branch,kể từ lúc này mà code ở master remote không đổi và khi push new branch lên thì gọi là fast-forwardt)
- delete remove branch: 
![[Pasted image 20240729211544.png]]
- flag --grep "...": to search
- git bisect: để debug lỗi (ví dụ code trước nó chạy, n sau một thời gian có ai đó sửa gây lỗi => sử dụng nó để phát hiện commit nào làm nó lỗi)
    - cung cấp thông tin cho commit là bad hay good để tìm kiếm nhị phân ra commit gây lỗi
    - ex: `git bisect start HEAD HEAD~100`
---
# Set up and Initialization

- `git --version`
- `git cofig --global user.name "firstname lastname"`
- `git config --global user.email "valid email"`
- `git config --global core.editor "name_editor"`
- `git remote`: Show your current Git directory’s remote repository
- `git -v remote`: verbose output, -v: flag

# Ignoring Files

![](https://i.imgur.com/WC7UJrr.png)


# Staging
- khi sử đổi một tệp và đánh dấu nó trong lần commit tiếp theo của bạn => 1 file staged file
- `git status`: check the status of your git
- `git add my_file`: add file specific
- `git add .`: add all
- `git reset my_file`: remove file from staging while retaining changes within your working directory

# Committing

- `git commit -m "Commit message"`: 
- `git commit --amend -m "New commit message"`: If you need to modify your commit message, you can do so with the --amend flag

# Branches

- `git branch -a`: get all local branch ( flag: -a)
- `git branch -r`: get all remote branch
- `git branch _new-branch`: new branch
- `git checkout _another-branch`: switch to any existing branch
- `git checkout -b _new-branch`: create and switch 
- `git branch -m _current-branch-name _new-branch-name`: rename current branch
- `git branch -d branch-name`: Khi bạn đã hợp nhất một nhánh và không cần nhánh đó nữa, bạn có thể xóa nhánh đó
- `git branch -D branch-name`: Nếu bạn chưa hợp nhất một nhánh với nhánh chính, nhưng chắc chắn muốn xóa nó, bạn có thể buộc xóa một nhánh

# Collaborate and Update

- `git pull`: Fetch and merge any commits from the tracking remote branch
- `git fetch`: get branch from remote

# Inspecting

- `git log`: Display the commit history for the currently active branch
- `git log --oneline`: hiển thị log ở một dòng`
- `git log --follow my_script.py`: Show the commits that changed a particular file. This follows the file regardless of file renaming
- `git log a-branch..b-branch`: show commits on `a-branch` that are not on `b-branch`
- `git reflog`: Look at reference logs (reflog) to see when the tips of branches and other references were last updated within the repository
- `git show _code`: Show any object in Git via its commit string or hash in a more human-readable format:

# Show Changes

- `git diff`: show change between commits, branches, ...
- `git diff --staged`: Compare modified files that are on the staging area
- `git diff a-branch..b-branch`: Display the diff of what is in a-branch but is not in b-branch
- `git diff 61ce3e6..e221d9c`: Show the diff between two specific commits
- `git rm file`: 

# Merge
- git merge để hợp nhất hai nhánh branch với nhau mà các commit vẫn sắp xếp theo mặc định. 
- git rebase hợp nhất các commit theo thứ tự thời gian

# Rebase
- Cách hoạt động của git rebase tương tự như git merge, nhưng thay vì tạo một commit gộp (merge commit), rebase tạo ra các commit mới trên nhánh đích, dẫn đến lịch sử commit trông như một dãy các commit liên tiếp.

- git rebase -i HEAD~2: Để kết hợp (combine) 2 commit thành 1 commit duy nhất
- -i: chế độ tương tác
```javascript=
Sau đó, ta chọn commit thứ 2 và sửa command của nó thành "squash" hoặc "fixup" (thay "pick" bằng "squash" hoặc "fixup"), rồi lưu và đóng file.

Nếu chọn "squash", thì Git sẽ kết hợp commit thứ 2 vào commit đầu tiên, và mở ra giao diện để sửa lại message cho commit mới kết hợp này.
Nếu chọn "fixup", thì Git sẽ kết hợp commit thứ 2 vào commit đầu tiên, nhưng không mở ra giao diện sửa message, mà lấy message của commit đầu tiên.
Sau khi lưu và đóng file rebase, Git sẽ tự động kết hợp 2 commit lại với nhau, và tạo ra 1 commit mới có message tùy chỉnh nếu ta sử dụng "squash". 
Ta cần push lại nhánh của mình sau khi hoàn thành quá trình này.
```
- `git rebase --continue`: Nếu bạn đang ở trạng thái interactive rebase mode và đã giải quyết xong xung đột, bạn cần chạy lệnh này để tiếp tục rebase và hoàn thành quá trình.

# Other

![](https://i.imgur.com/H9Mi7w2.png)

- Thuật ngữ:
	+ Working directory: thư mục làm việc
	+ Staging Environment: nơi lưu trữ các thay đổi (sử dụng git commit -m"" để lưu vào Repository)
	    + commit  coi là một 'save point'
	    + -m: message, bắt buộc
	+ Repository : muốn xem thì sử dụng git log)
- `git show + id` : xem content của một file
- `git checkout -b <name_branch>`: tạo branch
    - -b tạo và switch
    - -d xóa
- `git merge <name_branch>`: chuyển branch B về A ( A <--- B ) (gộp B về A)
	+ trước khi làm p kiểm tra xem đg ở branch nào ( git branch ), chuyển về branch chính (branch cần gộp - A) ( git checkout A )
- `git fetch`: nhận tất cả lịch sử thay đổi của một repo được theo dõi, pull là sự kết hợp của fetch và merge. Nó được sử dụng để kéo tất cả các thay đổi từ kho lưu trữ từ xa vào nhánh mà bạn đang làm việc.
- `git revert <id_commit>` : khi muốn cho một commit cũ tạo thành một commit mới mà vẫn giữ nguyên được lịch sử
![](https://i.imgur.com/LU11fuK.png)
- `git reset --soft <id_commit>` (id_commit: git log): chuyển một commit về staging area
- `git reset --mixed <id_commit>` (id_commit: git log): chuyển một commit về working directory
	==> áp dụng cho commit trước (soft, mix)
- `git log --oneline`: hiển thị log ở một dòng
- `git revert HEAD`: trở về commit mới nhất
- `git revert HEAD~x`: x là một số, 1. quay lại thêm một lần
- `*.gitignore` : từ chối một số file k muốn commit
- `git push origin <name_branch>`: push branch lên github
- `git diff`: xem những thay đổi của file đã được định nghĩa
- `git diff branch1..branch2`: xem sự thay đổi giữa hai nhánh
- `gitk` : để mở giao diện cho dễ
- `git checkout --<name_file>`: xóa bỏ thay đổi một số file khi chưa lên staging area
- `git restore <name_file>`: cho file từ staging area về working directory 
- `git branch`: hiển thị các branch (các nhánh) của project ( dấu * hiển thị branch hiện tại)

# Learn

![](https://i.imgur.com/ckcuEUb.png | 400)

