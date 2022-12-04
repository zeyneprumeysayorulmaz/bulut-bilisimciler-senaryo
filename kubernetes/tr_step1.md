# Seviye: Başlangıç

## Kubernetes Nedir?

Kubernetes, CNCF (Cloud Native Computing Foundation) tarafından yönetilen (başlangıçta Google’ ın tasarladığı), açık kaynak bir container orkestrasyon aracıdır.
### Container orkestrasyon nedir?

Çalışan çok sayıda container bulunduğu durumda onları kolayca yönetmek ve idare etmek için bir yoldur diyebiliriz.

K8S, hem beyan temelli yapılandırmayı hem de otomasyonu kolaylaştıran container iş yüklerini ve hizmetlerini yönetmek için oluşturulmuş, taşınabilir ve genişletilebilir açık kaynaklı bir platformdur.

Kubernetes platformu tamamen modüler bir şekilde tasarlanmış ve bu şekilde çalışıyor. K8s adında tek bir uygulama yok, k8s platformunu oluşturan bir çok servis mevcut ve bu servislerin bir arada düzgün kurgulanması sonucu k8s platformu oluşuyor.

Control planede master node dediğimiz yönetim nodeları yer alıyor. kube-apiserver, etcd, kube-scheduler ve kube-controller-manager komponentleri master node dediğimiz sistemler üzerinde çalışıyor. Master nodelar üzerinde iş yükü çalıştırmayız, burası yönetim altyapımızdır.

Diğer alanda ise worker nodelar çalışıyor ve her worker node üzerinde kubelet, kube-proxy ve container runtime bileşenleri yer alıyor. İş yükleri (podlar) bu nodelar üzerinde çalışır.

![](https://raw.githubusercontent.com/zeyneprumeysayorulmaz/bulut-bilisimciler-senaryo/main/kubernetes/img/k8scluster..JPG) 

## Control Plane Bileşenleri
### kube-apiserver
Control plane’in en önemli bileşeni, k8s’in beyni diyebiliriz. Diğer tüm komponentlerin, node bileşenlerinin ve dış dünyadan k8s platformu ile iletişim kuran tüm servislerin direkt olarak iletişim kurabildiği tek komponenttir. K8S’de kaynak oluşturma isteklerinin api doğrulamasından sorumludur. Kullanıcılar kubectl istemcisi ile veya rest api çağrıları ile kube-apiserver ile iletişim kurabilirler.

### etcd
Tüm cluster’ın verisi, metadata bilgileri ve K8S’ de oluşturulan tüm objelerin bilgilerinin tutulduğu anahtar-değer (key-value) veri deposudur. Kısacası k8s’in çalışması için gerekli olan veriyi üzerinde tutar.

Cluster’ın mevcut durumu ile ilgili veriyi üzerinde barındırır.
kube-apiserver hariç hiçbir bileşen direkt olarak etcd ile iletişime geçemez. İletişim gerektiğinde kube-apiserver aracılığıyla bunu yaparlar.
kube-scheduler
Yeni oluşturulan ya da bir node ataması yapılmamış podları izler ve üzerinde çalışacakları bir node seçer. Pod’un üzerinde çalışması için en uygun node’a karar verir.

### kube-controller-manager
Mantıksal olarak her controller ayrı bir süreçtir, ancak karmaşıklığı azaltmak için hepsi tek bir binary olarak derlenmiştir ve tek bir process olarak çalışır. Bu controllerların bazıları şunlardır:

* node controller
* job controller
* service account & token controller
* endpoints controller

K8S cluster’ın mevcut durumuyla ondan istenen durum arasında fark olup olmadığını gözlemler. kube-apiserver aracılığıyla etcd’de saklanan cluster durumunu izler. Eğer mevcut durum ve istenen durum arasında fark varsa bu farkı oluşturan kaynakları gerektiği gibi oluşturur, günceller veya silerek bu farkı kapatır.

Yukarıda değindiğimiz bu 4 komponent ( kube-apiserver, etcd, kube-scheduler, kube-control-manager) K8S’in yönetim ksımında yer alır. Control plane olarak adlandırılan bu yönetim alanı master node dediğimiz sistemler üzerinde çalışır. 

## Worker Node Bileşenleri
İş yüklerimiz worker node dediğimiz sistemler üzerinde koşar. İş yükü dediğimiz şey burada podlarımız yani containerlarımız oluyor. Üzerlerinde bir container rumtime (containerd, cri-o veya docker gibi) barındırırlar. Her worker node aşağıdaki 3 temel komonenti barındırır.

* kubelet
* k-proxy
* container runtime (Varsayılan dockerdı bazı nedenlerden dolayı desteğini bırakıp containerd’ye geçti.)
Bu bileşenleri inceleyelim.

### kubelet
kube-apiserver aracılığıyla etcd’yi kontrol eder ve kube-scheduler tarafından bulunduğu node üzerinde çalışması gereken pod belirtildiyse kubelet bu podu o node üzerinde oluşturur. Bunu yaparken containerd’ ye haber gönderir ve belirtilen özelliklerde bir container’ ın o sistemde çalışmasını sağlar.

### kube-proxy
Oluşturulan podların ağ kurallarını yönetir. Bu ağ kuralları cluster’ ın içindeki veya dışındaki ağ oturumlarından podlarınızla ağ iletişimine izin verir. Kısacası network proxy görevi görür.

### container runtime
Containerları çalıştırmaktan sorumlu olan yazılımdır. Kubernetes; containerd, CRI-O gibi contaner runtimeları ve Kubernetes CRI’nın (Container Runtme Interface) diğer tüm uygulamalarını destekler.

İşin sistem tarafında yer alıyor uzun vadede bir K8S ortamı yönetiyorsanız bu yapıyı iyi anlamak ve çalışma mantığını bilmek önemlidir.

Yukarıda kube-apiserver isimli bir komponentten bahsetmiştik. Tüm cluster kopmonentleri ve dış dünyadan bizler bu apiserver ile iletişime geçiyorduk. K8S cluster üzerinde bir işlem yapmak istiyorsak yani k8s’e bir komut vermek istiyorsak bunu kube-apiserver üzerinde yapıyoruz. Bunu gerçekleştirebilmenin ise 3 yöntemi var:

* Rest API çağrıları üzerinden iletişime geçebiliriz. Fakat her işlemi bu şekilde gerçekleştirmek oldukça zor.
* GUI istemcileri (apiserver ile iletişim kurmak için tasarlanmış çeşitli arayüzler) ile iletişime geçebiliriz.
* Kubectl resmi CLI aracı ile kubernetes cluster’ına komutlar gönderebiliriz.

## Öğrenme Hedefleri

Bu eğitim senaryosunda kubernetes platformunun en temel bileşeni olan pod objesini öğrenecek ve pod yaşam döngüsüne hakim olacaksınız. Aynı zamanda kubernetes platfromunu kullanmayı temelde bir pod objesi oluşturabilmeyi öğreneceksiniz.

## Ön gereksinimler

* Kubernetes platformunun çalışma presibini temel seviyede hakim olmalısınız.

## Senaryo 1

Bu bölümde pod objesine odaklanacak ve hem kubectl aracı ile hemde pod objesine ait yaml dosyasını kullanarak bir pod objesi oluşturacağız.

Kubernetes ortamında imperative yöntemle yani kubectl aracını kullanarak bir obje oluşturabilirsiniz. Bunun yanı sıra objeleri tanımladığınız bir yaml dosyasını uygulayarak da obje oluşturabilirsiniz.

K8s üzerinde oluşturduğumuz en temel obje pod dur. Pod, en küçük ve temel birimdir. Pod içerisinde bir veya daha fazla container barındırabilir fakat çoğu zaman bir pod içerisinde bir container çalışır. Her podun benzersiz bir “uid” değeri ve yine benzersiz bir ip adresi vardır.

Teorik kısımları geçelm ve birlikte bir pod oluşturalım. Pod oluşturmak için kubectl run komutunu kullanabilir ve yeni bir obje oluşturabiliriz. Aşağıdaki komut ile mypod adında bir pod oluşturuyor ve içerisinde çalıştırılacak container’ın bir nginx isimli imajdan oluşturulmasını söylüyorum. Son olarak da eğer podun içersindeki container kapanırsa yeniden çalıştırılmaması için -restart=Never parametresini kullanıyorum. 

`kubectl run mypod --image=nginx --restart=Never`

Podu oluşturduktan sonra peşinden clusterda çalışan podları listelemek için `kubectl get pods` komutunu çalıştırabilir ve oluşturduğunuz podları listeleyebilirsiniz.

**-o wide** ile geniş bir çıktı alabilirsiniz `kubectl get pods -o wide`. Burada status kısmı podun statüsünü belirtiyor. Bu çıktıda önemli olan bir diğer ayrıntı **Ready** sütunudur. Burada gördüğümüz 1/1, podda çalışması istenilen container sayısı 1 ve bu 1 container da şu anda çalışıyor ve hazır durumda anlamına gelmektedir.

Herhangi bir objeye ait daha detaylı bilgi edinmek istersek kubectl describe komutunu kullanabiliriz. Hangi obje tipini ayrıntılı görmek istiyorsak komuta o obje tpini ve ismini vermeliyiz. 

`kubectl describe pods mypod`

Container loglarını görmek isterseniz aşağıdaki gibi `kubectl logs mypod ` komutunu çalıştırabilirsiniz. `kubectl logs -f mypod` ile canlı logları da izlemeniz mümkün.

Podun içerisinde çalışan containere bağlanmak ve komut çalıştırmak isteyebilirsiniz, bunun için kubectl exec komutunu kullanabilirsiniz.

`kubectl exec mypod -- <çalıştırılmak istenen komut örneğin, ls, pwd gibi>`

Poda bağlanmak isterseniz `kubectl exec -it mypod -- /bin/sh` komutunu kullanmalısınız.

`kubectl delete pods mypod` komutu ile de oluşturduğunuz pod objesini silebilirsiniz.

Imperative yöntemle yani kubectl aracı ile bir pod objesi oluşturduk. Fakat pratikte biz objeleri direkt kubectl komutları ile oluşturmayız. Bu imperative yöntemdir. Bir obje oluştururken o objede olmasını istediğimzi özellikleri belirttiğimiz bir dosya oluşturur ve k8s’e oluşturduğumuz dosyadaki özelliklere göre bir obje istediğimizi söyleyerek deklarasyonda bulunuruz. Bu declarative yöntemdir.

Kubernetesde obje tanımlarının yaml formatında yapılmasına izin verilir. Bizler objeleri yaml dosyaları şeklinde tanımlar ve k8s’e deklare ederiz. Her objenin tanımı farklıdır ama temelde benzer olan bir formatı takip eder.

Örneğin pod objesinin tanımlandığı aşağıdaki gibi bir yaml dosyası oluşturalım ve bunu kubectl apply komutu ile kubernetes’e deklare edelim.

`vi pod.yaml` komutu ile yaml dosyasını oluşturalım.

````
apiVersion: v1
kind: Pod
metadata:
  name: mypod2
  labels:
    app: frontend
spec:
   containers:
   - name: nginx
     image: nginx:latest
     ports:
     - containerPort: 80
````
`kubectl apply -f pod.yaml` komutunu çalıştırarak pod objesini oluşturabilirsiniz.

Kendiniz bir pod objesi oluşturabilirsiniz. Burada **apiVersion, kind, metadata** anahtarlarının her k8s obje tanımında bulunması gerekir. **kind**, objenin ne olduğunu söylediğimiz anahtardır, yani objenin tipini burada söyleriz. **apiVersion** ise oluşturmak istediğimiz obje tipinin k8s apisinde hangi endpoint üstünde tanımlandığını belirttiğimiz anahtardır. Her objenin sunulduğu bir api vardır, obje tipine göre burada anahtara gerekli olan api bilgisini girmek gerekiyor. K8s dokümantasyonlarından veya kubectl aracı ile buradaki obje tipinin sunulduğu api bilgisini öğrenebiliriz. Aşağıdaki gibi `kubectl explain <obje-adı>` komutu ile bunu öğrenebilmemiz mümkün.

**metadata** ise obje ile ilgili uniq bilgilerin tanımlandığı yerdir. Objeye özel, objeyi betimleyen bilgiler burada tanımlanır. **spec** kısmı her obje tipine göre değişkenlik gösterir. Buradaki tanımlanacak bilgileri de dokümantasyonlardan elde edebiliriz.

`kubectl describe pod mypod2` komutu ile oluşturduğumuz pod objesi ile ilgili detaylı bilgi edebiliriz.

Son olarak pod yaşam döngüsünden kısaca bahsedelim.

## Pod Yaşam Döngüsü

Pod oluşturmak için ya imperative yöntemle kubectl aracını kullanarak bir pod oluşturuyoruz ya da bir yaml dosyasında istediğimiz poda dair detayları vererek yaml dosyasını kubectl apply komutu ile k8s api’ine deklare ederek bir pod oluşturuyoruz. K8s API Server bu yaml dosyasını alır, bizim belirttiğimiz pod özellikleri ile varsayılan set edilmiş ayarları birleştirir, poda uniq ID atar ve tüm bunları etcd’ye kaydeder. Bu noktadan itibaren pod oluşturulmuştur ve yaşam döngüsüne başlar.

### Pending
Sistemde bir pod oluşturuldu. Podla ilgili tüm gerekli tanımlamalar yapıldı ve veritabanına kaydedildi. Ama pod henüz herhangi bir node üzerinde oluşturulmadı. Eğer pod uzun bir süre bu aşamada bekliyorsa scheduler uygun bir node bulamamış demektir. Bunun bir çok nedeni olabilir. Scheduler, etcd’yi sürekli gözler ve yeni yaratılan ataması yapılmamış pod varsa çalışmaya başlar. Podun çalışabieceği en uygun node’a atamayı yapar ve bu aşamadan sonra pod creating duruma geçer.

### Creating
kubelet, etcd’yi sürekli gözleyerek bulunduğu node’a atanmış br podları tespit eder. Yeni oluşturulmuş bir pod varsa hemen işlemlere başlar. Pod tanımında belirtilen container imajlarını sisteme indirmeye başlar, eğer bir şekilde indirme yapılamazsa pod, errImagePull ve ardından ImagePullBackOff aşamasına geçer. ImagePullBackOff, node’un imajı repositoryden çekemediği anlamına gelir. Bunun bir kaç nedeni olabilir. Imaj düzgün çekilirse pod diğer statüye geçer.

### Running
kubelet, container engine ile haberleşir ve ilgili containerların oluşturulmasını sağlar. Containerlar çalışmaya başladığı an pod running duruma geçer. Artık pod oluşturulmuş olur. Podun altında çalışan containerlar çalışmaya devam ettikçe pod running durumda kalmaya devam eder.

### Succeeded
Eğer containerların hepsi hata vermeden doğal olarak kapanırsa ve restart policy, never ya da on-failru olarak set edildiyse pod succeeded/completed statüsüne döner ve pod yaşam döngüsünü başarılı bir şekilde tamamlar.

### Failed
Yine restart policy, never veya on-failru olarak işaretlendiyse ve containerlardan biri hata verip kapanırsa pod failed statüsüne geçer.

Fakat restart policy, always olarak ayarlandıysa ve containerlar hata kaynaklı veya hata olmaksızın kapansa bile restart olur ve pod, hiç bir zaman completed veya failed state’e geçmez. Bunun yerine podun içerisindeki container yeniden başlatılır ve running state’de devam eder. Fakat bu yeniden başlatma belirli aralıkta sık oluyorsa k8s bir hata olduğuna kanaat getirir ve podu CrashLoopBackOff statüsüne koyar.

### CrashLoopBackOff
Bir pod oluştu ama içerisindeki container veya containerlar belirli aralıklarla kapanıp duruyor demektir. Bir çok nedeni olabilir.

Temel Kural Container imajlarında, container oluşturulup çalıştırıldığı zaman başlatılması belirlenen bir uygulama bulunur. Bu uygulama çalıştığı sürece container da çalışır durumdadır. Fakat uygulama çalışmayı bıraktığı zaman containerda durdurulur. Uygulamanın çalışmayı bırakması da 3 şekilde olur.

* Uygulama görevlerini tamamlayıp otomatik şekilde hata vermeden kapanır.

* Kullanıcı ya da sistem uygulamayı kapanma sinyali gönderir ve hata vermeden kapanır.

* Ya da bir şekilde hata verir ve hata kodu oluşturarak kapanır.

Podun içerisinde çalışan containerlara podun içinde bir restart policy tanımlanır. Restart policy 3 değer alabilir.

* **Always:** Default değerdir. Aksi belirtilmedikçe bu ayarlanır. Podun içindeki container hata vererek veya vermeyerek hangi durumda durdurulursa durdurulsun o containerı yeniden başlat demektir.

* **On-failru:** Sadece hata alıp kapanırsa yeniden başlatılır.

* **Never:** Hiç bir zaman yeniden başlatılmaz.