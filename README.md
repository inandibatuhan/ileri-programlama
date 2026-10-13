# 🧠 Yapay Zeka Destekli Eğitim Yönetim Sistemi (AI-LMS)

Bu proje, geleneksel eğitim sistemlerinin (ör. Mergen) sınırlarını aşarak, yapay zeka entegrasyonu ile öğrenme sürecini interaktif hale getiren modern bir LMS projesi için oluşturulmuştur.

---

## 🎯 1. Projede Olması Gereken Özellikler (Sistem Gereksinimleri)

Bir bilgisayar mühendisliği projesi standartlarında, sistemin sahip olması gereken temel özellikler şunlardır:

*   **🔐 Rol Bazlı Erişim ve Yönetim:**
    *   **Öğretmenler:** Sisteme ders materyali (PDF, Word, TXT) yükleyebilir, sanal sınıflar oluşturabilir ve öğrencilerin gelişimini takip edebilir.
    *   **Öğrenciler:** İlgili derslere kaydolup materyallere erişebilir ve kendilerine özel üretilen sınavları çözebilir.

*   **🤖 AI Tabanlı Soru Jeneratörü (Ana Özellik):**
    *   Sistemin en yenilikçi yanıdır. Öğretmen tarafından yüklenen ders notları, arka planda çalışan yapay zeka tarafından analiz edilir.
    *   Algoritma, metnin içeriğine uygun **çoktan seçmeli sorular, doğru/yanlış testleri veya boşluk doldurma egzersizleri** üreterek veritabanına kaydeder.

*   **📝 İnteraktif Quiz ve Değerlendirme Modülü:**
    *   Öğrenciler sadece PDF okumakla kalmaz; anında üretilen bu soruları dinamik bir quiz ekranında çözer.
    *   Sistem, doğru ve yanlış cevapları analiz ederek öğrenciye anında geri bildirim sunar.

*   **📂 Gelişmiş Doküman Yönetimi:**
    *   Ders materyallerinin güvenli bir şekilde sunucuda tutulması ve ders bazlı düzenli bir yapıda listelenmesi.

---

## 🛠️ 2. Teknoloji Yığını (Tech Stack) ve Mimari Kararlar

Projenin ölçeklenebilir, hızlı ve yapay zeka entegrasyonuna tam uyumlu olması için aşağıdaki teknolojiler tercih edilmiştir:

### 📌 Backend & API (Sunucu Tarafı)
*   **Teknoloji:** `Python (FastAPI)`
*   **Neden?** Projenin merkezinde yapay zeka ve metin işleme işlemleri yer almaktadır. Python'un yapay zeka ekosistemindeki gücü ve FastAPI'nin asenkron yapısıyla sunduğu yüksek performans, bu mimari için en verimli çözümdür.

### 📌 Yapay Zeka Entegrasyonu (AI Layer)
*   **Teknoloji:** `Google Gemini API` & `PyMuPDF`
*   **Neden?** Yüklenen PDF'lerdeki metni eksiksiz bir şekilde çıkarmak (PyMuPDF) ve bu metinleri analiz edip JSON formatında dinamik soru-cevap setlerine dönüştürmek (Gemini API) için kullanılacaktır.

### 📌 Veritabanı (Database)
*   **Teknoloji:** `PostgreSQL`
*   **Neden?** Öğretmenler, öğrenciler, dersler ve AI tarafından üretilen sorular arasında detaylı bir veri ilişkisi olacaktır. Veri bütünlüğünü sağlamak ve karmaşık sorguları hızlı çalıştırmak için en sağlam ilişkisel veritabanı (RDBMS) seçeneğidir.

### 📌 Frontend (Kullanıcı Arayüzü)
*   **Teknoloji:** `React.js` & `Tailwind CSS`
*   **Neden?** Kullanıcıya sayfa yenilenmesi beklemeden, hızlı ve modern bir deneyim (SPA - Single Page Application) sunmak için seçilmiştir. Tailwind CSS ise arayüz tasarım sürecini profesyonelleştirecek ve hızlandıracaktır.