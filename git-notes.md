## Clone后Update Submodules
```
git submodule update --init --recursive
```

## 合并commit
```
# 合并commit
git rebase -i HEAD~2

pick xxxx -> squash xxxx
```

# download all branches
* if pull upstream, please change "origin" to "upstream"
```
git clone xxxxxx.git
git branch -r | grep -v '\->' | while read remote; do git branch --track "${remote#origin/}" "$remote"; done
git fetch --all
git pull --all
```
* push
```
# 切换到创建的空仓库的链接，注意此处没有git remote rename origin old-origin
git remote set-url origin https://gitlab.chehejia.com/xxxx/yyyy.git

# push所有branch和tag
git push -u origin --all
git push -u origin --tags
```

## git自动提示
* Ref：https://gist.github.com/m2lan/b6f04b92640d7bb40b19
`wget https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash`

mv git-completion.bash .git-completion.bash

~/.bashrc添加

```
if [ -f ~/.git-completion.bash ]; then
    . ~/.git-completion.bash
fi
```

`source ~/.bashrc` 或者 重新登陆
