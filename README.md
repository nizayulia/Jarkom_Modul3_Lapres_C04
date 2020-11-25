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

