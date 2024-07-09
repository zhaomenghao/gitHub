### git push/pull 出现refusing to merge unrelated histories
    问题原因：就是本地仓库和远程仓库不一致导致解决思路先把远程仓库拉下来解决冲突然后再提交即可
    解决方案 ：如果第一种不好使则使用第二种
            一、git merge origin 分支名 --allow-unrelated-history
            二、git pull origin 分支名 --allow-unrelated-histories
 