## Seviye: Başlangıç

## Bu senaryoda Python global ve yerel değişken kavramlarını öğreneceksiniz.

Bir fonksiyonun dışında tanımlanan değişkenler, global değişkenlerdir. Global değişkenler, hem fonksiyonların içinde hemde dışında herkes tarafından kullanılabilir.

Bir fonksiyonun dışında değişken oluşturalım ve bu eğişkeni fonksiyonun içinde kullanalım. Bunun için python komut satırını kullanmadan bir .py dosyası oluşturalım ve bu dosyayı çalıştıralım.

`vi variables.py` komutu ile variables isimli yeni bir .py dosyası oluşturalım. Örneklerimizi bu dosya içinde yapacağımız değişiklikler ile `python3 variables.py` komutunu kullanarak çalıştırabiliriz.

````
x = "Platformu"

def myfunc():
  print("Bulut Bilisimciler " + x)

myfunc()
````

````
Output:
Bulut Bilisimciler Platformu
````

Bir fonksiyon içinde aynı isimde bir değişken oluşturursanız, bu değişken yerel olur ve sadece fonksiyon içinde kullanılabilir. Aynı ada sahip global değişken, olduğu gibi, global ve orijinal değerde kalacaktır.

````
x = "global degiskendir"

def myfunc():
  x = "yerel degiskendir"
  print("Bu bir " + x)

myfunc()

print("Bu bir " + x)
````
````
Output:
Bu bir yerel değişkendir
Bu bir global değişkendir
````

Bir fonksiyon içinde de global değişken oluşturabilirsiniz. Bunun için global anahtar kelimesini kullanmalısınız.
````
def myfunc():
  global x
  x = "global degiskendir"

myfunc()

print("Bu bir " + x)
````
````
Output:
Bu bir global degiskendir.
````
Bir fonksiyon içindeki global değişkenin değerini değiştirmek için global anahtar sözcüğünü kullanabiliriz.
````
x = "global degiskendir"

def myfunc():
  global x
  x = "global degisken degeri degistirildi"

myfunc()

print(x)
````
````
Output:
global degisken degeri degistirildi
````