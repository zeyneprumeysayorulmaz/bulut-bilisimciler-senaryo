## Seviye: Başlangıç

## Bu senaryoda string veri türü ile çalışacaksınız.

- Python'daki stringler, tek tırnak işaretleri veya çift tırnak işaretleri ile çevrilidir.

- print() fonksiyonuyla bir string değişmezi görüntüleyebilirsiniz.
- Bir değişkene string değer atama, değişken adının ardından eşittir(=) işareti ve string ile yapılır:

`x="Bulut Bilisimciler` ile x değişkenine "Bulut Bilisimciler" string değerini atayalım.

`print(x)` fonksiyonuyla değişken adını vererek değişken değeri olan string ifadeyi ekrana yazdırabilirsiniz.

- Üç tırnak kullanarak bir değişkene çok satırlı bir string değer atayabilirsiniz.

````
x = """Lorem ipsum dolor sit amet,
consectetur adipiscing elit,
sed do eiusmod tempor incididunt
ut labore et dolore magna aliqua."""
print(x)

````
````
Output:
Lorem ipsum dolor sit amet,
consectetur adipiscing elit,
sed do eiusmod tempor incididunt
ut labore et dolore magna aliqua.
`````

- Diğer birçok popüler programlama dili gibi, Python'daki dizeler (arrays) de unicode karakterleri temsil eden bayt dizileridir.
Ancak Python'un bir karakter veri türü yoktur, tek bir karakter yalnızca 1 uzunluğunda bir dizedir.
Dize öğelerine erişmek için köşeli parantezler kullanılabilir.

````
x = "Hello, World!"
print(x[1])
````
````
Output:
e
````
- Stringler dizi(array) olduğundan, bir stringdeki karakterler arasında for döngüsü ile döngü yapabiliriz.

`for x in "deneme": print(x)` ile string döngüyü ekrana bastırabiliriz.

````
Output:
d
e
n
e
m
e
````
- Bir string değişkenin uzunluğunu almak için len() fonksiyonunu kullanabilirsiniz. Boşluk karakterini de saydığına dikkat edin.

````
x = "Bulut Bilisimciler"
print(len(x))
````
````
Output:
18
````

- Bir stringde belirli bir kelime öbeği veya karakterin olup olmadığını kontrol etmek için "in " veya "not in" sözcüğünü kullanabilirsiniz.

````
txt = "Lorem ipsum dolor sit amet"
print("dolor" in txt)
print("consectetur" in txt)
print("dolor" not in txt)
````
````
Output:
True
False
False
````
- upper() metodu ile string değeri büyük harfle döndürebilirsiniz.
````
x = "Bulut Bilisimciler!"
print(x.upper())
````
````
Output:
BULUT BILISIMCILER!
````
- lower() metodu ile string değeri küçük harfle döndürebilirsiniz.
````
x = "BULUT BILISIMCILER!"
print(x.lower())
````
````
Output:
bulut bilisimciler!
````
- strip() metodu, başlangıçtaki veya sondaki tüm boşlukları kaldırmanızı sağlar. İki kelime arasındaki boşluk karakterini kaldırmadığına dikkat edin.
````
x = " Bulut Bilisimciler! "
print(x.strip())
````
````
Output:
Bulut Bilisimciler!
````
- replace() metodu, bir string değeri başka bir string değer ile değiştirir. Python dilinin case-sensitive olduğunu unutmamalısınız.
````
x = " Bulut Bilisimciler! "
print(x.replace("B", "A"))

````
````
Output:
Aulut Ailisimciler! 
````
- String ifade ile integer bir değeri artı(+) ile birleştiremediğimizi biliyoruz. Ancak, format() metodunu kullanarak string ve sayıları birleştirebiliriz!

format() metodu iletilen bağımsız değişkenleri alır, biçimlendirir ve {} yer tutucularının bulunduğu string değişkene yerleştirir.

````
yil = 100
txt = "Büyük zaferin {}.yili"
print(txt.format(yil))
````
````
Output:
Büyük zaferin 100.yili
````