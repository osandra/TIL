### DNS (Domain Name System)

네이버의 IP주소는 [http://125.209.222.142](http://125.209.222.142/)다. 호스트의 IP주소를 알게 되면 클라이언트의 웹브라우저와 서버는 TCP 방식을 통해 커넥션을 생성한다. 그러나 우린 [www.naver.com](http://www.naver.com) 로 들어가기만 해도 네이버 서버와 통신할 수 있다. 그 이유는 `DNS가 호스트 네임을 받으면 몇몇 단계를 거쳐 호스트의 실제 IP 주소를 반환`해주기 때문이다.

DNS는 네임 서버의 `계층 구조로 구현된 분산 데이터베이스`와 변환 서비스를 제공하기 위해 호스트와 네임 서버가 통신할 수 있도록 하는 `애플리케이션 계층` 프로토콜이다. 대체로 서버명과 IP주소를 대응시키기 위해 많이 사용하지만 그 외에도 다양한 정보(host aliasing, mail server aliasing)를 이름에 대응해서 등록하거나 Load Distribution등의 서비스를 제공한다. 

만약 단 한 대의 DNS 서버에 의해 모든 IP 주소가 관리되면 해당 서버에 모든 클라이언트가 접근해야 하므로 트래픽량이 커지고 유지보수가 힘들어진다. 또한 DNS 서버가 고장나면 접속이 불가하며 해당 서버까지 가는 데 많은 시간이 소요된다는 단점 등이 있으므로 `DNS는 계층적 방식으로 분산`되어 있다.

- **Host aliasing**: 호스트 이름이 복잡한 경우 실제이름(canonical name)과 가명(alias)을 등록할 수 있다. DNS 쿼리가 가명으로 IP주소를 물을 때 실제 이름을 찾고 > 이를 통해 대응하는 IP주소를 받는 것
- **mail server aliasing**: gmail의 호스트네임은 gmail.com 보다 훨씬 복잡하므로 DNS 서버에서 가명인 gmail.com 호스트 네임을 통해 실제 이름(canonical name)을 찾고 > 이에 대응하는 IP주소를 받는 것
- **Load Distribution**:  여러 개의 IP 주소가 표준 호스트 이름에 연결된 경우를 말한다.  따라서 각 응답 내의 IP 주소 순서를 회전하며 서버의 부담을 분산해주는 기능

---

### DNS 구조

👉 `Root name server`: DNS 계층 트리에서 최상단에 위치하는 서버다. 루트 네임 서버는 도메인 별 TIL 서버들을 알고 있다. 현재 13개의 루트 네임 서버가 있다. 루트 네임 서버는 인터넷에 존재하는 DNS 서버에 전부 등록되어 있다.

👉 `TLB name server`: com, org, net, edu 등과 특정 나라의 top level domain 등을 관리하는 서버다. 우리나라의 경우 .kr 도메인을 한국 인터넷정보센터에서 관리한다. 따라서 추후 .kr 로 끝나는 도메인을 설정할 경우 해당 TLD 서버에 이를 알려서 본인의 IP 서버와 호스트네임 등을 등록해야 한다. <br>www.naver.com에서 com은 top-level 도메인이고 naver는 secondary domain 이라고 부른다.

👉 `Authorative name server`: 해당 서버는 일반적으로 IP 주소 전환과정의 마지막 단계다. 해당 서버에는 **자신이 서비스하는 도메인 이름과 관련된 모든 정보**가 포함된 서버다. 즉 자기 기관이 제공하는 모든 호스트네임-IP에 대한 매핑 정보를 보유하고 있다. 

👉 `Local DNS resolver server`: ISP(인터넷 서비스 공급자)를 사용하는 경우 DNS 서버가 ISP 에 있다. 

![https://upload.wikimedia.org/wikipedia/commons/b/b1/Dns-server-hierarchy.gif](https://upload.wikimedia.org/wikipedia/commons/b/b1/Dns-server-hierarchy.gif)
<p style="text-align:center"><a href="https://commons.wikimedia.org/wiki/File:Dns-server-hierarchy.gif" target="_blank">이미지 출처</a></p>

---
👉 사용자가 요청 보낼 때 예시: `www.naver.com` 으로 처음 요청시

1. 로컬 DNS 서버에게 물어봄 
2. 없으면 local DNS 서버에서 root DNS 서버에게 물어봄 → root DNS서버가  해당하는 TLD 서버(.com)를 알려줌
3. local DNS는 해당 TLD서버(.com)에게 물어봄 → TLD서버가 이에 해당하는 Authorative DNS 서버를 알려줌 (Naver DNS) 
4. local DNS서버에서 Authorative DNS(Naver DNS)한테 물어봄 → 해당 Authorative 서버에서 이에 해당하는 IP 주소 반환해줌 → 이를 받아서 답이 돌아왔으니 애플리케이션에게 건네줌

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2cdf8102-821c-490c-b252-ecb15dd098c9/Screen_Shot_2021-07-04_at_11.25.53_AM.png](https://i0.wp.com/www.wmlcloud.com/wp-content/uploads/2021/02/Understanding-DNS-Queries.jpg?w=500&ssl=1)
<p style="text-align:center"><a href="https://www.wmlcloud.com/windows/windows-server-2008-domain-name-system-and-ipv6-understanding-dns-queries/" target="_blank">이미지 출처</a></p>

DNS 서버는 한 번 조사한 이름을 `캐시에 기록`하기 때문에 검색한 이름에 해당하는 정보가 캐시에 있으면 바로 해당 정보를 얻을 수 있다.

대체로 TLD 서버들은 로컬 네임 서버에 캐시 되어 있다. 호스트가 IP 주소를 바꿀 경우, 이전 정보가 캐시되어 있을 수 있다. 이 경우 DNS 서버에 등록하는 정보에 유효기한(TTL-time to leave)을 설정하면 캐시에 저장한 데이터 유효기간이 지나면 캐시에서 삭제된다.

<details>
<summary>출처</summary>

- [https://www.net.t-labs.tu-berlin.de/teaching/computer_networking/02.05.htm](https://www.net.t-labs.tu-berlin.de/teaching/computer_networking/02.05.htm)

- [https://blog.devgenius.io/all-about-dns-hierarchy-36fabfcdc1f1](https://blog.devgenius.io/all-about-dns-hierarchy-36fabfcdc1f1)

- [https://www.inetdaemon.com/tutorials/internet/dns/servers/](https://www.inetdaemon.com/tutorials/internet/dns/servers/)

- [https://www.cloudflare.com/learning/dns/dns-server-types/](https://www.cloudflare.com/learning/dns/dns-server-types/)

- [네트워크 강의](http://www.kocw.net/home/cview.do?mty=p&kemId=1046412)

</details>