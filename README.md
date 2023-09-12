# GATK-gCNV
How to) Call common and rare germline copy number variants

(How to) Run GATK in a Docker container
https://gatk.broadinstitute.org/hc/en-us/articles/360035889991--How-to-Run-GATK-in-a-Docker-container


sudo apt-get install docker

sudo apt install docker.io
sudo docker pull broadinstitute/gatk:4.2.5.0
sudo docker run -v ~/my_project:/gatk/my_data -it broadinstitute/gatk:4.2.5.0

gatk PreprocessIntervals -R hg19.fa -L IAD200103_1000_Submitted_Mofifiedforexomedepth.bed --bin-length 0 -imr OVERLAPPING_ONLY -O targets.preprocessed.interval_list
for i in *.bam; do gatk CollectReadCounts -I $i -L targets.preprocessed.interval_list --interval-merging-rule OVERLAPPING_ONLY --format TSV -O ${i/.bam/.tsv}; done



The GATK team recommends generating mappability with Umap and Bismap.
 https://bismap.hoffmanlab.org/

 
 install conda 
https://www.youtube.com/watch?v=9Uy00NhbG6w


http://hgdownload.soe.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeMapability/
https://gatk.broadinstitute.org/hc/en-us/articles/360040098252-AnnotateIntervals


GATK Germline CNV Segmental Duplication filtering is potentially very dangerous
https://gatk.broadinstitute.org/hc/en-us/community/posts/360072134172-GATK-Germline-CNV-Segmental-Duplication-filtering-is-potentially-very-dangerous-


To make index from above file
1. extract the bed file
2. add .bed to the end of the file
3. sort using bed tools 
bedtools sort -i data.bed > data_sorted.bed
4.again compress the file
sudo apt install tabix
bgzip data_sorted.bed 
5. make index of files  
tabix -p bed data_sorted.bed.gz

 gatk AnnotateIntervals -R hg19.fasta -L targets.preprocessed.interval_list --mappability-track data_sorted.bed.gz --interval-merging-rule OVERLAPPING_ONLY -O annotated_intervals2.tsv
 
gatk FilterIntervals -L targets.preprocessed.interval_list --annotated-intervals annotated_intervals2.tsv -I F20GM2170.tsv -I F20GM2213.tsv -I F20GM2236.tsv -I F20GM2247.tsv -I F20GM2330.tsv -I F20GM2370.tsv -I F20GM2426.tsv -I F20GM2452.tsv -I F20GM2593.tsv -I F20GM2614.tsv -I F20GM2631.tsv -I F20GM2635.tsv -I F20GM2747.tsv -I F20GM2795.tsv -I F20GM2802.tsv -I F20GM2828.tsv -I F20GM2948.tsv -I F20GM2954.tsv -I F20GM2971.tsv -I F20GM3262-3291.tsv -I F20GM3268.tsv -I F20GM3362-3385.tsv -I F21GM0083.tsv -I F21GM0128.tsv -I F21GM0133.tsv -I F21GM0135.tsv -I F21GM0214.tsv -I F21GM0314.tsv -I F21GM0503.tsv -I F21GM0668.tsv -I F21GM0692.tsv -I F21GM0743.tsv -I F21GM0747.tsv -I F21GM0837.tsv -I F21GM0940-.tsv -I F21GM0942.tsv -I F21GM1138.tsv -I F21GM1172.tsv -I F21GM1283.tsv -I F21GM1304.tsv -I F21GM1376.tsv -I F21GM1379.tsv -I F21GM1406.tsv -I F21GM1575.tsv -I F21GM1737.tsv -I F21GM1740.tsv -I F21GM1782.tsv -I F21GM1863.tsv -I F21GM1866.tsv -I F21GM1881.tsv -I F21GM1912.tsv -I F21GM1913.tsv -I F21GM1917.tsv -I F21GM1952.tsv -I F21GM1975.tsv -I F21GM2011.tsv -I F21GM2125.tsv -I F21GM2128-2130.tsv -I F21GM2206.tsv -I F21GM2273-2231.tsv -I F21GM2274.tsv -I F21GM2278.tsv -I F21GM2430.tsv -I F21GM2496.tsv -I F21GM2500.tsv -I F21GM2577.tsv -I F21GM2661.tsv -I F21GM2698.tsv -I F21GM2727.tsv -I F21GM2796-2797.tsv -I F21GM2845.tsv -I F21GM3013.tsv -I F21GM3038.tsv -I F21GM3229.tsv -I F21GM3249.tsv -I F21GM3252.tsv -I F21GM3284.tsv -I F21GM3301.tsv -I F21GM3308.tsv -I F21GM3347.tsv -I F21GM3398.tsv -I F21GM3429.tsv -I F21GM3542.tsv -I F21GM3561.tsv -I F21GM3581.tsv -I F21GM3587.tsv -I F21GM3588.tsv -I F21GM3685.tsv -I F21GM3711.tsv -I F21GM3754.tsv -I F21GM3784.tsv -I F21GM3786.tsv -I F21GM3787.tsv -I M20GM2187.tsv -I M20GM2190.tsv -I M20GM2191.tsv -I M20GM2291.tsv -I M20GM2340.tsv -I M20GM2373-2380.tsv -I M20GM2374.tsv -I M20GM2450.tsv -I M20GM2453.tsv -I M20GM2518.tsv -I M20GM2557.tsv -I M20GM2575.tsv -I M20GM2613.tsv -I M20GM2615.tsv -I M20GM2679.tsv -I M20GM2771.tsv -I M20GM2864.tsv -I M20GM2883.tsv -I M20GM2884.tsv -I M20GM2955.tsv -I M20GM3057.tsv -I M20GM3107.tsv -I M20GM3146.tsv -I M20GM3207.tsv -I M20GM3290.tsv -I M20GM3319.tsv -I M20GM3389.tsv -I M20GM782.tsv -I M21GM0031.tsv -I M21GM0034.tsv -I M21GM0091.tsv -I M21GM0137.tsv -I M21GM0188.tsv -I M21GM0195.tsv -I M21GM0199.tsv -I M21GM0217.tsv -I M21GM0228.tsv -I M21GM0316.tsv -I M21GM0385.tsv -I M21GM0398.tsv -I M21GM0540.tsv -I M21GM0541.tsv -I M21GM0630-634.tsv -I M21GM0633.tsv -I M21GM0691.tsv -I M21GM0719.tsv -I M21GM0778.tsv -I M21GM1193.tsv -I M21GM1307.tsv -I M21GM1354.tsv -I M21GM1356.tsv -I M21GM1382.tsv -I M21GM1401.tsv -I M21GM1450.tsv -I M21GM1473.tsv -I M21GM1619.tsv -I M21GM1696.tsv -I M21GM1871.tsv -I M21GM1887.tsv -I M21GM1937.tsv -I M21GM1955.tsv -I M21GM1998.tsv -I M21GM2010.tsv -I M21GM2014.tsv -I M21GM2065.tsv -I M21GM2191.tsv -I M21GM2363.tsv -I M21GM2431.tsv -I M21GM2685.tsv -I M21GM2935.tsv -I M21GM2970.tsv -I M21GM2976.tsv -I M21GM3004-3008.tsv -I M21GM3078.tsv -I M21GM3225.tsv -I M21GM3250.tsv -I M21GM3251.tsv -I M21GM3253.tsv -I M21GM3334.tsv -I M21GM3340.tsv -I M21GM3396.tsv -I M21GM3432.tsv -I M21GM3490.tsv -I M21GM3580.tsv -I M21GM3586.tsv -I M21GM3589.tsv -I M21GM3590.tsv -I M21GM3597.tsv -I M21GM3605.tsv -I M21GM3633.tsv -I M21GM3634.tsv -I M21GM3669.tsv -I M21GM3727.tsv -imr OVERLAPPING_ONLY -O gc.filtered.interval_list --low-count-filter-count-threshold 30 --extreme-count-filter-percentage-of-samples 80


gatk DetermineGermlineContigPloidy -L targets.preprocessed.interval_list --interval-merging-rule OVERLAPPING_ONLY -I F20GM2170.tsv -I F20GM2213.tsv -I F20GM2236.tsv -I F20GM2247.tsv -I F20GM2330.tsv -I F20GM2370.tsv -I F20GM2426.tsv -I F20GM2452.tsv -I F20GM2593.tsv -I F20GM2614.tsv -I F20GM2631.tsv -I F20GM2635.tsv -I F20GM2747.tsv -I F20GM2795.tsv -I F20GM2802.tsv -I F20GM2828.tsv -I F20GM2948.tsv -I F20GM2954.tsv -I F20GM2971.tsv -I F20GM3262-3291.tsv -I F20GM3268.tsv -I F20GM3362-3385.tsv -I F21GM0083.tsv -I F21GM0128.tsv -I F21GM0133.tsv -I F21GM0135.tsv -I F21GM0214.tsv -I F21GM0314.tsv -I F21GM0503.tsv -I F21GM0668.tsv -I F21GM0692.tsv -I F21GM0743.tsv -I F21GM0747.tsv -I F21GM0837.tsv -I F21GM0940-.tsv -I F21GM0942.tsv -I F21GM1138.tsv -I F21GM1172.tsv -I F21GM1283.tsv -I F21GM1304.tsv -I F21GM1376.tsv -I F21GM1379.tsv -I F21GM1406.tsv -I F21GM1575.tsv -I F21GM1737.tsv -I F21GM1740.tsv -I F21GM1782.tsv -I F21GM1863.tsv -I F21GM1866.tsv -I F21GM1881.tsv -I F21GM1912.tsv -I F21GM1913.tsv -I F21GM1917.tsv -I F21GM1952.tsv -I F21GM1975.tsv -I F21GM2011.tsv -I F21GM2125.tsv -I F21GM2128-2130.tsv -I F21GM2206.tsv -I F21GM2273-2231.tsv -I F21GM2274.tsv -I F21GM2278.tsv -I F21GM2430.tsv -I F21GM2496.tsv -I F21GM2500.tsv -I F21GM2577.tsv -I F21GM2661.tsv -I F21GM2698.tsv -I F21GM2727.tsv -I F21GM2796-2797.tsv -I F21GM2845.tsv -I F21GM3013.tsv -I F21GM3038.tsv -I F21GM3229.tsv -I F21GM3249.tsv -I F21GM3252.tsv -I F21GM3284.tsv -I F21GM3301.tsv -I F21GM3308.tsv -I F21GM3347.tsv -I F21GM3398.tsv -I F21GM3429.tsv -I F21GM3542.tsv -I F21GM3561.tsv -I F21GM3581.tsv -I F21GM3587.tsv -I F21GM3588.tsv -I F21GM3685.tsv -I F21GM3711.tsv -I F21GM3754.tsv -I F21GM3784.tsv -I F21GM3786.tsv -I F21GM3787.tsv -I M20GM2187.tsv -I M20GM2190.tsv -I M20GM2191.tsv -I M20GM2291.tsv -I M20GM2340.tsv -I M20GM2373-2380.tsv -I M20GM2374.tsv -I M20GM2450.tsv -I M20GM2453.tsv -I M20GM2518.tsv -I M20GM2557.tsv -I M20GM2575.tsv -I M20GM2613.tsv -I M20GM2615.tsv -I M20GM2679.tsv -I M20GM2771.tsv -I M20GM2864.tsv -I M20GM2883.tsv -I M20GM2884.tsv -I M20GM2955.tsv -I M20GM3057.tsv -I M20GM3107.tsv -I M20GM3146.tsv -I M20GM3207.tsv -I M20GM3290.tsv -I M20GM3319.tsv -I M20GM3389.tsv -I M20GM782.tsv -I M21GM0031.tsv -I M21GM0034.tsv -I M21GM0091.tsv -I M21GM0137.tsv -I M21GM0188.tsv -I M21GM0195.tsv -I M21GM0199.tsv -I M21GM0217.tsv -I M21GM0228.tsv -I M21GM0316.tsv -I M21GM0385.tsv -I M21GM0398.tsv -I M21GM0540.tsv -I M21GM0541.tsv -I M21GM0630-634.tsv -I M21GM0633.tsv -I M21GM0691.tsv -I M21GM0719.tsv -I M21GM0778.tsv -I M21GM1193.tsv -I M21GM1307.tsv -I M21GM1354.tsv -I M21GM1356.tsv -I M21GM1382.tsv -I M21GM1401.tsv -I M21GM1450.tsv -I M21GM1473.tsv -I M21GM1619.tsv -I M21GM1696.tsv -I M21GM1871.tsv -I M21GM1887.tsv -I M21GM1937.tsv -I M21GM1955.tsv -I M21GM1998.tsv -I M21GM2010.tsv -I M21GM2014.tsv -I M21GM2065.tsv -I M21GM2191.tsv -I M21GM2363.tsv -I M21GM2431.tsv -I M21GM2685.tsv -I M21GM2935.tsv -I M21GM2970.tsv -I M21GM2976.tsv -I M21GM3004-3008.tsv -I M21GM3078.tsv -I M21GM3225.tsv -I M21GM3250.tsv -I M21GM3251.tsv -I M21GM3253.tsv -I M21GM3334.tsv -I M21GM3340.tsv -I M21GM3396.tsv -I M21GM3432.tsv -I M21GM3490.tsv -I M21GM3580.tsv -I M21GM3586.tsv -I M21GM3589.tsv -I M21GM3590.tsv -I M21GM3597.tsv -I M21GM3605.tsv -I M21GM3633.tsv -I M21GM3634.tsv -I M21GM3669.tsv -I M21GM3727.tsv --contig-ploidy-priors 4.tsv --output . --output-prefix ploidy --verbosity DEBUG


gatk GermlineCNVCaller --run-mode COHORT -L gc.filtered.interval_list  --interval-merging-rule OVERLAPPING_ONLY --contig-ploidy-calls /gatk/my_data/ploidy-calls/ -I F20GM2170.tsv -I F20GM2213.tsv -I F20GM2236.tsv -I F20GM2247.tsv -I F20GM2330.tsv -I F20GM2370.tsv -I F20GM2426.tsv -I F20GM2452.tsv -I F20GM2593.tsv -I F20GM2614.tsv -I F20GM2631.tsv -I F20GM2635.tsv -I F20GM2747.tsv -I F20GM2795.tsv -I F20GM2802.tsv -I F20GM2828.tsv -I F20GM2948.tsv -I F20GM2954.tsv -I F20GM2971.tsv -I F20GM3262-3291.tsv -I F20GM3268.tsv -I F20GM3362-3385.tsv -I F21GM0083.tsv -I F21GM0128.tsv -I F21GM0133.tsv -I F21GM0135.tsv -I F21GM0214.tsv -I F21GM0314.tsv -I F21GM0503.tsv -I F21GM0668.tsv -I F21GM0692.tsv -I F21GM0743.tsv -I F21GM0747.tsv -I F21GM0837.tsv -I F21GM0940-.tsv -I F21GM0942.tsv -I F21GM1138.tsv -I F21GM1172.tsv -I F21GM1283.tsv -I F21GM1304.tsv -I F21GM1376.tsv -I F21GM1379.tsv -I F21GM1406.tsv -I F21GM1575.tsv -I F21GM1737.tsv -I F21GM1740.tsv -I F21GM1782.tsv -I F21GM1863.tsv -I F21GM1866.tsv -I F21GM1881.tsv -I F21GM1912.tsv -I F21GM1913.tsv -I F21GM1917.tsv -I F21GM1952.tsv -I F21GM1975.tsv -I F21GM2011.tsv -I F21GM2125.tsv -I F21GM2128-2130.tsv -I F21GM2206.tsv -I F21GM2273-2231.tsv -I F21GM2274.tsv -I F21GM2278.tsv -I F21GM2430.tsv -I F21GM2496.tsv -I F21GM2500.tsv -I F21GM2577.tsv -I F21GM2661.tsv -I F21GM2698.tsv -I F21GM2727.tsv -I F21GM2796-2797.tsv -I F21GM2845.tsv -I F21GM3013.tsv -I F21GM3038.tsv -I F21GM3229.tsv -I F21GM3249.tsv -I F21GM3252.tsv -I F21GM3284.tsv -I F21GM3301.tsv -I F21GM3308.tsv -I F21GM3347.tsv -I F21GM3398.tsv -I F21GM3429.tsv -I F21GM3542.tsv -I F21GM3561.tsv -I F21GM3581.tsv -I F21GM3587.tsv -I F21GM3588.tsv -I F21GM3685.tsv -I F21GM3711.tsv -I F21GM3754.tsv -I F21GM3784.tsv -I F21GM3786.tsv -I F21GM3787.tsv -I M20GM2187.tsv -I M20GM2190.tsv -I M20GM2191.tsv -I M20GM2291.tsv -I M20GM2340.tsv -I M20GM2373-2380.tsv -I M20GM2374.tsv -I M20GM2450.tsv -I M20GM2453.tsv -I M20GM2518.tsv -I M20GM2557.tsv -I M20GM2575.tsv -I M20GM2613.tsv -I M20GM2615.tsv -I M20GM2679.tsv -I M20GM2771.tsv -I M20GM2864.tsv -I M20GM2883.tsv -I M20GM2884.tsv -I M20GM2955.tsv -I M20GM3057.tsv -I M20GM3107.tsv -I M20GM3146.tsv -I M20GM3207.tsv -I M20GM3290.tsv -I M20GM3319.tsv -I M20GM3389.tsv -I M20GM782.tsv -I M21GM0031.tsv -I M21GM0034.tsv -I M21GM0091.tsv -I M21GM0137.tsv -I M21GM0188.tsv -I M21GM0195.tsv -I M21GM0199.tsv -I M21GM0217.tsv -I M21GM0228.tsv -I M21GM0316.tsv -I M21GM0385.tsv -I M21GM0398.tsv -I M21GM0540.tsv -I M21GM0541.tsv -I M21GM0630-634.tsv -I M21GM0633.tsv -I M21GM0691.tsv -I M21GM0719.tsv -I M21GM0778.tsv -I M21GM1193.tsv -I M21GM1307.tsv -I M21GM1354.tsv -I M21GM1356.tsv -I M21GM1382.tsv -I M21GM1401.tsv -I M21GM1450.tsv -I M21GM1473.tsv -I M21GM1619.tsv -I M21GM1696.tsv -I M21GM1871.tsv -I M21GM1887.tsv -I M21GM1937.tsv -I M21GM1955.tsv -I M21GM1998.tsv -I M21GM2010.tsv -I M21GM2014.tsv -I M21GM2065.tsv -I M21GM2191.tsv -I M21GM2363.tsv -I M21GM2431.tsv -I M21GM2685.tsv -I M21GM2935.tsv -I M21GM2970.tsv -I M21GM2976.tsv -I M21GM3004-3008.tsv -I M21GM3078.tsv -I M21GM3225.tsv -I M21GM3250.tsv -I M21GM3251.tsv -I M21GM3253.tsv -I M21GM3334.tsv -I M21GM3340.tsv -I M21GM3396.tsv -I M21GM3432.tsv -I M21GM3490.tsv -I M21GM3580.tsv -I M21GM3586.tsv -I M21GM3589.tsv -I M21GM3590.tsv -I M21GM3597.tsv -I M21GM3605.tsv -I M21GM3633.tsv -I M21GM3634.tsv -I M21GM3669.tsv -I M21GM3727.tsv --output output_dir/ --output-prefix normal_cohort_run



mkdir output5

for ((i=0; i<$((`ls $bamDir | wc -l` / 2)); i++)); do gatk PostprocessGermlineCNVCalls --model-shard-path output_dir/normal_cohort_run-model/ --sample-index $i --calls-shard-path output_dir/normal_cohort_run-calls --allosomal-contig chrX --allosomal-contig chrY --contig-ploidy-calls ploidy-calls --output-genotyped-intervals output5/sample_${i}_cohort.vcf.gz --output-genotyped-segments output5/sample_${i}_segment.cohort.vcf.gz --output-denoised-copy-ratios output5/sample_${i}_ratio.txt --java-options -DGATK_STACKTRACE_ON_USER_EXCEPTION=true; sampleName=`zcat output5/sample_${i}_cohort.vcf.gz | grep "#CHROM" | cut -f 10`; echo -e $sampleName"\tSample_"$i >> output5/sample.list; mv output5/sample_${i}_cohort.vcf.gz output5/${sampleName}_cohort.vcf.gz; mv output5/sample_${i}_cohort.vcf.gz.tbi output5/${sampleName}_cohort.vcf.gz.tbi; mv output5/sample_${i}_segment.cohort.vcf.gz output5/${sampleName}_segment.cohort.vcf.gz; mv output5/sample_${i}_segment.cohort.vcf.gz.tbi output5/${sampleName}_segment.cohort.vcf.gz.tbi; mv output5/sample_${i}_ratio.txt output5/${sampleName}_ratio.txt; done
 




gzip -dv *_cohort.vcf.gz
 for i in *_cohort.vcf; do grep -v '\0:2' $i > ${i/.vcf/filterd.vcf}; done
 for i in *_cohort.vcf; do grep -v '\0:1' $i > ${i/.vcf/filterdd.vcf}; done
 for i in *filterd.vcf; do grep -v '\contig' $i > ${i/filterdd.vcf/filterd1.vcf}; done
  for i in *filterd1.vcf; do grep -v '\Number' $i > ${i/filterd1.vcf/filterd2.vcf}; done
  for i in *filterd2.vcf; do grep -v '\fileformat' $i > ${i/filterd2.vcf/filterd31.vcf}; done
  for i in *filterd31.vcf; do grep -v '\PostprocessGermlineCNVCalls' $i > ${i/filterd31.vcf/filterd4.vcf}; done
  

 cat *filterd4.vcf > 2.vcf
 
 
 
 
 
 
 
