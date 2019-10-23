# Midterm Exam

1. 使用者在進行系統升級的過程中誤把電腦的 boot loader 給破壞掉了，MBR 與 GRUB 皆已損壞，電腦已無法開機。所幸在 /root 底下仍留有 MBR 與 grub.conf 的備份，分別叫做 mbr.bak 與 grub.bak，所以仍有機會復原回來。你的任務就是修復這個錯誤，拯救水深火熱中的客戶。
   
   - Hint:
     - 你必須借助 Linux ISO 來開機，並從 rescue mode 來進行診斷和復原的工作。
     - 因為 GRUB 已經損壞，所以重新安裝 GRUB 是必要的。
     - grub.bak 的內容並不完整，你仍須要進一步去修正他。

   - 得分要件:
     - 正常重開機後，將下列命令的輸出畫面截圖，貼到你的 HackMD 帳號上:
       1. ll /boot/grub
       2. cat /boot/grub/grub.conf
   
2. 
