**Plastid Genome Annotator v2.0**<br />
Copyright (C) 2024 Xiao-Jian Qu<br />

**Contact**<br />
quxiaojian@sdnu.edu.cn<br />

**Citation**<br />
If you use PGA in your scientific research, please cite:<br />
Qu X-J, Moore MJ, Li D-Z, Yi T-S. 2019. PGA: a software package for rapid, accurate, and flexible batch annotation of plastomes. Plant Methods 15:50.<br />
[ResearchGate](https://www.researchgate.net/publication/333245667_PGA_a_software_package_for_rapid_accurate_and_flexible_batch_annotation_of_plastomes)<br />
[Plant Methods](https://plantmethods.biomedcentral.com/articles/10.1186/s13007-019-0435-7)<br />

**Notes in Chinese**<br />
https://www.jianshu.com/p/6ac8a9fad9c9

**Prerequisites**<br />
BLAST 2.8.1 or higher (latest)<br />
Perl 5<br />
Windows, Linux or Mac<br />

**General Introduction to PGA**<br />
PGA (Plastid Genome Annotator), a standalone command line tool, can perform rapid, accurate, and flexible batch annotation of newly generated target plastomes based on well-annotated reference plastomes. In contrast to current existing tools, PGA uses reference plastomes as the query and unannotated target plastomes as the subject to locate genes, which we refer to as the reverse query-subject BLAST search approach. PGA accurately identifies gene and intron boundaries as well as intron loss. The program outputs GenBank-formatted files as well as a log file to assist users in verifying annotations.<br />
We thank Rong Zhang, Ying-Ying Yang and Jian-Jun Jin from Kunming Institute of Botany Chinese Academy of Sciences, and Pin Gong from Institute of Botany Chinese Academy of Sciences for improving this tool.<br />

Following six steps will be conducted to annotate plastomes: (1) Preparation of GenBank-formatted reference plastomes; (2) Preparation of FASTA-formatted target plastomes; (3) Reference database generation; (4) BLAST search; (5) Determining feature boundaries; (6) Generating GenBank and log files.<br />

![flowchart](/flowchart.png)

**Preparations**<br />

(1) download the latest BLAST+ software [BLAST+](https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=Download).<br />
The latest version (2018/11/27) is ncbi-blast-2.8.1+-win64.exe for Windows, ncbi-blast-2.8.1+-x64-linux.tar.gz for Linux, ncbi-blast-2.8.1+-x64-macosx.tar.gz for Mac.<br />
For Windows, just install it following instructions. It will be in PATH automatically.<br />
For Linux or Mac, we suggest to put it in PATH following below steps.<br />
```
vim ~/.bashrc
export PATH=/home/xxx/blast-2.8.1+/bin:$PATH
source ~/.bashrc
```

Please check if the latest blast version is successfully installed by inputing "blastn -version" in cmd.
```
blastn -version
```
(2) download this repository to your local computer.<br />
For Windows, just download, unzip and use it.<br />
For Linux or Mac, we suggest to put it in PATH and make it read, write and executable folowing below steps.<br />
```
git clone https://github.com/quxiaojian/PGA.git
vim ~/.bashrc
export PATH=/home/xxx/PGA:$PATH
source ~/.bashrc
chmod a+rwx PGA_v2.pl
```

Then, you can test PGA_v2.pl by type "perl PGA_v2.pl", which will show the usage information.<br />
```
Usage:
        PGA_v2.pl -r -t [-i -p -l -d -q -o -f -w]
        Plastid Genome Annotator version 2.0
        Copyright (C) 2023 Xiao-Jian Qu
        Please contact <quxiaojian@sdnu.edu.cn>, if you have any bugs or questions.

        [-h -help]         help information.
        [-r -reference]    required: (default: reference) input directory name containing GenBank-
                           formatted file(s) that from the same or close families.
        [-t -target]       required: (default: target) input directory name containing FASTA-
                           formatted file(s) that will be annotated.
        [-i -ir]           optional: (default: 1000) minimum allowed inverted-repeat (IR) length.
        [-p -pidentity]    optional: (default: 40) any PCGs with a TBLASTN percent identity less
                           than this value will be listed in the log file and will not be annotated.
        [-l -link]         optional: (default: Y) if Y, the exons of the trans-splicing gene rps12
                           will be linked; if N, the exons of the rps12 gene will not be linked.
        [-d -redundancy]   optional: (default: N) if N, any PCGs with a query coverage per annotated
                           PCG less or greater than each of the two values (<1,>1) in the following
                           parameter will not be annotated; if Y, any PCGs with a query coverage per
                           annotated PCG less or greater than each of the two values (<1,>1) in the
                           following parameter will be annotated, which can be used to trace the
                           evolutionary remains (pseudo-genes) of functional PCGs.
        [-q -qcoverage]    optional: (default: 0.5,2.0) any PCGs with a query coverage per annotated
                           PCG less or greater than each of these two values (<1,>1) will be listed
                           in the log file.
        [-o -out]          optional: (default: gb) output directory name.
        [-f -form]         optional: (default: circular) circular or linear form for FASTA-formatted
                           file.
        [-w -warning]      optional: (default: warning) log file name containing warning information
                           for annotated GenBank-formatted file(s).
```

**Run Test**<br />
```
perl PGA_v2.pl -r test/angiosperms/reference -t test/angiosperms/target
```
equal to
```
perl PGA_v2.pl -r test/angiosperms/reference -t test/angiosperms/target -i 1000 -p 40 -l Y -d N -q 0.5,2 -o gb -f circular -l warning
```

![gif](/PGA.gif)

**Input and Output**<br />
Annotation of the plastome of Rosa roxburghii with the plastome of Amborella trichopoda as reference. (a) "Amborella_trichopoda.gb" shows the partial GenBank-formatted reference plastome of Amborella trichopoda, as revised from AJ506156. (b) "Rosa_roxburghii.fasta" shows the partial FASTA-formatted target plastome of Rosa roxburghii, revised from NC_032038. (c) "Rosa_roxburghii.gb" shows the output GenBank-formatted file containing partial annotation information for the target plastome of Rosa roxburghii. (d) "warning.log" shows warning and statistical items during the annotation of the target plastome of Rosa roxburghii. The log file indicates the loss of the atpF intron in Rosa roxburghii. There are 113 total genes in the reference and target plastomes.<br />

```
(a) Amborella_trichopoda.gb
LOCUS       Amborella_trichopoda      162686 bp    DNA     circular UNA 08-JUN-2015
DEFINITION  Amborella trichopoda chloroplast genomic DNA, complete sequence.
ACCESSION   AJ506156
VERSION     AJ506156.2  GI:34481608
KEYWORDS    complete genome.
SOURCE      chloroplast Amborella trichopoda
  ORGANISM  Amborella trichopoda
            Eukaryota; Viridiplantae; Streptophyta; Embryophyta; Tracheophyta;
            Spermatophyta; Magnoliophyta; basal Magnoliophyta; Amborellales;
            Amborellaceae; Amborella.
FEATURES          Location/Qualifiers
     source          1..162686
                    /organism="Amborella trichopoda"
                    /mol_type="genomic DNA"
     repeat_region    90951..117611
                    /note="inverted repeat region B; IRB repeat region"
                    /rpt_type="inverted"
     rRNA          complement(139284..142097)
                    /gene="rrn23"
                    /product="23S ribosomal RNA"
     gene           complement(139284..142097)
                    /gene="rrn23"
     tRNA          join(complement(4472..4508), complement(1840..1874))
                    /gene="trnK-UUU"
                    /product="tRNA-Lys"
     gene           complement(1840..4508)
                    /gene="trnK-UUU"
     CDS           join(complement(16186..16330), complement(14506..14915))
                    /gene="atpF"
                    /codon_start=1
                    /transl_table=11
                    /product="ATPase I subunit"
/translation="MKNVTDSFVSLGHWPSAGSFGFNTDIFATNPINLSVVLGVLIFF
                    GKGVLSDLLDNRKQRILSTIRNSEELRGGAIEQLEKARARLRKVEIEADEFRVNGYSE
                    IEREKSNLINAAYENLERLENYKNESIHFEQQRAMNQVRQRVFQQALQGALETLNSYL
                         NSELHLRTISANIGMLGTMKNITD"
     gene           complement(14506..16330)
                    /gene="atpF"

(b) Rosa_roxburghii.fasta
>Rosa_roxburghii
ATGGGCGAACGACGGGAATTGAACCCGCGCGTGGTGGATTCACAATCCACTGCCTTGATC

(c) Rosa_roxburghii.gb
LOCUS       Rosa_roxburghii  156749 bp    DNA     circular PLN 25-DEC-2017
FEATURES             Location/Qualifiers
     source          1..156749
                     /organism="Rosa_roxburghii"
                     /mol_type="genomic DNA"
     gene            106222..109027
                     /gene="rrn23"
     rRNA            106222..109027
                     /gene="rrn23"
                     /product="23S ribosomal RNA"
     gene            complement(1704..4278)
                     /gene="trnK-UUU"
     tRNA            join(complement(4242..4278), complement(1704..1738))
                     /gene="trnK-UUU"
                     /product="tRNA-Lys"
     gene            complement(12213..12767)
                     /gene="atpF"
     CDS             complement(12213..12767)
                     /gene="atpF"
                     /codon_start=1
                     /transl_table=11
                     /product="ATP synthase CF0 subunit I"

(d) warning.log
Rosa_roxburghii
Warning: atpF (negative one-intron PCG) lost intron!
Total number of genes in the reference plastome(s): 113.
Total number of genes annotated in the target plastome: 113.
All gene names from the reference plastome(s) that were not annotated in the target plastome:
```

**Recommendations for using PGA**<br />
(1) Users should carefully check the GenBank-formatted reference plastome. PGA is packaged with several properly annotated plastomes, and it is thus possible for users to use PGA to re-annotate a plastome that is intended to be used as a reference, in order to correct possible inaccuracies.<br />
(2) It is important that users select a reference plastome that contains sufficient numbers of annotated genes for the target taxa. The number of genes in the reference plastome(s) should equal or exceed the number in the target plastome(s). If the number of genes in the target is uncertain, it may be best to use multiple reference plastomes. The Amborella trichopoda (AJ506156) and Zamia furfuracea (JX416857) plastomes included within PGA are examples of plastomes that contain the highest gene numbers among known angiosperms and gymnosperms, and as such it is recommended that they be included as references during PGA runs.<br />
(3) We do not recommend annotating highly incomplete plastomes using a complete reference plastome, because BLAST may annotate some genes redundantly (i.e., BLAST may return hits for genes that were not sequenced or are otherwise absent in the incomplete plastome, resulting in spurious annotations). To annotate highly incomplete plastomes or plastome segments, we recommend using progressiveMauve (as implemented in Mauve 2.4.0; Darling et al., 2010) to align the incomplete plastome to the reference plastome, followed by the use of the corresponding homologous block of the reference plastome as the reference for annotation in PGA.<br />
(4) We suggest that users carefully check highly divergent or otherwise unusual target plastomes for incorrect annotations. This is particularly important for plastomes with a high degree of gene loss, pseudogenization or sequence divergence.<br />

