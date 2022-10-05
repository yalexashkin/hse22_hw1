# hse22_hw1
### Отбор случайных чтений
```
seqtk sample -s126 /usr/share/data-minor-bioinf/assembly/oil_R1.fastq 5000000 > R1.fastq
seqtk sample -s126 /usr/share/data-minor-bioinf/assembly/oil_R2.fastq 5000000 > R2.fastq
seqtk sample -s126 /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq 1500000 > MPR1.fastq
seqtk sample -s126 /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq 1500000 > MPR2.fastq
```
### Оценка качества чтения с помощью fastQC и multiQC
```
mkdir fastqc
ls *.fastq | xargs -P 4 -tI{} fastqc -o fastqc {}
mkdir multiqc
multiqc -o multiqc fastqc
```
### Отчет multiQC
![image](https://user-images.githubusercontent.com/88088621/194038936-4fb04955-80b9-4fd5-9429-f52940584855.png)
![image](https://user-images.githubusercontent.com/88088621/194039040-1f5d7851-4331-4302-8a2d-c7c9b213210d.png)
![image](https://user-images.githubusercontent.com/88088621/194039093-3cb302cb-4b81-493c-b1ec-31dd589a71ec.png)
![image](https://user-images.githubusercontent.com/88088621/194039161-1836f5f2-28a3-4bb6-a9eb-c1d859df9dda.png)
![image](https://user-images.githubusercontent.com/88088621/194039246-6a611f05-6c71-44cb-b975-518bec7e1ba7.png)
### Подрезание чтений с помощью platanus_trim и platanus_internal_trim
```
platanus_trim R1.fastq R2.fastq
platanus_internal_trim MPR1.fastq MPR2.fastq
```
### Оценка качества чтения с помощью fastQC и multiQC
```
mkdir trim
ls *trimmed | xargs -tI{} fastqc -o trim {}
mkdir multiqc2
multiqc -o multiqc2 trim
```
### Отчет multiQC
![image](https://user-images.githubusercontent.com/88088621/194040553-e98e51c8-80b9-4091-aba5-72c0820ece52.png)
![image](https://user-images.githubusercontent.com/88088621/194040587-5ed62fb4-8aee-48d1-8f7b-580e2543181d.png)
![image](https://user-images.githubusercontent.com/88088621/194040647-319aca9c-d232-4931-897c-f8dae3f58939.png)
![image](https://user-images.githubusercontent.com/88088621/194040710-5526dd4d-d6ea-4e7a-a9c4-3adc80424d1b.png)
![image](https://user-images.githubusercontent.com/88088621/194040796-f68c6ef4-8d37-4144-9e0e-c6a71add8486.png)
Уменьшились количество и средняя длина чтений; улучшилось качество.
### Сборка контигов из подрезанных чтений с помощью platanus assemble
```
platanus assemble -o Poil -t 8 -n 20 -f R1.fastq.trimmed R2.fastq.trimmed 2> Final1.log
```
### Сборка скаффолдов
```
platanus scaffold -o Poil -t 1 -c Poil_contig.fa -IP1 *.trimmed -OP2 *.int_trimmed 2> Final_scaffold.log
```
### Уменьшение количества гэпов
```
platanus gap_close -o Poil -t 1 -c Poil_scaffold.fa -IP1 *.trimmed -OP2 *.int_trimmed 2> gapclose.log
```
### Анализ в collab
https://colab.research.google.com/drive/1EuBIprNjM8t9GSJKYIKVu7NRcxuoOPAs?usp=sharing




