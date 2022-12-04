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