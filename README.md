# Centos 7 üzerine MariaDb 10.5 kurulumu

## Kullanılan Araç Ve Teknolojiler:
- VM --> Aws Ec2
- Centos7
- MariaDb 10.5
- MobaXterm
-----------
### Kurulum:

Öncelikle Aws Ec2 üzerinde instance oluşturarak başlıyoruz.Aws üzerinde üyelik oluşturduktan sonra servisler üzerinden Ec2'Ye ulaşıyoruz sonrasında örnek bir instance kurulumu için aşağıdaki videoya bakabiliriz:


https://user-images.githubusercontent.com/64022432/200653381-87315e4c-7e08-4cf1-87d2-d8875da014c0.mp4

------------
- Daha sonrasında oluşturulan Instance'ın Public ip'si ve videoda da gözüktüğü gibi oluşturduğumuz ssh key'in dosya konumunu belirterek MobaXterm üzerinden makinemize ulaşıyoruz.Benim için bu komut:
>ssh -i C:/aws/firstkey.pem centos@3.84.179.75
-----------------------------
### MariaDb kurulumumuza geçebiliriz:

- İlk olarak Centos'u güncelleyelim:
> sudo yum update

- MariaDB tarafından sağlanan depoyu CentOS sunucunuza eklemek için aşağıdaki komutları çalıştırıyoruz:
> curl -LsS -O https://downloads.mariadb.com/MariaDB/mariadb_repo_setup
> sudo bash mariadb_repo_setup --mariadb-server-version=10.5

- Önbelleği güncelleyerek Repository'nin çalıştığını onaylıyoruz:
> sudo yum makecache

- Kullanılabilir repositoryleri listeleyelim:
> sudo yum repolist
> ![sudo yum repolist](https://user-images.githubusercontent.com/64022432/200656148-6fc87c79-5305-4ee0-b49b-e6a6e583e62c.jpg)

### Kuruluma devam edelim:

> sudo yum install MariaDB-server MariaDB-client MariaDB-backup

- Kurulacak paketlerin listesini kontrol edebilir ve uygunsa kabul edebilirsiniz.

### RPM paketi ayrıntıları:

> rpm -qi  MariaDB-server 
> ![rpm -qi  MariaDB-server](https://user-images.githubusercontent.com/64022432/200657251-e85aa1b6-2c0f-4b7e-a03b-0ca6b2328143.jpg)

### CentOS 7, Systemd init sistemini kullanır. Aşağıdaki gibi systemctl komutu ile servisi başlatabiliriz:

> sudo systemctl start mariadb

- Sunucu yeniden başlatıldığında hizmetin başlatılmasını sağlamak için aşağıdaki komutu kullanıyoruz:

> sudo systemctl enable mariadb

### systemctl status komutuyla hizmet durumunu kontrol ediyoruz:
> systemctl status mariadb
> ![systemctl status mariadb](https://user-images.githubusercontent.com/64022432/200657927-01323d3d-f24d-4a47-b103-661f2f96df2f.jpg)


- MariaDB’nin güvenliğini sağlamlaştırmak için aşağıdaki
komutun verilmesi gereklidir. Bu betik programı, çeşitli
sorular soracak ve bazı yerlerde şifre girmenizi
isteyecektir:

> sudo mariadb-secure-installation
> ![sudo mariadb-secure-installation](https://user-images.githubusercontent.com/64022432/200658664-5ea31412-3500-43ae-9d15-dc3d76e4caba.jpg)

- MariaDB’nin konsoluna bağlanarak hangi
versiyonun çalıştığı görülebilir:

> mariadb -u root -p
> ![mysql -u root -p](https://user-images.githubusercontent.com/64022432/200658976-15b3d2d5-6c34-436b-897a-f8d6700d26fe.jpg)

- Veritabanı sunucunuz artık kullanıma hazır.


## Örnek bir veritabanı,tablo ve kullanıcı oluşturma ve bu kullanıcıya belirli bir ayrıcalık ekleme:

- Veritabanı Oluşturma:

> CREATE DATABASE <Your_db>;
> ![create databases](https://user-images.githubusercontent.com/64022432/200659759-09ac315e-00d2-4735-bf81-b527b55602b5.jpg

- Tablo Oluşturma:

> CREATE TABLE interviews (
> id int not null auto_increment,
> name varchar(255),
> primary key (id)
> );


> ![create table interviews](https://user-images.githubusercontent.com/64022432/200660463-fb1b507d-e432-401e-be0c-18a8aca1573c.jpg)
---------------------
- Belirli ayrıcalığa sahip kullanıcı oluşturma:


> ![user2 create](https://user-images.githubusercontent.com/64022432/200660853-c9be46af-cddc-4247-ae8e-15a60d95d7e9.jpg)

## Burada ben ayrıcalık olarak SELECT verdim SELECT ve diğer ayrıcalıklar için açıklamalar

- CREATE – Kullanıcıların veritabanı ve tablo oluşturma izni verir
- SELECT – Kullanıcıların veriyi okumasına izin verir
- INSERT – Kullanıcıların tablolara yeni veri eklemesine izin verir
- UPDATE – Kullanıcıların tablolarda bulunan mevcut verileri güncellemesine izin verir
- DELETE – Kullanıcıların tablo verilerini silmesine izin verir
- DROP – Kullanıcıların tüm veritabanını veya tabloları silmesine izin verir

## Quit komutu ile çıkış yaptıktan sonra yeni oluşturduğumuz user ile giriş yapalım ve yapmış olduğumuz değşiklikleri inceleyelim:


> ![describe user2](https://user-images.githubusercontent.com/64022432/200662615-7a7a152c-bd41-4ff7-bd0e-f9c54004af73.jpg)


> ![grant show for user2](https://user-images.githubusercontent.com/64022432/200662036-7af91f8d-9212-4198-81ca-2dc7b725bd40.jpg)

