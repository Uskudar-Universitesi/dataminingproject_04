# dataminingproject_04 · Proje Grubu 04

**BIL545 · Veri Madenciliğinde İleri Konular · 2026–2027 Güz**

**Konu:** 9.19 İstanbul'da Airbnb ilanları  
**Soru (tek cümle):** Bir ilanın yapısal, konum ve ev sahibi özelliklerinden fiyat dilimi (ucuz / orta / pahalı) ne kadar doğru tahmin edilebilir ve bunu hangi özellikler belirler?  
**Hedef:** Fiyat dilimi (`price` sütunundan türetilen sınıflar)  
**Grup tipi:** 5 kişi (temel kapsam + Ek paket 1 + Ek paket 2)

## Grup
| | Ad Soyad | Öğrenci No |
|---|---|---|
| Üye 1 | Nurseda Akgül | 244329048 |
| Üye 2 | Kübra Girit Kopuz | 244329057 |
| Üye 3 | Barış Refik Aykent | 244329058 |
| Üye 4 | Elif Öztürk | 244329059 |
| Üye 5 | Alicem Elverdi | 244329066 |

**Grup adı:** Grup04  
**İletişim sorumlusu:** Barış Refik Aykent

## Veri kümesi ve lisans
| Alan | Bilgi |
|---|---|
| Kaynak | [Inside Airbnb](http://insideairbnb.com/get-the-data/): İstanbul, Marmara, Türkiye |
| Dosya | `listings.csv.gz` (detaylı ilan dosyası, 90 sütun, 27.232 ilan) |
| Anlık görüntü tarihi | `2026-09-25` (dönem boyunca sabit) |
| Lisans | CC BY 4.0 |

## Verinin indirilmesi
Veri depoya konmaz. Defterin "Veriyi yükleme" hücresi dosyayı şu adresten indirir:

```
https://data.insideairbnb.com/turkey/marmara/istanbul/<SNAPSHOT>/data/listings.csv.gz
```

## Defterlerin çalıştırılması
1. `BIL545_Proje1_Birlesik.ipynb` dosyasını Colab'da açın: [Colab'da aç](https://colab.research.google.com/github/Uskudar-Universitesi/dataminingproject_04/blob/main/BIL545_Proje1_Birlesik.ipynb)
2. **Çalışma Zamanı → Tümünü çalıştır** (yaklaşık 45 dakika). Sürümler ilk hücrede yazdırılır, tohum `SEED = 42`.
3. Defterdeki sonuçlar Colab'ın scikit-learn 1.6.1 ve pandas 2.2.3 sürümleriyle üretildi. Bölüm 3'te mlxtend 0.23.4'e sabitlendi; daha yeni mlxtend scikit-learn'ü yükseltip sonuçları değiştirebilir.
4. Proje 2, Proje 1'in çıktılarını (`p1_temiz_veri.csv`, `p1_hat.joblib`) kullanır.

| Dosya | İçerik | Teslim |
|---|---|---|
| `BIL545_Proje1_Birlesik.ipynb` | Proje 1 defteri (vize): Bölüm 1–3, Ek paket 1–2, beyanlar | 8. hafta |
| Proje 2 defteri | Proje 2 (final) | 14. hafta |

## Sonuç özeti
İstanbul'daki 24.553 Airbnb ilanını fiyatına göre üç dilime ayırdık (ucuz < 2.750 TL, orta, pahalı ≥ 5.100 TL) ve ilanın özelliklerinden hangi dilimde olduğunu tahmin etmeye çalıştık. Kısaca öğrendiklerimiz:

| | Ne bulduk? |
|---|---|
| **En iyi skor** | Rastgele orman makro F1'de **0,663**, yani ilanların 3'te 2'sinin doğru dilimde olduğunu ortaya koyuyor. Rastgele tahmin 0,334'te kalıyor. Gradyan artırma biraz daha yüksek çıktı (0,667), ama gradyan artırma ile arasındaki fark anlamlı olmadığı için rastgele ormanı tercih ettik. |
| **En zor dilim** | Orta dilim. Hem ucuzla hem pahalıyla sınırı olduğu için model en çok burada karışıyor. Ucuzu pahalı sanma gibi büyük hatalar ise az. |
| **Fiyatı en çok belirleyenler** | En az kaç gece kalınması gerektiği (aylık kiralanan evler gecelik hesapta çok daha ucuz görünüyor), evin büyüklüğü (kapasite, yatak odası), lisans durumu ve konum. |
| **En büyük tuzak** | Aynı ev sahibinin birbirine çok benzeyen ilanları var. Bunlar hem eğitime hem teste düşerse model ezberleyip skoru yaklaşık **8 puan** şişiriyor. Bu yüzden bütün deneylerde veriyi ev sahibine göre böldük. |
| **İstatistik testler** | İlanları birbirinden bağımsız sayan testler olmayan bir farkı "anlamlı" gösterdi. Ev sahibini hesaba katan testlerde bu fark kayboldu. |
| **Birliktelik kuralları** | Örneğin "en az 100 gece → ucuz" ve "kısa kalış + 3 ve daha fazla yatak odası → pahalı" gibi kurallar, modelin önemli bulduğu özelliklerle aynı hikâyeyi anlatıyor. Weka aynı kuralları buldu. |
| **Ek paket 1** | Budanmış küçük bir ağaç (36 yaprak), 4.000'den fazla yapraklı budanmamış ağaçtan daha iyi. Ormanın kendi OOB skoru gerçek test skorundan çok daha iyimser (0,754'e karşı 0,659). Hiçbir anlamı olmayan rastgele bir sütun bile Gini öneminde ortalarda çıkabiliyor. |
| **Ek paket 2** | Öznitelik seçimi ormanı iyileştirmedi. En iyi durumda (ormanın kendi önemiyle 30 sütun) skor aynı kaldı, diğer yöntemlerde düştü. |

**Sınırlar:** Elimizdeki fiyat, ev sahibinin istediği fiyat; gerçekten kiralanan fiyatı ya da doluluğu bilmiyoruz. Veri tek bir günün (25 Eylül 2026) fotoğrafı. Bulduklarımız ilişki, neden-sonuç değil: büyük bir ev açmak fiyatı yükseltmiyor, büyük evler zaten pahalı.

Ayrıntılar, grafikler ve bütün sayılar `BIL545_Proje1_Birlesik.ipynb` defterinde.

## Katkı beyanı
_Ayrıntısı defterin sonundaki "Katkı beyanı" bölümündedir._
