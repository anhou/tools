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
