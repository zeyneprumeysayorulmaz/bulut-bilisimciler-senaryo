## Seviye: Başlangıç

## Bu senaryoda Python escape karakterlerini ve string methodlarını öğreneceksiniz.

Bir stringde geçersiz karakterler eklemek için bir escape karakteri kullanmalısınız.

Bir escape karakteri, bir ters eğik çizgidir \ ve ardından eklemek istediğiniz karakterdir.

| Code	| Result	    | 
| :-----:| :--------------:| 
| \ '	| Single Quote	| 
| \ \	| Backslash	    | 
| \n	| New Line	    | 
| \r	| Carriage Return| 	
| \t	| Tab	        | 
| \b	| Backspace	    | 
| \f	| Form Feed	    | 
| \ooo	| Octal value	| 
| \xhh	| Hex value     | 

````
txt = 'Turkiye\'nin Baskenti.'
print(txt) 
````
````
Output:
Turkiye'nin Baskenti.
````
- String üzerinde çalışacak birçok fonksiyon vardır. Elbetteki hepsini hatırlamak ve ezberlemek pek mümkün değil, ihtiyacınız dahilinde fonksiyonları inceleyebilirsiniz.
 [Bu adresten](https://www.digitalocean.com/community/tutorials/python-string-functions) fonksiyonlara göz atabilirsiniz.