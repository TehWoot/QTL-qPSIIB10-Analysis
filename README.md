# Introduction

A variant analysis was conducted on the Arahy20 region. This region has been shown as a locus possibly conferring aflatoxin resistance in peanut and is present across multiple environments. In the present study, susceptible and resistant varieties, Florida-07 and ICG 1471, were mapped to cultivated peanut, Tifrunner, to identify variants and candidate genes possibly contributing to aflatoxin resistance.

# Available Data

The data available in this repository is listed as follows:

"Flor07_chr20_snp_table.txt" & "ICG1471_chr20_snp_table.txt"
> Variants called using the SnpEff annotation tool on chromosome 20.

"updatedduip.fa.txt2.fa_mq10.sam.txt3.bed"
> Differentially expressed genes in response to *Aspergillus flavus* infection mapped by Walid Korani.

The data available in [this Google Drive](https://drive.google.com/drive/folders/1KtHvaQtU1dmY667oeUHx70eUEflsZOD3?usp=sharing) is listed as follows.

"Florida07.fa" & "ICG1471.fa"
> HiFi sequences of Florida-07 and ICG1471

"Flor07align_srt.bam" & "ICG1471align_srt.bam"
> Aligned sequences of Florida-07 and ICG1471 to Tifrunner.

"Flor07calls.vcf" & "ICG1471calls.vcf"
> Variant calls of Florida-07 and ICG1471.

"Flor07calls.bcf" & ICG1471calls.bcf"
> Variant calls of Florida-07 and ICG1471.





# Mapping and Variant Calling

The HiFi sequences of Florida-07 and ICG1471 were mapped to TifrunnerV2 using minimap2 2.26. Variants were called using samtools 1.16.1 and bcftools 1.18.

```
minimap2 -a arahy.Tifrunner.gnm2.J5K5.genome_main.fna ICG1471_HIFI.fastq > ICG1471align.sam

samtools view -bS -@ 8 ICG1471align.sam > ICG1471align.bam
samtools sort -@ 8 ICG1471align.bam -o ICG1471align_srt.bam
samtools index -c -@ 8 ICG1471align_srt.bam
```

```
bcftools mpileup -Ou -f arahy.Tifrunner.gnm2.J5K5.genome_main.fna ICG1471align_srt.bam | bcftools call -mv -Ob -o ICG1471calls.bcf

bcftools convert -Oz -o ICG1471calls.vcf ICG1471calls.bcf
```

```
# Filtering Variants
bcftools filter -i 'QUAL>=30' ICG1471calls.vcf | grep -v -c '^#'
```
Variants were viewed using IGV 2.18.4.

![image](https://github.com/user-attachments/assets/3a00b1ef-8f8e-4b97-b179-fbbb41e0d092)


# SnpEff Analysis

A SnpEff analysis was conducted on the mapped variants from Florida-07 and ICG 1471 on chromosome 20 using SnpEff 5.2a. SnpEff is a genetic variant annotation and functional effect prediction toolbox (Cingolani et al., 2012). Both the Florida-07 and ICG1471 cultivars were mapped and had variants called to allow for the detection of SNPs in this analysis. Building a SnpEff database and filtering was done according to [this tutorial](https://www.youtube.com/watch?v=-rmreyRAbkE&ab_channel=RodrigoBaptista).
```
# SnpEff Annotation with Filtered .vcfs

java -jar /snpeff_2/snpEff.jar ara_hypo vcfs/ICG1471calls_chr20_filtered.vcf > ICG1471calls_chr20_filtered.ann.vcf
```

```
# Filtering Annotated SnpEff.vcf

more ICG1471calls_chr20_filtered.ann.vcf | grep -v "#" | grep "PASS" | cut -f 1,2,8 | sed 's/|/\t/g' | cut -f 1,2,4,5,7 | grep -v "intergenic\|stream\|UTR" > ICG1471_snp_table
```
## ICG 1471 SnpEff Table

![Screenshot 2025-06-24 at 3 12 14 PM](https://github.com/user-attachments/assets/bafdb6e9-57e2-4259-9cb3-95d0f7a5c0ef)


![Screenshot 2025-06-24 at 3 14 05 PM](https://github.com/user-attachments/assets/25ea2c29-a6ab-4d66-a5aa-6f5e856ab630)

## Florida-07 SnpEff Table

![image](https://github.com/user-attachments/assets/b87ec091-7d02-4689-b0bc-103cc3edfbdb)


![Screenshot 2025-06-24 at 3 16 15 PM](https://github.com/user-attachments/assets/5f711516-d902-4e96-85ce-cf9c3b0cf7e4)


From the SnpEff analysis, 43,604 variants were identified from Florida-07 with a variant rate of 1 every 3,326 bases. From ICG 1471, 170,866 variants were identified with a variant rate of 1 every 848 bases, almost quadruple that of Florida-07. This supports the work done by [Chu et al (2018)](https://doi.org/10.3146/ps17-17.1) that Florida-07 is closely related to the currently cultivated peanut, Tifrunner. Variants located in intergenic regions, upstream and downstream, or untranslated regions were filtered out. This was done to focus on functional variants within genes. There were a total of 5,495 filtered variants for ICG 1471 and 1,803 for Florida-07. Filtered variants identified on Arahy20, along with the position, type of variant, functional class, and impact, are located in the repository. Candidate genes of resistance were also identified based on SnpEff results and previously published literature.

# Differentially Expressed Genes Mapped to Tifrunner

Korani et al. (2018) identified differentially expressed genes from the susceptible and resistant peanut cultivars Florida-07 and ICG1471. These DE genes were mapped to TifrunnerV2. The mapped DE genes were provided by Walid Korani (Clevenger Lab, HudsonAlpha Institute for Biotechnology). The differentially expressed genes were viewed in IGV 2.18.4.

<img width="1920" alt="DEGenesOutsideqPSBIIB10" src="https://github.com/user-attachments/assets/e487eb77-310b-4878-8db0-4fc53c329208" />

# Candidate Gene(s) for Aflatoxin Resistance

Utilizing the data gathered from this analysis, candidate genes were identified. It's hoped these markers and candidate genes can assist breeders in developing aflatoxin-resistant peanut cultivars.

## Ethylene insensitive 3 (*EIN3*) family protein, Ah20g165800

EIN3 plays a pivotal role in coordinating ethylene (ET) and jasmonic acid (JA) signaling, which can influence phenolic compound biosynthesis. Phenolic compound biosynthesis is essential for pathogen resistance [(He et al., 2017)](https://doi.org/10.1111/jipb.12524) and affect aflatoxin contamination. The JA and ET pathways work together to activate the Ethylene Response Factor (ERF) branch of JA signaling. Due to this gene's impact on JA pathways, it is anticipated that it will aid researchers and breeders in developing an aflatoxin-resistant variety.


Gene Position: Chr20(B10)TfrV2 23,483,367 - 23,485,240


Variant Position: Chr20(B10)TfrV2 23,484,961

### EIN3: Variant Amino Acid Changes

The varieties, Florida-07 and Tifrunner, contain the amino acid sequence GTT, producing a valine within the gene.

In resistant variety, ICG1471, the variant <ins>**T**</ins>TT, produces a phenylalanine

![image](https://github.com/user-attachments/assets/77389adc-79f3-414d-8b65-cd5ec5eea044)

<img width="260" alt="Valine" src="https://github.com/user-attachments/assets/4a6f6cd0-ed8f-4868-9238-d31118f22a80" />
<img width="120" alt="Arrow" src="https://github.com/user-attachments/assets/dc0b98fa-9139-494b-aa88-6c188c1c7eed" />
<img width="350" alt="Phenylalanine" src="https://github.com/user-attachments/assets/0f575c8b-f00e-486a-b602-e1a13ae214b4" />

Valine -> Phenylalanine


The variant present within an exonic region of the gene causes a non-conservative amino acid substitution changing the small non-polar amino acid, valine, to a bulkier hydrophobic aromatic amino acid, phenylalanine. This large change in amino acids has the potential to affect protein structure and function. 

### EIN3: Conserved Region Across Peanut

The EIN3 gene's ORF (open reading frame) is mostly conserved across multiple peanut varieties such as Shitouqi, Aisheng, Huayu23, and Chedouzi.

Blastp was used to find similar conserved amino acid regions and if the "K<ins>**F**</ins>C" variant is present in other varieties.

Most of the ORF where the EIN3 variant lies is conserved however differences occur as "K<ins>**V**</ins>C" or "K<ins>**I**</ins>C"

![image](https://github.com/user-attachments/assets/6e92d557-0b21-4546-a0ea-b923d17f85a7)

Sequence logo made by using the amino acid sequences of different peanut varieties.


The accessions used for the construction of this sequence logo are listed [here](https://docs.google.com/document/d/1nQ0eyiNNdJXp9fQUlmZx1UKssTKFMqnFfshjZ15in8c/edit?usp=sharing).

## LAG1 longevity assurance homolog 3, Ah20g180000

*LAG1* has a primary role in sphingolipid metabolism [(Megyeri et al., 2019)](https://doi.org/10.1242/jcs.228411). This class of lipids is crucial to stress responses like inducing salicylic acid responses and plant developmental regulation [(Li et al., 2022](https://doi.org/10.1111/tpj.15639); [Liu et al., 2021)](https://doi.org/10.1016/j.xplc.2021.100214). The protein produced by *LAG1*, Lag1p, is a ceramide synthase. Ceramides contribute to plant defenses and are synthesized in the endoplasmic reticulum [(Li et al., 2022](https://doi.org/10.1111/tpj.15639); [Zelnik et al., 2020)](https://doi.org/10.1016/j.bbalip.2019.06.015). In aflatoxin studies done by [Korani et al (2018)](https://doi.org/10.1534/genetics.118.300478), protein processing in the endoplasmic reticulum, which is a cell organelle heavily affected by sphingolipids [(Wang et al., 2023)](https://doi.org/10.1111/nph.19200), contributes to aflatoxin resistance. In a previous study, the overexpression of the sphingolipid metabolism gene, *BnaCERK*, in *Brassica napus* caused a decrease in fungal contamination by *Sclerotinia sclerotiorum*, while a knockout increased susceptibility [(Singh et al., 2025)](https://doi.org/10.1093/plphys/kiae656), showcasing sphingolipid metabolism genes as a possible contributor of fungal resistance.


Gene Position: Chr20(B10)TfrV2 26,210,430 - 26,217,502


Variant Position: Chr20(B10)TfrV2 26,216,863

### *LAG1*: Variant Amino Acid Changes

The varieties, Florida-07 and Tifrunner, contain the amino acid sequence CGG, producing an arginine within the gene.

In resistant variety, ICG1471, the variant <ins>**A**</ins>GG, an arginine is still produced.

![image](https://github.com/user-attachments/assets/d53a2c10-5657-42d7-8e78-3ee35cc82fe4)

Arginine -> Arginine

The variant present within an exonic region of this gene does not cause an amino acid substitution but rather a synonymous change, retaining an arginine. Synonymous mutations can contribute to the enhancement of translation, stabilizing mRNA molecules, and changes to protein folding [(Oelschlaeger, 2024)](https://doi.org/10.3390/biom14010132).

# Bioinformatic Tools Used

<ins>minimap2 2.26</ins>

Li, H. (2018). Minimap2: pairwise alignment for nucleotide sequences. Bioinformatics, 34:3094-3100. https://doi.org/10.1093/bioinformatics/bty191

<ins>samtools 1.16.1</ins>

Petr Danecek, James K Bonfield, Jennifer Liddle, John Marshall, Valeriu Ohan, Martin O Pollard, Andrew Whitwham, Thomas Keane, Shane A McCarthy, Robert M Davies, Heng Li, Twelve years of SAMtools and BCFtools, GigaScience, Volume 10, Issue 2, February 2021, giab008, https://doi.org/10.1093/gigascience/giab008

<ins>bcftools 1.18</ins>

Petr Danecek, James K Bonfield, Jennifer Liddle, John Marshall, Valeriu Ohan, Martin O Pollard, Andrew Whitwham, Thomas Keane, Shane A McCarthy, Robert M Davies, Heng Li, Twelve years of SAMtools and BCFtools, GigaScience, Volume 10, Issue 2, February 2021, giab008, https://doi.org/10.1093/gigascience/giab008

<ins>IGV 2.18.4</ins>

James T Robinson, Helga Thorvaldsdottir, Douglass Turner, Jill P Mesirov, igv.js: an embeddable JavaScript implementation of the Integrative Genomics Viewer (IGV), Bioinformatics, Volume 39, Issue 1 (2023) btac830, https://doi.org/10.1093/bioinformatics/btac830

<ins>SnpEff 5.2a</ins>

Cingolani, P., Platts, A., Wang, leL., Coon, M., Nguyen, T., Wang, L., Land, S. J., Lu, X., & Ruden, D. M. (2012). A program for annotating and predicting the effects of single nucleotide polymorphisms, SnpEff: SNPs in the genome of Drosophila melanogaster strain w1118; iso-2; iso-3. Fly, 6(2), 80–92. https://doi.org/10.4161/fly.19695

## Peanut Genome

arahy.Tifrunner.gnm2.J5K5

Bertioli, D.J., Jenkins, J., Clevenger, J. et al. The genome sequence of segmental allotetraploid peanut Arachis hypogaea. Nat Genet 51, 877–884 (2019). https://doi.org/10.1038/s41588-019-0405-z









