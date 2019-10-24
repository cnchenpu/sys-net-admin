# Midterm Exam

1. 使用者在進行系統升級的過程中誤把電腦的 boot loader 給破壞掉了，MBR 與 GRUB 皆已損壞，電腦已無法開機。所幸在 /root 底下仍留有 MBR 與 grub.conf 的備份，分別叫做 mbr.bak 與 grub.bak，所以仍有機會復原回來。你的任務就是修復這個錯誤，拯救水深火熱中的客戶。
   
   - Hint:
     - 你必須借助 Linux ISO 來開機，並從 rescue mode 來進行診斷和復原的工作。
     - 因為 GRUB 已經損壞，所以重新安裝 GRUB 是必要的。
     - grub.bak 的內容並不完整，你仍須要進一步去修正他。

   - 得分要件:
     - 正常重開機後，將下列命令的輸出畫面截圖，貼到你的 HackMD 筆記上:
       1. ll /boot/grub
       2. cat /boot/grub/grub.conf
   
2. 使用 Wireshark 解析這段封包，找出任一組 TCP 3-Way Handshake 的封包，以及 client 用來登入 web server 的 "username" 與 "password"。Client 與 web server 的 IP 分別是 172.22.2.221 與 172.22.11.121。

  - Hint:
    - 將 Wireshark filter 的條件限定在 client 或 server 的 IP 以及特定的 protocol 下，可以縮小搜尋的範圍。
    - 3-Way Handshake 的封包為三個一組，protocol 是 TCP。
    - Web server 登入的封包形式為 "POST /login"，protocol 是 HTTP。
      - Login 的資訊會顯示在 Wireshark 封包細節的視窗中，如下圖一。

  - 得分要件:
    - 將你所找到的資訊截圖並標示出來，貼到你的 HackMD 筆記上:
      1.  任一組 TCP 3-Way Handshake 的三個封包
      2.  Login 的 username 與 password 的值


