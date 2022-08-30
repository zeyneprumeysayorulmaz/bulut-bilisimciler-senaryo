## Docker Compose Nedir, Ne Amaçla Kullanılır?

![](https://miro.medium.com/max/1400/1*2G5KOQVzqVIbxxxeKECZkA.jpeg)

Docker Compose, çok containerlı karmaşık servislerin tanımlanmasını ve çalıştırılmasını sağlayan kullanımı oldukça basit, birden fazla container tanımlamasını tek bir dosyadan yapabildğiniz ve dilediğiniz zaman tek bir komut ile uygulamanızı tüm gereksinimleri ile çalıştırabildiğiniz bir docker aracıdır. 

Docker compose ile birden fazla container çalıştırabilirsiniz. Dilerseniz birbirine bağımlı comtainerlar oluşturabilirsiniz.

## Docker Compose Detayları

* Docker Compose yaml dosya formatındadır. 
* Projeniz için oluşturacağınız containerları tek bir konfigürasyon dosyası ile hızlıca servise dönüştürebilir ve çalıştırabilirsiniz.
* Oluşturulan servisler için tanımladığınız veri yığınları compose tarafından saklanabilir.
* Konfigürasyon dosyasında değişiklik yapmadığınız sürece docker-compose asla yeni bir container oluşturmaz.
* Containerlar arası konfigürasyon ilişkisi çok hızlı bir şekilde oluşturulabilir.

## Docker Compose kullanımı 3 adımdan oluşur
1- Özel Imagelar için dockerfile dosyasının tanımlanması

2- Oluşturacağınız servisler için docker-compose.yml tanımlanması

3- docker-compose yönetim komutunun çalıştırılması

Kısaca docker compose ile docker üzerindeki containerları, volumeleri, networkeleri, imageları tek bir dosya üzerinde tanımlayarak servisleri oluşturmakta ve akabinde oluşturulan bu dosyayı çalıştırıp tüm servisleri toplu halde veya tekil olarak ayağa kaldırmaktayız. İşimiz bittiğinde ise tüm servisleri toplu halde veya tekil olarak durdurabilmekteyiz.

