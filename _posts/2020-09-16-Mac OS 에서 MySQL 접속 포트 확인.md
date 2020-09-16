# Mac OS 에서 MySQL 접속 포트 확인
```zsh
$ mysql -uroot
```
터미널에서 입력해서 `mysql` 에 접속 후, 
```zsh
mysql> SHOW GLOBAL VARIABLES LIKE 'PORT';
```

결과

<img src='http://drive.google.com/uc?export=view&id=1pA7Ad7k_cPPRezxHxnwW6BamGhiP7xdq' /><br>