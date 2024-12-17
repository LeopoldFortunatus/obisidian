1. Comment: 
```
//c.nodeCtrl.Start()  
//c.vmCtrl.Start()  
//c.checkCtrl.Start()  
//c.dnsCtrl.Start()
```
2. setup local ip: `sudo ip addr add 172.29.77.2/24 dev lo`
3. run vmctl: `-c ../local-configs/native/vmctl-new.yml list node`
4. debug with grpcurl: 
```
$grpcurl -v -d '{}'   -cacert /home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl-ca.crt   -cert /home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl.crt   -key /home/arykalin/Dropbox/src/go/src/git.wildberries.ru/cloud/local-configs/native/ssl/vmctl/vmctl.key   -proto proto/vmctrl/v1/api.proto   -import-path proto/vmctrl/v1 -import-path  proto 172.29.77.2:2000 models.grpc.Service/ListNodes
```


```
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYmYiOjE3MzQ0MDUwNTMsInJvbGVzIjpbImFwcC1lOmRldi10OmNsb3VkLXA6Y2xvdWQtcGo6Y2xvdWQiXSwidHJhaXRzIjp7ImxvZ2lucyI6WyJhbGVrc2FuZGVyLnJ5a2FsaW4iXX0sInVzZXJuYW1lIjoiYWxla3NhbmRlci5yeWthbGluIiwic3ViIjoiYWxla3NhbmRlci5yeWthbGluIiwiYXVkIjpbImh0dHBzOlwvXC9sb2NhbGhvc3QiXSwiZXhwIjoxOTM0NDI1MDYzLCJpYXQiOjE3MzQyMjUwNjMsImlzcyI6InRlbGVwb3J0In0.dOtoV2JOZPRhK33_DnUSrYVtWMoHZluH6jjbZjclXbROy5VLvX2ROJUiYYxAiHIC4g4Sa8AKSdXGp8GHLiBpTBcCAmMiQwMo0sXrlpKmWV-IiNb_qk3BgyvzYGsWMEV9bcQQh9JpKVFh-qnvhbQ2Elq2i0PgDmC1rC5F4T_7mXmiwG3UFWQ-sSMmka6xAlrpXHnfl-2-04kw-UDetwJ-DH6URXPPjkkI4pnXtHbrh6HP5kB0ZQUBzL5ECGEBgx-mmdkaWIULlvUm5hndmKVq1aL_XWSQfBV5pkB-OaNt5bcP9ZZs4Kc4HZ1TTRS9gOv4_f0UJvnOOGEM7PsOlp0Had8kFOomp7_FmKtahpbMRI-6zjgY0ztZ27JsdmAt-h-rq5ocY8z4eEv4MvMuSb5bfZN3aWU1HqtN_z12ZBF3EUoPlsy3hw0dXEayG2yJ0EJEPyHLqYITDDp0Avm8T5UMVELRNAc1A5PTTkG8vVyXsQF5xiyuiESu1-JMPAItcKTE
```
