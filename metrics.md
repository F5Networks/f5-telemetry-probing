
~~中 划 线~~ 此字段为可选字段，将来实现中扩充

| 协议类型   | 命令示例     | 字段         | 字段含义   | 类型  | 字段举例 |
| :---      |    :----:   |          ---: |      :--- |      ---: |      ---: |
| 共同字段 | | | | | |
|||start_time | 探测发生时间  | string  |"2021-09-14 09:25:24.977"
|||status|探测成功，还是失败|string|up, down
|||error|探测失败时的错误信息|string|非格式化数据，多由探测底层lib直接抛上来的错误信息
|||latency|返回时间 ms|int|64
|||dest_addr|探测目标地址|string|1.2.3.4,fe80::7c56:24ff:fefd:87de
|||dest_port|目标端口|string|3128
|||src_addr|源地址|string|1.2.3.4,fe80::7c56:24ff:fefd:87de
|||src_port|源端口|string|5601
|||location|探测点信息|string|lab-bigip-15-cluster
|||device_type|探测设备类型：VE NGINX+|string|BIGIPVE, NGINX+
|||device_name|探测设备名|string|bigip-host.name.else.com
|||pool_name|业务探测池池名|string|/Common/mypool
|||protocol|探测协议|string|ICMP,TCP,HTTP,DNS,HTTPS
||||||
| ICMP  | ping 8.8.8.8  |icmp_type|icmp 类型|int|64
||ping -c 8.8.8.8|icmp_code|icmp 返回码|int|64
|||~~echo_bytes~~|ICMP发送包大小|int|48
|||~~reply_bytes~~|ICMP回复包大小|int|133
|||~~icmp_seq~~|ICMP包序列号|string|4.3.2.1 ,fe80::7c56:24ff:fefd:87de
|||~~waittime~~|milliseconds 发送ping包的时间间隔 |string|eth0
|||~~boundif~~|ICMP发送包socket所绑定的iface|int|5
|||~~count~~|ICMP发送数量|int|20
|||~~wait~~|milliseconds被确定为 timeout 前的等待时间|int|2000
||||||
TCP|nc -vvvvz -w 12 10.250.11.185 80|~~boundif~~|探测包发送接口|string|eth0
||nmap 10.250.11.185 -p 80|~~crlf~~|换行符|string|\r\n
||telnet 10.250.11.185 80|~~conntimeout~~|tcp连接超时时间 seconds|int|2
||nmap -sU 10.0.1.161 -p 9998 -Pn|~~keepidle~~|TCP keep alive失效时间 seconds|int|3
|||~~num_probes~~|尝试TCP连接的次数|int|5
|||~~proxy_address~~|通过代理address探测tcp连通性|string|10.2.3.5
|||~~proxy_port~~|通过代理port 探测tcp连通性|int|3128
||||||
DNS|nslookup baidu.com|TTL|time ro live，缓存时间，单位秒。600，代表缓存域名服务器，可以在缓存中保存10分钟该记录。|int|600
||dig baidu.com|domain_name|要解析的域名|string|baidu.com
|||ipaddrs|域名对应的ip地址。|string(array-like)|[123.125.114.144, 123.125.114.143]
|||class|要查询信息的类别，IN代表类别为IP协议，即Internet。还有其它类别，比如chaos等。|string|IN
|||timeout|查询超时时间 seconds|int|5
|||domain_type|要查询的记录类型，A记录(Address)，代表要查询ipv4地址。AAAA记录，代表要查询ipv6地址。|string|A
|||query_status|状态信息|string|NOERROR
|||~~opcode~~|操作码|string|query
|||~~id~~|数字，在dns协议中，通过编号匹配返回和查询。|int|54846
|||~~flags~~|如果出现就表示有标志，如果不出现就未设置标志：|string|qr rd ra
|||~~query_count~~|查询数，1代表1个查询，对应QUESTION SECTION中的记录数|int|1
|||~~answer_count~~|结果数，4代表有4项结果，对应ANSWER SECTION中的记录数|int|4
|||~~authority~~|权威域名服务器记录数，0代表该域名有0个权威域名服务器，可供域名解析用。|int|0
|||~~additional~~|格外记录数，0代表有0项格外记录。|int|0
|||~~reply_size~~|回应的大小。收到(rcve, recieved) bytes|int|91
|||||
HTTP|curl|request_url|请求url|string|http://baidu.com/?asf=wer
||python requests|request_headers|http 请求头|string(json-like)|{ all headers }
|||request_body|http 请求体|string|…
|||request_method|http请求方法|string|GET
|||response_headers|http 应答头部|string(json-like)|{ all headers }
|||response_code|http status code|int|200,400,404
|||response_body|http回复报文|string|…
||||||
HTTPS|openssl x509 -in ca-cert.pem -inform pem -noout -text|ssl_verion_number|规范的版本号，目前为版本3，值为0x2；|string|0x2
||curl|serial_number|由CA维护的为它所发的每个证书分配的一的列号|string|10:e6:fc:62…....53:45:b6
||python requests|issuer|发证书单位的标识信息|string|C=CN, ST=BJ, L=BJ, O=F5, OU=PD, CN=21f/emailAddress=a.zong@f5.com
|||validity_begin|证书的有效期开始|string|Feb 17 07:48:06 2020 GMT
|||validity_end|证书的有效期结束|string|Mar 18 07:48:06 2020 GMT
|||expired|证书是否过期|string|True, False
|||signature_algorithm|数字签名所采用的算法|string|sha256-with-RSA-Encryption
|||subject|证书拥有者的标识信息（Distinguished Name）|string|C=CN, ST=BJ, L=BJ, O=F5, OU=PD, CN=21f/emailAddress=a.zong@f5.com
|||public_key_info|所保护的公钥相关的信息|string|Public Key Algorithm: rsaEncryption \n …
|||public_key|公钥|string|----PUBLIC KEY ...
|||public_key_length|公钥长度|int|2048
|||~~issuer_id~~|代表颁发者的唯一信息，仅2、3版本支持，可选；|string|..
|||~~subject_id~~|代表拥有证书实体的唯一信息，仅2，3版本支持，可选：|string|..
|||~~extensions~~|可选的一些扩展|string|.|.