


## Put your topology config image here!

<img width="1533" height="685" alt="image" src="https://github.com/user-attachments/assets/fa607c8e-b61a-48e2-a874-13cf9a731237" />


## Put your GNS3 Project file here!

[`gns3project & all script used in project`](https://drive.google.com/drive/folders/1pC2Ywlo_6wPUB1YdncGI_zMA5zH-EGmL?usp=sharing)

<br>

## Soal 1

> Setup Topo

> _Document the results of the subnet grouping that has been created._

**Answer:**

- Screenshot

![WhatsApp Image 2025-10-31 at 13 53 54](https://github.com/user-attachments/assets/282c9c67-6c13-47a0-824c-d0c05d390d2b)


- Explanation

  IKUTI SAJA SUBNET YANG DITENTUKAN DAN TAMBAHKAN PREFIX SESUAI MILIK SENDIRI SERTA TENTUKAN IP SENDIRI

## Soal 2

> Buatlah konfigurasi untuk domain 
> **lune33.com** → ke IP node Lune , 
> **sciel33.com** → ke IP node Sciel ,
> **gustave33.com** → ke IP node Gustave 
> pada DNS Master Renoir. Kemudian konfigurasikan node Verso sebagai DNS Slave yang bekerja untuk DNS Master Renoir.

> _Dns Configuration , on  the DNS Master (Renoir)_
> _lune33.com → IP of node Lune ,_
> _sciel33.com → IP of node Sciel ,_
> _gustave33.com → IP of node Gustave_
> _Configure Verso as the DNS Slave that works with DNS Master Renoir._

**Answer:**

- Screenshot
  
  ### lune33.com
  
  <img width="715" height="476" alt="image" src="https://github.com/user-attachments/assets/b129d384-feeb-4a9f-a15e-2e40393fd3b9" />
  
  ### sciel33.com
  
  <img width="715" height="476" alt="image" src="https://github.com/user-attachments/assets/f29d2b97-7282-41ef-91c7-812e7d082544" />
  
  ### gustave33.com
  
  <img width="715" height="476" alt="image" src="https://github.com/user-attachments/assets/a36d9310-8955-4cd2-96bf-dc1d4cb40aaf" />

- Explanation

  ### TAMBAHKAN SCRIPT BERIKUT DAN IKUTI INSTRUKSI UNTUK MENYALAKAN `SERVER MASTER`

  ```bash
  #!/bin/bash

  IP_RENOIR="10.91.3.2"
  IP_VERSO="10.91.3.3"
  IP_LUNE="10.91.2.2"
  IP_SCIEL="10.91.2.3"
  IP_GUSTAVE="10.91.2.4"
  
  echo "Setting up DNS Master with IPs:"
  echo "Renoir: $IP_RENOIR"
  echo "Verso: $IP_VERSO"
  echo "Lune: $IP_LUNE"
  echo "Sciel: $IP_SCIEL"
  echo "Gustave: $IP_GUSTAVE"

  mkdir -p /myscripts/dns
  cd /myscripts/dns
  
  # Create named.conf
  cat > named.conf << EOF
  options {
      directory "/myscripts/dns";
      listen-on { any; };
      allow-query { any; };
      allow-notify { 10.91.3.3; };
      also-notify {10.91.3.3; };
      allow-transfer { 10.91.3.3; };
  };
  
  zone "lune33.com" {
      type master;
      file "db.lune33.com";
      allow-transfer { 10.91.3.3; };
  };
  
  zone "sciel33.com" {
      type master;
      file "db.sciel33.com";
      allow-transfer { 10.91.3.3; };
  };
  
  zone "gustave33.com" {
      type master;
      file "db.gustave33.com";
      allow-transfer { 10.91.3.3; };
  };
  EOF
  
  # Create db.lune33.com
  cat > db.lune33.com << EOF
  \$TTL 86400
  @ IN SOA nsi.lune33.com. admin.lune33.com. (
      1 ; serial
      3600 ; refresh
      1800 ; retry
      604800 ; expire
      86400 ) ; minimum
  
  ; Name servers
  @       IN NS    nsi.lune33.com.
  @       IN NS    verso.lune33.com.
  
  ; Address records
  nsi     IN A     $IP_RENOIR
  verso   IN A     $IP_VERSO
  @       IN A     $IP_LUNE
  EOF

  # Create db.sciel33.com
  cat > db.sciel33.com << EOF
  \$TTL 86400
  @ IN SOA nsi.sciel33.com. admin.sciel33.com. (
      1 ; serial
      3600 ; refresh
      1800 ; retry
      604800 ; expire
      86400 ) ; minimum
  
  ; Name servers
  @       IN NS    nsi.sciel33.com.
  @       IN NS    verso.sciel33.com.
  
  ; Address records
  nsi     IN A     $IP_RENOIR
  verso   IN A     $IP_VERSO
  @       IN A     $IP_SCIEL
  EOF

  # Create db.gustave33.com
  cat > db.gustave33.com << EOF
  \$TTL 86400
  @ IN SOA nsi.gustave33.com. admin.gustave33.com. (
      1 ; serial
      3600 ; refresh
      1800 ; retry
      604800 ; expire
      86400 ) ; minimum
  
  ; Name servers
  @       IN NS    nsi.gustave33.com.
  @       IN NS    verso.gustave33.com.
  
  ; Address records
  nsi     IN A     $IP_RENOIR
  verso   IN A     $IP_VERSO
  @       IN A     $IP_GUSTAVE
  EOF

  # Create start_dns.sh
  cat > start_dns.sh << 'EOF'
  #!/bin/sh
  echo "Starting BIND DNS server..."
  named -g -c /myscripts/dns/named.conf
  EOF
  
  chmod +x start_dns.sh
  
  chmod 644 *.conf
  chmod 644 db.*

  echo "==============================================================="
  echo "DNS Master configuration completed (Clean Version)!"
  echo "Directory: /myscripts/dns"
  echo "Files created:"
  echo "  - named.conf"
  echo "  - db.lune33.com"
  echo "  - db.sciel33.com"
  echo "  - db.gustave33.com"
  echo "  - start_dns.sh"
  echo ""
  echo "Configured domains:"
  echo "  lune33.com -> $IP_LUNE"
  echo "  sciel33.com -> $IP_SCIEL"
  echo "  gustave33.com -> $IP_GUSTAVE"
  echo ""
  echo "To start DNS server: cd /myscripts/dns && ./start_dns.sh"
  echo "==============================================================="
  ```

  ### TAMBAHKAN SCRIPT BERIKUT DAN IKUTI INSTRUKSI UNTUK MENYALAKAN `SERVER SLAVE`

  ```bash
  #!/bin/bash
  
  IP_RENOIR="10.91.3.2"
  IP_VERSO="10.91.3.3"
  
  echo "Setting up DNS Slave (Verso) with IP: $IP_VERSO"
  echo "Master DNS (Renoir) IP: $IP_RENOIR"
    
  mkdir -p /myscripts/dns/slave
  cd /myscripts/dns
  
  # Create named.conf for slave
  cat > named.conf << EOF
  options {
      directory "/myscripts/dns";
      listen-on { any; };
      allow-query { any; };
  };
  
  zone "localhost" {
      type master;
      file "db.localhost";
  };
  
  zone "lune33.com" {
      type slave;
      file "slave/db.lune33.com";
      masters { $IP_RENOIR; };
  };
  
  zone "sciel33.com" {
      type slave;
      file "slave/db.sciel33.com";
      masters { $IP_RENOIR; };
  };
  
  zone "gustave33.com" {
      type slave;
      file "slave/db.gustave33.com";
      masters { $IP_RENOIR; };
  };
  EOF

  # Create db.localhost
  cat > db.localhost << 'EOF'
  $TTL 86400
  @ IN SOA localhost. root.localhost. (
      1       ; serial
      3600    ; refresh
      1800    ; retry
      604800  ; expire
      86400 ) ; minimum
  
  @       IN NS    localhost.
  @       IN A     127.0.0.1
  localhost. IN A  127.0.0.1
  EOF
  
  # Create start_dns.sh
  cat > start_dns.sh << 'EOF'
  #!/bin/sh
  
  echo "Starting BIND DNS server..."
  named -g -c /myscripts/dns/named.conf
  EOF
  
  chmod +x start_dns.sh
  chmod 644 named.conf db.localhost
  
  echo "==================================================================="
  echo "DNS Slave configuration completed!"
  echo "Slave will automatically transfer zones from Master: $IP_RENOIR"
  echo ""
  echo "To start DNS server: cd /myscripts/dns && ./start_dns.sh"
  echo "==================================================================="
  ```

  ### CONFIGURASTION NETWORK SETIAP NODE KECUALI KEDUA SERVER HARUS DITAMBAHKAN `up echo -e "nameserver 10.91.3.2\nnameserver 10.91.3.3" | tee /etc/resolv.conf > /dev/null`

<br>

## Soal 3

> Tambahkan subdomain alias berupa exp.lune33.com yang mengarah ke alamat lune33.com dan exp.sciel33.com yang mengarah ke alamat sciel33.com (HINT: CNAME). Selain itu, tambahkan konfigurasi untuk melakukan reverse DNS lookup untuk domain gustave33.com

> _Subdomain Configuration,_ 
> _Add alias subdomains (HINT: CNAME)._
> _exp.lune33.com → alias to lune33.com_
> _exp.sciel33.com → alias to sciel33.com_
> _Also, configure reverse DNS lookup for the domain gustave33.com._

**Answer:**

- Screenshot

  ### ping exp.lune33.com
  
  <img width="715" height="476" alt="image" src="https://github.com/user-attachments/assets/95a1f718-5de5-454f-a475-8864f748260b" />

  ### ping exp.sciel33.com
  
  <img width="715" height="476" alt="image" src="https://github.com/user-attachments/assets/cc023337-aaf4-4c46-9617-c66eed45c1cb" />

  ### nslookup 10.91.2.4

  <img width="715" height="476" alt="image" src="https://github.com/user-attachments/assets/a960b6a7-397c-40ab-a54f-3c77a39f4398" />

- Explanation

  
  ### UBAH SCRIPT BERIKUT DAN IKUTI INSTRUKSI UNTUK MENYALAKAN `SERVER MASTER`

  ```bash
  #!/bin/bash

  IP_RENOIR="10.91.3.2"
  IP_VERSO="10.91.3.3"
  IP_LUNE="10.91.2.2"
  IP_SCIEL="10.91.2.3"
  IP_GUSTAVE="10.91.2.4"
  
  echo "Setting up DNS Master with IPs:"
  echo "Renoir: $IP_RENOIR"
  echo "Verso: $IP_VERSO"
  echo "Lune: $IP_LUNE"
  echo "Sciel: $IP_SCIEL"
  echo "Gustave: $IP_GUSTAVE"

  mkdir -p /myscripts/dns
  cd /myscripts/dns
  
  # Create named.conf
  cat > named.conf << EOF
  options {
      directory "/myscripts/dns";
      listen-on { any; };
      allow-query { any; };
      allow-notify { 10.91.3.3; };
      also-notify {10.91.3.3; };
      allow-transfer { 10.91.3.3; };
  };
  
  zone "lune33.com" {
      type master;
      file "db.lune33.com";
      allow-transfer { 10.91.3.3; };
  };
  
  zone "sciel33.com" {
      type master;
      file "db.sciel33.com";
      allow-transfer { 10.91.3.3; };
  };
  
  zone "gustave33.com" {
      type master;
      file "db.gustave33.com";
      allow-transfer { 10.91.3.3; };
  };

  zone "2.91.10.in-addr.arpa" {
    type master;
    file "db.rev.10.91.2";
    allow-transfer { 10.91.3.3; };
  };
  EOF
  

  # Create db.lune33.com
  cat > db.lune33.com << EOF
  \$TTL 86400
  @ IN SOA nsi.lune33.com. admin.lune33.com. (
      1 ; serial
      3600 ; refresh
      1800 ; retry
      604800 ; expire
      86400 ) ; minimum
  
  ; Name servers
  @       IN NS    nsi.lune33.com.
  @       IN NS    verso.lune33.com.
  
  ; Address records
  nsi     IN A     $IP_RENOIR
  verso   IN A     $IP_VERSO
  @       IN A     $IP_LUNE
  exp     IN CNAME lune33.com.
  EOF

  # Create db.sciel33.com
  cat > db.sciel33.com << EOF
  \$TTL 86400
  @ IN SOA nsi.sciel33.com. admin.sciel33.com. (
      1 ; serial
      3600 ; refresh
      1800 ; retry
      604800 ; expire
      86400 ) ; minimum
  
  ; Name servers
  @       IN NS    nsi.sciel33.com.
  @       IN NS    verso.sciel33.com.
  
  ; Address records
  nsi     IN A     $IP_RENOIR
  verso   IN A     $IP_VERSO
  @       IN A     $IP_SCIEL
  exp     IN CNAME sciel33.com.
  EOF

  # Create db.gustave33.com
  cat > db.gustave33.com << EOF
  \$TTL 86400
  @ IN SOA nsi.gustave33.com. admin.gustave33.com. (
      1 ; serial
      3600 ; refresh
      1800 ; retry
      604800 ; expire
      86400 ) ; minimum
  
  ; Name servers
  @       IN NS    nsi.gustave33.com.
  @       IN NS    verso.gustave33.com.
  
  ; Address records
  nsi     IN A     $IP_RENOIR
  verso   IN A     $IP_VERSO
  @       IN A     $IP_GUSTAVE
  EOF

  # Create reverse zone file for Gustave
  cat > db.rev.10.91.2 << EOF
  \$TTL 86400
  @ IN SOA nsi.gustave33.com. admin.gustave33.com. (
      1 ; serial
      3600 ; refresh
      1800 ; retry
      604800 ; expire
      86400 ) ; minimum
  
  ; Name servers
  @       IN NS    nsi.gustave33.com.
  @       IN NS    verso.gustave33.com.
  
  ; PTR records
  4       IN PTR gustave33.com.
  EOF

  # Create start_dns.sh
  cat > start_dns.sh << 'EOF'
  #!/bin/sh
  echo "Starting BIND DNS server..."
  named -g -c /myscripts/dns/named.conf
  EOF
  
  chmod +x start_dns.sh
  
  chmod 644 *.conf
  chmod 644 db.*

  echo "==============================================================="
  echo "DNS Master configuration completed (Clean Version)!"
  echo "Directory: /myscripts/dns"
  echo "Files created:"
  echo "  - named.conf"
  echo "  - db.lune33.com"
  echo "  - db.sciel33.com"
  echo "  - db.gustave33.com"
  echo "  - start_dns.sh"
  echo ""
  echo "Configured domains:"
  echo "  lune33.com -> $IP_LUNE"
  echo "  sciel33.com -> $IP_SCIEL"
  echo "  gustave33.com -> $IP_GUSTAVE"
  echo ""
  echo "To start DNS server: cd /myscripts/dns && ./start_dns.sh"
  echo "==============================================================="
  ```

  ### UBAH SCRIPT BERIKUT DAN IKUTI INSTRUKSI UNTUK MENYALAKAN `SERVER SLAVE`

  ```bash
  #!/bin/bash
  
  IP_RENOIR="10.91.3.2"
  IP_VERSO="10.91.3.3"
  
  echo "Setting up DNS Slave (Verso) with IP: $IP_VERSO"
  echo "Master DNS (Renoir) IP: $IP_RENOIR"
    
  mkdir -p /myscripts/dns/slave
  cd /myscripts/dns
  
  # Create named.conf for slave
  cat > named.conf << EOF
  options {
      directory "/myscripts/dns";
      listen-on { any; };
      allow-query { any; };
  };
  
  zone "localhost" {
      type master;
      file "db.localhost";
  };
  
  zone "lune33.com" {
      type slave;
      file "slave/db.lune33.com";
      masters { $IP_RENOIR; };
  };
  
  zone "sciel33.com" {
      type slave;
      file "slave/db.sciel33.com";
      masters { $IP_RENOIR; };
  };
  
  zone "gustave33.com" {
      type slave;
      file "slave/db.gustave33.com";
      masters { $IP_RENOIR; };
  };

  zone "2.91.10.in-addr.arpa" {
    type slave;
    masters { $IP_RENOIR; };
    file "slave/db.rev.10.91.2";
  };
  EOF

  # Create db.localhost
  cat > db.localhost << 'EOF'
  $TTL 86400
  @ IN SOA localhost. root.localhost. (
      1       ; serial
      3600    ; refresh
      1800    ; retry
      604800  ; expire
      86400 ) ; minimum
  
  @       IN NS    localhost.
  @       IN A     127.0.0.1
  localhost. IN A  127.0.0.1
  EOF
  
  # Create start_dns.sh
  cat > start_dns.sh << 'EOF'
  #!/bin/sh
  
  echo "Starting BIND DNS server..."
  named -g -c /myscripts/dns/named.conf
  EOF
  
  chmod +x start_dns.sh
  chmod 644 named.conf db.localhost
  
  echo "==================================================================="
  echo "DNS Slave configuration completed!"
  echo "Slave will automatically transfer zones from Master: $IP_RENOIR"
  echo ""
  echo "To start DNS server: cd /myscripts/dns && ./start_dns.sh"
  echo "==================================================================="
  ```



<br>

## Soal 4

> Buatlah subdomain berupa expedition.gustave33.com dan delegasikan subdomain tersebut dari Renoir ke Verso dengan alamat IP tujuan adalah node Gustave. Kemudian, matikan Renoir dan coba lakukan ping ke semua domain dan subdomain yang telah dikonfigurasikan pada nomor 2, 3, dan 4.

> _Create a subdomain expedition.gustave33.com and delegate it from Renoir to Verso, with the target IP being node Gustave.Then, turn off Renoir and try pinging all domains and subdomains configured in tasks 2, 3, and 4 to verify delegation works correctly._

**Answer:**

- Screenshot

  ### ping expedition.gustave33.com
  
  <img width="717" height="478" alt="image" src="https://github.com/user-attachments/assets/c6ac3e21-de0b-4015-8dde-17e38a02bf47" />

  ### ping lune33.com (master mati)
  
  <img width="1275" height="827" alt="image" src="https://github.com/user-attachments/assets/d97bfc77-fc76-4959-ba24-92817e1ab7cc" />

  ### ping sciel33.com (master mati)
  
  <img width="1275" height="827" alt="image" src="https://github.com/user-attachments/assets/812d32ae-be06-4445-b7de-caab115037c2" />

  ### ping gustave33.com (master mati)
  
  <img width="1275" height="827" alt="image" src="https://github.com/user-attachments/assets/ee48217b-4d6e-46be-be99-b55c58028cc5" />

  ### ping exp.lune33.com (master mati)
  
  <img width="1275" height="827" alt="image" src="https://github.com/user-attachments/assets/c82624a2-9b89-473a-b26b-2720900fd645" />

  ### ping exp.sciel33.com (master mati)
  
  <img width="1275" height="827" alt="image" src="https://github.com/user-attachments/assets/2f97fcb8-ba63-4f38-9454-8fb059bd3fe3" />

  ### nslookup 10.91.2.4 (master mati)
  
  <img width="1331" height="832" alt="image" src="https://github.com/user-attachments/assets/ead39252-af51-424f-b9a9-4e0d5ee633ea" />

  ### ping expedition.gustave33.com (master mati)
  
  <img width="1275" height="827" alt="image" src="https://github.com/user-attachments/assets/688127d7-587b-413d-8ac7-49ac3bb6a89b" />

- Explanation

  ### UBAH SCRIPT BERIKUT DAN IKUTI INSTRUKSI UNTUK MENYALAKAN `SERVER MASTER`

  ```bash
  #!/bin/bash

  IP_RENOIR="10.91.3.2"
  IP_VERSO="10.91.3.3"
  IP_LUNE="10.91.2.2"
  IP_SCIEL="10.91.2.3"
  IP_GUSTAVE="10.91.2.4"
  
  echo "Setting up DNS Master with IPs:"
  echo "Renoir: $IP_RENOIR"
  echo "Verso: $IP_VERSO"
  echo "Lune: $IP_LUNE"
  echo "Sciel: $IP_SCIEL"
  echo "Gustave: $IP_GUSTAVE"

  mkdir -p /myscripts/dns
  cd /myscripts/dns
  
  # Create named.conf
  cat > named.conf << EOF
  options {
      directory "/myscripts/dns";
      listen-on { any; };
      allow-query { any; };
      allow-notify { 10.91.3.3; };
      also-notify {10.91.3.3; };
      allow-transfer { 10.91.3.3; };
  };
  
  zone "lune33.com" {
      type master;
      file "db.lune33.com";
      allow-transfer { 10.91.3.3; };
  };
  
  zone "sciel33.com" {
      type master;
      file "db.sciel33.com";
      allow-transfer { 10.91.3.3; };
  };
  
  zone "gustave33.com" {
      type master;
      file "db.gustave33.com";
      allow-transfer { 10.91.3.3; };
  };

  zone "2.91.10.in-addr.arpa" {
    type master;
    file "db.rev.10.91.2";
    allow-transfer { 10.91.3.3; };
  };

  zone "expedition.gustave33.com" {
    type master;
    file "db.expedition.gustave33.com";
    allow-transfer { 10.91.3.3; };
  };
  EOF
  

  # Create db.lune33.com
  cat > db.lune33.com << EOF
  \$TTL 86400
  @ IN SOA nsi.lune33.com. admin.lune33.com. (
      1 ; serial
      3600 ; refresh
      1800 ; retry
      604800 ; expire
      86400 ) ; minimum
  
  ; Name servers
  @       IN NS    nsi.lune33.com.
  @       IN NS    verso.lune33.com.
  
  ; Address records
  nsi     IN A     $IP_RENOIR
  verso   IN A     $IP_VERSO
  @       IN A     $IP_LUNE
  exp     IN CNAME lune33.com.
  EOF

  # Create db.sciel33.com
  cat > db.sciel33.com << EOF
  \$TTL 86400
  @ IN SOA nsi.sciel33.com. admin.sciel33.com. (
      1 ; serial
      3600 ; refresh
      1800 ; retry
      604800 ; expire
      86400 ) ; minimum
  
  ; Name servers
  @       IN NS    nsi.sciel33.com.
  @       IN NS    verso.sciel33.com.
  
  ; Address records
  nsi     IN A     $IP_RENOIR
  verso   IN A     $IP_VERSO
  @       IN A     $IP_SCIEL
  exp     IN CNAME sciel33.com.
  EOF

  # Create db.gustave33.com
  cat > db.gustave33.com << EOF
  \$TTL 86400
  @ IN SOA nsi.gustave33.com. admin.gustave33.com. (
      1 ; serial
      3600 ; refresh
      1800 ; retry
      604800 ; expire
      86400 ) ; minimum
  
  ; Name servers
  @       IN NS    nsi.gustave33.com.
  @       IN NS    verso.gustave33.com.
  
  ; Address records
  nsi     IN A     $IP_RENOIR
  verso   IN A     $IP_VERSO
  @       IN A     $IP_GUSTAVE
  expedition       IN NS   verso.expedition.gustave33.com.
  verso.expedition IN A    $IP_VERSO
  EOF

  # Create reverse zone file for Gustave
  cat > db.rev.10.91.2 << EOF
  \$TTL 86400
  @ IN SOA nsi.gustave33.com. admin.gustave33.com. (
      1 ; serial
      3600 ; refresh
      1800 ; retry
      604800 ; expire
      86400 ) ; minimum
  
  ; Name servers
  @       IN NS    nsi.gustave33.com.
  @       IN NS    verso.gustave33.com.
  
  ; PTR records
  4       IN PTR gustave33.com.
  EOF

  # Create start_dns.sh
  cat > start_dns.sh << 'EOF'
  #!/bin/sh
  echo "Starting BIND DNS server..."
  named -g -c /myscripts/dns/named.conf
  EOF
  
  chmod +x start_dns.sh
  
  chmod 644 *.conf
  chmod 644 db.*

  echo "==============================================================="
  echo "DNS Master configuration completed (Clean Version)!"
  echo "Directory: /myscripts/dns"
  echo "Files created:"
  echo "  - named.conf"
  echo "  - db.lune33.com"
  echo "  - db.sciel33.com"
  echo "  - db.gustave33.com"
  echo "  - start_dns.sh"
  echo ""
  echo "Configured domains:"
  echo "  lune33.com -> $IP_LUNE"
  echo "  sciel33.com -> $IP_SCIEL"
  echo "  gustave33.com -> $IP_GUSTAVE"
  echo ""
  echo "To start DNS server: cd /myscripts/dns && ./start_dns.sh"
  echo "==============================================================="
  ```

  ### UBAH SCRIPT BERIKUT DAN IKUTI INSTRUKSI UNTUK MENYALAKAN `SERVER SLAVE`

  ```bash
  #!/bin/bash
  
  IP_RENOIR="10.91.3.2"
  IP_VERSO="10.91.3.3"
  
  echo "Setting up DNS Slave (Verso) with IP: $IP_VERSO"
  echo "Master DNS (Renoir) IP: $IP_RENOIR"
    
  mkdir -p /myscripts/dns/slave
  cd /myscripts/dns
  
  # Create named.conf for slave
  cat > named.conf << EOF
  options {
      directory "/myscripts/dns";
      listen-on { any; };
      allow-query { any; };
  };
  
  zone "localhost" {
      type master;
      file "db.localhost";
  };
  
  zone "lune33.com" {
      type slave;
      file "slave/db.lune33.com";
      masters { $IP_RENOIR; };
  };
  
  zone "sciel33.com" {
      type slave;
      file "slave/db.sciel33.com";
      masters { $IP_RENOIR; };
  };
  
  zone "gustave33.com" {
      type slave;
      file "slave/db.gustave33.com";
      masters { $IP_RENOIR; };
  };

  zone "2.91.10.in-addr.arpa" {
    type slave;
    masters { $IP_RENOIR; };
    file "slave/db.rev.10.91.2";
  };

  zone "expedition.gustave33.com" {
    type slave;
    file "slave/db.expedition.gustave33.com";
    masters { $IP_RENOIR; };
  };
  EOF

  # Create db.localhost
  cat > db.localhost << 'EOF'
  $TTL 86400
  @ IN SOA localhost. root.localhost. (
      1       ; serial
      3600    ; refresh
      1800    ; retry
      604800  ; expire
      86400 ) ; minimum
  
  @       IN NS    localhost.
  @       IN A     127.0.0.1
  localhost. IN A  127.0.0.1
  EOF
  
  # Create start_dns.sh
  cat > start_dns.sh << 'EOF'
  #!/bin/sh
  
  echo "Starting BIND DNS server..."
  named -g -c /myscripts/dns/named.conf
  EOF
  
  chmod +x start_dns.sh
  chmod 644 named.conf db.localhost
  
  echo "==================================================================="
  echo "DNS Slave configuration completed!"
  echo "Slave will automatically transfer zones from Master: $IP_RENOIR"
  echo ""
  echo "To start DNS server: cd /myscripts/dns && ./start_dns.sh"
  echo "==================================================================="
  ```



<br>

## Soal 5

> Konfigurasi node Lune, Sciel, dan Gustave agar berfungsi sebagai web server Nginx yang akan menyajikan halaman profil, dimana halaman profil akan berbeda untuk setiap node. Dari folder berikut, gunakan profile_lune.html untuk menyajikan halaman profil di node Lune, profile_sciel.html untuk menyajikan halaman profil di node Sciel, dan profile_gustave.html untuk menyajikan halaman profil di node Gustave. Konfigurasikan Nginx di setiap node untuk menyimpan custom access log ke file /tmp/access.log dan error log ke file /tmp/error.log. 

> _Configure Lune, Sciel, and Gustave as Nginx web servers serving profile pages, where each node has a unique profile page:_
> _- Use profile_lune.html for Lune_
> _- Use profile_sciel.html for Sciel_
> _- Use profile_gustave.html for Gustave_
> _In each web server, Configure Nginx to store custom logs:_
> _- Access log: /tmp/access.log_
> _- Error log: /tmp/error.log_

**Answer:**

- Screenshot

  ### curl -vL http://lune33.com
  
  <img width="1731" height="995" alt="image" src="https://github.com/user-attachments/assets/8a0f13de-fd9a-4e62-97fd-fbe765ddf90c" />
  <img width="1731" height="995" alt="image" src="https://github.com/user-attachments/assets/338d9b10-cc03-45eb-bcc7-c7aa81ef39d5" />

  ### curl -vL http://sciel33.com
  
  <img width="1731" height="995" alt="image" src="https://github.com/user-attachments/assets/5f5f654d-262f-434f-b933-ed6cdceed1e4" />
  <img width="1731" height="995" alt="image" src="https://github.com/user-attachments/assets/112c218e-d9ad-4988-9496-5e776c11cda8" />

  ### curl -vL http://gustave33.com
  
  <img width="1731" height="995" alt="image" src="https://github.com/user-attachments/assets/327df536-91f0-4003-82e2-7a561219a0d9" />
  <img width="1731" height="995" alt="image" src="https://github.com/user-attachments/assets/484fd9de-1596-4336-b258-c919986eb3d4" />

  ### access.log lune
  
  <img width="723" height="482" alt="image" src="https://github.com/user-attachments/assets/aa61b510-3577-45c8-8bcd-4cc57001c23a" />

  ### access.log sciel
  
  <img width="723" height="482" alt="image" src="https://github.com/user-attachments/assets/f171e17a-5979-43ff-8832-e9314dc74acc" />

  ### access.log gustave
  
  <img width="723" height="482" alt="image" src="https://github.com/user-attachments/assets/ff942a04-182e-4ee0-8855-e554342b59a9" />


- Explanation

  ###   TAMBAHKAN SCRIPT BERIKUT PADA LUNE DAN JALAN KAN SEBAGAI WEBSERVER

  ```bash
  #!/bin/bash
  NODE_NAME="Lune"
  PROFILE_FILE="profile_${NODE_NAME,,}.html"

  mkdir -p /myscripts/myconfig
  mkdir -p /myscripts/myweb
  mkdir -p /myscripts/mylogs
  
  cat > /myscripts/myweb/$PROFILE_FILE << EOF
  <!DOCTYPE html>
  <html lang="en">
  <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Lune</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, Helvetica, sans-serif;
      color: #fff;
      background: linear-gradient(270deg, #0a043c, #4a0c25, #7b1b0c, #f9a826);
      background-size: 800% 800%;
      animation: bgShift 20s ease infinite;
    }
    @keyframes bgShift {
      0% {background-position: 0% 50%;}
      50% {background-position: 100% 50%;}
      100% {background-position: 0% 50%;}
    }
    .container {
      max-width: 700px;
      margin: 4rem auto;
      background: rgba(255, 255, 255, 0.08);
      border-radius: 16px;
      padding: 2.5rem;
      box-shadow: 0 4px 20px rgba(0,0,0,0.5);
      backdrop-filter: blur(6px);
    }
    h1 {
      text-align: center;
      color: #00e5ff;
      text-shadow: 0 0 10px #00e5ff;
    }
    h2 {
      color: #ffd166;
      margin-top: 1.5rem;
    }
    p, li { line-height: 1.6; }
    ul { list-style: none; padding-left: 0; }
    li::before { content: "▹ "; color: #00e5ff; }
    footer { text-align: center; font-size: 0.85rem; color: #ccc; margin-top: 2rem; }
  </style>
  </head>
  <body>
    <div class="container">
      <h1>Lune</h1>
      <p><i>“When one falls, we continue.”</i></p>
  
      <h2>Core Expertise</h2>
      <ul>
        <li>Analysis of Ancient Technologies</li>
        <li>Equipment Calibration & Field Maintenance</li>
        <li>Defense System Deployment</li>
        <li>Data Collection & Anomaly Detection</li>
      </ul>
  
      <h2>Profile Summary</h2>
      <p>Lune is the mind behind the expedition’s technology. 
         Her understanding of ancient mechanisms and magical artifacts 
         is key to decoding the Paintress’ secrets and keeping the team operational.</p>
    </div>
  
    <footer>© 2025 Expedition 33 — For Those Who Come After</footer>
  </body>
  </html>
  EOF

  cat > /myscripts/myconfig/nginx.conf << EOF
  user www-data;
  worker_processes auto;
  pid /tmp/nginx.pid;
  
  events {
      worker_connections 768;
  }
  
  http {
      access_log /tmp/access.log;
      error_log /tmp/error.log;
  
      server {
          listen 80;
          server_name _;
  
          root /myscripts/myweb;
          index ${PROFILE_FILE};
  
          location / {
              try_files \$uri \$uri/ =404;
          }
      }
  }
  EOF
  
  cat > /myscripts/myconfig/start_http.sh << 'EOF'
  #!/bin/bash
  nginx -c /myscripts/myconfig/nginx.conf -g 'daemon off;'
  EOF
  
  chmod +x /myscripts/myconfig/start_http.sh
  echo "To start: cd /myscripts/myconfig && ./start_http.sh"
  ```

  ###   TAMBAHKAN SCRIPT BERIKUT PADA SCIEL DAN JALAN KAN SEBAGAI WEBSERVER

  ```bash
  #!/bin/bash
  NODE_NAME="Sciel"
  PROFILE_FILE="profile_${NODE_NAME,,}.html"

  mkdir -p /myscripts/myconfig
  mkdir -p /myscripts/myweb
  mkdir -p /myscripts/mylogs
  
  cat > /myscripts/myweb/$PROFILE_FILE << EOF
  <!DOCTYPE html>
  <html lang="en">
  <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sciel</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, Helvetica, sans-serif;
      color: #fff;
      background: linear-gradient(270deg, #0a043c, #4a0c25, #7b1b0c, #f9a826);
      background-size: 800% 800%;
      animation: bgShift 20s ease infinite;
    }
    @keyframes bgShift {
      0% {background-position: 0% 50%;}
      50% {background-position: 100% 50%;}
      100% {background-position: 0% 50%;}
    }
    .container {
      max-width: 700px;
      margin: 4rem auto;
      background: rgba(255, 255, 255, 0.08);
      border-radius: 16px;
      padding: 2.5rem;
      box-shadow: 0 4px 20px rgba(0,0,0,0.5);
      backdrop-filter: blur(6px);
    }
    h1 {
      text-align: center;
      color: #00e5ff;
      text-shadow: 0 0 10px #00e5ff;
    }
    h2 {
      color: #ffd166;
      margin-top: 1.5rem;
    }
    p, li { line-height: 1.6; }
    ul { list-style: none; padding-left: 0; }
    li::before { content: "▹ "; color: #00e5ff; }
    footer { text-align: center; font-size: 0.85rem; color: #ccc; margin-top: 2rem; }
  </style>
  </head>
  <body>
    <div class="container">
      <h1>Sciel</h1>
      <p><i>“Tomorrow comes.”</i></p>
  
      <h2>Core Expertise</h2>
      <ul>
        <li>Stealth and Infiltration Tactics</li>
        <li>Long-Range Surveillance</li>
        <li>Navigation and Terrain Mapping</li>
        <li>Rapid Response & Target Acquisition</li>
      </ul>
  
      <h2>Profile Summary</h2>
      <p>Sciel serves as the eyes and ears of Expedition 33. 
         Agile and perceptive, he scouts ahead, tracks enemy movement, 
         and secures safe passage before every major confrontation.</p>
    </div>
  
    <footer>© 2025 Expedition 33 — For Those Who Come After</footer>
  </body>
  </html>
  EOF

  cat > /myscripts/myconfig/nginx.conf << EOF
  user www-data;
  worker_processes auto;
  pid /tmp/nginx.pid;
  
  events {
      worker_connections 768;
  }
  
  http {
      access_log /tmp/access.log;
      error_log /tmp/error.log;
  
      server {
          listen 80;
          server_name _;
  
          root /myscripts/myweb;
          index ${PROFILE_FILE};
  
          location / {
              try_files \$uri \$uri/ =404;
          }
      }
  }
  EOF
  
  cat > /myscripts/myconfig/start_http.sh << 'EOF'
  #!/bin/bash
  nginx -c /myscripts/myconfig/nginx.conf -g 'daemon off;'
  EOF
  
  chmod +x /myscripts/myconfig/start_http.sh
  echo "To start: cd /myscripts/myconfig && ./start_http.sh"
  ```

  ###   TAMBAHKAN SCRIPT BERIKUT PADA GUSTAVE DAN JALAN KAN SEBAGAI WEBSERVER

  ```bash
  #!/bin/bash
  NODE_NAME="Gustave"
  PROFILE_FILE="profile_${NODE_NAME,,}.html"

  mkdir -p /myscripts/myconfig
  mkdir -p /myscripts/myweb
  mkdir -p /myscripts/mylogs
  
  cat > /myscripts/myweb/$PROFILE_FILE << EOF
  <!DOCTYPE html>
  <html lang="en">
  <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Gustave</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, Helvetica, sans-serif;
      color: #fff;
      background: linear-gradient(270deg, #0a043c, #4a0c25, #7b1b0c, #f9a826);
      background-size: 800% 800%;
      animation: bgShift 20s ease infinite;
    }
    @keyframes bgShift {
      0% {background-position: 0% 50%;}
      50% {background-position: 100% 50%;}
      100% {background-position: 0% 50%;}
    }
    .container {
      max-width: 700px;
      margin: 4rem auto;
      background: rgba(255, 255, 255, 0.08);
      border-radius: 16px;
      padding: 2.5rem;
      box-shadow: 0 4px 20px rgba(0,0,0,0.5);
      backdrop-filter: blur(6px);
    }
    h1 {
      text-align: center;
      color: #00e5ff;
      text-shadow: 0 0 10px #00e5ff;
    }
    h2 {
      color: #ffd166;
      margin-top: 1.5rem;
    }
    p, li {
      line-height: 1.6;
    }
    ul {
      list-style: none;
      padding-left: 0;
    }
    li::before {
      content: "▹ ";
      color: #00e5ff;
    }
    footer {
      text-align: center;
      font-size: 0.85rem;
      color: #ccc;
      margin-top: 2rem;
    }
  </style>
  </head>
  <body>
    <div class="container">
      <h1>Gustave</h1>
      <p><i>“For those who come after. Right?”</i></p>
  
      <h2>Core Expertise</h2>
      <ul>
        <li>Tactical Combat Planning</li>
        <li>Frontline Leadership & Morale Management</li>
        <li>Heavy Weapon Proficiency</li>
        <li>Risk Assessment & Survival Techniques</li>
      </ul>
  
      <h2>Profile Summary</h2>
      <p>Gustave is a battle-hardened veteran and the backbone of Expedition 33. 
         With experience from countless missions, he oversees every tactical decision 
         made in the war against the Paintress.</p>
    </div>
  
    <footer>© 2025 Expedition 33 — For Those Who Come After</footer>
  </body>
  </html>
  EOF

  cat > /myscripts/myconfig/nginx.conf << EOF
  user www-data;
  worker_processes auto;
  pid /tmp/nginx.pid;
  
  events {
      worker_connections 768;
  }
  
  http {
      access_log /tmp/access.log;
      error_log /tmp/error.log;
  
      server {
          listen 80;
          server_name _;
  
          root /myscripts/myweb;
          index ${PROFILE_FILE};
  
          location / {
              try_files \$uri \$uri/ =404;
          }
      }
  }
  EOF
  
  cat > /myscripts/myconfig/start_http.sh << 'EOF'
  #!/bin/bash
  nginx -c /myscripts/myconfig/nginx.conf -g 'daemon off;'
  EOF
  
  chmod +x /myscripts/myconfig/start_http.sh
  echo "To start: cd /myscripts/myconfig && ./start_http.sh"
  ```


<br>
 
## Soal 6

> Setelah website berhasil dideploy pada masing-masing node web server dan halaman dapat menampilkan profil yang sesuai,  buatlah custom access log ke file /tmp/access.log di masing-masing node web server menggunakan format log tertentu seperti di bawah:
> - Tanggal dan waktu akses dalam format standar log.
> - Nama node yang sedang diakses.
> - Alamat IP klien yang mengakses website.
> - Metode HTTP dan URI yang diakses oleh klien.
> - Status respons HTTP yang diberikan oleh server.
> - Jumlah byte yang dikirimkan dalam respons.
> - Waktu yang dihabiskan oleh server untuk menangani permintaan.
> - Contoh format log yang sesuai:
>   [01/Oct/2024:11:30:45 +0000] Jarkom Node Lune Access from 192.168.1.15 using method "GET /resep/bayam HTTP/1.1" returned status 200 with 2567 bytes sent in 0.038 seconds

> _After successfully deploying each website and verifying the correct profile page is displayed, create a custom access log in /tmp/access.log on each web server using the following format:_
> _- Date and time of access (standard log format)_
> _- Name of the node being accessed_
> _- IP address of the client accessing the website_
> _- HTTP method and URI accessed by the client_
> _- HTTP response status code_
> _- Number of bytes sent in the response_
> _- Time taken by the server to process the request_
> _- Example Log Format:_
> _[01/Oct/2024:11:30:45 +0000] Jarkom Node Lune Access from 192.168.1.15 using method "GET /resep/bayam HTTP/1.1" returned status 200 with 2567 bytes sent in 0.038 seconds_

**Answer:**

- Screenshot

  ### curl -vL http://lune33.com
  
  <img width="723" height="482" alt="image" src="https://github.com/user-attachments/assets/09e1e686-689c-4abc-bf01-a8cd5333150c" />

  ### access.log lune
  
  <img width="723" height="482" alt="image" src="https://github.com/user-attachments/assets/9c3682ac-eb12-4301-bccb-d425edb52594" />

  ### curl -vL http://sciel33.com
  
  <img width="723" height="482" alt="image" src="https://github.com/user-attachments/assets/11ac5058-c88c-4421-9a69-c3dbd6d2212a" />

  ### access.log sciel
  
  <img width="723" height="482" alt="image" src="https://github.com/user-attachments/assets/d6ab4deb-39a3-4a0c-b2ea-5e1677fd6fda" />

  ### curl -vL http://gustave33.com
  
  <img width="723" height="482" alt="image" src="https://github.com/user-attachments/assets/9f5cbce8-c196-455c-a940-1c947425653b" />

  ### access.log gustave
  
  <img width="723" height="482" alt="image" src="https://github.com/user-attachments/assets/64296bce-1445-48c7-82a1-f8a033e56b47" />

- Explanation

  ### UBAH SETIAP SCRIPT WEB SERVER PADA BAGIAN BLOK `server` PADA BAGIAN BERIKUT
  ### UBAH

  ```bash
  access_log /tmp/access.log;
  ```

  ### MENJADI

  ```bash
  log_format custom '[\$time_local] Jarkom Node ${NODE_NAME} Access from \$remote_addr using method "\$request" returned status \$status with \$body_bytes_sent bytes sent in \$request_time seconds';
  access_log /tmp/access.log custom;
  ```

  ### JALANKAN ULANG SCRIPT WEB SERVER

<br>

## Soal 7

> Gustave merupakan web server yang tidak disarankan untuk dilihat oleh publik. Maka dari itu, ubahlah konfigurasi nginx sehingga halaman profil Gustave menjadi hanya bisa di akses melalui port 8080 dan 8888.

> _The Gustave web server should not be publicly accessible.
Modify the Nginx configuration so that Gustave’s profile page can only be accessed through ports 8080 and 8888._

**Answer:**

- Screenshot

  ### ERROR curl -vL http://gustave33.com
  
  <img width="1018" height="253" alt="image" src="https://github.com/user-attachments/assets/db059ee6-930d-45d4-970c-235da105a62b" />

  ### curl -vL http://gustave33.com:8888
  
  <img width="802" height="468" alt="image" src="https://github.com/user-attachments/assets/9bfb7185-f046-4c6e-ba22-fd547154059c" />

  ### curl -vL http://gustave33.com:8888
  
  <img width="802" height="468" alt="image" src="https://github.com/user-attachments/assets/3c2197a3-0acf-45f6-8e66-7e82c5970fac" />


- Explanation

  ### UBAH SCRIPT WEB SERVER `Gustave` PADA BAGIAN BLOK `server` Di BAGIAN BERIKUT
  ### UBAH

  ```bash
  listen 80;
  ```

  ### MENJADI

  ```bash
  listen 8080;
  listen 8888;
  ```

  ### JALANKAN ULANG SCRIPT WEB SERVER

<br>

## Soal 8

> Untuk mempermudah program ekspedisi, maka node Lune, Sciel, Gustave sepakat untuk membuat halaman informasi dengan konten yang sama. Maka dari itu, buatlah lagi 1 server block di dalam konfigurasi nginx yang akan menyajikan file HTML ini. Namun, mereka ingin menyajikan halaman informasi tersebut di port yang berbeda-beda, yaitu Lune menggunakan port 8000, Sciel menggunakan port 8100, dan Gustave menggunakan port 8200.

> _To simplify coordination for the expedition program, Lune, Sciel, and Gustave agree to create a shared information page with the same content. Add one more server block in each node’s Nginx configuration that serves this HTML file 
Each node should serve the information page on a different port:_
> _- Lune → port 8000_
> _- Sciel → port 8100_
> _- Gustave → port 8200_

**Answer:**

- Screenshot

  ### curl -vL http://lune33.com:8000 (Info)
  
  <img width="1563" height="988" alt="image" src="https://github.com/user-attachments/assets/568d1978-6a8f-42a2-a3e2-9651ecdf6de9" />
  <img width="1563" height="988" alt="image" src="https://github.com/user-attachments/assets/0cbc2f71-8e8b-4898-9d82-4dfc2183ea91" />

  ### curl -vL http://sciel33.com:8100 (Info)
  
  <img width="1563" height="988" alt="image" src="https://github.com/user-attachments/assets/61e1d29f-1abb-4633-a1d3-0f7a9a309b20" />
  <img width="1563" height="988" alt="image" src="https://github.com/user-attachments/assets/35bdc077-4d8b-48ea-ac28-8a1c10c9f48e" />

  ### curl -vL http://gustave33.com:8200 (Info)
  
  <img width="1563" height="988" alt="image" src="https://github.com/user-attachments/assets/ff945c7c-a423-494f-a4d5-c7392c7cde14" />
  <img width="1563" height="988" alt="image" src="https://github.com/user-attachments/assets/2442a517-75d7-4319-89b2-31a2a281c971" />

- Explanation

  ### UNTUK SETIAP WEBSERVER TAMBAHKAN VARIABLE INI

  ```bash
  INFO_FILE="expedition33.html"
  ```

  ### LALU TAMBAHKAN HTML BARU UNTUK `INFORMATION`

  ```bash
  cat > /myscripts/myweb/$INFO_FILE << EOF
  <!DOCTYPE html>
  <html lang="en">
  <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Expedition 33 — Mission Brief</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, Helvetica, sans-serif;
      color: #fff;
      background: linear-gradient(270deg, #0a043c, #4a0c25, #7b1b0c, #f9a826);
      background-size: 800% 800%;
      animation: bgShift 20s ease infinite;
    }
    @keyframes bgShift {
      0% {background-position: 0% 50%;}
      50% {background-position: 100% 50%;}
      100% {background-position: 0% 50%;}
    }
    header {
      text-align: center;
      padding: 3rem 1rem 1rem;
    }
    header h1 {
      font-size: 2.8rem;
      color: #00e5ff;
      letter-spacing: 1px;
      text-shadow: 0 0 10px #00e5ff;
    }
    header p {
      font-size: 1rem;
      color: #b5eaff;
      margin-top: 0.3rem;
    }
    .container {
      max-width: 700px;
      margin: 2rem auto 3rem;
      background: rgba(255, 255, 255, 0.08);
      border-radius: 16px;
      padding: 2rem 2.5rem;
      box-shadow: 0 4px 20px rgba(0,0,0,0.4);
      backdrop-filter: blur(6px);
    }
    h2 {
      color: #00e5ff;
      margin-top: 1.5rem;
    }
    p, li {
      line-height: 1.6;
    }
    ul {
      list-style: none;
      padding: 0;
    }
    li {
      margin: 0.4rem 0;
    }
    strong {
      color: #ffd166;
    }
    .motto {
      text-align: center;
      margin-top: 1.5rem;
      font-style: italic;
      color: #ccc;
      opacity: 0.9;
    }
    footer {
      text-align: center;
      font-size: 0.9rem;
      color: #ddd;
      padding: 1rem;
      background: rgba(0,0,0,0.4);
    }
  </style>
  </head>
  <body>
  <header>
    <h1>Expedition 33</h1>
    <p>Mission Log #E33-00</p>
  </header>
  
  <div class="container">
    <h2>Mission Brief</h2>
    <p>Expedition 33 is a digital exploration led by <strong>Lune</strong>, <strong>Sciel</strong>, and <strong>Gustave</strong>.  
       Together, they traverse the unknown layers of the Paintress system — seeking order within chaos.</p>
  
    <h2>Active Nodes</h2>
    <ul>
      <li><strong>Lune</strong> — “When one falls, we continue.”</li>
      <li><strong>Sciel</strong> — “Tomorrow comes.”</li>
      <li><strong>Gustave</strong> — “For those who come after. Right?”</li>
    </ul>
  
    <h2>Ports</h2>
    <ul>
      <li>Lune: Port <strong>8000</strong></li>
      <li>Sciel: Port <strong>8100</strong></li>
      <li>Gustave: Port <strong>8200</strong></li>
    </ul>
  
    <div class="motto">
      “Despite the odds, Expedition 33 marches forward.”
    </div>
  </div>
  
  <footer>
    © 2025 Expedition 33 — For Those Who Come After
  </footer>
  </body>
  </html>
  EOF
  ```

  ### TAMBAHKAN BLOCK `server` YANG BARU ( ganti "PORT" dengan ketentuan yang diberikan tanpa tanda petik )

  ```bash
  server {
        listen "PORT";
        server_name _;
   
        root /myscripts/myweb;
        index ${INFO_FILE};

        location / {
            try_files \$uri \$uri/ =404;
        }
    }
  ```

<br>

## Soal 9

> Untuk mempermudah akses ke profil tiap anggota ekspedisi, buatlah 1 domain lagi yaitu "expeditioners.com" yang akan mengarah ke Alicia. Lalu, untuk mencegah overload dari salah satu web server, konfigurasikan reverse proxy Alicia agar bisa forward request ke server yang sesuai berdasarkan URL profile yang diminta oleh klien dengan ketentuan sebagai berikut:
> -  Request untuk “expeditioners.com/profil_lune” harus dialihkan ke halaman profil web server Lune.
> -  Request untuk “expeditioners.com/profil_sciel” harus dialihkan ke halaman profil web server Sciel.
> -  Request untuk “expeditioners.com/profil_gustave” harus dialihkan ke halaman profil web server Gustave.
> Jika terdapat request ke URL selain profil yang ditentukan, reverse proxy akan mengalihkan ke halaman informasi pada web server Lune.

> _To make it easier to access each member’s profile, create a new domain “expeditioners.com” that points to Alicia. "
Configure Alicia’s reverse proxy (Nginx) to forward requests to the correct web server based on the requested URL, with the following rules:_
> _- Request URL expeditioners.com/profil_lune, Forward To Lune’s profile page_
> _- Request URL expeditioners.com/profil_sciel, Forward To Sciel’s profile page_
> _- Request URL expeditioners.com/profil_gustave, Forward To Gustave’s profile page_
> _- Any other URL, Forward To Lune’s information page_

**Answer:**

- Screenshot

  ### curl -vL http://expeditioners.com/profile_lune
  
  <img width="672" height="989" alt="image" src="https://github.com/user-attachments/assets/e1a82989-cb57-4fea-8a87-300ffddf96f0" />

  ### curl -vL http://expeditioners.com/profile_sciel
  
  <img width="672" height="989" alt="image" src="https://github.com/user-attachments/assets/8ecd929f-b1f9-47b2-af46-3294e2ebe095" />

  ### curl -vL http://expeditioners.com/profile_gustave
  
  <img width="672" height="989" alt="image" src="https://github.com/user-attachments/assets/5ebf0b68-3141-4408-95c0-c90134dec359" />

  ### curl -vL http://expeditioners.com
  
  <img width="672" height="989" alt="image" src="https://github.com/user-attachments/assets/5e1f31df-c958-44f0-98bf-8dd6728b454d" />


- Explanation

  ### UBAH SCRIPT PADA SERVER MASTER JADI SEPERTI INI

  ```bash
  #!/bin/bash

  IP_RENOIR="10.91.3.2"
  IP_VERSO="10.91.3.3"
  IP_LUNE="10.91.2.2"
  IP_SCIEL="10.91.2.3"
  IP_GUSTAVE="10.91.2.4"
  IP_ALICIA="10.91.4.2"
  
  echo "Setting up DNS Master with IPs:"
  echo "Renoir: $IP_RENOIR"
  echo "Verso: $IP_VERSO"
  echo "Lune: $IP_LUNE"
  echo "Sciel: $IP_SCIEL"
  echo "Gustave: $IP_GUSTAVE"
  echo "Alicia: $IP_ALICIA"
  
  mkdir -p /myscripts/dns
  cd /myscripts/dns
  
  # Create named.conf
  cat > named.conf << EOF
  options {
      directory "/myscripts/dns";
      listen-on { any; };
      allow-query { any; };
      allow-notify { 10.91.3.3; };
      also-notify {10.91.3.3; };
      allow-transfer { 10.91.3.3; };
  };
  
  zone "lune33.com" {
      type master;
      file "db.lune33.com";
      allow-transfer { 10.91.3.3; };
  };
  
  zone "sciel33.com" {
      type master;
      file "db.sciel33.com";
      allow-transfer { 10.91.3.3; };
  };
  
  zone "gustave33.com" {
      type master;
      file "db.gustave33.com";
      allow-transfer { 10.91.3.3; };
  };
  
  zone "2.91.10.in-addr.arpa" {
      type master;
      file "db.rev.10.91.2";
      allow-transfer { 10.91.3.3; };
  };
  
  zone "expedition.gustave33.com" {
      type master;
      file "db.expedition.gustave33.com";
      allow-transfer { 10.91.3.3; };
  };
  
  zone "expeditioners.com" {
      type master;
      file "db.expeditioners.com";
      allow-transfer { 10.91.3.3; };
  };
  EOF
  
  # Create db.lune33.com
  cat > db.lune33.com << EOF
  \$TTL 86400
  @ IN SOA nsi.lune33.com. admin.lune33.com. (
      1 ; serial
      3600 ; refresh
      1800 ; retry
      604800 ; expire
      86400 ) ; minimum
  
  ; Name servers
  @       IN NS    nsi.lune33.com.
  @       IN NS    verso.lune33.com.
  
  ; Address records
  nsi     IN A     $IP_RENOIR
  verso   IN A     $IP_VERSO
  @       IN A     $IP_LUNE
  exp     IN CNAME lune33.com.
  EOF
  
  # Create db.sciel33.com
  cat > db.sciel33.com << EOF
  \$TTL 86400
  @ IN SOA nsi.sciel33.com. admin.sciel33.com. (
      1 ; serial
      3600 ; refresh
      1800 ; retry
      604800 ; expire
      86400 ) ; minimum
  
  ; Name servers
  @       IN NS    nsi.sciel33.com.
  @       IN NS    verso.sciel33.com.
  
  ; Address records
  nsi     IN A     $IP_RENOIR
  verso   IN A     $IP_VERSO
  @       IN A     $IP_SCIEL
  exp     IN CNAME sciel33.com.
  EOF
  
  # Create db.gustave33.com
  cat > db.gustave33.com << EOF
  \$TTL 86400
  @ IN SOA nsi.gustave33.com. admin.gustave33.com. (
      1 ; serial
      3600 ; refresh
      1800 ; retry
      604800 ; expire
      86400 ) ; minimum
  
  ; Name servers
  @       IN NS    nsi.gustave33.com.
  @       IN NS    verso.gustave33.com.
  
  ; Address records
  nsi     IN A     $IP_RENOIR
  verso   IN A     $IP_VERSO
  @       IN A     $IP_GUSTAVE
  expedition       IN NS   verso.expedition.gustave33.com.
  verso.expedition IN A    $IP_VERSO
  EOF
  
  # Create reverse zone file for Gustave
  cat > db.rev.10.91.2 << EOF
  \$TTL 86400
  @ IN SOA nsi.gustave33.com. admin.gustave33.com. (
      1 ; serial
      3600 ; refresh
      1800 ; retry
      604800 ; expire
      86400 ) ; minimum
  
  ; Name servers
  @       IN NS    nsi.gustave33.com.
  @       IN NS    verso.gustave33.com.
  
  ; PTR records
  4       IN PTR gustave33.com.
  EOF
  
  # Expedition subdomain delegated to Verso
  cat > db.expedition.gustave33.com << EOF
  \$TTL 86400
  @ IN SOA nsi.expedition.gustave33.com. admin.expedition.gustave33.com. (
      1       ; serial
      3600    ; refresh
      1800    ; retry
      604800  ; expire
      86400 ) ; minimum
  
  ; Name servers
  @       IN NS    verso.expedition.gustave33.com.
  
  ; Address records
  verso   IN A     $IP_VERSO
  @       IN A     $IP_GUSTAVE
  EOF
  
  # Expeditioners domain for Alicia (reverse proxy target)
  cat > db.expeditioners.com << EOF
  \$TTL 86400
  @ IN SOA nsi.expeditioners.com. admin.expeditioners.com. (
      1 ; serial
      3600 ; refresh
      1800 ; retry
      604800 ; expire
      86400 ) ; minimum
  
  @       IN NS    nsi.expeditioners.com.
  nsi     IN A     $IP_RENOIR
  @       IN A     $IP_ALICIA
  EOF
  
  # Create start_dns.sh
  cat > start_dns.sh << 'EOF'
  #!/bin/sh
  echo "Starting BIND DNS server..."
  named -g -c /myscripts/dns/named.conf
  EOF
  
  chmod +x start_dns.sh
  
  chmod 644 *.conf
  chmod 644 db.*
  
  echo "==============================================================="
  echo "DNS Master configuration completed (Clean Version)!"
  echo "Directory: /myscripts/dns"
  echo "Files created:"
  echo "  - named.conf"
  echo "  - db.lune33.com"
  echo "  - db.sciel33.com"
  echo "  - db.gustave33.com"
  echo "  - db.rev.10.91.2"
  echo "  - db.expedition.gustave33.com"
  echo "  - db.expeditioners.com"
  echo "  - start_dns.sh"
  echo ""
  echo "Configured domains:"
  echo "  lune33.com -> $IP_LUNE"
  echo "  sciel33.com -> $IP_SCIEL"
  echo "  gustave33.com -> $IP_GUSTAVE"
  echo "  expeditioners.com -> $IP_ALICIA"
  echo ""
  echo "To start DNS server: cd /myscripts/dns && ./start_dns.sh"
  echo "==============================================================="
  ```

  ### UBAH JUGA SLAVE SERVER SCRIPTNYA MENJADI SEPERTI BERIKUT

  ```bash
  #!/bin/bash
  
  IP_RENOIR="10.91.3.2"
  IP_VERSO="10.91.3.3"
  
  echo "Setting up DNS Slave (Verso) with IP: $IP_VERSO"
  echo "Master DNS (Renoir) IP: $IP_RENOIR"
  
  mkdir -p /myscripts/dns/slave
  cd /myscripts/dns
  
  # Create named.conf for slave
  cat > named.conf << EOF
  options {
      directory "/myscripts/dns";
      listen-on { any; };
      allow-query { any; };
  };
  
  zone "localhost" {
      type master;
      file "db.localhost";
  };
  
  zone "lune33.com" {
      type slave;
      file "slave/db.lune33.com";
      masters { $IP_RENOIR; };
  };
  
  zone "sciel33.com" {
      type slave;
      file "slave/db.sciel33.com";
      masters { $IP_RENOIR; };
  };
  
  zone "gustave33.com" {
      type slave;
      file "slave/db.gustave33.com";
      masters { $IP_RENOIR; };
  };
  
  zone "2.91.10.in-addr.arpa" {
      type slave;
      masters { $IP_RENOIR; };
      file "slave/db.rev.10.91.2";
  };
  
  zone "expedition.gustave33.com" {
      type slave;
      file "slave/db.expedition.gustave33.com";
      masters { $IP_RENOIR; };
  };
  
  zone "expeditioners.com" {
      type slave;
      file "slave/db.expeditioners.com";
      masters { $IP_RENOIR; };
  };
  EOF
  
  # Create db.localhost
  cat > db.localhost << 'EOF'
  $TTL 86400
  @ IN SOA localhost. root.localhost. (
      1       ; serial
      3600    ; refresh
      1800    ; retry
      604800  ; expire
      86400 ) ; minimum
  
  @       IN NS    localhost.
  @       IN A     127.0.0.1
  localhost. IN A  127.0.0.1
  EOF
  
  # Create start_dns.sh
  cat > start_dns.sh << 'EOF'
  #!/bin/sh
  
  echo "Starting BIND DNS server..."
  named -g -c /myscripts/dns/named.conf
  EOF
  
  chmod +x start_dns.sh
  chmod 644 named.conf db.localhost
  
  echo "==================================================================="
  echo "DNS Slave configuration completed!"
  echo "Slave will automatically transfer zones from Master: $IP_RENOIR"
  echo ""
  echo "To start DNS server: cd /myscripts/dns && ./start_dns.sh"
  echo "==================================================================="
  ```

  ### SELAIN ITU BUAT SCRIPT JUGA UNTUK `Alicia` SEBAGAI BERIKUT

  ```bash
  #!/bin/bash
  
  IP_LUNE="10.91.2.2"
  IP_SCIEL="10.91.2.3"
  IP_GUSTAVE="10.91.2.4"
  
  echo "Setting up Nginx Reverse Proxy for Alicia (expeditioners.com)..."
  
  mkdir -p /myscripts/myconfig
  cd /myscripts/myconfig
  
  # Create nginx.conf
  cat > nginx.conf << EOF
  user www-data;
  worker_processes auto;
  pid /tmp/nginx.pid;
  
  events {
      worker_connections 1024;
  }
  
  http {
      log_format custom '[$time_local] Expeditioners Proxy Access from $remote_addr requested "$request" -> upstream $upstream_addr with status $status';
      access_log /tmp/access.log custom;
      error_log /tmp/error.log;
  
      server {
          listen 80;
          server_name expeditioners.com www.expeditioners.com;
  
          # Profil routes
          location /profil_lune {
              proxy_pass http://$IP_LUNE:80/;
          }
  
          location /profil_sciel {
              proxy_pass http://$IP_SCIEL:80/;
          }
  
          location /profil_gustave {
              proxy_pass http://$IP_GUSTAVE:8888/;
          }
  
          location / {
              proxy_pass http://$IP_LUNE:8000;
          }
      }
  }
  EOF
  
  # Start script for nginx
  cat > start_nginx.sh << 'EOF'
  #!/bin/bash
  nginx -c /myscripts/myconfig/nginx.conf -g "daemon off;"
  EOF
  
  chmod +x start_nginx.sh
  
  echo "===================================================================="
  echo "Nginx Reverse Proxy setup complete on Alicia!"
  echo "Domain: expeditioners.com"
  echo "Routes:"
  echo "  /profil_lune     -> Lune ($IP_LUNE)"
  echo "  /profil_sciel    -> Sciel ($IP_SCIEL)"
  echo "  /profil_gustave  -> Gustave ($IP_GUSTAVE)"
  echo "  others           -> Lune information page"
  echo ""
  echo "To start Nginx: cd /myscripts/myconfig && ./start_nginx.sh"
  echo "===================================================================="
  ```


<br>

## Soal 10

> Untuk mendistribusikan traffic halaman informasi, atur Reverse Proxy Alicia agar dapat membagi pekerjaan kepada web server Lune, Sciel, dan Gustave secara optimal menggunakan algoritma Round-robin. Pastikan target pembagian load merupakan halaman informasi, bukan halaman profil masing-masing web server.

> _To distribute traffic for the information page, configure the reverse proxy (Alicia) to use Round-robin load balancing between the three web servers: Lune, Sciel, and Gustave.
Ensure that only the information page is included in the load-balancing configuration - not the profile pages._

**Answer:**

- Screenshot

  ### curl -vL http://expeditioners.com
  
  <img width="672" height="989" alt="image" src="https://github.com/user-attachments/assets/b536da05-49a9-440a-8406-208f3f7fab68" />


- Explanation

  ### UBAH SCRIPT DI `Alicia` MENJADI SEPERTI BERIKUT
  ### Mengubah yang sebelumnya `Information` selalu di forward ke `Lune`, sekarang jadi rata kesemuanya.

  ```bash
  #!/bin/bash
  
  IP_LUNE="10.91.2.2"
  IP_SCIEL="10.91.2.3"
  IP_GUSTAVE="10.91.2.4"
  
  echo "Setting up Nginx Reverse Proxy for Alicia (expeditioners.com)..."
  
  mkdir -p /myscripts/myconfig
  cd /myscripts/myconfig
  
  # Create nginx.conf
  cat > nginx.conf << EOF
  user www-data;
  worker_processes auto;
  pid /tmp/nginx.pid;
  
  events {
      worker_connections 1024;
  }
  
  http {
      log_format custom '[$time_local] Expeditioners Proxy Access from $remote_addr requested "$request" -> upstream $upstream_addr with status $status';
      access_log /tmp/access.log custom;
      error_log /tmp/error.log;
      
      upstream info_servers {
          server $IP_LUNE:8000;
          server $IP_SCIEL:8100;
          server $IP_GUSTAVE:8200;
      }
  
      server {
          listen 80;
          server_name expeditioners.com www.expeditioners.com;
  
          # Profil routes
          location /profil_lune {
              proxy_pass http://$IP_LUNE:80/;
          }
  
          location /profil_sciel {
              proxy_pass http://$IP_SCIEL:80/;
          }
  
          location /profil_gustave {
              proxy_pass http://$IP_GUSTAVE:8888/;
          }
  
          location / {
              proxy_pass http://info_servers;
          }
      }
  }
  EOF
  
  # Start script for nginx
  cat > start_nginx.sh << 'EOF'
  #!/bin/bash
  nginx -c /myscripts/myconfig/nginx.conf -g "daemon off;"
  EOF
  
  chmod +x start_nginx.sh
  
  echo "===================================================================="
  echo "Nginx Reverse Proxy setup complete on Alicia!"
  echo "Domain: expeditioners.com"
  echo "Routes:"
  echo "  /profil_lune     -> Lune ($IP_LUNE)"
  echo "  /profil_sciel    -> Sciel ($IP_SCIEL)"
  echo "  /profil_gustave  -> Gustave ($IP_GUSTAVE)"
  echo "  others           -> Lune information page"
  echo ""
  echo "To start Nginx: cd /myscripts/myconfig && ./start_nginx.sh"
  echo "===================================================================="
  ```

<br>
  
## Problems

### IMAGE DOCKER GABISA NYIMPEN SCRIPT

## Revisions (if any)
