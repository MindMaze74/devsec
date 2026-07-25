# Домашнее задание к занятию «Уязвимости и атаки на информационные системы» - Старцев Данила Антонович

### Инструкция по выполнению домашнего задания

1. Сделайте fork [репозитория c шаблоном решения](https://github.com/netology-code/sys-pattern-homework) к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw).
2. Выполните клонирование этого репозитория к себе на ПК с помощью команды `git clone`.
3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
   - впишите вверху название занятия и ваши фамилию и имя;
   - в каждом задании добавьте решение в требуемом виде: текст/код/скриншоты/ссылка;
   - для корректного добавления скриншотов воспользуйтесь инструкцией [«Как вставить скриншот в шаблон с решением»](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md);
   - при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в [инструкции по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md).
4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`).
5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
6. Любые вопросы задавайте в разделе «Вопросы по заданию» в личном кабинете.

Желаем успехов в выполнении домашнего задания.

------

### Задание 1

Скачайте и установите виртуальную машину Metasploitable: https://sourceforge.net/projects/metasploitable/.

Это типовая ОС для экспериментов в области информационной безопасности, с которой следует начать при анализе уязвимостей.

Просканируйте эту виртуальную машину, используя **nmap**.

Попробуйте найти уязвимости, которым подвержена эта виртуальная машина.

Сами уязвимости можно поискать на сайте https://www.exploit-db.com/.

Для этого нужно в поиске ввести название сетевой службы, обнаруженной на атакуемой машине, и выбрать подходящие по версии уязвимости.

**Обнаружение служб и уязвимостей**

```bash
#Комплексное сканирование (все порты + версии)
sudo nmap -sS -sV -p- -T4 <ip-host>
- sS-(SYN-сканирование (полуоткрытое))
- sV-(с определением версий)
- p-(-все 65535 портов)
- T4(-ускоренный режим)
```

```bash
#Комплексное сканирование (все порты + версии + ОС)
sudo nmap -sS -sV -O -p- -T4 <ip-host>
```

```bash
#Сканирование с использованием NSE-скриптов для поиска уязвимостей
sudo nmap -sV --script vuln <ip-host>
```
**результат сканирования:**
>![скриншот_1](https://github.com/MindMaze74/devsec/blob/main/img/1.png)
>![скриншот_2](https://github.com/MindMaze74/devsec/blob/main/img/2.png)
>![скриншот_3](https://github.com/MindMaze74/devsec/blob/main/img/3.png)
>![скриншот_4](https://github.com/MindMaze74/devsec/blob/main/img/4.png)

**выводы по скану:**
>При сканировании Metasploitable 2 было обнаружено 30 открытых TCP-портов. Основные службы:

<details>
  <summary>Нажмите, чтобы увидеть детали по sudo nmap -sS -sV -p- -T4</summary>
  
  ```bash
Порт	Служба	Версия
21	FTP	vsftpd 2.3.4
22	SSH	OpenSSH 4.7p1
23	Telnet	Linux telnetd
25	SMTP	Postfix smtpd
53	DNS	ISC BIND 9.4.2
80	HTTP	Apache 2.2.8
111	RPCBind	v2
139/445	SMB	Samba 3.x
3306	MySQL	5.0.51a
5900	VNC	Protocol 3.3
6667	IRC	UnrealIRCd 3.2.8.1
8180	HTTP	Apache Tomcat 5.5
```
</details>


<details>
  <summary>Нажмите, чтобы увидеть детали по sudo nmap -sS -sV -O -p- -T4</summary>

```bash
  Starting Nmap 7.99 ( https://nmap.org ) at 2026-07-25 14:09 +0500
Nmap scan report for 192.168.0.56
Host is up (0.00013s latency).                                                          
Not shown: 65505 closed tcp ports (reset)                                               
PORT      STATE SERVICE     VERSION                                                     
21/tcp    open  ftp         vsftpd 2.3.4                                                
22/tcp    open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)                
23/tcp    open  telnet      Linux telnetd                                               
25/tcp    open  smtp        Postfix smtpd                                               
53/tcp    open  domain      ISC BIND 9.4.2                                              
80/tcp    open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)                         
111/tcp   open  rpcbind     2 (RPC #100000)                                             
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)                 
445/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)                 
512/tcp   open  exec        netkit-rsh rexecd                                           
513/tcp   open  login       OpenBSD or Solaris rlogind                                  
514/tcp   open  shell       Netkit rshd                                                 
1099/tcp  open  java-rmi    GNU Classpath grmiregistry
1524/tcp  open  bindshell   Metasploitable root shell
2049/tcp  open  nfs         2-4 (RPC #100003)
2121/tcp  open  ftp         ProFTPD 1.3.1
3306/tcp  open  mysql       MySQL 5.0.51a-3ubuntu5
3632/tcp  open  distccd     distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
5432/tcp  open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
5900/tcp  open  vnc         VNC (protocol 3.3)
6000/tcp  open  X11         (access denied)
6667/tcp  open  irc         UnrealIRCd
6697/tcp  open  irc         UnrealIRCd
8009/tcp  open  ajp13       Apache Jserv (Protocol v1.3)
8180/tcp  open  http        Apache Tomcat/Coyote JSP engine 1.1
8787/tcp  open  drb         Ruby DRb RMI (Ruby 1.8; path /usr/lib/ruby/1.8/drb)
47767/tcp open  nlockmgr    1-4 (RPC #100021)
54348/tcp open  status      1 (RPC #100024)
56607/tcp open  java-rmi    GNU Classpath grmiregistry
57074/tcp open  mountd      1-3 (RPC #100005)
MAC Address: 08:00:27:F4:9A:A4 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6
OS details: Linux 2.6.9 - 2.6.33
Network Distance: 1 hop
Service Info: Hosts:  metasploitable.localdomain, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 128.97 seconds
```
</details>


<details>
  <summary>Нажмите, чтобы увидеть детали по sudo nmap -sV --script vuln</summary>

```bash
Starting Nmap 7.99 ( https://nmap.org ) at 2026-07-25 13:45 +0500
Nmap scan report for 192.168.0.56
Host is up (0.00015s latency).                                                                                     
Not shown: 977 closed tcp ports (reset)                                                                            
PORT     STATE SERVICE     VERSION                                                                                 
21/tcp   open  ftp         vsftpd 2.3.4                                                                            
| ftp-vsftpd-backdoor:                                                                                             
|   VULNERABLE:                                                                                                    
|   vsFTPd version 2.3.4 backdoor                                                                                  
|     State: VULNERABLE (Exploitable)                                                                              
|     IDs:  CVE:CVE-2011-2523  BID:48539                                                                           
|       vsFTPd version 2.3.4 backdoor, this was reported on 2011-07-04.                                            
|     Disclosure date: 2011-07-03                                                                                  
|     Exploit results:                                                                                             
|       Shell command: id                                                                                          
|       Results: uid=0(root) gid=0(root)                                                                           
|     References:                                                                                                  
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2011-2523                                               
|       https://github.com/rapid7/metasploit-framework/blob/master/modules/exploits/unix/ftp/vsftpd_234_backdoor.rb
|       https://www.securityfocus.com/bid/48539
|_      http://scarybeastsecurity.blogspot.com/2011/07/alert-vsftpd-download-backdoored.html
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp   open  telnet      Linux telnetd
25/tcp   open  smtp        Postfix smtpd
| smtp-vuln-cve2010-4344: 
|_  The SMTP server is not Exim: NOT VULNERABLE
| ssl-dh-params: 
|   VULNERABLE:
|   Anonymous Diffie-Hellman Key Exchange MitM Vulnerability
|     State: VULNERABLE
|       Transport Layer Security (TLS) services that use anonymous
|       Diffie-Hellman key exchange only provide protection against passive
|       eavesdropping, and are vulnerable to active man-in-the-middle attacks
|       which could completely compromise the confidentiality and integrity
|       of any data exchanged over the resulting session.
|     Check results:
|       ANONYMOUS DH GROUP 1
|             Cipher Suite: TLS_DH_anon_WITH_AES_128_CBC_SHA
|             Modulus Type: Safe prime
|             Modulus Source: postfix builtin
|             Modulus Length: 1024
|             Generator Length: 8
|             Public Key Length: 1024
|     References:
|       https://www.ietf.org/rfc/rfc2246.txt
|   
|   Transport Layer Security (TLS) Protocol DHE_EXPORT Ciphers Downgrade MitM (Logjam)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2015-4000  BID:74733
|       The Transport Layer Security (TLS) protocol contains a flaw that is
|       triggered when handling Diffie-Hellman key exchanges defined with
|       the DHE_EXPORT cipher. This may allow a man-in-the-middle attacker
|       to downgrade the security of a TLS session to 512-bit export-grade
|       cryptography, which is significantly weaker, allowing the attacker
|       to more easily break the encryption and monitor or tamper with
|       the encrypted stream.
|     Disclosure date: 2015-5-19
|     Check results:
|       EXPORT-GRADE DH GROUP 1
|             Cipher Suite: TLS_DHE_RSA_EXPORT_WITH_DES40_CBC_SHA
|             Modulus Type: Safe prime
|             Modulus Source: Unknown/Custom-generated
|             Modulus Length: 512
|             Generator Length: 8
|             Public Key Length: 512
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-4000
|       https://www.securityfocus.com/bid/74733
|       https://weakdh.org
|   
|   Diffie-Hellman Key Exchange Insufficient Group Strength
|     State: VULNERABLE
|       Transport Layer Security (TLS) services that use Diffie-Hellman groups
|       of insufficient strength, especially those using one of a few commonly
|       shared groups, may be susceptible to passive eavesdropping attacks.
|     Check results:
|       WEAK DH GROUP 1
|             Cipher Suite: TLS_DHE_RSA_WITH_DES_CBC_SHA
|             Modulus Type: Safe prime
|             Modulus Source: postfix builtin
|             Modulus Length: 1024
|             Generator Length: 8
|             Public Key Length: 1024
|     References:
|_      https://weakdh.org
| ssl-poodle: 
|   VULNERABLE:
|   SSL POODLE information leak
|     State: VULNERABLE
|     IDs:  CVE:CVE-2014-3566  BID:70574
|           The SSL protocol 3.0, as used in OpenSSL through 1.0.1i and other
|           products, uses nondeterministic CBC padding, which makes it easier
|           for man-in-the-middle attackers to obtain cleartext data via a
|           padding-oracle attack, aka the "POODLE" issue.
|     Disclosure date: 2014-10-14
|     Check results:
|       TLS_RSA_WITH_AES_128_CBC_SHA
|     References:
|       https://www.imperialviolet.org/2014/10/14/poodle.html
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-3566
|       https://www.securityfocus.com/bid/70574
|_      https://www.openssl.org/~bodo/ssl-poodle.pdf
|_sslv2-drown: ERROR: Script execution failed (use -d to debug)
53/tcp   open  domain      ISC BIND 9.4.2
80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
|_http-sql-injection: ERROR: Script execution failed (use -d to debug)
| http-slowloris-check: 
|   VULNERABLE:
|   Slowloris DOS attack
|     State: LIKELY VULNERABLE
|     IDs:  CVE:CVE-2007-6750
|       Slowloris tries to keep many connections to the target web server open and hold
|       them open as long as possible.  It accomplishes this by opening connections to
|       the target web server and sending a partial request. By doing so, it starves
|       the http server's resources causing Denial Of Service.
|       
|     Disclosure date: 2009-09-17
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2007-6750
|_      http://ha.ckers.org/slowloris/
|_http-vuln-cve2017-1001000: ERROR: Script execution failed (use -d to debug)
|_http-server-header: Apache/2.2.8 (Ubuntu) DAV/2
|_http-trace: TRACE is enabled
| http-fileupload-exploiter: 
|   
|_    Couldn't find a file-type field.
| http-csrf: 
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=192.168.0.56
|   Found the following possible CSRF vulnerabilities: 
|     
|     Path: http://192.168.0.56:80/dvwa/
|     Form id: 
|     Form action: login.php
|     
|     Path: http://192.168.0.56:80/twiki/TWikiDocumentation.html
|     Form id: 
|     Form action: http://TWiki.org/cgi-bin/passwd/TWiki/WebHome
|     
|     Path: http://192.168.0.56:80/twiki/TWikiDocumentation.html
|     Form id: 
|     Form action: http://TWiki.org/cgi-bin/passwd/Main/WebHome
|     
|     Path: http://192.168.0.56:80/twiki/TWikiDocumentation.html
|     Form id: 
|     Form action: http://TWiki.org/cgi-bin/edit/TWiki/
|     
|     Path: http://192.168.0.56:80/twiki/TWikiDocumentation.html
|     Form id: 
|     Form action: http://TWiki.org/cgi-bin/view/TWiki/TWikiSkins
|     
|     Path: http://192.168.0.56:80/twiki/TWikiDocumentation.html
|     Form id: 
|     Form action: http://TWiki.org/cgi-bin/manage/TWiki/ManagingWebs
|     
|     Path: http://192.168.0.56:80/dvwa/login.php
|     Form id: 
|_    Form action: login.php
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
| http-enum: 
|   /tikiwiki/: Tikiwiki
|   /test/: Test page
|   /phpinfo.php: Possible information file
|   /phpMyAdmin/: phpMyAdmin
|   /doc/: Potentially interesting directory w/ listing on 'apache/2.2.8 (ubuntu) dav/2'
|   /icons/: Potentially interesting folder w/ directory listing
|_  /index/: Potentially interesting folder
|_http-dombased-xss: Couldn't find any DOM based XSS.
111/tcp  open  rpcbind     2 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2            111/tcp   rpcbind
|   100000  2            111/udp   rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/udp   nfs
|   100005  1,2,3      34142/udp   mountd
|   100005  1,2,3      57074/tcp   mountd
|   100021  1,3,4      47767/tcp   nlockmgr
|   100021  1,3,4      59046/udp   nlockmgr
|   100024  1          34192/udp   status
|_  100024  1          54348/tcp   status
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
512/tcp  open  exec        netkit-rsh rexecd
513/tcp  open  login       OpenBSD or Solaris rlogind
514/tcp  open  shell       Netkit rshd
1099/tcp open  java-rmi    GNU Classpath grmiregistry
| rmi-vuln-classloader: 
|   VULNERABLE:
|   RMI registry default configuration remote code execution vulnerability
|     State: VULNERABLE
|       Default configuration of RMI registry allows loading classes from remote URLs which can lead to remote code execution.
|       
|     References:
|_      https://github.com/rapid7/metasploit-framework/blob/master/modules/exploits/multi/misc/java_rmi_server.rb
1524/tcp open  bindshell   Metasploitable root shell
2049/tcp open  nfs         2-4 (RPC #100003)
2121/tcp open  ftp         ProFTPD 1.3.1
3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
|_ssl-ccs-injection: No reply from server (TIMEOUT)
5432/tcp open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
| ssl-dh-params: 
|   VULNERABLE:
|   Diffie-Hellman Key Exchange Insufficient Group Strength
|     State: VULNERABLE
|       Transport Layer Security (TLS) services that use Diffie-Hellman groups
|       of insufficient strength, especially those using one of a few commonly
|       shared groups, may be susceptible to passive eavesdropping attacks.
|     Check results:
|       WEAK DH GROUP 1
|             Cipher Suite: TLS_DHE_RSA_WITH_AES_128_CBC_SHA
|             Modulus Type: Safe prime
|             Modulus Source: Unknown/Custom-generated
|             Modulus Length: 1024
|             Generator Length: 8
|             Public Key Length: 1024
|     References:
|_      https://weakdh.org
| ssl-poodle: 
|   VULNERABLE:
|   SSL POODLE information leak
|     State: VULNERABLE
|     IDs:  CVE:CVE-2014-3566  BID:70574
|           The SSL protocol 3.0, as used in OpenSSL through 1.0.1i and other
|           products, uses nondeterministic CBC padding, which makes it easier
|           for man-in-the-middle attackers to obtain cleartext data via a
|           padding-oracle attack, aka the "POODLE" issue.
|     Disclosure date: 2014-10-14
|     Check results:
|       TLS_RSA_WITH_AES_128_CBC_SHA
|     References:
|       https://www.imperialviolet.org/2014/10/14/poodle.html
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-3566
|       https://www.securityfocus.com/bid/70574
|_      https://www.openssl.org/~bodo/ssl-poodle.pdf
| ssl-ccs-injection: 
|   VULNERABLE:
|   SSL/TLS MITM vulnerability (CCS Injection)
|     State: VULNERABLE
|     Risk factor: High
|       OpenSSL before 0.9.8za, 1.0.0 before 1.0.0m, and 1.0.1 before 1.0.1h
|       does not properly restrict processing of ChangeCipherSpec messages,
|       which allows man-in-the-middle attackers to trigger use of a zero
|       length master key in certain OpenSSL-to-OpenSSL communications, and
|       consequently hijack sessions or obtain sensitive information, via
|       a crafted TLS handshake, aka the "CCS Injection" vulnerability.
|           
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-0224
|       http://www.openssl.org/news/secadv_20140605.txt
|_      http://www.cvedetails.com/cve/2014-0224
5900/tcp open  vnc         VNC (protocol 3.3)
6000/tcp open  X11         (access denied)
6667/tcp open  irc         UnrealIRCd
|_irc-unrealircd-backdoor: Looks like trojaned version of unrealircd. See http://seclists.org/fulldisclosure/2010/Jun/277
8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
8180/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-sql-injection: ERROR: Script execution failed (use -d to debug)
| http-csrf: 
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=192.168.0.56
|   Found the following possible CSRF vulnerabilities: 
|     
|     Path: http://192.168.0.56:8180/admin/
|     Form id: username
|     Form action: j_security_check;jsessionid=FF426CDFF1E0E4DD8EFFA64C3CCBA9EF
|     
|     Path: http://192.168.0.56:8180/admin/login.jsp
|     Form id: username
|     Form action: j_security_check;jsessionid=F8E6FA0AE45E1C4F111E119E3A3E3D4B
|     
|     Path: http://192.168.0.56:8180/jsp-examples/jsp2/el/functions.jsp?foo=JSP+2.0
|     Form id: 
|     Form action: functions.jsp
|     
|     Path: http://192.168.0.56:8180/jsp-examples/error/error.html
|     Form id: 
|_    Form action: err.jsp
|_http-server-header: Apache-Coyote/1.1
| http-slowloris-check: 
|   VULNERABLE:
|   Slowloris DOS attack
|     State: LIKELY VULNERABLE
|     IDs:  CVE:CVE-2007-6750
|       Slowloris tries to keep many connections to the target web server open and hold
|       them open as long as possible.  It accomplishes this by opening connections to
|       the target web server and sending a partial request. By doing so, it starves
|       the http server's resources causing Denial Of Service.
|       
|     Disclosure date: 2009-09-17
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2007-6750
|_      http://ha.ckers.org/slowloris/
| http-enum: 
|   /admin/: Possible admin folder
|   /admin/index.html: Possible admin folder
|   /admin/login.html: Possible admin folder
|   /admin/admin.html: Possible admin folder
|   /admin/account.html: Possible admin folder
|   /admin/admin_login.html: Possible admin folder
|   /admin/home.html: Possible admin folder
|   /admin/admin-login.html: Possible admin folder
|   /admin/adminLogin.html: Possible admin folder
|   /admin/controlpanel.html: Possible admin folder
|   /admin/cp.html: Possible admin folder
|   /admin/index.jsp: Possible admin folder
|   /admin/login.jsp: Possible admin folder
|   /admin/admin.jsp: Possible admin folder
|   /admin/home.jsp: Possible admin folder
|   /admin/controlpanel.jsp: Possible admin folder
|   /admin/admin-login.jsp: Possible admin folder
|   /admin/cp.jsp: Possible admin folder
|   /admin/account.jsp: Possible admin folder
|   /admin/admin_login.jsp: Possible admin folder
|   /admin/adminLogin.jsp: Possible admin folder
|   /manager/html/upload: Apache Tomcat (401 Unauthorized)
|   /manager/html: Apache Tomcat (401 Unauthorized)
|   /admin/view/javascript/fckeditor/editor/filemanager/connectors/test.html: OpenCart/FCKeditor File upload
|   /admin/includes/FCKeditor/editor/filemanager/upload/test.html: ASP Simple Blog / FCKeditor File Upload
|   /admin/jscript/upload.html: Lizard Cart/Remote File upload
|_  /webdav/: Potentially interesting folder
| http-cookie-flags: 
|   /admin/: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/index.html: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/login.html: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/admin.html: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/account.html: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/admin_login.html: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/home.html: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/admin-login.html: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/adminLogin.html: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/controlpanel.html: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/cp.html: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/index.jsp: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/login.jsp: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/admin.jsp: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/home.jsp: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/controlpanel.jsp: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/admin-login.jsp: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/cp.jsp: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/account.jsp: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/admin_login.jsp: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/adminLogin.jsp: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/view/javascript/fckeditor/editor/filemanager/connectors/test.html: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/includes/FCKeditor/editor/filemanager/upload/test.html: 
|     JSESSIONID: 
|       httponly flag not set
|   /admin/jscript/upload.html: 
|     JSESSIONID: 
|_      httponly flag not set
MAC Address: 08:00:27:F4:9A:A4 (Oracle VirtualBox virtual NIC)
Service Info: Hosts:  metasploitable.localdomain, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_smb-vuln-ms10-054: false
|_smb-vuln-ms10-061: false
|_smb-vuln-regsvc-dos: ERROR: Script execution failed (use -d to debug)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 327.12 seconds
```
</details>

Ответьте на следующие вопросы:

- Какие сетевые службы в ней разрешены?

Порт	Протокол	Служба	Версия
```bash
21/tcp    open  ftp         vsftpd 2.3.4                                                
22/tcp    open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)                
23/tcp    open  telnet      Linux telnetd                                               
25/tcp    open  smtp        Postfix smtpd                                               
53/tcp    open  domain      ISC BIND 9.4.2                                              
80/tcp    open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)                         
111/tcp   open  rpcbind     2 (RPC #100000)                                             
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)                 
445/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)                 
512/tcp   open  exec        netkit-rsh rexecd                                           
513/tcp   open  login       OpenBSD or Solaris rlogind                                  
514/tcp   open  shell       Netkit rshd                                                 
1099/tcp  open  java-rmi    GNU Classpath grmiregistry
1524/tcp  open  bindshell   Metasploitable root shell
2049/tcp  open  nfs         2-4 (RPC #100003)
2121/tcp  open  ftp         ProFTPD 1.3.1
3306/tcp  open  mysql       MySQL 5.0.51a-3ubuntu5
3632/tcp  open  distccd     distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
5432/tcp  open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
5900/tcp  open  vnc         VNC (protocol 3.3)
6000/tcp  open  X11         (access denied)
6667/tcp  open  irc         UnrealIRCd
6697/tcp  open  irc         UnrealIRCd
8009/tcp  open  ajp13       Apache Jserv (Protocol v1.3)
8180/tcp  open  http        Apache Tomcat/Coyote JSP engine 1.1
8787/tcp  open  drb         Ruby DRb RMI (Ruby 1.8; path /usr/lib/ruby/1.8/drb)
47767/tcp open  nlockmgr    1-4 (RPC #100021)
54348/tcp open  status      1 (RPC #100024)
56607/tcp open  java-rmi    GNU Classpath grmiregistry
```


- Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)

1. **vsftpd 2.3.4 — Backdoor Command Execution (CVE-2011-2523)**
   - Версия vsftpd 2.3.4 содержит вредоносную закладку (бэкдор), внедрённую в официальный бинарный файл в 2011 году. Закладка активируется при отправке имени пользователя, содержащего последовательность `:)`. После успешной аутентификации бэкдор открывает root-командную оболочку на TCP-порту 6200.
   - Способ эксплуатации: удалённый злоумышленник может выполнить произвольные команды с правами root.
   - Ссылки:
     - [Exploit-DB](https://www.exploit-db.com/exploits/17491)
     - CVE: CVE-2011-2523

2. **Samba — username map script Command Execution (CVE-2007-2447)**
   - Уязвимость внедрения команд в Samba версиях 3.0.0 – 3.0.25rc3. Проблема существует в функции `SamrChangePassword()`, которая не выполняет должную санитацию пользовательского ввода, позволяя злоумышленнику внедрять shell-метасимволы в поле имени пользователя при SMB-аутентификации.
   - Способ эксплуатации: удалённый злоумышленник может выполнить произвольные команды на сервере.
   - Ссылки:
     - [Exploit-DB](https://www.exploit-db.com/exploits/16320)
     - CVE: CVE-2007-2447

3. **UnrealIRCd 3.2.8.1 — Backdoor Command Execution (CVE-2010-2075)**
   - Версия UnrealIRCd 3.2.8.1, распространявшаяся на некоторых зеркалах с ноября 2009 по июнь 2010 года, содержит внешнее вредоносное изменение (троян) в макросе `DEBUG3_DOLOG_SYSTEM`. Скомпрометированный бинарный файл выполняет произвольные команды, переданные с префиксом `AB;` на IRC-порт.
   - Способ эксплуатации: удалённый злоумышленник может выполнить произвольный код в целевой системе.
   - Ссылки:
     - [Exploit-DB](https://www.exploit-db.com/exploits/16922)
     - CVE: CVE-2010-2075

**Помимо трёх перечисленных, в Metasploitable 2 также присутствуют:**

- Слабые пароли по умолчанию (VNC, MySQL, PostgreSQL, SSH)
- DistCC Remote Code Execution (CVE-2004-2687)
- Tomcat с паролем по умолчанию
- NFS с небезопасными настройками общего доступа

Общий уровень критичности: идентифицировано 6 уязвимостей критического уровня и 5 высокого
*Приведите ответ в свободной форме.*  

### Задание 2

Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.

Запишите сеансы сканирования в Wireshark.

Ответьте на следующие вопросы:

- Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?
- Как отвечает сервер?

*Приведите ответ в свободной форме.*