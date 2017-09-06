##  循环执行的例行性工作排程



 当用户使用 crontab 这个指令来建立工作排程之后，该项工作就会被纪录到 /var/spool/cron/ 里面去 了，而且是以账号来作为判别的喔！举例来说， dmtsai 使用 crontab 后， 他的工作会被纪录到 /var/spool/cron/dmtsai 里头去！但请注意，不要使用 vi 直接编辑该文件， 因为可能由于输入语法错 误，会导致无法执行 cron 喔！另外， cron 执行的每一项工作都会被纪录到 /var/log/cron 这个登录 档中，所以啰，如果你的 Linux 不知道有否被植入木马时，也可以搜寻一下 /var/log/cron 这个登录 档呢！

