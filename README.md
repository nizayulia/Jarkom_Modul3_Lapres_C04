# Jarkom_Modul3_Lapres_C04

#### Anggota :
#### 05111840000029	Khofifah Nurlaela
#### 05111840000053	Yulia Niza

<br>

#### 1.	Membuat topologi jaringan
![alt text](https://github.com/nizayulia/Jarkom_Modul3_Lapres_C04/blob/main/dhcp/Picture1.jpg?raw=true)
 ```
nano topologi.sh 
  ```
![alt text](https://github.com/nizayulia/Jarkom_Modul3_Lapres_C04/blob/main/dhcp/Picture2.png?raw=true)
Mengatur interface pada setiap uml :  ```nano /etc/network/interfaces  ```

<img src="dhcp/Picture3.png" width="400" height="300">
<img src="dhcp/Picture4.png" width="400" height="300">
<img src="dhcp/Picture5.png" width="400" height="300">
<img src="dhcp/Picture6.png" width="400" height="300">

Mengatur interface pada uml client agar tidak menggunakan konfigurasi IP Statis :

<img src="dhcp/Picture7.png" width="400" height="300">
<img src="dhcp/Picture8.png" width="400" height="300">
<img src="dhcp/Picture9.png" width="400" height="300">
<img src="dhcp/Picture10.png" width="400" height="300">

Restart pada setiap uml :  ``` service networking restart ```

#### 2. SURABAYA ditunjuk sebagai perantara (DHCP Relay) antara DHCP Server dan client
-	Install dhcp-relay pada surabaya : ``` apt-get install isc-dhcp-relay ```
-	Konfigurasi interface dhcp-relay pada surabaya : ``` nano /etc/default/isc-dhcp-relay  ```. 
- Atur agar server mengarah ke tuban dengan ``` SERVERS="10.151.77.44" ```dan  ```INTERFACESv4="eth1 eth2 eth3" ```
- Lalu Restart : ``` service isc-dhcp-relay restart ```

<img src="dhcp/Picture11.png" width="500" height="450">

- Konfigurasi interfaces dhcp-server pada Tuban : ``` nano /etc/default/isc-dhcp-server ``` dengan ```INTERFACESv4="eth0" ``` 

<img src="dhcp/Picture12.png" width="500" height="450">

- Konfigurasi pada dhcp-server :  ``` nano /etc/dhcp/dhcpd.conf  ```
-	Restart :  ``` service isc-dhcp-relay restart  ```

<img src="dhcp/Picture13.png" width="450" height="400">


#### 3. Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200
Lakukan ``` Service networking restart``` pada client lalu ``` ifconfig ``` untuk memeriksa ipnya :

<img src="dhcp/Picture14.png" width="450" height="400">
<img src="dhcp/Picture15.png" width="450" height="400">

#### 4.	Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70.
Lakukan ``` Service networking restart``` pada client lalu ``` ifconfig ``` untuk memeriksa ipnya :

<img src="dhcp/Picture16.png" width="450" height="400">
<img src="dhcp/Picture17.png" width="450" height="400">

#### 5.	Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP

Periksa pada setiap client  ``` cat /etc/resolv.conf ```

<img src="dhcp/Picture18.png" width="500" height="450">

#### 6.	Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan client pada subnet 3 mendapatkan peminjaman IP selama 10 menit.

Pada konfigurasi terlihat pada default-lease-time pada subnet 1 adalah 300 detik atau 5 menit dan pada subnet 3 adalah 600 detik atau 10 meniit

<img src="dhcp/Picture19.png" width="450" height="400">

#### 7.	Akses ke proxy hanya bisa dilakukan oleh Anri sendiri sebagai user TA. User autentikasi milik Anri memiliki format:

``` 
User : userta_c04
Password : inipassw0rdta_c04 
```


- Pastikan pada UML Mojokerto sudah terinstall `squid` dan `apache2-utils`. Lalu ketikkan:
- Buat user dan password baru dengan mengetikkan:

```
htpasswd -c /etc/squid/passwd userta_c04
```

Ketikkan password yang diinginkan, yaitu `inipassw0rdta_c04 `. Jika sudah makan akan muncul notifikasi.

- Edit konfigurasi squid pada `/etc/squid/squid.conf` menjadi:

![alt text](https://github.com/nizayulia/Jarkom_Modul3_Lapres_C04/blob/main/proxy/proxy_squid_7.JPG?raw=true)

- Restart squid: `service squid restart`
- Ubah pengaturan proxy browser (di sini saya menggunakan firefox). Gunakan IP MOJOKERTO : `10.151.77.43` sebagai host dan isikan port `8080`.

![alt text](https://github.com/nizayulia/Jarkom_Modul3_Lapres_C04/blob/main/proxy/proxy_squid_7_0.png?raw=true)

- Kemudian buka browser, akan muncul pop up login:

![alt text](https://github.com/nizayulia/Jarkom_Modul3_Lapres_C04/blob/main/proxy/proxy_squid_7_1.png?raw=true)


#### 8. Anri sudah menjadwal pengerjaan TA-nya setiap hari Selasa-Rabu pukul 13.00-18.00. 

- Edit konfigurasi squid pada `/etc/squid/squid.conf`, tambahkan :
```
acl AVAILABLE_WORKING time TW 13:00-18:00
```
Maknanya Anri hanya dapat mengakses internet di hari Selasa-Rabu pukul 13.00-18.00. 
- Selain itu, edit konfigurasi menjadi:

![alt text](https://github.com/nizayulia/Jarkom_Modul3_Lapres_C04/blob/main/proxy/proxy_squid_8.JPG?raw=true)

- Restart squid: `service squid restart`
- Kemudian buka browser. Akan muncul halaman error jika mengakses di luar waktu yang ditentukan.

#### 9. Jadwal bimbingan dengan Bu Meguri adalah setiap hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya (sampai Jumat jam 09.00). 

- Edit konfigurasi squid pada `/etc/squid/squid.conf`, tambahkan :
```
acl BIMBINGAN time TWH 21:00-23:59
acl BIMBINGAN time WHF 00:00-09:00
```

Selain dapat mengakses internet pada waktu yang telah ditentukan, Anri juga bisa mengakses internet saat ada jadwal bimbingan dengan Bu Meguri, yaitu  hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya.

- Selain itu, edit konfigurasi menjadi:

![alt text](https://github.com/nizayulia/Jarkom_Modul3_Lapres_C04/blob/main/proxy/proxy_squid_9.JPG?raw=true)

- Restart squid: `service squid restart`
- Kemudian buka browser. Akan muncul halaman error jika mengakses di luar waktu yang ditentukan.

#### 10. Agar Anri bisa fokus mengerjakan TA, setiap dia mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id agar Anri selalu ingat untuk mengerjakan TA🙂.

- Edit konfigurasi squid pada `/etc/squid/squid.conf`, tambahkan :
```
acl BLACKLISTS dstdomain .google.com
http_access deny BLACKLISTS
deny_info monta.if.its.ac.id BLACKLISTS
```

- Konfigurasi squid menjadi:

![alt text](https://github.com/nizayulia/Jarkom_Modul3_Lapres_C04/blob/main/proxy/proxy_squid_10_1.JPG?raw=true)

- Restart squid: `service squid restart`
- Kemudian buka browser dan ketikkan `google.com` maka akan diredirect ke `monta.if.ac.id`.

#### 11. Untuk menandakan bahwa Proxy Server ini adalah Proxy yang dibuat oleh Anri, Bu Meguri meminta Anri untuk mengubah error page default squid.

- Unduh file error dengan mengetikkan `wget 10.151.36.202/ERR_ACCESS_DENIED`
- Copy file tersebut ke `/usr/share/squid/errors/English/`
- Restart squid: `service squid restart`
- Kemudian buka browser. Akan muncul halaman error yang terbaru jika mengakses di luar waktu yang ditentukan.

![alt text](https://github.com/nizayulia/Jarkom_Modul3_Lapres_C04/blob/main/proxy/proxy_squid_11.png?raw=true)

#### 12. Karena Bu Meguri dan Anri adalah tipe orang pelupa, maka untuk memudahkan mereka, Anri memiliki ide ketika menggunakan proxy cukup dengan mengetikkan domain janganlupa-ta.c04.pw dan memasukkan port 8080. 

- Pastikan pada UML Malang telah terinstall `bind9`.
- Buka file : `nano /etc/bind/named.conf.local`.
- Isikan konfigurasi domain `janganlupa-ta.c04.pw`, sesuai dengan syntax berikut:
```
zone "janganlupa-ta.c04.pw" {
	type master;
	file "/etc/bind/jarkom/janganlupa-ta.c04.pw";
};
```

![alt text](https://github.com/nizayulia/Jarkom_Modul3_Lapres_C04/blob/main/proxy/dns_server_1.JPG?raw=true)

- Buat folder jarkom di dalam /etc/bind : `mkdir /etc/bind/jarkom`
- Copykan file db.local pada path /etc/bind ke dalam folder jarkom yang baru saja dibuat dan ubah namanya menjadi janganlupa-ta.c04.pw : `cp /etc/bind/db.local /etc/bind/jarkom/janganlupa-ta.c04.pw`
- Kemudian buka file janganlupa-ta.c04.pw dan edit seperti gambar berikut dengan IP MOJOKERTO: `nano /etc/bind/jarkom/janganlupa-ta.c04.pw` 

![alt text](https://github.com/nizayulia/Jarkom_Modul3_Lapres_C04/blob/main/proxy/dns_server_2.JPG?raw=true)

- Restart bind9 : `service bind9 restart`
- Lakukan setting proxy pada browser (di sini saya menggunakan firefox). Gunakan domain : `janganlupa-ta.c04.pw` sebagai host dan isikan port `8080`.

![alt text](https://github.com/nizayulia/Jarkom_Modul3_Lapres_C04/blob/main/proxy/proxy_squid_12.png?raw=true)

- Pop up log in setelah host proxy diubah:

![alt text](https://github.com/nizayulia/Jarkom_Modul3_Lapres_C04/blob/main/proxy/proxy_squid_12_1.png?raw=true)