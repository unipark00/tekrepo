---
title: "Docker Networking"
search: false
categories:
  - Kubernetes
last_modified_at: 2019-05-01T21::00+09:00
toc: false
---

This post shows the overview of docker networking.

### docker0
![docker networking](https://github.com/unipark00/tekrepo/blob/master/_posts/2750F23D55A37EF801.png?raw=true)
* Linux Bridge (L2)
* IP Pool 내에서 Private IP 자동 할당
* 동일 호스트에 생성되는 모든 Container들은 같은 `docker0`에 바인딩
* 각 Containter는 외부 또는 다른 Container와 통신을 위해 Linux Bridge로 통신
* Container와 Linux Bridge 간 veth pair 생성
* Container 각각은 Linux namespace를 이용해 독립된 network 영역을 할당
* 각 Container는 의도한 Port만 노출(Expose)해서 통신
```console
@
```
