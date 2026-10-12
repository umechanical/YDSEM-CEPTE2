# YDSEM CEPTE

Mobil öncelikli, çevrimdışı kullanılabilen kişisel YDS kelime uygulaması. İlk veri paketi, gönderilen **1. gün / Adjective Set 1** fotoğraflarındaki sıfatlara dayanır. 8 set, 40 açıklamalı soru, 5 mini oyun, tekrar ve ilerleme ekranları içerir.

## Çalıştırma

Bilgisayarda `index.html` dosyasına çift tıklayarak Chrome veya Edge ile açabilirsiniz. Tek dosyalık sürümün HTML, CSS ve JavaScript'i bu dosyaya gömülüdür. Kaynak dosyaları değiştirdikten sonra `npm run build` ile `index.html` dosyasını yeniden üretin. Alternatif olarak proje klasöründe `python -m http.server 4173` çalıştırıp `http://localhost:4173` adresini açabilirsiniz. `npm test` testleri çalıştırır. `file://` açılışında çevrimdışı önbellek kurulmaz; dosya zaten yerel olduğu için bilgisayarda internet olmadan açılabilir.

## iPhone / iPad

Projeyi HTTPS ile yayınlayın, Safari'de açın, Paylaş → **Ana Ekrana Ekle** seçin. Service worker, ilk çevrimiçi yüklemeden sonra uygulama dosyalarını önbelleğe alır. `localhost` bilgisayarda çalışırken telefondan erişim için aynı ağda bilgisayarın IP adresi kullanılabilir; iOS'ta service worker için HTTPS gereklidir. Bu paket yerel prototip ve statik dağıtım için hazırdır; App Store paketi değildir.

## Veri ve yedek

Kelime içeriği `data/words.js`, onaylı soru bankası `data/questions.js` dosyasındadır. İlerleme tarayıcının localStorage alanında `ydsem-progress-v1` anahtarıyla tutulur. Profil → **Yedeği indir** ile JSON yedek alın, **Yedek yükle** ile geri yükleyin. Tarayıcı verisi silinirse ve yedek yoksa ilerleme kaybolur. Farklı cihazlar arasında otomatik eşitleme yoktur.

## Haftalık içerik güncelleme

1. Yeni fotoğraflardaki kelimeleri ve yıldızları insan gözüyle doğrulayın. Emin olunmayan yıldızları öncelikli işaretlemeyin.
2. `data/words.js` içine benzersiz `id` ile yeni sözcük, tanım, IPA, üç örnek, çeviri ve set bilgilerini ekleyin. Mevcut ID'leri değiştirmeyin.
3. `data/questions.js` içine yeni set başına en az beş **elle gözden geçirilmiş** soru ekleyin. Yanlış seçenekleri aynı anlam grubundan seçmeyin ve her şık için açıklama verin.
4. `service-worker.js` içindeki önbellek sürümünü artırın. `npm test` çalıştırın. Mevcut ilerleme kelime ID'sine bağlı olduğundan yeni içerik eklenince korunur.

## Algoritma

Tekrar basamakları 1, 3, 7, 14 ve 30 gündür. Yanlış yanıt aynı güne döner, art arda doğrular aralığı uzatır; yıldızlı kelimelerin aralığı biraz kısalır. Yıldızlı kelimeler tekrar kuyruğunda önce gelir, diğerleri de kuyrukta kalır. Soru ve kelime başına günlük XP sınırı vardır. Set geçiş eşiği %80'dir.

## İçerik sınırı ve doğrulama

İlk sürümde kitap fotoğrafındaki sıfatlar işlenmiştir. El yazısındaki ek fiiller ve edatlar profil sayfasında not olarak yer alır; doğrulanmış soru havuzuna henüz eklenmemiştir. Fotoğraftaki silik yıldız işaretleri kullanıcı onayı olmadan işlenmez. Kelime kökeni güvenilir biçimde doğrulanmadığı için boş bırakılmıştır. Telaffuz, cihazın İngilizce konuşma motorunu kullanır; ses kalitesi cihazdan cihaza değişebilir. Gerçek iOS Safari cihaz testi bu ortamda yapılmamıştır.
