# Bài 2: Cấu hình phân quyền Nhóm và sudoers bằng visudo

## 1. Các lệnh đã thực thi
Tạo nhóm, tài khoản và thêm user vào nhóm:
\`\`\`bash
sudo groupadd devops-admin
sudo adduser deployer
sudo usermod -aG devops-admin deployer
\`\`\`

## 2. Cấu hình sudoers
Mở tệp cấu hình bằng lệnh `sudo visudo` và thêm dòng sau vào cuối file để cấp quyền chạy `systemctl` không cần mật khẩu cho nhóm `devops-admin`:
\`\`\`text
%devops-admin ALL=(ALL) NOPASSWD: /usr/bin/systemctl start *, /usr/bin/systemctl stop *, /usr/bin/systemctl restart *, /usr/bin/systemctl status *
\`\`\`

## 3. Báo cáo kết quả kiểm tra
Đăng nhập vào tài khoản `deployer` bằng lệnh `su - deployer` và chạy lệnh kiểm tra quyền sudo.

**Đầu ra của lệnh `sudo -l`:**
\`\`\`text
Matching Defaults entries for deployer on server-ubuntu:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User deployer may run the following commands on server-ubuntu:
    (ALL) NOPASSWD: /usr/bin/systemctl start *, /usr/bin/systemctl stop *, /usr/bin/systemctl restart *, /usr/bin/systemctl status *
\`\`\`

**Chạy thử nghiệm dịch vụ:**
\`\`\`bash
sudo systemctl restart cron
\`\`\`
*Kết quả:* Lệnh thực thi thành công, hệ thống không yêu cầu nhập mật khẩu của user `deployer`.