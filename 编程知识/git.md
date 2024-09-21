重写git commit 记录，清除大文件
git filter-branch --tree-filter 'find . -size +100M -type f -delete' --prune-empty HEAD
