## Öğrenme Hedefleri

Bu eğitim senaryosunda kubernetes objelerinden biri olan deployment objesine değinecek, aynı zamanda kubernetes platfromunda bir deployment objesi oluşturabilmeyi öğreneceksiniz.

## Ön gereksinimler

* Kubernetes platformunun çalışma presibini temel seviyede hakim olmalısınız.

## Senaryo 5

Bu bölümde imperative yöntemle ve deklarative yöntemle deployment objesi oluşturacağız.

Pod bölümünde, podların kubernetesin en temel objesi olduğuna değinmiştik. Bizler kubernetesde container imajına çevirdiğimiz uygulamalarımızı pod olarak deploy ederiz. Genelde kubernetes üzerinde singleton olarak adlandırılan tekil, yönetilmeyen podlar yaratmayız. Podları yöneten üst seviye objeler yaratırız ve podlar bu objeler tarafından yaratılıp, yönetilir. Bu objelerin en sık kullanılanlarından bir tanesi **deployment** objesidir. Neredeyse iş yüklerinin tamamı deployment objesi olarak deploy edilir.

``kubectl create deployment uygulama1 --image=nginx:latest --replicas=3`` komutu ile imperative olarak bir deployment objesi oluşturabiliriz. Oluşturmak istediğim obje tipinin **deployment** olduğunu söyledim, buna bir isim verdim ve oluşturmak istediğim container imajının **nginx** olmasını , **-replicas** ile de kaç adet pod oluşturacağımı belirttim.

``kubectl get deployment`` ile bu obje tanımına bakabiliriz.

Örneğin replicas olarak 3 belirttiğiniz bir deploymentta manuel olarak bir podu sildiniz veya silindi diyelim.

kubectl delete pod <pod-name>ğoıxfedxpşuoıkyftdxguoıcfx