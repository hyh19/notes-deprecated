# [第十五章、例行性工作排程(crontab)](http://linux.vbird.org/linux_basic/0430cron.php)

## 15.1 什麼是例行性工作排程

### 15.1.1 Linux 工作排程的種類： at, cron

> - at ：at 是個可以處理僅執行一次就結束排程的指令，不過要執行 at 時， 必須要有 atd 這個服務 (第十七章) 的支援才行。在某些新版的 distributions 中，atd 可能預設並沒有啟動，那麼 at 這個指令就會失效呢！不過我們的 CentOS 預設是啟動的！
> 
> - crontab ：crontab 這個指令所設定的工作將會循環的一直進行下去！ 可循環的時間為分鐘、小時、每週、每月或每年等。crontab 除了可以使用指令執行外，亦可編輯 /etc/crontab 來支援。 至於讓 crontab 可以生效的服務則是 crond 這個服務喔！

```bash
yum install -y at
at -h
man at
```

### 15.1.2 CentOS Linux 系統上常見的例行性工作

N/A

## 15.2 僅執行一次的工作排程

### 15.2.1 atd 的啟動與 at 運作的方式

> ```
> [root@study ~]# systemctl restart atd  # 重新啟動 atd 這個服務
> [root@study ~]# systemctl enable atd   # 讓這個服務開機就自動啟動
> [root@study ~]# systemctl status atd   # 查閱一下 atd 目前的狀態
> ```

> 我們使用 at 這個指令來產生所要運作的工作，並將這個工作以文字檔的方式寫入 /var/spool/at/ 目錄內，該工作便能等待 atd 這個服務的取用與執行了。

> 我們可以利用 /etc/at.allow 與 /etc/at.deny 這兩個檔案來進行 at 的使用限制呢！ 加上這兩個檔案後， at 的工作情況其實是這樣的：
> 
> 1. 先找尋 /etc/at.allow 這個檔案，寫在這個檔案中的使用者才能使用 at ，沒有在這個檔案中的使用者則不能使用 at (即使沒有寫在 at.deny 當中)；
> 
> 1. 如果 /etc/at.allow 不存在，就尋找 /etc/at.deny 這個檔案，若寫在這個 at.deny 的使用者則不能使用 at ，而沒有在這個 at.deny 檔案中的使用者，就可以使用 at 咯；
> 
> 1. 如果兩個檔案都不存在，那麼只有 root 可以使用 at 這個指令。

### 15.2.2 實際運作單一工作排程

> ```
> 範例一：再過五分鐘後，將 /root/.bashrc 寄給 root 自己
> [root@study ~]# at now + 5 minutes  <==記得單位要加 s 喔！
> at> /bin/mail -s "testing at job" root < /root/.bashrc
> at> <EOT>   <==這裡輸入 [ctrl] + d 就會出現 <EOF> 的字樣！代表結束！
> job 2 at Thu Jul 30 19:35:00 2015
> # 上面這行資訊在說明，第 2 個 at 工作將在 2015/07/30 的 19:35 進行！
> # 而執行 at 會進入所謂的 at shell 環境，讓你下達多重指令等待運作！
> 
> 範例二：將上述的第 2 項工作內容列出來查閱
> [root@study ~]# at -c 2
> #!/bin/sh               <==就是透過 bash shell 的啦！
> # atrun uid=0 gid=0
> # mail root 0
> umask 22
> ....(中間省略許多的環境變數項目)....
> cd /etc/cron\.d || {
>          echo 'Execution directory inaccessible' >&2
>          exit 1
> }
> ${SHELL:-/bin/sh} << 'marcinDELIMITER410efc26'
> /bin/mail -s "testing at job" root < /root/.bashrc    # 這一行最重要！
> marcinDELIMITER410efc26
> # 你可以看到指令執行的目錄 (/root)，還有多個環境變數與實際的指令內容啦！
> 
> 範例三：由於機房預計於 2015/08/05 停電，我想要在 2015/08/04 23:00 關機？
> [root@study ~]# at 23:00 2015-08-04
> at> /bin/sync
> at> /bin/sync
> at> /sbin/shutdown -h now
> at> <EOT>
> job 3 at Tue Aug  4 23:00:00 2015
> # 您瞧瞧！ at 還可以在一個工作內輸入多個指令呢！不錯吧！
> ```

```bash
yum install -y mailx
mail
```

> 有些朋友會希望『我要在某某時刻，在我的終端機顯示出 Hello 的字樣』，然後就在 at 裡面下達這樣的資訊『 echo "Hello" 』。等到時間到了，卻發現沒有任何訊息在螢幕上顯示，這是啥原因啊？這是因為 at 的執行與終端機環境無關，而所有 standard output/standard error output 都會傳送到執行者的 mailbox 去啦！所以在終端機當然看不到任何資訊。那怎辦？沒關係， 可以透過終端機的裝置來處理！假如你在 tty1 登入，則可以使用『 echo "Hello" > /dev/tty1 』來取代。


> ```
> [root@study ~]# atq
> [root@study ~]# atrm (jobnumber)
> 
> 範例一：查詢目前主機上面有多少的 at 工作排程？
> [root@study ~]# atq
> 3       Tue Aug  4 23:00:00 2015 a root
> # 上面說的是：『在 2015/08/04 的 23:00 有一項工作，該項工作指令下達者為 
> # root』而且，該項工作的工作號碼 (jobnumber) 為 3 號喔！
> 
> 範例二：將上述的第 3 個工作移除！
> [root@study ~]# atrm 3
> [root@study ~]# atq
> # 沒有任何資訊，表示該工作被移除了！
> ```
> 

> 其實 batch 是利用 at 來進行指令的下達啦！只是加入一些控制參數而已。這個 batch 神奇的地方在於：他會在 CPU 的工作負載小於 0.8 的時候，才進行你所下達的工作任務啦！ 那什麼是工作負載 0.8 呢？這個工作負載的意思是： CPU 在單一時間點所負責的工作數量。不是 CPU 的使用率喔！ 舉例來說，如果我有一隻程式他需要一直使用 CPU 的運算功能，那麼此時 CPU 的使用率可能到達 100% ， 但是 CPU 的工作負載則是趨近於『 1 』，因為 CPU 僅負責一個工作嘛！如果同時執行這樣的程式兩支呢？ CPU 的使用率還是 100% ，但是工作負載則變成 2 了！瞭解乎？

```bash
$ uptime
 14:41:55 up 12 min,  1 user,  load average: 0.00, 0.03, 0.05
```

## 15.3 循環執行的例行性工作排程

> 相對於 at 是僅執行一次的工作，循環執行的例行性工作排程則是由 cron (crond) 這個系統服務來控制的。剛剛談過 Linux 系統上面原本就有非常多的例行性工作，因此這個系統服務是預設啟動的。另外， 由於使用者自己也可以進行例行性工作排程，所以囉， Linux 也提供使用者控制例行性工作排程的指令 (crontab)。

```bash
systemctl status cron
```

### 15.3.1 使用者的設定

> 使用者想要建立循環型工作排程時，使用的是 crontab 這個指令啦～不過，為了安全性的問題， 與 at 同樣的，我們可以限制使用 crontab 的使用者帳號喔！使用的限制資料有：
> 
> - /etc/cron.allow：
> 將可以使用 crontab 的帳號寫入其中，若不在這個檔案內的使用者則不可使用 crontab；
> 
> - /etc/cron.deny：
> 將不可以使用 crontab 的帳號寫入其中，若未記錄到這個檔案當中的使用者，就可以使用 crontab 。

> 當使用者使用 crontab 這個指令來建立工作排程之後，該項工作就會被紀錄到 /var/spool/cron/ 裡面去了，而且是以帳號來作為判別的喔！舉例來說， dmtsai 使用 crontab 後， 他的工作會被紀錄到 /var/spool/cron/dmtsai 裡頭去！但請注意，不要使用 vi 直接編輯該檔案， 因為可能由於輸入語法錯誤，會導致無法執行 cron 喔！另外， cron 執行的每一項工作都會被紀錄到 /var/log/cron 這個登錄檔中，所以囉，如果你的 Linux 不知道有否被植入木馬時，也可以搜尋一下 /var/log/cron 這個登錄檔呢！

```bash
man crontab
```

> ```
> [root@study ~]# crontab [-u username] [-l|-e|-r]
> 選項與參數：
> -u  ：只有 root 才能進行這個任務，亦即幫其他使用者建立/移除 crontab 工作排程；
> -e  ：編輯 crontab 的工作內容
> -l  ：查閱 crontab 的工作內容
> -r  ：移除所有的 crontab 的工作內容，若僅要移除一項，請用 -e 去編輯。
> 
> 範例一：用 dmtsai 的身份在每天的 12:00 發信給自己
> [dmtsai@study ~]$ crontab -e
> # 此時會進入 vi 的編輯畫面讓您編輯工作！注意到，每項工作都是一行。
> 0   12  *  *  * mail -s "at 12:00" dmtsai < /home/dmtsai/.bashrc
> #分 時 日 月 週 |<==============指令串========================>|
> ```

> ```
> [dmtsai@study ~]$ crontab -l
> 0 12 * * * mail -s "at 12:00" dmtsai < /home/dmtsai/.bashrc
> 59 23 1 5 * mail kiki < /home/dmtsai/lover.txt
> */5 * * * * /home/dmtsai/test.sh
> 30 16 * * 5 mail friend@his.server.name < /home/dmtsai/friend.txt
> 
> # 注意，若僅想要移除一項工作而已的話，必須要用 crontab -e 去編輯～
> # 如果想要全部的工作都移除，才使用 crontab -r 喔！
> [dmtsai@study ~]$ crontab -r
> [dmtsai@study ~]$ crontab -l
> no crontab for dmtsai
> ```

### 15.3.2 系統的設定檔： /etc/crontab, /etc/cron.d/*

> 這個『 crontab -e 』是針對使用者的 cron 來設計的，如果是『系統的例行性任務』時， 該怎麼辦呢？是否還是需要以 crontab -e 來管理你的例行性工作排程呢？當然不需要，你只要編輯 /etc/crontab 這個檔案就可以啦！有一點需要特別注意喔！那就是 crontab -e 這個 crontab 其實是 /usr/bin/crontab 這個執行檔，但是 /etc/crontab 可是一個『純文字檔』喔！你可以 root 的身份編輯一下這個檔案哩！


```bash
$ ll /etc/ | grep cron
-rw-------.  1 root root      541 Aug  3  2017 anacrontab
drwxr-xr-x.  2 root root       21 Jan  8 16:28 cron.d
drwxr-xr-x.  2 root root       42 Jan  8 16:29 cron.daily
-rw-------.  1 root root        0 Aug  3  2017 cron.deny
drwxr-xr-x.  2 root root       22 Jan  8 16:28 cron.hourly
drwxr-xr-x.  2 root root        6 Jun  9  2014 cron.monthly
-rw-r--r--.  1 root root      451 Jun  9  2014 crontab
drwxr-xr-x.  2 root root        6 Jun  9  2014 cron.weekly
```

```bash
cat /etc/crontab 
```

> 一般來說，crond 預設有三個地方會有執行腳本設定檔，他們分別是：
> 
> - /etc/crontab
> - /etc/cron.d/*
> - /var/spool/cron/*
> 
> 這三個地方中，跟系統的運作比較有關係的兩個設定檔是放在 /etc/crontab 檔案內以及 /etc/cron.d/* 目錄內的檔案， 另外一個是跟用戶自己的工作比較有關的設定檔，就是放在 /var/spool/cron/ 裡面的檔案群。 

> 放在 /etc/cron.hourly/ 的檔案，必須是能被直接執行的指令腳本， 而不是分、時、日、月、周的設定值喔！注意注意！

> 最後，讓我們總結一下吧：
> 
> - 個人化的行為使用『 crontab -e 』：如果你是依據個人需求來建立的例行工作排程，建議直接使用 crontab -e 來建立你的工作排程較佳！ 這樣也能保障你的指令行為不會被大家看到 (/etc/crontab 是大家都能讀取的權限喔！)；
> 
> - 系統維護管理使用『 vim /etc/crontab 』：如果你這個例行工作排程是系統的重要工作，為了讓自己管理方便，同時容易追蹤，建議直接寫入 /etc/crontab 較佳！
> 
> - 自己開發軟體使用『 vim /etc/cron.d/newfile 』：如果你是想要自己開發軟體，那當然最好就是使用全新的設定檔，並且放置於 /etc/cron.d/ 目錄內即可。
> 
> - 固定每小時、每日、每週、每天執行的特別工作：如果與系統維護有關，還是建議放置到 /etc/crontab 中來集中管理較好。 如果想要偷懶，或者是一定要再某個週期內進行的任務，也可以放置到上面談到的幾個目錄中，直接寫入指令即可！

### 15.3.3 一些注意事項

N/A

## 15.4 可喚醒停機期間的工作任務

### 15.4.1 什麼是 anacron

> anacron 並不是用來取代 crontab 的，anacron 存在的目的就在於我們上頭提到的，在處理非 24 小時一直啟動的 Linux 系統的 crontab 的執行！ 以及因為某些原因導致的超過時間而沒有被執行的排程工作。
> 
> 其實 anacron 也是每個小時被 crond 執行一次，然後 anacron 再去檢測相關的排程任務有沒有被執行，如果有超過期限的工作在， 就執行該排程任務，執行完畢或無須執行任何排程時，anacron 就停止了。
> 
> 由於 anacron 預設會以一天、七天、一個月為期去偵測系統未進行的 crontab 任務，因此對於某些特殊的使用環境非常有幫助。 舉例來說，如果你的 Linux 主機是放在公司給同仁使用的，因為週末假日大家都不在所以也沒有必要開啟， 因此你的 Linux 是週末都會關機兩天的。但是 crontab 大多在每天的凌晨以及週日的早上進行各項任務， 偏偏你又關機了，此時系統很多 crontab 的任務就無法進行。 anacron 剛好可以解決這個問題！
> 
> 那麼 anacron 又是怎麼知道我們的系統啥時關機的呢？這就得要使用 anacron 讀取的時間記錄檔 (timestamps) 了！ anacron 會去分析現在的時間與時間記錄檔所記載的上次執行 anacron 的時間，兩者比較後若發現有差異， 那就是在某些時刻沒有進行 crontab 囉！此時 anacron 就會開始執行未進行的 crontab 任務了！

### 15.4.2 anacron 與 /etc/anacrontab

> anacron 其實是一支程式並非一個服務！這支程式在 CentOS 當中已經進入 crontab 的排程喔！同時 anacron 會每個小時被主動執行一次喔！ 

```bash
$ which anacron
/usr/sbin/anacron

$ ll /etc/cron.hourly/
total 4
-rwxr-xr-x. 1 root root 392 Aug  3  2017 0anacron
```