# kavik-public

[Kavik](https://play.google.com/store/apps/dev) uygulamaları için **statik yapılandırma
dosyaları ve gizlilik politikaları**. GitHub Pages üzerinden yayınlanır.

Bu repo kaynak kod içermez. Yalnızca uygulamaların çalışma zamanında okuduğu ve Google
Play'in zorunlu tuttuğu, herkese açık olması gereken dosyaları barındırır.

> **Neden ayrı ve public bir repo:** GitHub Pages ücretsiz katmanda yalnızca herkese açık
> repolarda çalışır. Uygulamaların kaynak kodu ayrı ve private bir repoda durur; yalnızca
> aşağıdaki dosyaların halka açık olması gerektiği için bu repo ondan ayrıldı.

## İçerik

| Yol | Ne işe yarar |
|---|---|
| `config/dbmeter.json` | DB Meter uzaktan yapılandırması — desteklenen en düşük sürüm ve özellik anahtarları |
| `config/roadwatch.json` | Rotaİkaz uzaktan yapılandırması |
| `roadwatch/*.json` | Rotaİkaz veri kümesi — hız koridorları ve sabit kamera listeleri |
| `roadwatch/yol-limitleri/` | Yol hız limiti **hücreleri** (0,25° ızgara) + `index.json`; uygulama yalnız bulunduğu bölgenin hücrelerini indirir |
| `privacy/db-meter.html` | DB Meter gizlilik politikası (Türkçe) |
| `privacy/db-meter.en.html` | DB Meter gizlilik politikası (İngilizce) |

## Rotaİkaz veri kümesi

`roadwatch/` altındaki dosyalar bir veri hattı tarafından üretilir; elle düzenlenmez.
Kaynaklar: T.C. İçişleri Bakanlığı güzergâh denetim verisi, EGM sabit kamera listesi ve
OpenStreetMap katkıları (ODbL — OSM kaynaklı dosyalar ayrı tutulur, tek kayıtta
birleştirilmez).

Yol limitleri tek bir ülke dosyası değil **hücrelere** bölünmüştür: uygulama konumunun
çevresindeki birkaç hücreyi indirir, böylece hem indirme hem de cihaz belleği bulunulan
bölge kadar kalır. `yol-limitleri/index.json` hangi hücrenin var olduğunu ve kaç kayıt
taşıdığını listeler.

## Uzaktan yapılandırma

Her uygulama açılışta kendi yapılandırma dosyasını okur ve sürümünü
`min_supported_version` ile karşılaştırır. Eskiyse kullanıcıyı güncellemeye yönlendirir.

```json
{
  "min_supported_version": "1.0.0",
  "ads_enabled": false,
  "premium_enabled": false,
  "notice_message": null
}
```

| Anahtar | Tip | Anlamı |
|---|---|---|
| `min_supported_version` | metin | Bundan eski sürümler güncelleme ekranı görür |
| `ads_enabled` | boolean | Reklam anahtarı |
| `premium_enabled` | boolean | Uygulama içi satın alma anahtarı |
| `notice_message` | metin veya `null` | Kullanıcıya gösterilecek duyuru |

**Hata durumu fail-open'dır:** dosyaya ulaşılamazsa uygulama son bilinen değeri kullanır,
o da yoksa kullanıcıyı normal akışa geçirir. Ağ hatası yüzünden kimse kendi uygulamasından
kilitlenmez.

> **Dikkat:** Bu dosyalar canlı uygulamalar tarafından okunur. `min_supported_version`
> değerini yükseltmek, o sürümden eski tüm kullanıcılara güncelleme ekranı gösterir.
> Değiştirmeden önce iki kez düşün.

### Yayılma süresi

GitHub Pages bu dosyaları `Cache-Control: max-age=600` ile sunar — yani bir değişiklik
cihazlara **10 dakikaya kadar** gecikmeyle ulaşır (commit'in Pages tarafından derlenmesi de
buna eklenir).

Pratik sonucu: burası anlık bir acil durum düğmesi **değildir**. Yanlışlıkla konulan bir
güncelleme duvarını kaldırmak da aynı gecikmeye tabidir. Bu yüzden `min_supported_version`
değişiklikleri aceleyle değil, düşünülerek yapılır.
