# Aula 1: Formato de Arquivos e Controle de Qualidade

Nesta aula são discutidoos alguns dos principais formatos de arquivo produzidos por plataformas de sequenciamento
de nova geração (NGS), bem como por pipelines de análise de dados, e também como é realizado o processo de controle
de qualidade dos dados brutos em formato `.fastq`.


[Slides da aula](https://docs.google.com/presentation/d/1rQpKpfjVakIVSuomY33r-7HTY45lbMx2Yl6Uo88YpVQ/edit?usp=sharing)


## Instalando dependências

Para este tutorial é recomendado uso de um computador com pelo menos 8 Gb de RAM e sistema operacional baseado em Linux,
sendo recomendado o Ubuntu Linux. Também utilizaremos as seguintes ferramentas:

- SRA-Toolkit: pacote de ferramentas para realizar o download de dados de sequenciamento do NCBI SRA.
- FastQC: ferramenta para análise da qualidade de dados em formato `.fastq`.
- Trimmomatic: ferramenta para realizar **trimming**, **filtering** e outros processos de processamento de leituras.

Para instalar os softwares que serão utilizados, no Ubuntu é possível utilizar oo seguinte comando:

```
$ sudo apt install sra-toolkit fastqc trimmomatic
```

## Download dos dados

Para este tutorial utilizaremos dois **datasets** (SRA:DRR084179, ERR2730419) com dados produzidos pela plataforma de sequenciamento Illumina MiSeq para o organismo **Escherichia coli**.

```
$ mkdir -p data/raw
$ cd data/raw/
$ fastq-dump DRR084179 --split-3
$ fastq-dump ERR2730419 --split-3
$ cd ../../
```

## Passo 2: análise de qualidade com o fastqc

### Executar a aplicação com GUI

```
$ fastqc 
```

### Executar a aplicação com CLI

```
$ mkdir -p data/qc/DRR084179
$ fastqc -o data/qc/DRR084179 --extract data/raw/DRR084179\_*
```

## Passo 3: realizar o processamento dos dados com o Trimmomatic

```
$ mkdir -p data/processed
$ TrimmomaticPE  data/raw/DRR084179_1.fastq data/raw/DRR084179_2.fastq \
	-baseout data/processed/DRR084179.fastq \
	MINLEN:100 SLIDINGWINDOW:5:25 MINLEN:150 HEADCROP:10
```
