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
| `privacy/db-meter.html` | DB Meter gizlilik politikası (Türkçe) |
| `privacy/db-meter.en.html` | DB Meter gizlilik politikası (İngilizce) |

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
