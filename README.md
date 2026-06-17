# 🛒 E-Ticaret Backend ve AWS Bulut Mimari Projesi

Bu proje, Java Spring Boot ve MySQL kullanılarak geliştirilmiş, modüler bir e-ticaret backend sistemidir  Aynı zamanda uygulamanın, Amazon Web Services (AWS) üzerinde yüksek erişilebilirlik, yük dengeleme ve otomatik ölçeklendirme (Auto Scaling) prensipleriyle nasıl canlıya alındığını göstermektedir 

## 🚀 Kullanılan Teknolojiler

* **Backend:** Java 17, Spring Boot, RESTful API 
* **Veritabanı:** MySQL (Sürüm 8.4.8), Spring Data JPA, HikariCP Bağlantı Havuzu 
* **Bulut Altyapısı (AWS):** EC2 (Ubuntu Server, t3.micro), Application Load Balancer (ALB), Auto Scaling Group (ASG), Target Group 

## 🏗️ Yazılım Mimarisi (Backend Katmanı)

* **Modüler Yapı:** Proje `com.bulut.eticaret` kök paketi altında yapılandırılmıştır.
* **Sınıflar ve Arayüzler:** Uygulama içerisinde ürünler, kullanıcılar ve siparişler için (`Product`, `User`, `Order`) temel veri sınıfları ve veritabanı işlemlerini (CRUD) SQL yazmadan otomatikleştiren JPA Repository arayüzleri bulunmaktadır.
* **API Yönetimi:** Dışarıdan gelen HTTP istekleri Controller katmanı (örneğin `ProductController`) üzerinden karşılanır. Sistemimizin çalıştığını kanıtlayan ürün listeleme uç noktası `/api/products` üzerinden hizmet vermektedir.

## ☁️ AWS Bulut Kurulumu ve Otomatik Ölçeklendirme

Sistem, tek bir sunucu arızasından etkilenmeyecek (Fault Tolerance) şekilde kurgulanmıştır 

* **Temel İmaj (AMI) ve Şablon:** Java ve uygulama (.jar) dosyalarımızı barındıran `eticaret-ami-v2` adlı özel imajımız oluşturulmuş, bu imajın `nohup java -jar eticaret-0.0.1-SNAPSHOT.jar &` komutuyla otomatik başlaması için `eticaret-template-v2` başlatma şablonu yapılandırılmıştır 
* **Yük Dengeleyici (Load Balancer):** İnternete açık Application Load Balancer (HTTP 80), dış dünyadan gelen trafiği karşılar ve Hedef Gruptaki (Target Group) sağlıklı sunucuların 8080 portlarına eşit şekilde dağıtır 
* **Otomatik Ölçeklendirme (Auto Scaling):** Trafik durumuna göre minimum 2, maksimum 4 sunucu çalışacak şekilde ayarlanmıştır  Sistem, arızalanan sunucuları tespit edip otomatik olarak yenilerini ayağa kaldırarak kesintisiz bir deneyim sunar.

## 🛠️ Sorun Giderme (Troubleshooting) ve Optimizasyon

* Uygulamanın geliştirme sürecinde yaşanan "502 Bad Gateway" ve tarayıcı HTTPS zorlaması kaynaklı çakışmalar, mimarinin uçtan uca düz HTTP protokolüne geçirilmesiyle aşılmıştır 
* Fiziksel SSH anahtar (`.pem`) bağımlılığı, tarayıcı tabanlı AWS EC2 Instance Connect kullanılarak ortadan kaldırılmıştır 
* Güvenlik Grubu (Security Group) kuralları güncellenerek 8080 ve 22 (SSH) portlarının doğru şekilde dış dünyaya açılması sağlanmıştır 
