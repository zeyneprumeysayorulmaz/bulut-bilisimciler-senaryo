## Seviye: Başlangıç

## Bu senaryoda docker-compose dosya yapısını inceleyecek ve docker-compose yönetim komutlarını öğreneceksiniz.

docker-compose.yaml bir konfigürasyon dosyasıdır ve tüm servisler bu konfigürasyon dosyası içinde tanımlanır. Dosyayı çalıştırarak tüm servisleri ayağa kaldırabilirsiniz. docker-compose aracı çoklu container yönetimi için docker-compose.yaml dosyasını kullanmakta ve bu sayede servis başlamadan önce çalışması gereken hizmetlerin oluşturulmasını, bu hizmetler arasındaki ilişkilerin oluşturulmasını ve çalışma zamanlarının düzenlenip servislerin ayağa kaldırılmasını sağlamaktadır. 

![](https://raw.githubusercontent.com/zeyneprumeysayorulmaz/bulut-bilisimciler-senaryo/main/img/compose_yaml.PNG)

docker-compose.yaml dosyası en üstte version bilgisi ile başlar. docker-compose için kullanılan yaml sürüm bilgisi burada yazılmaktadır. version bilgisinden hemen sonra servis bilgisi yer almaktadır. Her ayrı servis için yeni bir servis satırı yazmalı ve gerekli bilgileri tanımlamalısınız. Servisler arası bağımlılık varsa bunları belirtmelisiniz. Servis tanımları yapıldıktan sonra varsa volumes ve networks bilgisini de girmelisiniz. 

## docker-compose.yaml dosyası içerisindeki popüler standart komutlara aşağıdan ulaşabilir ve detaylarına bakabilirsiniz.

[Bu adresten](https://docs.docker.com/compose/compose-file/) tüm docker-compose versiyonları ve komutlarını detaylı inceleyebilirsiniz.

![](https://raw.githubusercontent.com/zeyneprumeysayorulmaz/bulut-bilisimciler-senaryo/main/docker-compose/img/command.PNG)

