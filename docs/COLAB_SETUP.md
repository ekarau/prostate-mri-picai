# PI-CAI Mülakat Projesi — Colab + Drive Kurulum Rehberi

## 🎯 Hedef
Hocanın istediği 5 çıktıyı 1 hafta içinde hazırlamak:
1. PI-CAI dataset indirme
2. Baseline algoritmaları çalıştırma (re-implement / use)
3. Data + ground truth görselleri (slaytlar için)
4. Prostat anatomik bölge segmentasyon performansı
5. Lezyon segmentasyon performansı (PI-CAI baseline)

---

## 📁 Drive Klasör Yapısı

Google Drive'ında **`MyDrive/Prostate_MRI_Project/`** altında şu yapıyı oluştur:

```
MyDrive/Prostate_MRI_Project/
├── code/                         ← bu lokal klasörün içeriği yüklenecek
│   ├── picai_baseline-main/
│   ├── picai_labels-main/
│   ├── prostate_mri_anatomy/
│   └── TCIA_PROSTATEx_*.ipynb
│
├── input/                        ← Colab'dan indirilecek (boş başla)
│   ├── images/                   ← PI-CAI fold 0 unzip edilecek
│   └── picai_labels/             ← picai_labels-main buraya kopyalanacak
│
├── workdir/                      ← preprocess çıktıları
│   ├── nnUNet_raw_data/
│   ├── nnUNet_preprocessed/
│   └── results/
│
├── output/                       ← model çıktıları, görseller, metrikler
│   ├── anatomy_predictions/
│   ├── lesion_predictions/
│   ├── figures/                  ← sunum için PNG'ler
│   └── metrics/
│
└── notebooks/                    ← Colab notebook'ları
    ├── step1_setup.ipynb
    ├── step2_download_data.ipynb
    ├── step3_anatomy_inference.ipynb
    ├── step4_lesion_inference.ipynb
    └── step5_visualizations.ipynb
```

---

## 📤 1. Adım — Yerel Klasörü Drive'a Yükle

**Bu klasörü** (`C:\Users\karau\Desktop\Genel\Prostate_MRI_Project\`) zip'le ve Drive'a `MyDrive/Prostate_MRI_Project/code/` altına yükle.

### PowerShell ile zip oluşturma:
```powershell
cd C:\Users\karau\Desktop\Genel
Compress-Archive -Path "Prostate_MRI_Project\*" -DestinationPath "Prostate_MRI_Project_code.zip" -Force
```

Bu `~410 MB` bir zip dosyası üretir. Drive'a yükle, sonra Colab'da unzip ederiz.

**Alternatif:** Klasörü olduğu gibi Drive sync ile yükleyebilirsin (Backup and Sync veya web yükleme).

---

## ☁️ 2. Adım — Colab Notebook'larını Sırayla Çalıştır

| Notebook | Süre | Ne yapar |
|----------|------|----------|
| `step1_setup.ipynb` | ~10 dk | Drive mount, kütüphaneleri kur, GPU check, code zip'i aç |
| `step2_download_data.ipynb` | ~2-4 saat | PI-CAI fold 0'ı Zenodo'dan Drive'a indir + unzip |
| `step3_anatomy_inference.ipynb` | ~30 dk | Anatomi bundle ile zonal segmentation inference + Dice |
| `step4_lesion_inference.ipynb` | ~1-2 saat | PI-CAI U-Net pretrained inference + metrikler |
| `step5_visualizations.ipynb` | ~1 saat | Slaytlar için PNG'ler |

---

## ⚠️ Önemli Uyarılar

1. **Colab Free** session ~12 saat sonra disconnect olur, idle 90 dk sonra düşer.
   - Veri indirme/preprocess uzun süreceğinden Colab Pro düşünebilirsin (aylık ~$10).
   - Free yine de çalışır, sadece bölerek çalıştırmak gerekir.

2. **GPU runtime seç:** Runtime → Change runtime type → GPU (T4 veya P100)

3. **Drive I/O yavaştır:** Preprocess edilmiş dosyaları Colab local disk'e cache'leyeceğiz, sonra Drive'a tek seferde geri yazacağız.

4. **PI-CAI fold 0** zaten ~14 GB. Hepsi gerek yok — demo için **5-10 case** yeter.
   - Alternatif: Sadece birkaç dosyayı seçici indir (Zenodo API listesi)

---

## 📊 Mülakat Sunum İskeleti (15 slayt)

Bölüm 2 (15 dk PI-CAI sunumu) için slayt planı:

1. **Title** — Proje adı, kendi adın, üniversite
2. **Problem** — Klinik anlamlı prostat kanseri (csPCa), neden mpMRI?
3. **PI-CAI Challenge** — Grand Challenge, 1500 case, 3T MR, çoklu merkez
4. **Dataset** — T2W + DWI + ADC + sBVAL örnekleri (görsel)
5. **Annotations** — Whole gland, zonal (PZ/TZ), csPCa lezyon — ground truth örnekleri
6. **Pipeline overview** — preprocess → train/infer → eval
7. **Anatomical model** — MONAI bundle (Adams & Bressem 2022), U-Net + focal soft-Dice
8. **Anatomical results** — Dice tablosu + overlay görseli
9. **Lesion model** — PI-CAI baseline U-Net architecture
10. **Lesion results** — AP, AUROC, lesion overlay görseli
11. **Failure cases** — Modelin başarısız olduğu örnek(ler)
12. **Limitations** — 4 GB VRAM, fold 0 only, vs.
13. **Future work** — nnU-Net full training, Transformer-based, multi-modal fusion
14. **Conclusion** — Ana bulgular, programdan beklentilerle bağlantı
15. **Questions** — Q&A slidesı

---

## 🆘 Hata Olursa

- **Colab disconnect**: Drive bağlı, kaldığın yerden devam (notebook'ları kısa tut)
- **Out of memory**: Batch size → 1, image size küçült
- **Disk dolarsa**: `!df -h` ile kontrol, gereksiz cache temizle
- **MONAI uyumsuzluğu**: `pip install monai==1.3.0` gibi sürüm sabitle
