# hse23_hw1
#### 1. Создание ссылок в папке
```bash
ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq
```
#### 2. Выбор случайных чтений
```bash
SEED = 915
seqtk sample -s$SEED oil_R1.fastq 5000000 > sub1.fastq
seqtk sample -s$SEED oil_R2.fastq 5000000 > sub2.fastq
seqtk sample -s$SEED oilMP_S4_L001_R1_001.fastq 1500000 > matep1.fastq
seqtk sample -s$SEED oilMP_S4_L001_R2_001.fastq 1500000 > matep2.fastq
```
#### 3. Оценим чтения, используя fastqc, и поместим файлы в папку fastQ
```bash
mkdir fastQ
fastqc oil_R1.fastq -o fastQ
fastqc oil_R2.fastq -o fastQ
fastqc oilMP_S4_L001_R1_001.fastq -o fastQ
fastqc oilMP_S4_L001_R2_001.fastq -o fastQ
```
#### 4. Получим статистику с помощью multiqc и поместим их в папку multiqc
```bash
mkdir multiqc
multiqc -o multiqc fastQ
```
#### 5. Подрежем чтения
```bash
platanus_trim sub1.fastq
platanus_trim sub2.fastq
platanus_internal_trim matep1.fastq
platanus_internal_trim matep2.fastq
```

#### 6. Удалим файлы
```bash
rm oil_R1.fastq
rm oil_R2.fastq
rm oilMP_S4_L001_R1_001.fastq
rm oilMP_S4_L001_R2_001.fastq
```

#### 7. Оценим подрезанные чтения, используя fastqc, и поместим файлы в папку fatstq_trimmed
```bash
mkdir fatstq_trimmed
fastqc sub1.fastq.trimmed -o fatstq_trimmed
fastqc sub2.fastq.trimmed -o fatstq_trimmed
fastqc matep1.fastq.int_trimmed -o fatstq_trimmed
fastqc matep2.fastq.int_trimmed -o fatstq_trimmed
```
#### 8. Создадим отчет по подрезанным чтениям с помощью multiqс в папке multiq_trimmed
```bash
mkdir multiq_trimmed
multiqc -o multiq_trimmed fastq_trimmed
```
#### 9. Сборка контигов из подрезанных чтений, используя “platanus assemble”
Чтобы не видеть в консоле большой вывод, сделаем вывод в assemble.log
```bash
platanus assemble -f sub1.fastq.trimmed sub2.fastq.trimmed 2>assemble.log
```
#### 10. Сборка скаффолдов из подрезанных чтений и контигов, используя “platanus scaffold”
Чтобы не видеть в консоле большой вывод, сделаем вывод в scaffold.log
```bash
platanus scaffold -c out_contig.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 matep1.fastq.int_trimmed matep2.fastq.int_trimmed 2> scaffold.log
```
#### 11. Уменньшим кол-во гэпов, используя “platanus gap_close”
Чтобы не видеть в консоле большой вывод, сделаем вывод в gap_close.log
```bash
platanus gap_close -c out_contig.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 matep1.fastq.int_trimmed matep2.fastq.int_trimmed 2> gap_close.log
```

#### 12. Удалим подрезанные файлы
```bash
rm sub1.fastq.trimmed
rm sub2.fastq.trimmed
rm matep1.fastq.int_trimmed 
rm matep2.fastq.int_trimmed
```
## Отчёты multiQC
#### Для исходных чтений
![](https://github.com/prayforanya/hse23_hw1/blob/main/images/general_statistics.png)
![](https://github.com/prayforanya/hse23_hw1/blob/main/images/per_sequence_quality_score.png)
![](https://github.com/prayforanya/hse23_hw1/blob/main/images/adapter_content.png)

#### Для подрезанных чтений
![](https://github.com/prayforanya/hse23_hw1/tree/blob/images/general_statistics_trimmed.png)
![](https://github.com/prayforanya/hse23_hw1/tree/blob/images/per_sequence_quality_score_trimmed.png)
![](https://github.com/prayforanya/hse23_hw1/tree/blob/images/adapter_content_trimmed.png)
