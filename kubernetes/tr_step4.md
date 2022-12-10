## Öğrenme Hedefleri

Bu eğitim senaryosunda kubernetes objelerinden biri olan servis objesine değinecek, aynı zamanda kubernetes platfoormunda bir servis objesi oluşturabilmeyi öğreneceksiniz. Aynı kubernetes network yapısına da değineceğiz.

## Ön gereksinimler

* Kubernetes platformunun çalışma presibini temel seviyede hakim olmalısınız.

## Senaryo 8

### Servis Objesi

Bu bölümde servis objesine değinecek ve bir servis objesinin nasıl oluşturulduğunu öğreneceğiz.

![](https://raw.githubusercontent.com/zeyneprumeysayorulmaz/bulut-bilisimciler-senaryo/main/kubernetes/img/services.JPG)

Servis objeleri, pod veya podlar üzerinde çalışan bir uygulamayı ağ hizmeti olarak göstermenin bir yoludur. 4 tipi vardır:

**ClusterIP:** Servislerin birbirleri arasında iletişim kurabilmesi için kullanılan servis tipidir. ClusterIP tipinde bir servis objesi yaratıp bunu label selectorler aracılığıyla podlarla ilişkilendirebiliriz. Bu obje yaratıldığı zaman cluster içerisindeki tüm podların ve kaynakların çözebileceği unique bir dns ismine sahip olur. Bunun yanında bizler her kubernetes kurulumunda -service-cluster-ip-range anahtarı ile bir sanal ip boğu belirleriz. Bir ClusterIP servis objesi yarattığımız zaman bu objeye bu ip bloğundan bir sanal ip adresi atanır ve ismiyle bu ip adresi clusterın dns mekanizmasında kaydedilir. Bu ip adresi virtual bir ip adrestir. kube-proxy tarafından tüm nodelardaki iptable’lara eklenir ve buraya ulaşan trafik servisin ilişkilendirildiği podlara yönlendirilir. Özetle; ClusterIP servisleri kubernetes altında servis discovery ve load balancing hizmetleri sunan obje tipidir.

**NodePort:** Bu servis tipi ile node üzerinde dışarıdan erişilebilir bir port erişime açılır. Bir NodePort servisi yaratıp bunu label selectorler aracılığıyla podlarla eşleştirebilirsiniz. Bu obje yaratıldığında 30.000–32.762 aralığında bir port seçilir ve tüm worker nodelarda bu port sizin servisinize proxylenir. Kısacası worker node’un ip adresinin bu portuna gelen tüm paketler ilişkilendirdiğiniz podların publish ettiğiniz portuna yönlendirilir.

**LoadBalancer:** Bu servis tipini sadece Azure Kubernetes Service ya da Google Kubernetes Engine gibi cloud servis sağlayıcıların managed kubernetes servislerinde kullanabiliriz. LoadBalancer tipinde servis objesi oluşturulduğunda cloud servis sağayıcı dış dünyadan erişilebilecek public bir ip adresine sahip bir loadbalancer oluşturur. Sonrasında bu loadbalancer’ı sizin servis objenizle ilişkilendirir. Dolayısıyla bu public ip adresine ulaşan paketler içeriden podlara yönlendirilir.

**ExternalName:** Bu servisler, selector ile değil bir dns adı ile eşleşirler. ExternalName tipinde yönlendirme işlemi bir proxying veya forwarding ile değil, DNS seviyesinde gerçekleşmektedir.

Servislerde diğer tüm kubernetes objeleri gibi yaml dosyaları ile oluşturulur. Örneğin aşağıdaki gibi ClusterIP tipinde bir servis objesi oluşturabiliriz.

`vi cluster-ip-service.yaml`

````
apiVersion: v1
kind: Service
metadata:
  name: myservice
spec:
  type: ClusterIP
  selector:
    app: myservice
  ports:
    - protocol: TCP
      port: 500
      targetPort: 500
````
kind ile yine obje tipimizi belirtiyor ve metadata kısmında objeye ait bilgileri giriyoruz. Buradaki name kısmı önemli, isim artık cluster içerisinden bu servise ulaşacağımız isim olacak. spec kısmında bu servisin ClusterIP tipinde bir servis olduğunu, selector kısmında ise app: backend anahtar veri eşleniğine sahip olan podlara trafiği yönlendirmesini söylüyoruz. Yani servis hangi podlara hizmet edeceğini selector ile seçmiş oluyor.

Ports kısmında ise; port: 5000 ile servisin dinleyeceği portu, targetPort: 5000 ile de podun expose ettiği portu belirtiyoruz.

``kubectl apply -f service.yaml`` komutu ile servis objesini apply edebilirsiniz.

``kubectl get services`` komutu ile de servislerinizi listeleyebilirsiniz. Bu çıktıda yer alan servise ait **Cluster-Ip** bilgisi yalnızca cluster içerisinde geçerlidir. Cluster içerisinde herhangi bir kaynak bu ip'nin ilgili portuna istek gönderdiğinde bu istek bu servisin selector ile seçtiği podlardan bir tanesine yönlendirilecektir.

Örneğin aşağıdaki gibi NodePort tipinde bir servis objesi oluşturalım.

``vi node-port-service.yaml``

````
apiVersion: v1
kind: Service
metadata:
  name: myservice2
spec:
  type: NodePort
  selector:
    app: myservice2
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
````
`kubectl apply -f node-port-service.yaml` nodePort tipindeki servis objesini apply edelim.

Bu servis içinde aslında ClusterIP tipi gibi cluster içinden bu servisin 80 portuna gidersek arkadaki podların 80 portuna servis yönlendirilecek ama bunun yanında ek bir işlem daha var. **Tüm worker nodeların dış bacağında ilgili portu dinlemeye başlıyor. Biz herhangi bir nodeun bu portuna ulaşırsak servis podlarından birine trafik yönlenecektir.**

Imperative yöntemle de servis objesi oluşturabiliriz.

Örneğin myapp isimli bir deployment objesi oluşturalım ve deployment objesi için bir servis tanımı yapalım.

``kubectl create deployment myapp --image=nginx:latest --replicas=3`` --> deployment objesini oluşturalım.

``kubectl expose deployment myapp --type=ClusterIP --name=myservice --port=80`` -> **kubectl expose** komutu ile expose etmek istediğimiz objeyi ve adını belirtiyor sonrasında servis tipini ve adını belirliyoruz.

Deployment konusunu ele alırken podları deploymentın değil ReplicaSet objesinin oluşturduğunu söylemiştik. **Deployment replicaSet oluşturur, replicaSet podları oluşturur. Benzer bir durum service objesi için de geçerlidir.**

Bir servis oluşturduğumuz zaman bu servis **endpoint** adında bir obje oluşturur. Bu endpoint objesi, bizim belirlediğimiz selector tarafından seçilen podların ip adreslerini üzerinde barındırır. Service trafiğini nereye yönlendireceğini bu listeye göre düzenler.

``kubectl get endpoints`` ile oluşturduğunuz bütün servisler ile aynı isimle bir endpoint objesi oluştuğunu görebilirsiniz.

