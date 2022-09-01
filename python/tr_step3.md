## Seviye: Başlangıç

## Bu senaryoda python veri türlerini öğreniyor olacaksınız.

Programlamada veri tipi önemli bir kavramdır.

Değişkenler farklı türlerdeki verileri depolayabilir ve farklı türler farklı şeyler yapabilir.

Python, bu kategorilerde varsayılan olarak aşağıdaki veri türlerine sahiptir:

````
Text Type:	    str
Numeric Types:	int, float, complex
Sequence Types:	list, tuple, range
Mapping Type:	dict
Set Types:	    set, frozenset
Boolean Type:	bool
Binary Types:	bytes, bytearray, memoryview
None Type:	    NoneType
````

type() fonksiyonu ile bir değişkenin veri türünü elde edebilirsiniz. Python'da bir değişkene değer aatdığınızda veri türü ayarlanır.

````
x = 5
y = "Bulut Bilisimciler"
print(type(x))
print(type(y))
````
````
Output: 
<class 'int'>
<class 'str'>
````

Değişkenin veri türünü belirtmek isterseniz aşağıdaki constructor(yapıcı) fonksiyonlarını kullanabilirsiniz.
 
| Örnek                                        | Veri Türü| 
| :------------------------------------------: | :-------:| 
| x = str("Hello World")	                   | str	  | 
| x = int(20)	                               | int	  | 
| x = int(20)	                               | int	  | 
| x = int(20)	                               | int	  | 
| x = float(20.5)	                           | float	  | 
| x = complex(1j)	                           | complex  | 	
| x = list(("apple", "banana", "cherry"))      | list	  | 
| x = tuple(("apple", "banana", "cherry"))     | tuple	  | 
| x = range(6)	                               | range	  | 
| x = dict(name="John", age=36)	               | dict	  | 
| x = set(("apple", "banana", "cherry"))       | set	  | 
| x = frozenset(("apple", "banana", "cherry")) | frozenset| 	
| x = bool(5)	                               | bool	  | 
| x = bytes(5)	                               | bytes	  | 
| x = bytearray(5)	                           | bytearray| 	
| x = memoryview(bytes(5))	                   | memoryview| 
|                                              |           |

- Int veya integer, pozitif veya negatif, ondalıksız, sınırsız uzunlukta bir tam sayıdır.
- Float veya "Floating point number", bir veya daha fazla ondalık basamak içeren pozitif veya negatif bir sayıdır.
- Float, 10'un kuvvetini belirtmek için "e" harfiyle gösterilen bilimsel sayılar da kullanılabilir.
Örneğin;

````
x = -67.6e100

print(type(x))
````
````
Output:
<class 'float'>
````

Bir değişkenin veri tipini belirtmek isterseniz bunu casting ile yapabilirsiniz. Python, nesne yönelimli bir dildir ve bu nedenle, ilkel türleri de dahil olmak üzere veri türlerini tanımlamak için sınıfları kullanır.

Python'da casting bu nedenle yapıcı(constructor) fonksiyonlar kullanılarak yapılır:

````
x = str(3)    # x will be '3'
y = int(3)    # y will be 3
z = float(3)  # z will be 3.0

print(x)
print(y)
print(z)
````
````
Output:
3
3
3.0
````
