# Python Giriş

![](https://raw.githubusercontent.com/zeyneprumeysayorulmaz/bulut-bilisimciler-senaryo/main/python/img/python-logo.png)

## Python Nedir?

Python popüler bir programlama dilidir. Guido van Rossum tarafından yaratılmış ve 1991 yılında piyasaya sürülmüştür.

## Ne için kullanılır?

- web development (server-side),
- software development,
- mathematics,
- system scripting


## Diğer Programlama Dillerine Göre Python Syntax'ı
- Diğer programlama dillerine kıyasla daha okunabilir ve anlaşılması kolaydır. 

- Python, genellikle noktalı virgül veya parantez kullanan diğer programlama dillerinin aksine, bir komutu tamamlamak için yeni satırlar kullanır.

- Python, kapsamı tanımlamak için boşluk kullanarak girintiye güvenir; döngüler, fonksiyonlar ve sınıfların kapsamı gibi. Diğer programlama dilleri genellikle bu amaç için kaşlı ayraçlar kullanır.

- Python'daki girinti çok önemlidir. Diğer programlama dillerinde koddaki girinti yalnızca okunabilirlik içindir, Python, bir kod bloğunu belirtmek için girinti kullanır.

Öncelikle sistemde python olup olmadığını öğrenmek için
`python3 --version` komutunu çalıştıralım. Çıktıda versiyon bilgisini elde ediyorsak devam edebiliriz.


Python shell'ine erişmek için `python3` komutunu çalıştıralım. Bu shell ekranı kodlarımızı yorumlayan sayfa olacak, buraya yazdığınız kodların cevaplarını alabileceksiniz.

Programlamanın bir geleneğidir, öncellikle "Hello World" metnini ekrana yazdıralım.

`print("Hello World")`

Ekrana veya herhangi bir standart output cihazına çıktı yazdırmak istiyorsanız print fonksiyonunu kullanmalısınız. Ekrana yazdıracağınız mesaj bir string veya nesne olabilir.

Birden fazla nesneyi yazdırmak istiyorsak;

`print("Hello", "how are you?")`

Python interpreted bir programlama dilidir. Bu bir geliştirici olarak Python (.py) dosyalarını bir metin düzenleyiciye yazmanız ve ardından bu dosyaları yürütülmek üzere python yorumlayıcısına koymanız anlamına gelir. Komut satırında bir python dosyasını çalıştırmanın yolu `python3 dosya_adi.py` şeklinde olacaktır.

Bunu bir örnekle daha iyi anlayabiliriz.

`vi deneme.py` komutu ile yeni bir dosya açalım ve içinde yukarıda yaptığımız gibi bir print fonksiyonu yerleştirelim.

`print("Bu bir denemedir.")` satırını ekledikten sonra dosyayı kaydederek çıkabiliriz.

Daha sonra .py uzantılı dosyayı `python3 deneme.py` komutu ile çalıştıralım. Ekranda print fonksiyonu içerisinde verdiğimiz metni göreceksiniz. 

Python komut satırında işiniz bittiğinde `exit()` yazmanız yeterlidir.