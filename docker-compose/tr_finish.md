## Seviye: Başlangıç

## Bu senaryoda docker compose dosyasının nasıl hazırlandığını öğrenecek ve wordpress, phpMyAdmin, MySQL servislerinin çalıştığı bir uygulama ayağa kaldıracaksınız.

![](https://raw.githubusercontent.com/zeyneprumeysayorulmaz/bulut-bilisimciler-senaryo/main/img/app.jpg)

Wordpress açık kaynak bir CMS aracıdır. Burada wordpress kurulumu yapacağız. phpMyadmin web tabanlı bir veritabanı yönetim platformudur. Mysql ise açık kaynak bir ilişkisel veritabanı yönetim sistemdir. phpMyadmin aracı ile internet üzerinden mysql veritabanını yönetebiliyor olacağız. Bu 3 servisi aynı anda çalıştıran bir docker-compose konfigürasyon dosyası oluşturacağız.

Aşağıdaki adresten docker-compose.yml dosyasını localimize alalım. wget, git vb. bir araç kullanabilirsiniz veya dosyayı manuel oluşturabilirisiniz.

`wget -O docker-compose.yaml https://github.com/zeyneprumeysayorulmaz/bulut-bilisimciler-senaryo/blob/f8ec6eb9c0023709fe64f0e90a61fd322e54b546/docker-compose.yml`

`cd /docker-compose` ile ilgili dizin altına girelim.

`ls` komutunu kullanarak dizin altındaki dosyaları kontrol edelim.

`cat docker-compose.yml` komutu ile dosyayı açabilir ve her servisin nasıl oluşturulduğunu inceleyebilirsiniz.

`docker-compose -f docker-compose.yml" up -d --build` komutunu kullanarak docker compose dosyasını çalıştırabilir ve servisleri başlatabilirsiniz.
docker-compose up komutu compose ile console bağlanmış(attached) şekilde çalıştırır. Arka planda çalıştırmak için detached mode (-d) ile çalıştırılmalıdır.

`docker-compose ps` ile çalışan containerları gözlemleyelim.

Container loglarına gidelim. Uygulama loglarını görmek her zaman bir ihtiyaçtır. Eğer bir uygulama çalıştırıyor isek loglarını kontrol etmeyi isteyebiliriz;`docker-compose logs` ile logları inceleyebilirsiniz.

Eğer containerı silmek istiyorsanız `docker-compose down` komutunu kullanabilirsiniz.

Containerları silerken volume'ler ile birlikte silmek, data'ları yok etmek istiyorsanız `docker-compose down –volumes` komutunu kullanabilirsiniz.

### Bu komutlar docker-compose dosyanızın var olduğu dizinde çalıştırılmalıdır. Aksi halde docker engine komutları çalıştıramaz. 

`Interaktif session üzerinde bulunan port kısmına 7080 yazıp phpmyadmin arayüzüne bağlanabilirsiniz.`

`kullanıcı adı: root`
`password: pass123 (docker-compose.yml dosyasında belirlediğimiz parola)`

`Interaktif session üzerinde bulunan port kısmına 8080 yazıp wordpress sayfasına bağlanabilirsiniz. Tabii wordpress olduğundan çok zamanınızı almayacak bir kayıt adımı bulunmakta, hızlıca kayıt olup wordpress uygulamasına erişebilirsiniz.`