## Öğrenme Hedefleri

Bu eğitim senaryosunda kubernetes objelerinden biri olan deployment objesine değinecek, aynı zamanda kubernetes platfromunda bir deployment ve replicaSet objelerini oluşturabilmeyi öğreneceksiniz. Aynı zamanda Rollout ve Rollback konusuna da hakim olacaksınız.

## Ön gereksinimler

* Kubernetes platformunun çalışma presibini temel seviyede hakim olmalısınız.

## Senaryo 5

### Deployment Objesi

Bu bölümde imperative yöntemle ve deklarative yöntemle deployment objesi oluşturacağız.

Pod bölümünde, podların kubernetesin en temel objesi olduğuna değinmiştik. Bizler kubernetesde container imajına çevirdiğimiz uygulamalarımızı pod olarak deploy ederiz. Genelde kubernetes üzerinde singleton olarak adlandırılan tekil, yönetilmeyen podlar yaratmayız. Podları yöneten üst seviye objeler yaratırız ve podlar bu objeler tarafından yaratılıp, yönetilir. Bu objelerin en sık kullanılanlarından bir tanesi **deployment** objesidir. Neredeyse iş yüklerinin tamamı deployment objesi olarak deploy edilir.

``kubectl create deployment uygulama1 --image=nginx:latest --replicas=3`` komutu ile imperative olarak bir deployment objesi oluşturabiliriz. Oluşturmak istediğim obje tipinin **deployment** olduğunu söyledim, buna bir isim verdim ve oluşturmak istediğim container imajının **nginx** olmasını , **-replicas** ile de kaç adet pod oluşturacağımı belirttim.

``kubectl get deployment`` ile bu obje tanımına bakabiliriz.

Örneğin replicas olarak 3 belirttiğiniz bir deploymentta manuel olarak bir podu sildiniz veya silindi diyelim.

``kubectl delete pod <pod-name>`` komutu ile podu silebilirsiniz.

Burada deployment controller devreye girer ve silinen podun yerine desired state’te belirtildiği gibi bir pod daha oluşturur. Yani tanımda verilen istenen durum ile mevcut durum arasında fark varsa bunu hemen düzeltir ve istenen duruma getirir. Aslında bunu ReplicaSet objesi yapıyor ama bir sonraki senaryoda bu ayrıntıya değineceğiz.

Mevcut deployment üzerinde değişiklik yapmak istersem örneğin imaj değiştirmek istiyorum diyelim;

``kubectl set image deployment/uygulama1 nginx=httpd``  burada **uygulama1** adlı deployment objesindeki imaj değerinin **nginx** yerine **httpd** olmasını belirttim. Deployment objesi istenen değişiklikleri yapmak için sırasıyla podları kapatır ve yeni imajla ayağa kaldırır.

``kubectl scale deployment uygulama1 --replicas=4 ``ile çalışan pod sayısının 4 olmasını söyleyebilirim. **scale** komutuyla pod sayısında değişiklikler yapabiliriz.

``kubectl delete deployments uygulama1`` deployment objesini silmek için delete kullanabiliriz.

Kubectl ile imperative yöntemle işlemleri yapabildik, peki deklaretive yöntemle bunu nasıl yapıyoruz?

Örneğin aşağıdaki gibi bir deployment objesini tanımlayan dosya oluşturabilir ve bunu k8s api’sine deklare edebiliriz.

`vi uygulama1.yaml`

````
apiVersion: apps/v1
kind: Deployment
metadata:
  name: uygulama1
  labels: 
    team: development
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
````

`kubectl apply -f uygulama1.yaml` ile deklerative olarak oluşturduğunuz deploymentı apply edebilirsiniz.

## Senaryo 6

### ReplicaSet Objesi

Bu bölümde bir replicaset objesinin görevini öğrenecek ve replicaset objesi oluşturmayı öğreneceğiz.

**Bir replicaSet’in amacı, herhangi bir zamanda çalışan kararlı bir replika pod setini sürdürmektir. Bu nedenle, genellikle belirli sayıda özdeş podun kullanılabilirliğini garanti etmek için kullanılır.**

Bir deployment objesi oluşturduğumuz zaman bu deployment objesi kendi yönettiği bir replicaSet objesi oluşturur ve bu replicaSet objeside podları yaratır/yönetir. Eğer deployment tanımında bir değişiklik yaparsak; deployment, bu yeni tanımla yeni bir replicaSet objesi oluşturur. İlk yaratılan replicaSet objesi yavaş yavaş kendi oluşturduğu podları silmeye başlar, yeni replicaSet ise yeni podları yaratır. Bu silme ve oluşturma işleminin hangi sırayla ne şekilde olacağını biz belirleyebiliriz. Bu özellik bize **herhangi bir kesinti olmadan uygulama güncelleme ve yeni versiyon geçiş imkanı** sağlar.

Aşağıdaki gibi bir yaml dosyasını oluşturarak manuel olarak replicaSet objesi yaratılabilir. Fakat ReplicaSet; rollout, undo gibi özellikleri bize sunmadığı için replicaSet yaratmak yerine deployment yaratırız ve deployment bu replicaSeti yaratır, bizde bu işlemlerimizi gerçekleştirebiliyor oluruz.

``vi replicaset.yaml`

````
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: firstreplicas
  labels: 
    app: rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: rs
  template:
    metadata:
      labels:
        app: rs
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
````

``kubectl apply -f replicaset.yaml`
## Senaryo 7

### Rollout ve Rollback

Bu bölümde rollout ve rollback kavramlarını uygulayarak öğreneceğiz.

Deployment yaml dosyalarında **spec** altında **strategy** anahtarıyla bizler, deploymentı güncellediğimiz zaman rollout işlemlerinin nasıl yapılacağını belirleriz. Kullanabileceğimiz iki tip rollout stratejisi mevcuttur. Aşağıdaki yaml dosyasında bunlardan biri olan **recreate** set edilmiştir. Recreate şu anlama gelir: Bu deployment üzerinde bir değişiklik yapılırsa öncelikle mevcut tüm podları sil ve bu işlem tamamlandıktan sonra yeni podları oluştur. Bu yapıda kesinti olacaktır fakat major değişikliklerde bu starteji tercih sebebi olabilmektedir.

````
strategy:
  type: Recreate
````

Eğer yaml dosyasında bir strateji belirtmezsek varsayılan olarak **RollingUpdate** Startejisi seçilir.

````
strategy:
  type: RollingUpdate
    maxUnavailable: 2
    maxSurge: 2
````
RollingUpdate, recreate stratejisinin tam tersidir. Deploymentta bir değişiklik yaptığım zaman podlar üzerinde bu işlemi aşamalı olarak yapar. Bu aşamaların nasıl olacağını belirleyebileceğimiz iki opsiyon mevcuttur. Bu opsiyonlar yukarıda da görebileceğiniz üzere **maxUnavailable** ve **maxSurge** değerleridir. Bu strateji bizlere zero downtime geçiş yapma imkanı verir.

**maxUnavailable: Bir deploymentta değişiklik yaptığım zaman en fazla burada belirlediğim sayıdaki podu sil. Değişiklikleri uyglarken aynı anda en fazla x pod silebilir demektir aslında. Burada sayı yerine yüzdelik ifade de kullanılabilir.**

**maxSurge: Geçiş sırasında toplam pod sayısının en fazla kaç olabileceğini belirler. Örneğin desired state 10 pod olan bir ortamda geçiş sırasında 2 pod ayağa kalkacak ve toplamda 12 pod sistemde aynı anda çalışabilir.**

Örneğin aşağıdaki gibi deployment objesi oluşturalım ve bu objenin image değerini değiştiriyor olalım.

`vi deployment.yaml`

````
apiVersion: apps/v1
kind: Deployment
metadata:
  name: uygulama1
  labels: 
    team: development
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
````

`kubectl apply -f deployment.yaml`  bu deployment objesini apply edelim.

``kubectl set image deployment uygulama1 nginx=httpd:alpine --record=true`` yeni imaj değerini set ettik.

Buradaki record bize ne sağlıyor? Deployment rollout işlemlerinde bir sıkıntı olduğu zaman daha önceki duruma geçebilmemiz mümkün. **--record** ile ne zaman, neyi değiştiridik bu bilgiyi izleyebiliyoruz. Eğer bir değişiklikte -record parametresini kullanırsanız liste halinde history alabilmeniz mümkün.

``kubectl rollout history deployment uygulama1`` -> rollout history komutu bizlere deployment üzerinde yapılan değişiklikleri gösterme imkanı sunuyor. Bu komut çıktısında uygulama1 deployment objesi içinde -record parametresi set edilerek yapılan tüm değişiklikler bizlere listenir.

Eski bir versiyonu görmek istersem:

``kubectl rollout history deployment uygulama1 --revision=2`` -> örneğin revision 2 numaralı değişikliği gösterir.

``kubectl rollout undo deployment uygulama1`` dersem bir önceki versiyona/bir önceki değişikliğe geri dönebilirim.

Eğer spesifik olarak dönmek istediğim bir revision varsa;

``kubectl rollout undo deployment uygulama1 --to-revision=3`` -> örneğin 3 numaralı revision'a geri dönebilirim.

Özetle bu sayede bizler deployment üzerinde yaptığımız değişiklikleri kataloglama ve sonrasında da geri dönme imkanına kavuşuruz.

Son olarak;

``kubectl rollout status deployment uygulama1 -w`` -> rollout status komutu ile bir deployment üzerinde yapılan değişiklikleri canlı olarak adım adım izleyebiliriz.

``kubectl rollout pause deployment uygulama1`` -> rollout pause komutu ile değişikliği direkt geri almaktansa değişikliği durdurabiliriz ki geçiş esnasında oluşan bir sıkıntının yani yeni versiyonun bir soruna yol açması ihtimalinin analizini yapmak kolay olsun. Direkt geçişi iptal ettiğim eski versiyona döndüğüm durumda sıkıntının/sorunun kaynağının değişiklik kaynaklı mı olduğunu anlayamam. Sorunu çözdüğünüz durumda veya sorunun değişiklik kaynaklı olmadığını anladığınız durumda ``kubectl rollout resume deployment uygulama1`` komutu ile işleme devam edebilirsiniz.