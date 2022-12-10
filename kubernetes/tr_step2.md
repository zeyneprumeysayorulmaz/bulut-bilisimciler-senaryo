## Label ve Selector

Etiketler/tagler kubernetes’de her türlü objeye atayabildiğimiz anahtar değer eşleniklerdir. Oluşturduğumuz objeye bizlerin daha iyi anlamlandırabileceği gruplama yaparken kullanabileceğimiz bilgiler eklemiş oluyoruz. Obje oluştururken veya oluşturulduktan sonra etiket tanımı yapabiliriz.

Kubernetes’de objeler arası bağlantı, etiketler aracılığıyla gerçekleştirilir. Örneğin service ve deployment objeleri hangi podların onlara ait olduğunu ve hangi podlarla ilişki kuracaklarını etiketler sayesinde belirlerler.

Label tanımları **metadata** içerisinde yapılır ve objelere istendiği kadar label atanabilir, burada bir kısıt yoktur. Tek bir kural var, **bir poda aynı anahtarı iki defa atayamayız**. Yani aynı anahtarla bir label daha ekleyemeyiz.

## Öğrenme Hedefleri

Bu eğitim senaryosunda label, selector ve annotation kavramlarını öğrenecek aynı zamanda namespace objesini öğrenecek ve bu obje ile işlemler yapacağız.

## Ön gereksinimler

* Kubernetes platformunun çalışma presibini temel seviyede hakim olmalısınız.

## Senaryo 2

Bu bölümde geçtiğimiz senaryoda öğrendiğimiz pod objesi üzerinden **label, selector** ve **annotations** pratikleri yapacağız.

![](https://raw.githubusercontent.com/zeyneprumeysayorulmaz/bulut-bilisimciler-senaryo/main/kubernetes/img/labelselector.JPG)

Aşağıdaki yaml dosyasını oluşturarak bir pod objesi oluşturalım ve prtaiklere başlayalım. Pod objesini tanımlarken label anahtarını kulanarak label ekleyebiliriz.

`vi pod2.yaml`

````
apiVersion: v1
kind: Pod
metadata:
  name: mypod
  labels:
    app: firstapp
    tier: frontend
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
````

`kubectl apply -f pod2.yaml` komutunu kullarak pod objesini oluşturabiliriz.

Labellar ile gruplandırma ve tanımlama yapabiliyoruz demiştik. Burada biz labelları kullanarak ortak özelliklere sahip objeleri listeleyebiliriz. Örneğin `kubectl get pods -l "app" --show-labels` komutu ile app isimli anahtarın atanmış olduğu podları listeleyebiliriz. -l parametresi ile labela göre ayrıştırmasını ve -show-labels ile de objeye atanmış labelları göstermesini isteyebiliriz

``kubectl get pods -l "app=firstapp" --show-labels`` app anahtarına firstapp atanmış podları listelemesini söyleyebiliriz. Bu **equality based- eşitlik temelli selector** kullanımıdır.

``kubectl get pods -l 'app in (firstapp)'`` ile set based olarak aynı sorguyu atabilir ve firstapp labelına sahip podları listeleyebiliriz. Bu **set based- atama temelli selector** kullanımıdır.

‘mypod’ isimli pod objesine aşağıdaki gibi imperative yöntemle bir label ekleyebiliriz.

`kubectl label pods mypod rel=beta`

``kubectl label pods mypod rel-`` komutun sonuna - ekleyerek bu anahtarı ve değerini pod objesinden kaldırabiliriz.

``kubectl label --overwrite pods mypod app=secondapp`` ile belirtilen anahtarda labelı güncelleyebiliriz.

``kubectl label pods --all app=lastapp`` ile bir namespace içindeki tüm podlara label atayabiliriz.

### Objeler arasında labellar üzerinden ilişki kurmak

Normal şartlarda kubeScheduler kendi algoritmasına göre bir node seçimi yapar. Daha önceki yazımda bahsetmiştim. Ama bizler nodeSelector ile bu podun çalışmasını istediğimiz node’u belirtebiliyoruz. Bunu da labellar aracılığı ile yapıyoruz.

````
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
    nodeSelector:
      hddtype: ssd
````
Yukarıdaki pod tanımında **nodeSelector** belirtilmiş ve **hddtype: ssd** olarak belirtilmiş. Burada kubeSchedular’a bu podu schedule ederken **hddtype: ssd** atanmış bir node bul ve bu podu orada çalıştır şekliinde bir direktif veriyoruz. Bu sayede pod ve node objesini labellar vasıtasıyla eşleştirmiş oluyoruz.

## Senaryo 3

Bu senaryoda annotation kavramına değinecek ve bir pod objesinde annotation tanımlamayı öğreneceksiniz.

## Annotation
Tıpkı label gibi anahtar veri eşlenikleri şeklinde metadata ekleyebileceğimiz bir opsiyondur. Fakat labelları her zaman kullanmayız, objeler arası kurduğu ilişkiden dolayı hassastır. Label eklemek veya çıkarmak kubernetes içerisinde bir şeyleri etkileyebilir veya tetikleyebilir. Dolayısıyla her türlü bilgiyi label olarak eklemek doğru değildir.

Objeye onu tanımlayan fakat label olarak eklememizin sakıncalı olacağı bilgileri annotation ‘açıklama’ olarak ekleriz. Yine bir kaç yazım kuralına tabidir.

Örneğin bir objeye oluşturulma tarihi ve kimin oluşturduğu bilgisini eklemek istersem bunu label ekleyerek yapmak doğru olmaz. Çünkü bu bilgiyi herhangi bir selector kullanarak seçme isteğim yok veya herhangi bir ilişki oluşturmak için bu bilgiyi kullanmayacağım. Dolayısıyla annotation olarak eklemek daha doğru olacaktır.

Bunun dışında annotationlar, kubernetes’in ana komponenti olmayan fakat kubernetesle bağlantısı olan yazılımlar tarafından ihtiyaç duyulacak bilgilerin yazıldığı yerdir.

Aşağıdaki gibi bir yaml dosyası ile pod objesini oluşturalım.

`vi pod_annotation.yaml`

````
apiVersion: v1
kind: Pod
metadata:
  name: lordvader
  annotations:
    owner: "Anakin Skywalker"
    notification-email: "lord_vader@starwars.com"
    releasedate: "22.06.2022"
    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
````
Yukarıdaki obje tanımında annotatiton için 4 anahtar eklenmiştir. Tıpkı label gibi metadata içinde tanımlanmıştır.

`kubectl apply -f pod_annotation.yaml` komutu ile yaml dosyasını apply edebilirsiniz.

Bu pod objesine ait annotation tanımlarını görmek isterseniz describe komutunun çıktısını inceleyebilirsiniz.

`kubectl describe pod lordvader`

Imperative yöntem ile de annotation ile işlem yapabilmek mümkün. 

`kubectl annotate pods lordvader message=maythe4bewithyou`

Annotationları silmek istersneiz `kubectl annotate pods mypod message-` komutu ile message anahtarı ile atanmış annotation tanımını silebilirsiniz.

## Senaryo 4

Bu senaryoda namespace objesine değinecek ve namespace oluşturmayı öğrneceksiniz.
## Namespace

Namespaceler adlar için bir kapsam sağlarlar. Kaynak adlarını bir namespace içinde benzersiz olması gerekir. Namespace birbirinin içine yerleştirilemez ve her K8S kaynağı yalnızca bir namespace içinde olabilir.

Namespaceler cluster kaynaklarını birden çok kullanıcı arasında bölmenin bir yoludur. Her kubernetes kurulumunda varsayılan olarak bir kaç namespace oluşturulur. Bunlar kube-system, kube-public, kube-node-lease ve default namespaceleridir. Kısaca kube ile başlayan namespaceler kubernetes tarafından oluşturulur ve clusterın işleyişi ile alaklı objelerin tutulduğu yerlerdir.

* **kube-system ->** Kubernetes tarafından oluşturulan objelerin tutulduğu namespacedir.

* **kube-public ->** Kimliği doğrulanmamış olanlar da dahil tüm kullanıcıların erişilmesine ihtiyaç duyulan objelerin oluşturulabileceği yerdir.

* **kube-node-lease ->** Node işlemleri için gereken özel bir namespacedir.

* **default ->** Aksi belirtilmedikçe varsayılan olarak objeler bu namespace içerisinde oluşturulur.

Senaryo ile uygulamalar yaparak konuyu pekiştirelim.

`kubectl get pods -n kube-system` komutunda -n parametresiyle kube-system namespaceindeki podları listeleyebiliriz.

**-all-namespaces** veya **-A** parametresi ile tüm namespacelerde işlem yapabiliriz.

`kubectl get pods --al-namespaces`

Yeni bir namespace oluşturmak için yine kullanabileceğimiz iki yöntem vardır. Namespacelerde bir k8s objesi olduğundan obje tanımının olduğu bir yaml dosyasını deklare edebilir veya kubectl komutu ile imperative yöntemle bunu yapabiliriz.

``kubectl create namespace uygulama`` komutu ile **uygulama** isimli bir namespace oluşturabiliriz.

``kubectl get namespaces`` ile kubernetes clusterdaki tüm namespaceleri listeleyebiliriz.

Oluşturacağımız objenin spesifik olarak bir namespacede konuşlanmasını istersek bunu obje tanımında belirtmeliyiz. Aşağıda iki obje tanımı yer almakta, namespace ve pod. Oluşacak app1 isimli pod objesinin hangi namespace altında konuşlanacağını metadata altında belirtiyoruz.

``vi example.yaml``

````
apiVersion: v1
kind: Namespace
metadata:
  name: development
---
apiVersion: v1
kind: Pod
metadata:
  namespace: development
  name: app1
spec:
  containers:
  - name: app1
    image: nginx:latest
    ports:
    - containerPort: 80
````

`kubectl apply -f example.yaml` komutu ile yukarıdaki yaml dosyasını apply edelim ve namespace ve pod olmak üzere iki obje oluşturalım.

Eğer bir obje ile iletişim kurmak isteniyorsa bu objenin bulunduğu namespace belirtilmelidir. Örneğin development namespace içindeki app1 podunu listelemek ve poda bağlanmak istersem;

``kubectl get pods -n development`` 

``kubectl exec -it app1 -n development -- /bin/bash``

Eğer kubectl ile config dosyasında varsayılan namespace belirtir isek -n parametresiyle her defasında namespace ismini belirtmeme gerek kalmadan config dosyasında belirtilen namespace üzerinde komutlar işlenir.

``kubectl config set-context --current --namespace=development``

Bir namespace silmek için aşağıdaki komutu kullanabilirsiniz. Burada namespace silinirken tüm objeler de beraberinde silinir bu sebeple dikkatli olunmasında fayda vardır.

``kubectl delete namespaces development``

