Genome specific bed files from the Global Alliance for Genomics and Health (GA4GH) Benchmarking Team and the Genome in a Bottle Consortium

These files are intended as standard resource of bed files for use in stratifying true positive, false positive, and false negative variant calls into different categories of variants for specific benchmark genomes like NA12878/HG001 and the GIAB AJ and Chinese Trios.  

These files were created by Justin Zook at the National Institute of Standards and Technology.  Currently, this folder contains 2 types of files: (1) regions containing putative compound heterozygous variants and (2) regions containing multiple variants within 50bp of each other from v3.2.2 vcf of the GIAB samples HG001 (NA12878) and HG002 (AJ Son). Methods used to generate these files are below:

Compound heterozygous variants when phased variants within 50bp are merged for HG002 (HG002_GIAB_highconf_IllFB-IllGATKHC-CG-Ion-Solid_CHROM1-22_v3.2.2_highconf_geno2haplo.vcf.gz):
vcflib_140404/bin/vcfgeno2haplo -r human_g1k_v37.fasta  -w 50 AJSNPindels/AshkenazimTrio/HG002_NA24385_son/NISTv3.2.2/HG002_GIAB_highconf_IllFB-IllGATKHC-CG-Ion-Solid_CHROM1-22_v3.2.2_highconf.vcf.gz > AJSNPindels/HG002_GIAB_highconf_IllFB-IllGATKHC-CG-Ion-Solid_CHROM1-22_v3.2.2_highconf_geno2haplo.vcf

grep '1/2\|1|2\|2|1\|2/1' AJSNPindels/HG002_GIAB_highconf_IllFB-IllGATKHC-CG-Ion-Solid_CHROM1-22_v3.2.2_highconf_geno2haplo.vcf | awk 'BEGIN {FS = OFS = "\t"} {print $1,$2-50,$2+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | bedtools2.25.0/bin/mergeBed -i stdin -d 50 > AJSNPindels/HG002_GIAB_highconf_IllFB-IllGATKHC-CG-Ion-Solid_CHROM1-22_v3.2.2_highconf_geno2haplo_compoundhet_slop50.bed


Compound heterozygous variants when phased variants within 50bp are merged for HG001 (NA12878_GIAB_highconf_IllFB-IllGATKHC-CG-Ion-Solid_ALLCHROM_v3.2.2_highconf_geno2haplo_compoundhet_slop50.bed.gz):
/Applications/bioinfo/vcflib_140404/bin/vcfgeno2haplo -r /Volumes/SSD960/references/human_g1k_v37.fasta  -w 50 /Volumes/UHTS1/NA12878SNPindels/NISTv3.2.2/NA12878_GIAB_highconf_IllFB-IllGATKHC-CG-Ion-Solid_ALLCHROM_v3.2.2_highconf.vcf.gz > /Volumes/UHTS1/NA12878SNPindels/NA12878_GIAB_highconf_IllFB-IllGATKHC-CG-Ion-Solid_ALLCHROM_v3.2.2_highconf_geno2haplo.vcf

grep '1/2\|1|2\|2|1\|2/1' /Volumes/UHTS1/NA12878SNPindels/NA12878_GIAB_highconf_IllFB-IllGATKHC-CG-Ion-Solid_ALLCHROM_v3.2.2_highconf_geno2haplo.vcf  | awk 'BEGIN {FS = OFS = "\t"} {print $1,$2-50,$2+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2.25.0/bin/mergeBed -i stdin -d 50 > /Volumes/UHTS1/NA12878SNPindels/NA12878_GIAB_highconf_IllFB-IllGATKHC-CG-Ion-Solid_ALLCHROM_v3.2.2_highconf_geno2haplo_compoundhet_slop50.bed


Regions with nearby variants in HG002 (HG002_GIAB_highconf_IllFB-IllGATKHC-CG-Ion-Solid_CHROM1-22_v3.2.2_highconf_varswithin50bp.bed.gz):
zgrep -v ^# /Volumes/UHTS1/AJSNPindels/AshkenazimTrio/HG002_NA24385_son/NISTv3.2.2/HG002_GIAB_highconf_IllFB-IllGATKHC-CG-Ion-Solid_CHROM1-22_v3.2.2_highconf.vcf.gz | awk 'BEGIN {FS = OFS = "\t"} {print $1,$2-1,$2,1}' | /Applications/bioinfo/bedtools2.25.0/bin/mergeBed -i stdin -d 50 -c 4 -o sum | awk '{if($4>1) print}' | /Applications/bioinfo/bedtools2.25.0/bin/slopBed -i stdin -b 50 -g /Applications/bioinfo/BEDTools-Version-2.16.2/genomes/human.b37.genome | /Applications/bioinfo/bedtools2.25.0/bin/mergeBed -i stdin > /Volumes/UHTS1/AJSNPindels/HG002_GIAB_highconf_IllFB-IllGATKHC-CG-Ion-Solid_CHROM1-22_v3.2.2_highconf_varswithin50bp.bed


Regions with nearby variants in HG001 (NA12878_GIAB_highconf_IllFB-IllGATKHC-CG-Ion-Solid_ALLCHROM_v3.2.2_highconf_varswithin50bp.gz):
zgrep -v ^# /Volumes/UHTS1/NA12878SNPindels/NISTv3.2.2/NA12878_GIAB_highconf_IllFB-IllGATKHC-CG-Ion-Solid_ALLCHROM_v3.2.2_highconf.vcf.gz  | awk 'BEGIN {FS = OFS = "\t"} {print $1,$2-1,$2,1}' | /Applications/bioinfo/bedtools2.25.0/bin/mergeBed -i stdin -d 50 -c 4 -o sum | awk '{if($4>1) print}' | /Applications/bioinfo/bedtools2.25.0/bin/slopBed -i stdin -b 50 -g /Applications/bioinfo/BEDTools-Version-2.16.2/genomes/human.b37.genome | /Applications/bioinfo/bedtools2.25.0/bin/mergeBed -i stdin > /Volumes/UHTS1/NA12878SNPindels/NA12878_GIAB_highconf_IllFB-IllGATKHC-CG-Ion-Solid_ALLCHROM_v3.2.2_highconf_varswithin50bp.bed


New for v3.3.2:
#HG001

#PG
/Applications/bioinfo/vcflib_140404/bin/vcfgeno2haplo -w 10 -r ~/Documents/references/human_g1k_v37.fasta /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37.vcf.gz > /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_geno2haplo10.vcf

#Find compound hets with no length change (i.e., only compound hets where both alleles contain only SNPs or MNPs within 10bp) 
grep '1/2\|1|2\|2|1\|2/1' /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_geno2haplo10.vcf | awk 'BEGIN {FS = "\t\|,"; OFS = "\t"} {if (length($4)==length($5) && length($4)==length($6)) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_comphetsnp10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37.vcf.gz -b /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_comphetsnp10bp_slop50.bed | wc -l
#   56005
/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_geno2haplo10.vcf -b /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_comphetsnp10bp_slop50.bed | wc -l
#   30998

#Find compound hets where at least 1 allele has a length change (i.e., only compound hets where at least one allele contains an indel within 10bp) 
grep '1/2\|1|2\|2|1\|2/1' /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_geno2haplo10.vcf | awk 'BEGIN {FS = "\t\|,"; OFS = "\t"} {if (length($4)!=length($5) || length($4)!=length($6)) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_comphetindel10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37.vcf.gz -b /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_comphetindel10bp_slop50.bed | wc -l
#   46618
/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_geno2haplo10.vcf -b /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_comphetindel10bp_slop50.bed | uniq | wc -l
#   37222

#Find complex variants that are not compound hets where there is a length change and it is not a simple insertion or deletion
grep -v '1/2\|1|2\|2|1\|2/1' /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_geno2haplo10.vcf | awk 'BEGIN {FS = "\t"; OFS = "\t"} {if (length($4)!=length($5) && length($4)>1 && length($5)>1) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_complexindel10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37.vcf.gz -b /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_complexindel10bp_slop50.bed | wc -l
#   58590
/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_geno2haplo10.vcf -b /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_complexindel10bp_slop50.bed | uniq | wc -l
#   36247

#Find SNPs within 10bp that are not compound hets
grep -v '1/2\|1|2\|2|1\|2/1' /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_geno2haplo10.vcf | awk 'BEGIN {FS = "\t"; OFS = "\t"} {if (length($4)==length($5) && length($4)>1 ) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_snpswithin10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37.vcf.gz -b /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_snpswithin10bp_slop50.bed | wc -l
#   194330
/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_geno2haplo10.vcf -b /Users/jzook/Documents/NA12878/PlatinumGenomes/PG2016-1.0/PG2016-1.0_NA12878_b37_snpswithin10bp_slop50.bed | uniq | wc -l
#   107755


##RTG
/Applications/bioinfo/vcflib_140404/bin/vcfgeno2haplo -w 10 -r ~/Documents/references/human_g1k_v37.fasta /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878.vcf.gz > /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_geno2haplo10.vcf

#Find compound hets with no length change (i.e., only compound hets where both alleles contain only SNPs or MNPs within 10bp) 
grep '1/2\|1|2\|2|1\|2/1' /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_geno2haplo10.vcf | awk 'BEGIN {FS = "\t\|,"; OFS = "\t"} {if (length($4)==length($5) && length($4)==length($6)) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_comphetsnp10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878.vcf.gz -b /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_comphetsnp10bp_slop50.bed | wc -l
#   60576
/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_geno2haplo10.vcf -b /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_comphetsnp10bp_slop50.bed | wc -l
#   36461

#Find compound hets where at least 1 allele has a length change (i.e., only compound hets where at least one allele contains an indel within 10bp) 
grep '1/2\|1|2\|2|1\|2/1' /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_geno2haplo10.vcf | awk 'BEGIN {FS = "\t\|,"; OFS = "\t"} {if (length($4)!=length($5) || length($4)!=length($6)) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_comphetindel10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878.vcf -b /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_comphetindel10bp_slop50.bed | wc -l
#   110016
/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_geno2haplo10.vcf -b /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_comphetindel10bp_slop50.bed | uniq | wc -l
#   107856

#Find complex variants that are not compound hets where there is a length change and it is not a simple insertion or deletion
grep -v '1/2\|1|2\|2|1\|2/1' /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_geno2haplo10.vcf | awk 'BEGIN {FS = "\t"; OFS = "\t"} {if (length($4)!=length($5) && length($4)>1 && length($5)>1) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_complexindel10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878.vcf -b /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_complexindel10bp_slop50.bed | wc -l
#   126365
/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_geno2haplo10.vcf -b /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_complexindel10bp_slop50.bed | uniq | wc -l
#   121610

#Find SNPs within 10bp that are not compound hets
grep -v '1/2\|1|2\|2|1\|2/1' /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_geno2haplo10.vcf | awk 'BEGIN {FS = "\t"; OFS = "\t"} {if (length($4)==length($5) && length($4)>1 ) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_snpswithin10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878.vcf -b /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_snpswithin10bp_slop50.bed | wc -l
#   247432
/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_geno2haplo10.vcf -b /Users/jzook/Documents/NA12878/RTG/RTG_Illumina_Segregation_Phasing_161004/sp_v37.7.3.NA12878_snpswithin10bp_slop50.bed | uniq | wc -l
#   163493

#HG001 NIST v3.3.2
/Applications/bioinfo/vcflib_140404/bin/vcfgeno2haplo -w 10 -r ~/Documents/references/human_g1k_v37.fasta /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer.vcf.gz > /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_geno2haplo10.vcf

#Find compound hets with no length change (i.e., only compound hets where both alleles contain only SNPs or MNPs within 10bp) 
grep '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_geno2haplo10.vcf | awk 'BEGIN {FS = "\t\|,"; OFS = "\t"} {if (length($4)==length($5) && length($4)==length($6)) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_comphetsnp10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer.vcf.gz -b /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_comphetsnp10bp_slop50.bed | wc -l
#   52693
/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_geno2haplo10.vcf -b /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_comphetsnp10bp_slop50.bed | wc -l
#   28935

#Find compound hets where at least 1 allele has a length change (i.e., only compound hets where at least one allele contains an indel within 10bp) 
grep '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_geno2haplo10.vcf | awk 'BEGIN {FS = "\t\|,"; OFS = "\t"} {if (length($4)!=length($5) || length($4)!=length($6)) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_comphetindel10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer.vcf.gz -b /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_comphetindel10bp_slop50.bed | wc -l
#   62457
/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_geno2haplo10.vcf -b /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_comphetindel10bp_slop50.bed | uniq | wc -l
#   54061

#Find complex variants that are not compound hets where there is a length change and it is not a simple insertion or deletion
grep -v '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_geno2haplo10.vcf | awk 'BEGIN {FS = "\t"; OFS = "\t"} {if (length($4)!=length($5) && length($4)>1 && length($5)>1) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_complexindel10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer.vcf.gz -b /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_complexindel10bp_slop50.bed | wc -l
#   43740
/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_geno2haplo10.vcf -b /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_complexindel10bp_slop50.bed | uniq | wc -l
#   23079

#Find SNPs within 10bp that are not compound hets
grep -v '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_geno2haplo10.vcf | awk 'BEGIN {FS = "\t"; OFS = "\t"} {if (length($4)==length($5) && length($4)>1 ) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_snpswithin10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer.vcf.gz -b /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_snpswithin10bp_slop50.bed | wc -l
#   191485
/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_geno2haplo10.vcf -b /Users/jzook/Downloads/HG001_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-X_v.3.3.2_highconf_RTGandPGphasetransfer_snpswithin10bp_slop50.bed | uniq | wc -l
#   105014




#HG002
/Applications/bioinfo/vcflib_140404/bin/vcfgeno2haplo -w 10 -r ~/Documents/references/human_g1k_v37.fasta /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased.vcf.gz > /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_geno2haplo10.vcf

#Find compound hets with no length change (i.e., only compound hets where both alleles contain only SNPs or MNPs within 10bp) 
grep '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_geno2haplo10.vcf | awk 'BEGIN {FS = "\t\|,"; OFS = "\t"} {if (length($4)==length($5) && length($4)==length($6)) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_comphetsnp10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased.vcf.gz -b /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_comphetsnp10bp_slop50.bed | wc -l
#   49212
/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_geno2haplo10.vcf -b /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_comphetsnp10bp_slop50.bed | wc -l
#   27654

#Find compound hets where at least 1 allele has a length change (i.e., only compound hets where at least one allele contains an indel within 10bp) 
grep '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_geno2haplo10.vcf | awk 'BEGIN {FS = "\t\|,"; OFS = "\t"} {if (length($4)!=length($5) || length($4)!=length($6)) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_comphetindel10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased.vcf.gz -b /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_comphetindel10bp_slop50.bed | wc -l
#   70041
/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_geno2haplo10.vcf -b /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_comphetindel10bp_slop50.bed | uniq | wc -l
#   57696

#Find complex variants that are not compound hets where there is a length change and it is not a simple insertion or deletion
grep -v '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_geno2haplo10.vcf | awk 'BEGIN {FS = "\t"; OFS = "\t"} {if (length($4)!=length($5) && length($4)>1 && length($5)>1) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_complexindel10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased.vcf.gz -b /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_complexindel10bp_slop50.bed | wc -l
#   58590
/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_geno2haplo10.vcf -b /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_complexindel10bp_slop50.bed | uniq | wc -l
#   36247

#Find SNPs within 10bp that are not compound hets
grep -v '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_geno2haplo10.vcf | awk 'BEGIN {FS = "\t"; OFS = "\t"} {if (length($4)==length($5) && length($4)>1 ) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_snpswithin10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased.vcf.gz -b /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_snpswithin10bp_slop50.bed | wc -l
#   184467
/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_geno2haplo10.vcf -b /Users/jzook/Downloads/HG002_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X-SOLID_CHROM1-22_v.3.3.2_highconf_triophased_snpswithin10bp_slop50.bed | uniq | wc -l
#   101929



##HG003 v3.3.2
gunzip -c /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf.vcf.gz | sed 's/1\/1/1|1/' | /Applications/bioinfo/vcflib_140404/bin/vcfgeno2haplo -w 10 -r ~/Documents/references/human_g1k_v37.fasta | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | uniq > /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf

#Find compound hets with no length change (i.e., only compound hets where both alleles contain only SNPs or MNPs within 10bp) 
grep '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | awk 'BEGIN {FS = "\t\|,"; OFS = "\t"} {if (length($4)==length($5) && length($4)==length($6)) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_comphetsnp10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf.vcf.gz -b /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_comphetsnp10bp_slop50.bed | wc -l
#   23413
awk 'BEGIN {FS = "\t"; OFS = "\t"} { print $1,$2,$2+length($4)}' /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | /Applications/bioinfo/bedtools2/bin/intersectBed -a stdin -b /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_comphetsnp10bp_slop50.bed | wc -l
#   12323

#Find compound hets where at least 1 allele has a length change (i.e., only compound hets where at least one allele contains an indel within 10bp) 
grep '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | awk 'BEGIN {FS = "\t\|,"; OFS = "\t"} {if (length($4)!=length($5) || length($4)!=length($6)) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_comphetindel10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf.vcf.gz -b /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_comphetindel10bp_slop50.bed | wc -l
#   52597
awk 'BEGIN {FS = "\t"; OFS = "\t"} { print $1,$2,$2+length($4)}' /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | /Applications/bioinfo/bedtools2/bin/intersectBed -a stdin -b /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_comphetindel10bp_slop50.bed | uniq | wc -l
#   49252

#Find complex variants that are not compound hets where there is a length change and it is not a simple insertion or deletion
grep -v '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | awk 'BEGIN {FS = "\t"; OFS = "\t"} {if (length($4)!=length($5) && length($4)>1 && length($5)>1) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_complexindel10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf.vcf.gz -b /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_complexindel10bp_slop50.bed | wc -l
#   37212
awk 'BEGIN {FS = "\t"; OFS = "\t"} { print $1,$2,$2+length($4)}' /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | /Applications/bioinfo/bedtools2/bin/intersectBed -a stdin -b /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_complexindel10bp_slop50.bed | uniq | wc -l
#   19948

#Find SNPs within 10bp that are not compound hets
grep -v '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | awk 'BEGIN {FS = "\t"; OFS = "\t"} {if (length($4)==length($5) && length($4)>1 ) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_snpswithin10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf.vcf.gz -b /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_snpswithin10bp_slop50.bed | wc -l
#   182800
awk 'BEGIN {FS = "\t"; OFS = "\t"} { print $1,$2,$2+length($4)}' /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | /Applications/bioinfo/bedtools2/bin/intersectBed -a stdin -b /Users/jzook/Downloads/HG003_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_snpswithin10bp_slop50.bed | uniq | wc -l
#   101314



##HG004 v3.3.2
gunzip -c /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf.vcf.gz | sed 's/1\/1/1|1/' | /Applications/bioinfo/vcflib_140404/bin/vcfgeno2haplo -w 10 -r ~/Documents/references/human_g1k_v37.fasta | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | uniq > /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf

#Find compound hets with no length change (i.e., only compound hets where both alleles contain only SNPs or MNPs within 10bp) 
grep '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | awk 'BEGIN {FS = "\t\|,"; OFS = "\t"} {if (length($4)==length($5) && length($4)==length($6)) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_comphetsnp10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf.vcf.gz -b /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_comphetsnp10bp_slop50.bed | wc -l
#   20958
awk 'BEGIN {FS = "\t"; OFS = "\t"} { print $1,$2,$2+length($4)}' /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | /Applications/bioinfo/bedtools2/bin/intersectBed -a stdin -b /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_comphetsnp10bp_slop50.bed | wc -l
#   11165

#Find compound hets where at least 1 allele has a length change (i.e., only compound hets where at least one allele contains an indel within 10bp) 
grep '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | awk 'BEGIN {FS = "\t\|,"; OFS = "\t"} {if (length($4)!=length($5) || length($4)!=length($6)) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_comphetindel10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf.vcf.gz -b /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_comphetindel10bp_slop50.bed | wc -l
#   53647
awk 'BEGIN {FS = "\t"; OFS = "\t"} { print $1,$2,$2+length($4)}' /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | /Applications/bioinfo/bedtools2/bin/intersectBed -a stdin -b /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_comphetindel10bp_slop50.bed | uniq | wc -l
#   50650

#Find complex variants that are not compound hets where there is a length change and it is not a simple insertion or deletion
grep -v '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | awk 'BEGIN {FS = "\t"; OFS = "\t"} {if (length($4)!=length($5) && length($4)>1 && length($5)>1) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_complexindel10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf.vcf.gz -b /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_complexindel10bp_slop50.bed | wc -l
#   37794
awk 'BEGIN {FS = "\t"; OFS = "\t"} { print $1,$2,$2+length($4)}' /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | /Applications/bioinfo/bedtools2/bin/intersectBed -a stdin -b /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_complexindel10bp_slop50.bed | uniq | wc -l
#   20227

#Find SNPs within 10bp that are not compound hets
grep -v '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | awk 'BEGIN {FS = "\t"; OFS = "\t"} {if (length($4)==length($5) && length($4)>1 ) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_snpswithin10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf.vcf.gz -b /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_snpswithin10bp_slop50.bed | wc -l
#   183985
awk 'BEGIN {FS = "\t"; OFS = "\t"} { print $1,$2,$2+length($4)}' /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | /Applications/bioinfo/bedtools2/bin/intersectBed -a stdin -b /Users/jzook/Downloads/HG004_GRCh37_GIAB_highconf_CG-IllFB-IllGATKHC-Ion-10X_CHROM1-22_v.3.3.2_highconf_snpswithin10bp_slop50.bed | uniq | wc -l
#   102023


##HG005 v3.3.2
gunzip -c /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf.vcf.gz | sed 's/1\/1/1|1/' | /Applications/bioinfo/vcflib_140404/bin/vcfgeno2haplo -w 10 -r ~/Documents/references/human_g1k_v37.fasta | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | uniq > /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf

#Find compound hets with no length change (i.e., only compound hets where both alleles contain only SNPs or MNPs within 10bp) 
grep '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | awk 'BEGIN {FS = "\t\|,"; OFS = "\t"} {if (length($4)==length($5) && length($4)==length($6)) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_comphetsnp10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf.vcf.gz -b /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_comphetsnp10bp_slop50.bed | wc -l
#   12123
awk 'BEGIN {FS = "\t"; OFS = "\t"} { print $1,$2,$2+length($4)}' /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | /Applications/bioinfo/bedtools2/bin/intersectBed -a stdin -b /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_comphetsnp10bp_slop50.bed | wc -l
#   7070

#Find compound hets where at least 1 allele has a length change (i.e., only compound hets where at least one allele contains an indel within 10bp) 
grep '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | awk 'BEGIN {FS = "\t\|,"; OFS = "\t"} {if (length($4)!=length($5) || length($4)!=length($6)) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_comphetindel10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf.vcf.gz -b /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_comphetindel10bp_slop50.bed | wc -l
#   34788
awk 'BEGIN {FS = "\t"; OFS = "\t"} { print $1,$2,$2+length($4)}' /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | /Applications/bioinfo/bedtools2/bin/intersectBed -a stdin -b /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_comphetindel10bp_slop50.bed | uniq | wc -l
#   33299

#Find complex variants that are not compound hets where there is a length change and it is not a simple insertion or deletion
grep -v '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | awk 'BEGIN {FS = "\t"; OFS = "\t"} {if (length($4)!=length($5) && length($4)>1 && length($5)>1) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_complexindel10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf.vcf.gz -b /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_complexindel10bp_slop50.bed | wc -l
#   31602
awk 'BEGIN {FS = "\t"; OFS = "\t"} { print $1,$2,$2+length($4)}' /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | /Applications/bioinfo/bedtools2/bin/intersectBed -a stdin -b /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_complexindel10bp_slop50.bed | uniq | wc -l
#   16846

#Find SNPs within 10bp that are not compound hets
grep -v '1/2\|1|2\|2|1\|2/1' /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | awk 'BEGIN {FS = "\t"; OFS = "\t"} {if (length($4)==length($5) && length($4)>1 ) print $1,$2-50,$2+length($4)+50}' | sed 's/^X/23/' | sort -k1,1n -k2,2n | sed 's/^23/X/' | /Applications/bioinfo/bedtools2/bin/mergeBed -i stdin -d 50 > /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_snpswithin10bp_slop50.bed

/Applications/bioinfo/bedtools2/bin/intersectBed -a /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf.vcf.gz -b /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_snpswithin10bp_slop50.bed | wc -l
#   179205
awk 'BEGIN {FS = "\t"; OFS = "\t"} { print $1,$2,$2+length($4)}' /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_geno2haplo10.vcf | /Applications/bioinfo/bedtools2/bin/intersectBed -a stdin -b /Users/jzook/Downloads/HG005_GRCh37_highconf_CG-IllFB-IllGATKHC-Ion-SOLID_CHROM1-22_v.3.3.2_highconf_snpswithin10bp_slop50.bed | uniq | wc -l
#   99532

