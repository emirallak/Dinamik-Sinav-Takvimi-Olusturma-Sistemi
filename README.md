# Dinamik Sınav Takvimi Oluşturma Sistemi

> A Java Swing desktop application that automatically builds university exam schedules and seating plans from Excel course and student lists.

Kocaeli Üniversitesi Bilgisayar Mühendisliği, **Yazılım Laboratuvarı I – Proje 1** kapsamında geliştirilmiş bir masaüstü uygulaması. Ders ve öğrenci listelerini Excel'den okur. Derslik kapasitesi, öğrenci çakışmaları ve zaman kısıtlarını gözeterek sınav takvimini ve oturma planlarını otomatik oluşturur. Sonuçlar Excel ve PDF olarak dışa aktarılır.

## Özellikler

- **Rol tabanlı giriş:** E-posta ve şifre ile giriş yapılır.
  - **Admin** tüm bölümleri yönetir ve kullanıcı ekler.
  - **Bölüm Koordinatörü** yalnızca kendi bölümünün verileriyle çalışır.
- **Derslik yönetimi:** Kapasite, enine/boyuna sıra sayısı ve sıra yapısı (2'li/3'lü) ile derslik ekleme, düzenleme ve görüntüleme.
- **Excel'den veri yükleme:** Ders listesi ve öğrenci–ders kayıt listesi Apache POI ile okunur.
- **Otomatik sınav programı:**
  - Programa dahil edilmeyecek dersler ve hariç tutulan günler seçilebilir.
  - Günlük başlangıç/bitiş saatleri ve derse özel sınav süreleri ayarlanabilir.
- **Oturma planı:** Her sınav oturumu için öğrenciler dersliklere ve sıralara yerleştirilir. Plan PDF olarak alınır.
- **Dışa aktarma:** Sınav programı Excel'e (Apache POI), oturma planları PDF'e (Apache PDFBox) aktarılır.

## Zamanlama Algoritması

`SinavProgramiOlusturucu` sınıfı açgözlü (greedy) bir yerleştirme yaklaşımı kullanır:

1. **İlişkilendirme:** Her ders için o dersi alan öğrenciler çıkarılır. Dersin hangi sınıf seviyesine ait olduğu, öğrencilerinin çoğunluğuna göre belirlenir.
2. **Zaman dilimleri:** Hariç tutulan günler ve günlük saat aralığı dikkate alınarak zaman dilimleri üretilir.
3. **Sıralama:**
   - Dersler kalabalıktan aza sıralanır, böylece en zor yerleşen dersler önce yerleşir.
   - Derslikler kapasitesi küçükten büyüğe sıralanır, böylece alan verimli kullanılır.
4. **Yerleştirme:**
   - Dersler sınıf seviyesine göre gruplanır ve günlere dağıtılır.
   - Aynı sınıf için günlük sınav sayısı sınırlandırılır.
   - Bir öğrencinin aynı anda iki sınavı olmasını engellemek için her öğrenci için bir takvim tutulur.
5. **Raporlama:** Yerleştirilemeyen dersler kullanıcıya uyarı olarak raporlanır.

## Teknolojiler

| Alan | Teknoloji |
| --- | --- |
| Dil | Java 23 |
| Arayüz | Java Swing (NetBeans GUI Builder), JCalendar |
| Veritabanı | PostgreSQL (JDBC) |
| Excel | Apache POI (poi-ooxml) |
| PDF | Apache PDFBox |
| Derleme | Maven |
| Loglama | Log4j 2 |

## Proje Yapısı

```
src/main/java/com/mycompany/yazlab1/
├── DTO/                          # Ders, Derslik, Ogrenci, SinavOturumu, ZamanDilim, OturmaPlaniDTO
├── SinavProgramiOlusturucu.java  # Sınav takvimi algoritması
├── OturmaPlaniOlusturucu.java    # Oturma planı yerleştirme
├── OturmaPlaniPDFOlusturucu.java # PDF çıktısı
├── SinavProgramiExcelAktar.java  # Excel çıktısı
├── DersListesiParseleme.java     # Ders listesi Excel okuyucu
├── OgrenciListesiParseleme.java  # Öğrenci listesi Excel okuyucu
├── VeritabaniBaglanti.java       # PostgreSQL bağlantısı
└── *Ekrani.java                  # Swing ekranları (giriş, admin, koordinatör, derslik, ders, öğrenci…)
```

## Kurulum ve Çalıştırma

1. Depodaki `yazlab1.rar` arşivini açın.
2. PostgreSQL'de `yazlab1` adında bir veritabanı oluşturun.
3. `VeritabaniBaglanti.java` içindeki kullanıcı adı ve şifreyi kendi ortamınıza göre düzenleyin. Varsayılan bağlantı adresi: `jdbc:postgresql://localhost:5432/yazlab1`.
4. Projeyi derleyip çalıştırın:

   ```bash
   cd yazlab1
   mvn clean package
   mvn exec:java -Dexec.mainClass="com.mycompany.yazlab1.Yazlab1"
   ```

   Alternatif olarak proje NetBeans veya IntelliJ IDEA ile açılıp `Yazlab1` sınıfından çalıştırılabilir.
5. Örnek veriler için arşivdeki `ders_listesi.xlsx` ve `ogrenci_listesi.xlsx` dosyalarını uygulama içinden yükleyin.

## Geliştirici

**Muhammed Emir Allak**: [GitHub](https://github.com/emirallak) · [LinkedIn](https://www.linkedin.com/in/muhammedemir-allak-a445a3246/)
