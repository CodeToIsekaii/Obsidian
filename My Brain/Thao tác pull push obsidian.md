cd /storage/emulated/0/Documents/ObsidianVault

 git config --global --add safe.directory /storage/emulated/0/Documents/ObsidianVault

 git pull origin main

 Nếu bạn muốn khi git pull *luôn lấy phiên bản mới nhất từ GitHub về và ghi đè hoàn toàn lên bản cũ trên máy*, thì đây là cách làm:

---

### *Cách 1: Hard reset về remote (cực mạnh – xóa luôn thay đổi local)*

git fetch origin
git reset --hard origin/main

- git fetch origin: Lấy dữ liệu mới từ GitHub về (chưa áp dụng).
    
- git reset --hard origin/main: Ghi đè toàn bộ nhánh hiện tại bằng phiên bản trên GitHub.
    

*Lưu ý*: Mọi thay đổi chưa commit ở local *sẽ bị mất vĩnh viễn*.

---

### *Cách 2: Xóa thư mục local rồi clone lại (cách bạn từng làm)*

rm -rf ObsidianVault
git clone https://github.com/CodeToIsekaii/Obsidian.git 

---

### Gợi ý dùng cách 1 nếu bạn chỉ muốn "làm mới thư mục" mà không cần clone lại.


git add .
git commit -m "update"
git push origin main