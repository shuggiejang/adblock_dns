# ADBlock DNS
라즈베리파이와 dnsmasq를 이용하여 광고방지 DNS를 만듭니다.

## 원리
인터넷 상의 광고들은 주로 ads.realclick.co.kr 또는 이와 유사한 주소에 있습니다.
이 프로그램은 이러한 주소를 127.0.0.1 로 연결하는 DNS를 만듭니다.

## 사용법
1. 라즈베리파이에 dnsmasq 를 설치합니다.
```
raspberrypi ~ $ sudo apt-get install dnsmasq
```

2. 이 프로젝트를 라즈베리파이에 clone 합니다.
```
raspberrypi ~ $ git clone https://github.com/shuggiejang/adblock_dns.git
```

3. adblock 필터를 다운로드합니다. 그리고 사용자가 직접 광고 사이트를 추가합니다.
```
raspberrypi ~ $ cd adblock_dns/
raspberrypi ~/adblock_dns $ ./update_adblock_filter.sh
raspberrypi ~/adblock_dns $ vi custom_ad_domains.txt
```

4. 위의 자료를 바탕으로 DNS를 갱신합니다.
```
raspberrypi ~/adblock_dns $ ./update_dns.sh
```

5. 공유기의 DNS를 라즈베리파이로 설정합니다(1회만 적용).
```
raspberrypi ~ $ ifconfig eth0
eth0      Link encap:Ethernet  HWaddr b8:27:eb:72:9a:d3
          inet addr:192.168.0.8  Bcast:192.168.0.255  Mask:255.255.255.0
...
```
![ipTime 수동 DNS 설정](dns_config_on_iptime_router.png)

## 상세
1. 주소 목록에서 중복된 주소를 제거합니다.
2. 상위도메인이 등록된 경우, 서브 도메인을 제거합니다.
```
# 광고사이트 목록
raspberrypi ~/adblock_dns $ grep -Hrn realclick ./*.txt
./ad_punisher_abp.txt:382:||realclick.co.kr
./custom_ad_domains.txt:74:realclick.co.kr
./custom_ad_domains.txt:112:click.realclick.co.kr
./custom_ad_domains.txt:113:ads.realclick.co.kr
./custom_ad_domains.txt:114:ptimg.realclick.co.kr
./custom_ad_domains.txt:115:rsense-ad.realclick.co.kr
./custom_ad_domains.txt:116:ade.realclick.co.kr
./custom_ad_domains.txt:117:img.realclick.co.kr
./custom_ad_domains.txt:118:adr.realclick.co.kr
./custom_ad_domains.txt:119:hcimg.realclick.co.kr
./custom_ad_domains.txt:120:image3.realclick.co.kr
./custom_ad_domains.txt:121:image13.realclick.co.kr
./custom_ad_domains.txt:122:mdimg.realclick.co.kr
./custom_ad_domains.txt:123:ptimg.realclick.co.kr
./custom_ad_domains.txt:124:scimg.realclick.co.kr
./easylist.txt:24887:||realclick.co.kr^$third-party

# DNS 설정 파일
raspberrypi ~/adblock_dns $ grep -Hrn realclick ./*.conf
./dnsmasq.adlist.conf:3769:address=/.realclick.co.kr/127.0.0.1 #realclick
```
