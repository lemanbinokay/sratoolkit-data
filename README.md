# sratoolkit-data
## FASTQ Veri İndirme (SRA Toolkit)

RNA-seq analizi için FASTQ dosyaları SRA (Sequence Read Archive) üzerinden indirilebilir.

---

### SRA Toolkit Kurulumu

conda install -c bioconda sra-tools -y

---

### FASTQ İndirme

Örnek bir SRA accession (SRR) kullanarak veri indirme:

prefetch SRRXXXXX

Bu komut veriyi indirir.

Ardından FASTQ formatına çevir:

fasterq-dump SRR26305352

Paired-end veri için:

fasterq-dump SRR26305352 --split-files

---

### Dosyaları data klasörüne taşı

mv SRR26305352*.fastq data/

---

### ÖNEMLİ

- `prefetch` → veriyi indirir  
- `fasterq-dump` → FASTQ üretir  
- Paired-end için mutlaka `--split-files` kullan  
- Büyük datasetlerde disk alanı yeterli olmalıdır  

---

### Örnek

prefetch SRR26305352  
fasterq-dump SRR26305352 --split-files  
mv SRR26305352*.fastq data/
