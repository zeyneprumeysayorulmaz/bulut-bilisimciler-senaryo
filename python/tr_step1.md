## Seviye: Başlangıç

## Bu senaryoda python programlama dilinde değişkenlerin kullanım şeklini öğreneceksiniz.

Değişkenler, herhangi bir veri içeren ifadelerdir ve veri değerlerini depolamak için kullanılırlar. Diğer programlama dillerinden farklı olarak pythonda değişken önden tanımlanmaz. Değişken yazıldığı zaman ilk değer eklenir. Python'da değer atadığınız herhangi bir değişken değer atamanızla oluşturulur. 

## Python Değişkenleri için Kurallar
- Değişken adı bir harf veya alt çizgi karakteri ile başlamalıdır
- Değişken adı bir sayı ile başlayamaz
- Değişken adı yalnızca alfasayısal karakterler ve alt çizgiler içerebilir (A-z, 0-9 ve _ )
- Değişken adları büyük/küçük harfe duyarlıdır (yaş, Yaş ve YAŞ üç farklı değişkendir)

Bir örnek ile daha iyi anlamaya çalışalım. Python komut satırında sırasıyla önce `x=5` ile bir x değişkeni oluşturalım ve bu değişkene 5 değerini atayalım. Daha sonra `print(x)` ile print fonksiyonunun bu değişkeni ekrana yazdırmasını isteyelim.

````
x=5
print(x)
````
````
Output: 5
````

Bir y değişkeni oluşturalım ve bu kez bir string değer atayalım.

`y="bulut"`

Daha sonra print fonskiyonu ile bu iki değişkeni (x ve y) ekrana yazdıralım.

````
print(x)
print(y)
````

````
Output: 
5
bulut
````

Python dilinde yorumlar diyez (#) ile başlar ve python satırın geri kalanını yorum olarak oluşturur.

````
# Bu bir yorumdur.
print(x)
````

````
Output: 5
````

veya yorumu bir satırın devamına yerleştirebilirsiniz, python # işaretinden sonrasını yorum olarak algılayacaktır.

````
print("Bulut Bilisimciler")  # Bu bir yorumdur.
````

````
Output: Bulut Bilisimciler
````

Değişkenlerin belirli bir türle (integer, string, float vs.) tanımlanmasına gerek yoktur. Bir değişkene değer atadıktan sonra da farklı bir türde veya aynı türde değer atayarak değiştirebilirsiniz. (Integer bir değer atarken çift tırnak kullanmamaya dikkat etmeli, aynı şekilde string bir değer atarken çift tırnak kullanmaya dikkat etmelisiniz.)

````
x=5
x="Bulut Bilisimciler"
print(x)
````

````
Output: Bulut Bilisimciler
````
String değişkenler tek tırnak veya çift tırnak içinde belirtilebilir. y değişkenine aşağıdaki gibi değişken atamanız aynıdır.

````
y="Bulut Bilisimciler"
print(y)
y='Bulut Bilisimciler'
print(y)
````
````
Output:
Bulut Bilisimciler
Bulut Bilisimciler
````

Değişken isimleri case-sensitive yani büyük küçük harf duyarlıdır. a ve A değişkenleri iki farklı değişkendir.

````
a=5
A="A degiskeni"
print(a)
print(A)
````
````
Output:
5
A degiskeni
````

Değişkene birden fazla kelime kullanarak bir isim vermek istiyorsanız, değişkeni daha okunaklı kılabilmek adına kullanabileceğiniz bir kaç teknik mevcuttur.

- Camel Case

İlki hariç her kelime büyük harf ile başlar.
````
camelVariableName="Camel Case"
print(camelVariableName)
````
````
Output:
camelVariableName
````

- Pascal Case

Her kelime büyük harfle başlar.

````
PascalVariableName="Pascal Case"
print(PascalVariableName)
````
````
Output:
PascalVariableName
````
- Snake Case

Her kelime bir alt çizgi karakteri ile ayrılır.

````
snake_variable_name="Snake Case"
print(snake_variable_name)
````
````
Output:
Snake Case
`````

Bir satırda birden fazla değişkene değer atamanız mümkündür.

````
x, y, z = "Bulut", "Bilisimciler", "Platformu"
print(x)
print(y)
print(z)
````
````
Output:
Bulut
Bilisimciler
Platformu
````

Aynı zamanda bir değeri birden fazla değişkene bir satırda atayabilirsiniz.

````
x = y = z = "Bulut Bilisimciler"
print(x)
print(y)
print(z)
````
````
Output:
Bulut Bilisimciler
Bulut Bilisimciler
Bulut Bilisimciler
````
Pythonda 4 farklı veri tipi koleksiyonu vardır. Bu koleksiyonların her birinin kendine özel özellikleri vardır. Liste, bir veri tipi koleksiyonudur, sıralı ve değiştirilebilirdir. Eğer herhangi bir veri tipi koleksiyonunuz (örneğin liste: language = ["go", "python", "java"]) varsa python, değerleri değişkenlere çıkarmanıza izin verir. Buna koleksiyon açma (unpack a collection) denir.

````
language = ["go", "python", "java"]
x, y, z = language

print(x)
print(y)
print(z)
````
````
Output:
go
python
java
````

print() fonksiyonu ile birden fazla değişkeni ekrana yazdırabilirsiniz. Bunun için virgül (,) veya (+) kullanabilirsiniz. Bulut ve Bilisimciler kelimelerinden sonra boşluk karekteri bırakıldığına dikkat edin, bunlar olmadan sonuç `BulutBilisimcilerPlatformu` olacaktır.
````
x="Bulut "
y="Bilisimciler "
z="Platformu"
print(x, y, z)
print(x + y + z)
````
````
Output:
Bulut  Bilisimciler  Platformu
Bulut Bilisimciler Platformu
````
Dikkat etmeniz gereken sayılar için artı(+) karakterinin matematiksel bir fonksiyon olarak çalıştığıdır. 

````
x=5
y=10
z=15
print(x, y, z)
print(x + y + z)
````
````
Output:
5 10 15
30
````

Bir diğer dikkat etmeniz gereken nokta artı(+) operatörüyle bir string ve sayı değişkenini ekrana yazdırmaya çalıştığınızda hata alacağınızdır.

````
x=5
y="Bulut "
print(x + y)
````
````
Output:
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for +: 'int' and 'str'
````
İki farklı değişken tipiniz varsa virgül(,) kullanarak bunu yapmalısınız. `print(x, y)` komutu ile ile çıktının `5 bulut``olduğunu görebilirsiniz.

