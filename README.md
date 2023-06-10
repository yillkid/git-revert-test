# git-revert-test

## 實驗
- 實驗 git revert 是否會影響檔案

## 步驟
- 創造一個檔案 a.txt，內容為 a
- push a
- 創造一個檔案 b.txt，內容為 b
- push b
- 使用 git revert 修改 a 的 commit log
- 到 github 觀察檔案 a.txt 的情況

## log

#### 創造 a.txt, b.txt 分別推送:
```bash=
yillkid@Orz:~/workspace/git-revert-test$ ls
README.md
yillkid@Orz:~/workspace/git-revert-test$ touch a.txt
yillkid@Orz:~/workspace/git-revert-test$ vi a.txt 
yillkid@Orz:~/workspace/git-revert-test$ git add a.txt ; git commit -m "a" ; git push
[main 8609929] a
 1 file changed, 1 insertion(+)
 create mode 100644 a.txt
枚舉物件: 4, 完成.
物件計數中: 100% (4/4), 完成.
使用 8 個執行緒進行壓縮
壓縮物件中: 100% (2/2), 完成.
寫入物件中: 100% (3/3), 261 位元組 | 261.00 KiB/s, 完成.
總共 3 (差異 0)，復用 0 (差異 0)，重用包 0
To https://github.com/yillkid/git-revert-test
   eb5679b..8609929  main -> main
yillkid@Orz:~/workspace/git-revert-test$ ls
a.txt  README.md
yillkid@Orz:~/workspace/git-revert-test$ touch b.txt
yillkid@Orz:~/workspace/git-revert-test$ vi b.txt 
yillkid@Orz:~/workspace/git-revert-test$ git add b.txt ; git commit -m "b" ; git push
[main ac167d2] b
 1 file changed, 1 insertion(+)
 create mode 100644 b.txt
枚舉物件: 4, 完成.
物件計數中: 100% (4/4), 完成.
使用 8 個執行緒進行壓縮
壓縮物件中: 100% (2/2), 完成.
寫入物件中: 100% (3/3), 287 位元組 | 287.00 KiB/s, 完成.
總共 3 (差異 0)，復用 0 (差異 0)，重用包 0
To https://github.com/yillkid/git-revert-test
   8609929..ac167d2  main -> main
yillkid@Orz:~/workspace/git-revert-test$ 
```

#### git log:
```bash=
yillkid@Orz:~/workspace/git-revert-test$ git log
commit ac167d2aee7a77cc95aaa3a73575fd1a59fe89b2 (HEAD -> main, origin/main, origin/HEAD)
Author: yillkid <yillkid@gmail.com>
Date:   Sat Jun 10 12:25:24 2023 +0800

    b

commit 86099297d226c98c0a4d3d7bb5bff5581c83a744
Author: yillkid <yillkid@gmail.com>
Date:   Sat Jun 10 12:23:14 2023 +0800

    a

commit eb5679bc58ac9631f06d40690cb83682d82a5d1e
Author: HuangJyunYu <yillkid@gmail.com>
Date:   Sat Jun 10 12:17:33 2023 +0800

    Initial commit
yillkid@Orz:~/workspace/git-revert-test$ 
```

#### git revert:
```bash=
yillkid@Orz:~/workspace/git-revert-test$ git revert 86099297d226c98c0a4d3d7bb5bff5581c83a744
[main 30b5ab4] Revert "new a"
 1 file changed, 1 deletion(-)
 delete mode 100644 a.txt
yillkid@Orz:~/workspace/git-revert-test$ git push
枚舉物件: 3, 完成.
物件計數中: 100% (3/3), 完成.
使用 8 個執行緒進行壓縮
壓縮物件中: 100% (2/2), 完成.
寫入物件中: 100% (2/2), 304 位元組 | 304.00 KiB/s, 完成.
總共 2 (差異 0)，復用 0 (差異 0)，重用包 0
To https://github.com/yillkid/git-revert-test
   ac167d2..30b5ab4  main -> main
```

#### git log:
```bash=
yillkid@Orz:~/workspace/git-revert-test$ git log
commit 30b5ab41084b7c43064bebcf0469e79c3ed8b7da (HEAD -> main, origin/main, origin/HEAD)
Author: yillkid <yillkid@gmail.com>
Date:   Sat Jun 10 12:29:53 2023 +0800

    Revert "new a"
    
    This reverts commit 86099297d226c98c0a4d3d7bb5bff5581c83a744.

commit ac167d2aee7a77cc95aaa3a73575fd1a59fe89b2
Author: yillkid <yillkid@gmail.com>
Date:   Sat Jun 10 12:25:24 2023 +0800

    b

commit 86099297d226c98c0a4d3d7bb5bff5581c83a744
Author: yillkid <yillkid@gmail.com>
Date:   Sat Jun 10 12:23:14 2023 +0800

    a

commit eb5679bc58ac9631f06d40690cb83682d82a5d1e
Author: HuangJyunYu <yillkid@gmail.com>
Date:   Sat Jun 10 12:17:33 2023 +0800

    Initial commit
```

## GitHub
![image](https://github.com/yillkid/git-revert-test/assets/185872/df6d751c-7fcb-42d6-b619-d3a2d20ac010)

## 解說
- git revert 抽掉了 86099297d226c98c0a4d3d7bb5bff5581c83a744
- git revert 產生了一個新的 sha-1
- 與 soft reset 不同的是，revert 也撤回了 86099297d226c98c0a4d3d7bb5bff5581c83a744 的整個 commit
- 與 hard reset 不同的是，revert 保留了所有的 commit log
