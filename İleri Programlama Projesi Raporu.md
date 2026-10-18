# 🚀 AI-LMS Projesi - Frontend (Önyüz) Mimari ve Teknoloji Kullanım Raporu

Bu doküman, yapay zeka destekli Eğitim Yönetim Sistemi (AI-LMS) projesinin kullanıcı arayüzü (frontend) geliştirme süreçlerinde tercih edilen araçların teknik detaylarını, proje içerisindeki konumlarını ve uygulama yöntemlerini açıklamaktadır.

---

## 1. Temel Çatı ve Geliştirme Ortamı

### ⚛️ React.js & Vite
- **Nedir?** Kullanıcı arayüzleri oluşturmak için kullanılan bileşen (component) tabanlı bir JavaScript kütüphanesidir. Vite ise kodu saniyeler içinde derleyen modern bir yapı aracıdır.
- **Nerede Kullanılacak?** Projenin tamamı React üzerinde koşacaktır. `App.jsx` ana bileşen olacak; sınav ekranı, öğretmen paneli, giriş ekranı gibi her bir sayfa ayrı birer React Bileşeni (Component) olarak tasarlanacaktır.
- **Nasıl Kullanılacak?** 
  Proje aşağıdaki komutla başlatılır. SPA (Single Page Application) mimarisi uygulanacağı için, `react-router-dom` paketi ile sayfalar arası yönlendirmeler (Örn: `/dashboard`, `/sinav-ol`) sayfa yenilenmeden, anında gerçekleştirilir.
  ```bash
  npm create vite@latest ai-lms-frontend -- --template react
  ```

---

## 2. Tasarım ve Stil Yönetimi

### 🎨 Tailwind CSS
- **Nedir?** Yardımcı sınıf (utility-class) tabanlı, modern ve son derece hızlı bir CSS framework'üdür.
- **Nerede Kullanılacak?** Sınav ekranındaki butonlar, öğretmen paneli, formlar, navigasyon çubukları ve "Skeleton UI" (Yükleniyor) ekranları dâhil arayüzün %100'ünün renklendirilmesi ve konumlandırılmasında.
- **Nasıl Kullanılacak?** 
  Harici CSS dosyaları yazmak yerine, HTML etiketlerinin içine doğrudan sınıflar yazılır. Mobil uyumluluk için özel dosyalar açılmaz; doğrudan class içine `md:` veya `sm:` ön ekleri konularak tüm arayüzün cihazlara uyumu sağlanır.
  ```jsx
  <button className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded md:w-auto w-full">
    Sınavı Başlat
  </button>
  ```

---

## 3. Veri ve Durum Yönetimi (State Management)

### 🐻 Zustand (veya Redux Toolkit)
- **Nedir?** Uygulama genelinde kullanılacak değişkenleri (kullanıcı kimliği, temalar, aktif sınav verisi) merkezi bir depoda tutan, minimal ve hızlı bir durum yönetim aracıdır.
- **Nerede Kullanılacak?** Dinamik Yetki Kontrolü (Role-based UI) için kullanıcının "Öğretmen" mi "Öğrenci" mi olduğu bilgisini tutmak ve sınav ekranındaki anlık verileri (kalan süre, işaretlenen şıklar) hafızada tutmak için.
- **Nasıl Kullanılacak?** 
  Örneğin `useAuthStore.js` adında bir dosya oluşturulur. Kullanıcı giriş yaptığında dönen kullanıcı bilgileri ve yetkisi (Role: student/teacher) bu Store'a kaydedilir.
  ```javascript
  const { role } = useAuthStore();
  // role "teacher" ise paneli göster, değilse gizle
  {role === 'teacher' && <UploadDocumentPanel />}
  ```

---

## 4. Sunucu İletişimi ve Dosya İşlemleri

### 🌐 Axios
- **Nedir?** Backend'e (FastAPI vb.) HTTP istekleri (GET, POST) atmamızı sağlayan esnek kütüphane.
- **Nerede Kullanılacak?** Giriş formunda, öğretmen PDF yüklerken ve yapay zekadan JSON formatında sınav soruları çekilirken.
- **Nasıl Kullanılacak?** 
  Axios **Interceptors** özelliği kullanılarak her isteğin başlığına (Headers: Authorization) JWT token'ı otomatik eklenir. Yapay zeka büyük bir PDF'i işlerken süreç uzayacağı için istek atıldığında "Skeleton UI" gösterilecek, `.then()` bloğu çalıştığında veriler ekrana basılacaktır.

### 📄 react-pdf & Sürükle-Bırak (Drag & Drop)
- **Nasıl Kullanılacak?** Yüklenen PDF dosyaları çok büyükse cihazı dondurmamak için `react-pdf` kütüphanesi ile "Sayfalama (Pagination)" mantığı uygulanır. Sadece aktif olan sayfa render edilir, "Sonraki Sayfa" butonuna basıldıkça diğer sayfalar çizdirilir.

---

## 5. Kullanıcı Deneyimi (UX) ve Güvenlik

### 🔔 react-hot-toast
- **Nedir?** Ekranda beliren şık ve modern bildirim (toast) mesajları kütüphanesidir.
- **Nasıl Kullanılacak?** Standart `alert()` fonksiyonu yerine; başarılı işlemler (`toast.success('Sınav oluşturuldu')`) veya hatalar (`toast.error('Şifre yanlış')`) için ekranın köşesinden animasyonlu düşen ve 3 saniye sonra kaybolan bildirimler olarak kullanılır.

### 🔐 Güvenlik ve JWT (JSON Web Token)
- **Nasıl Kullanılacak?** Backend'den dönen JWT kesinlikle güvenilmeyen yerlerde düz metin olarak tutulmaz. XSS (Cross-Site Scripting) saldırılarını önlemek için en güvenli yöntem olan **HTTP-Only Cookie** kullanılmalıdır. Eğer LocalStorage kullanılacaksa, Token ömrü kısa tutulmalı ve Axios ile her istekte yenilenmelidir.

---

## 🛠 Kısıtlar ve Çözüm Stratejileri

| Zorluk / Kısıt | Frontend Çözüm Stratejisi |
| :--- | :--- |
| **AI API Gecikmesi (Latency)** | Butonlar pasife alınır (disabled), Tailwind ile hareketli Skeleton Loader animasyonları gösterilir. |
| **Büyük PDF'lerin Performansı** | `react-pdf` ile tüm döküman DOM'a basılmaz. Sadece o an bakılan sayfa (Virtual DOM üzerinde) render edilir. |
| **İnteraktif Sınav Ekranı** | Backend'den gelen JSON dizisi `map()` ile dönülür. Her şık tıklandığında Zustand'a kaydedilir. `setInterval` ile süre geriye saydırılır. |