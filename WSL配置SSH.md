---
template: OBJECTIVE/Knowledge-Strucuture_v1.0
created: 2022-03-28 19:43
updated: 2022-03-30 14:50
---
# WSL配置SSH
#OBJECTIVE/Knowledge 
Created:: 2022-03-28 19:43
Category:: #Computer/Software/SSH 
Tags:: #SSH #WSL 

---

```bash
ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
service ssh restart
```