for vmcontroller sciprt:  
  
```  
grep rpc test/sandbox/vmcontroller/proto/v2/vmctrl/v2/public/api.proto|awk '{print "\| " $2 " \| \| \| \| \| \| \| \|" }'  
```  
  
for gateway script:  
  
```  
find proto -name "*.proto" -exec sh -c 'echo "file: {}" && grep " rpc " {} && grep "service " {}' \; | awk '{print "| " $2 " | | | | | | | |"}'  
```
