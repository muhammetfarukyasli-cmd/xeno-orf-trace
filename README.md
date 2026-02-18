# FMDV recombinant-window phylogeny + selection scans (RDP5 → IQ-TREE → iTOL → GARD → BLAST)

Bu repo, **benim kendi inisiyatifimle** seçtiğim bir hipotezi AI yardımı ile (rekombinasyon pencereleri arasında topoloji farkı var mı? / olası non-kanonik ORF sinyali var mı?) test etmek için yaptığım analiz hattının derli toplu çıktısıdır.

> Not: Buradaki dosyalar “ham veri arşivi” değil; tekrarlanabilirlik için **gerekli minimum** girdi/çıktılardır.

## Amaç
1. RDP5 ile olası rekombinasyon olayını ve kırılma bölgelerini (breakpoint) bulmak  
2. Breakpoint’e göre hizalamayı iki pencereye bölmek (**WIN1** ve **WIN2**)  
3. Her pencere için ayrı maksimum olabilirlik ağacı üretip (**IQ-TREE**) topoloji değişimini görselleştirmek (**iTOL**)  
4. Aynı hizalamada **GARD** ile bağımsız breakpoint desteği almak  
5. “Xeno / dış köken” olasılığı için kısa ORF ürününü **BLAST/BLAST+** ile taramak (şu an web BLAST tarafı CPU limitine takılabildiği için, yerel BLAST+ önerilir)

## Klasör yapısı
- `data/input/`
  - `FMDV_W_W1.fasta`, `FMDV_W_W2.fasta`: RDP5 / genel analizde kullanılan hizalamalar
  - `original.fasta`: GARD’a verilen hizalama (aynı datasetin kopyası olabilir)
- `data/windows_nt/`
  - `WIN1_nt_1-265.fas`: WIN1 nükleotid penceresi
  - `WIN2_nt_266-end.fas`: WIN2 nükleotid penceresi
- `data/windows_aa/`
  - `WIN1_frame2.faa`: WIN1 için frame2’den çevrilmiş amino asit dizileri (BLAST/ORF taramaları için)
- `results/rdp5/`
  - `RDP5_recombination_events.csv`: olay özeti
  - `RDP5_recombination_eventsRecIDTests.*`: (varsa) rekombinant ID test çıktıları
- `results/iqtree/`
  - `WIN1.treefile`, `WIN2.treefile`: iTOL’a yüklenen ağaçlar
  - `WIN*.iqtree`, `WIN*.log`: model/arama özeti
- `results/gard/`
  - `result.json`: GARD çıktı JSON’u
  - `screened_data.nex`, `log.txt`: GARD’ın ara/yardımcı çıktıları
- `docs/drafts/`
  - Analiz sırasında yazılmış notlar / taslak metinler
- `notebooks/`
  - Deney notebook’u (varsa)

## Şu ana kadar hangi **ön bulgular / destekleyici sinyaller** var?
- **WIN1 ve WIN2 ağaçlarının topolojisi aynı değil** → breakpoint çevresinde evrimsel sinyal ayrışması var. Bu durum **rekombinasyonla uyumlu olabilir**, ama tek açıklama değildir (hizalama kalitesi, hız heterojenliği/konverjans vb. de etkileyebilir).
- **AU/SH testi (IQ-TREE)**: W1 verisi altında W2 topolojisi çok güçlü biçimde dışlanıyor (ör. p-AU ≪ 0.05). Bu, “iki bölge aynı evrimsel tarihi taşımıyor olabilir” iddiasını **istatistiksel olarak destekler**.
- **GARD**: hizalamada 1 adet breakpoint için destek rapor ediyor (JSON’da `breakpointData`). Bu da “rekombinasyon / template-switching” hipotezini **mekanistik olarak güçlendiren** bir bulgu.
- Kısa bir protein (query length ~88 aa) için web BLAST araması bazen “no significant similarity” + CPU limit uyarısı verebiliyor; bu, **kesinlikle yok** demek değil. (Kısa ORF + uzak homoloji + filtreler + veritabanı sınırları.)

> Not: Bu aşama **nihai kanıt** değil; “xeno/donör izi” için sistematik BLAST/HMMER taraması + negatif kontroller + hizalama trimming + ek filogenetik testler gerekir.

## Özet
1. **RDP5**: hizalamayı aç → rekombinasyon event → breakpoint bölgesi → pencereleri dışa aktar
2. **IQ-TREE**: her pencere için ML ağaç üret
3. **iTOL**: `WIN1.treefile` ve `WIN2.treefile` yükle → rekombinant aday(lar)ı renklendir → topoloji farkını görselleştir
4. **GARD (Datamonkey)**: aynı hizalamayı yükle → breakpoint koordinatını kaydet
5. **BLAST/BLAST+**: `WIN1_frame2.faa` gibi kısa ORF ürünlerini geniş veritabanında ara

## Lisans / Atıf
- Bu repo akademik atıf yerine “analiz günlüğü” amaçlıdır.
- Kullandığım araçların (RDP5, IQ-TREE, HyPhy/Datamonkey, iTOL, NCBI BLAST) kendi atıf yönergeleri geçerlidir.
