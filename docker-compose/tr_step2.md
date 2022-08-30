## Seviye: Başlangıç

## Bu senaryoda docker-compose dosya yapısını inceleyecek ve docker-compose yönetim komutlarını öğreneceksiniz.

docker-compose.yaml bir konfigürasyon dosyasıdır ve tüm servisler bu konfigürasyon dosyası içinde tanımlanır. DOsyayı çalıştırarak tüm servisleri ayağaa kaldırabilirsiniz. docker-compose aracı çoklu container yönetimi için docker-compose.yaml dosyasını kullanmakta ve bu sayede servis başlamadan önce çalışması gereken hizmetlerin oluşturulması, bu hizmetler arasındaki ilişkilerin oluşturulması ve çalışma zamanlarının düzenlenip servislerin ayağa kaldırılmasını sağlamaktadır. 

docker-compose.yaml dosyası en üstte version bilgisi ile başlar. docker-compose için kullanılan yaml sürüm bilgisi burada yazılmaktadır. version bilgisinden hemen sonra servis bilgisi yer almaktadır. Her ayrı servis için yeni bir servis satırı yazmalı ve gerekli bilgileri tanımlamalısınız. Servisler arası bağımlılık varsa bunları belirtmelisiniz. Servis tanımları yapıldıktan sonra varsa volumes ve networks bilgisini de girmelisiniz. 