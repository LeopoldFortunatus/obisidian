```
export REPO=cloud-native
git remote add github git@github.com:LeopoldFortunatus/${REPO}.git
git remote set-url github git@github.com-LeopoldFortunatus:LeopoldFortunatus/${REPO}.git
git push --set-upstream github master
```
ssh config:
```
# Github
Host github.com-LeopoldFortunatus
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_leopold
```