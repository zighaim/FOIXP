# FOIXP - Free Open Internet Exchange Point

Welcome to **FOIXP**, a free and open Internet Exchange Point (IXP) for IPv6 peering using Private AS Numbers and IPv6 HE Tunnels that you get from Tunnel Broker or other providers.
Follow the instructions below to connect to our IXP using your HE Tunnel settings.

Selamat datang di **FOIXP**, Internet Exchange Point (IXP) gratis dan terbuka untuk peering IPv6 menggunakan Nomor AS Pribadi dan Terowongan HE IPv6 yang Anda dapatkan dari Tunnel Broker atau penyedia lainnya.
Ikuti petunjuk di bawah ini untuk terhubung ke IXP kami menggunakan pengaturan Terowongan HE Anda.

## Purpose of FOIXP Creation / Tujuan Pembuatan FOIXP
1. **Accelerating IPv6 Network Development / Mempercepat Pengembangan Jaringan IPv6**
2. **Improving Internet Service Quality / Meningkatkan Kualitas Layanan Internet**
3. **Supporting Open and Free Network Infrastructure / Mendukung Infrastruktur Jaringan yang Terbuka dan Gratis**
4. **Improving Service Availability and Reliability / Meningkatkan Ketersediaan dan Keandalan Layanan**
5. **Encouraging Collaboration and Knowledge Exchange / Mendorong Kolaborasi dan Pertukaran Pengetahuan**
6. **Supporting Local Network Development / Mendukung Perkembangan Jaringan Lokal**

## Prerequisites
1. A registered account on [HE Tunnel](https://tunnelbroker.net/) with a working tunnel setup.
2. MikroTik routers configured for IPv6 or devices that are already configured for IPv6 and support Gre
3. You must create a **pull request** in our [GitHub repository](https://github.com/muh-ramadhan/FOIXP) to request peering.
4. Make sure your Personal AS Number is in the range **4200000000-4294967295**.
## Prasyarat
1. Akun yang terdaftar di [HE Tunnel](https://tunnelbroker.net/) dengan pengaturan tunnel yang berfungsi.
2. Router MikroTik dikonfigurasi untuk IPv6 atau perangkat yang sudah dikonfigurasi untuk IPv6 dan mendukung Gre
3. Anda harus membuat **pull request** di repositori [GitHub kami](https://github.com/muh-ramadhan/FOIXP) untuk meminta peering.
4. Pastikan Nomor AS Pribadi Anda berada dalam rentang **4200000000-4294967295**.

## Step 1 : Copy the Example File

1. Fork this Repository to your local Device
2. Navigate to the **[Examples](https://github.com/muh-ramadhan/FOIXP/blob/main/Examples)** folder in the GitHub repository.
3. Copy the content from `Examples` to your local system.
4. Modify the content of the file as shown below:
```
aut-num:      4200000000                 #As the Private Number that you will use
endpoint:     2001:470:23:394::2/64      #Your IPv6 Tunnel Endpoints / Client IPv6 Address
cidr:         2001:470:24:39b::/64       #Your Routed IPv6 Prefixes / Routed /64
e-mail:       john.doe@example.com       #Your email
```
   
## Langkah 1 : Salin File Contoh
1. Fork Repository ini ke Perangkat lokal Anda
2. Arahkan ke folder **[Examples](https://github.com/muh-ramadhan/FOIXP/blob/main/Examples)** di repositori GitHub.
3. Salin konten dari file `Examples` ke sistem lokal Anda.
4. Modifikasi konten file seperti di bawah ini:
```
aut-num:      4200000000                 #Sebagai Nomor Pribadi yang akan Anda gunakan
endpoint:     2001:470:23:394::2/64      #Titik Akhir Terowongan IPv6 Anda / Alamat IPv6 Klien
cidr:         2001:470:24:39b::/64       #Awalan IPv6 Anda yang Dirutekan / Dirutekan /64
e-mail:       john.doe@example.com       #mail Anda
```
## Step 2 : Create a Pull Request
1. Rename `Examples` to aut-num (Example 4200002000, 4202000132, etc.)
2. After modifying the file, upload it to your GitHub repository.
3. Create a pull request (PR) on the FOIXP repository.
4. Wait for the approval of your pull request.
   
## Langkah 2 : Buat Pull Request
1. Ganti nama `Examples` menjadi aut-num (Contoh 4200002000, 4202000132, dst.)
2. Setelah memodifikasi file, unggah ke repositori GitHub Anda.
3. Buat pull request (PR) ke repositori FOIXP.
4. Tunggu persetujuan pull request Anda.

## Step 3 : Configure GRE6 Tunnel on MikroTik / Langkah 3: Konfigurasi GRE6 Tunnel di MikroTik

After your pull request is approved, follow the steps below to set up the GRE6 Tunnel on your MikroTik router.

Setelah pull request Anda disetujui, ikuti langkah-langkah berikut untuk mengatur GRE6 Tunnel di router MikroTik Anda.

1. Create the GRE6 tunnel interface:
   - Buat interface GRE6 tunnel:
```
/interface gre6 add name=gre6 remote-address=2001:470:35:72::2 local-address=YOUR_CLIENT_IPV6_ADDRESS
```
Replace `YOUR_CLIENT_IPV6_ADDRESS` with the IPv6 address from your HE tunnel setup.

Ganti `YOUR_CLIENT_IPV6_ADDRESS` dengan alamat IPv6 dari pengaturan HE tunnel Anda.

2. Add the IPv6 address to the GRE6 interface:
   - Tambahkan alamat IPv6 ke interface GRE6:
```
/ipv6 address add address=YOUR_ROUTED_PREFIX_64 interface=gre6 advertise=no
```
Replace `YOUR_ROUTED_PREFIX_6`4 with the IPv6 prefix assigned to you by HE Tunnel, for example `2001:470:24:39b::1/64`.

Ganti `YOUR_ROUTED_PREFIX_64` dengan prefix IPv6 yang ditugaskan oleh HE Tunnel, misalnya `2001:470:24:39b::1/64`.

## Step 4 : Configure BGP on MikroTik / Langkah 4: Konfigurasi BGP di MikroTik

1. Add a new BGP instance:
   - Tambahkan instance BGP baru:
```
/routing bgp instance add name=bgp as=YOUR_AS_NUMBER
```
Replace `YOUR_AS_NUMBER` with the AS Number you created in the pull request file.

Ganti `YOUR_AS_NUMBER` dengan AS Number yang Anda buat di file pull request.

2. Add the BGP peer:
   - Tambahkan peer BGP:
```
/routing bgp peer add name=peer instance=bgp remote-address=2001:470:35:72::2 remote-as=YOUR_AS_NUMBER multihop=yes address-families=ipv6
```
Replace `remote-as=YOUR_AS_NUMBER` with the AS Number you have requested.

Ganti `remote-as=YOUR_AS_NUMBER` dengan AS Number yang Anda minta.

3. Add your routed IPv6 prefix to BGP:
   - Tambahkan prefix IPv6 yang Anda routing ke BGP:
```
/routing bgp network add network=YOUR_ROUTED_PREFIX_64_IPV6
```
Replace `YOUR_ROUTED_PREFIX_64_IPV6` with the prefix you received from HE Tunnel, for example `2001:470:24:39b::/64`.

Ganti `YOUR_ROUTED_PREFIX_64_IPV6` dengan prefix yang Anda terima dari HE Tunnel, misalnya `2001:470:24:39b::/64`.

## Troubleshooting / Pemecahan Masalah
### If you experience any issues during setup, check the following:
1. Ensure that your GRE tunnel is correctly configured.
2. Verify that the BGP instance and peer settings are correctly entered.
3. Check your IPv6 routing table to ensure that the prefix is advertised correctly.
4. If you need another way to connect for example IPIPv6, EOIPv6 you can send me a message at support@dalonet.com

### Jika Anda mengalami masalah selama pengaturan, periksa hal-hal berikut:
1. Pastikan bahwa tunnel GRE Anda telah dikonfigurasi dengan benar.
2. Verifikasi bahwa instance BGP dan pengaturan peer telah dimasukkan dengan benar.
3. Periksa tabel routing IPv6 Anda untuk memastikan prefix telah diiklankan dengan benar.
4. Jika Anda memerlukan cara lain untuk terhubung misalnya IPIPv6, EOIPv6 Anda dapat mengirim pesan kepada saya di support@dalonet.com

## License / Lisensi
This project is licensed under the MIT License - see the LICENSE file for details.

Proyek ini dilisensikan di bawah Lisensi MIT - lihat file LICENSE untuk rincian.
