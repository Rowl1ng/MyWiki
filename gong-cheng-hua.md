# git

遇到 server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none
```
git config --global http.sslverify false
```
## clone branch
```
git clone -b breast https://.....
```

## push更新

```
git pull origin breast
git add .
git commit -m 'message'
git push origin breast
```

## .gitignore

[A collection of useful .gitignore templates](https://github.com/github/gitignore)
# requirements.txt

```
pip install -r requirements.txt
```