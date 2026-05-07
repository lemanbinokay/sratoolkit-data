## FASTQ Veri İndirme (SRA Toolkit – Çalıştırma Rehberi)

Bu adımda SRA (Sequence Read Archive) üzerinden RNA-seq FASTQ dosyaları indirilecektir.

İndirilecek datasetler:
- SRR26305352  
- SRR26305353  

---

## 1. Conda environment oluştur

source /opt/conda/etc/profile.d/conda.sh  

conda create -n rnaseq_course -y  
conda activate rnaseq_course  

---

## 2. SRA Toolkit kurulumu (GÜNCEL VERSİYON)

⚠️ ÖNEMLİ: Eski versiyonlar TLS (sertifika) hatası verir.

conda install -c conda-forge -c bioconda "sra-tools>=3.1" ca-certificates openssl -y  

Kurulum kontrolü:

prefetch --version  
fasterq-dump --version  

---

## 3. Download script oluştur

mkdir -p scripts data/fastq  

cat > scripts/00_download_fastq.sh <<'EOF'
#!/bin/bash

set -e

source /opt/conda/etc/profile.d/conda.sh
conda activate rnaseq_course

mkdir -p data/fastq

SRA_IDS=("SRR26305352" "SRR26305353")

for sra in "${SRA_IDS[@]}"
do
  echo "Downloading: $sra"

  prefetch "$sra"
  fasterq-dump "$sra" --split-files --outdir data/fastq

  echo "$sra completed."
done

echo "All FASTQ downloads completed."
EOF

---

## 4. Script çalıştırma

chmod +x scripts/00_download_fastq.sh  
bash scripts/00_download_fastq.sh  

---

## 5. Beklenen çıktı

data/fastq/

İçinde:

- SRR26305352_1.fastq  
- SRR26305352_2.fastq  
- SRR26305353_1.fastq  
- SRR26305353_2.fastq  

---

## HATA DURUMU (ÇOK ÖNEMLİ)

Eğer aşağıdaki hatayı alırsanız:

TLS / certificate verification failed

şu komutu çalıştırın:

vdb-config --restore-defaults  

ve scripti tekrar çalıştırın:

bash scripts/00_download_fastq.sh  

---

## Notlar

- `prefetch` → veriyi indirir  
- `fasterq-dump` → FASTQ formatına çevirir  
- `--split-files` → paired-end veriyi ayırır  
- Büyük veri indirirken disk alanını kontrol edin  
