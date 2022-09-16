# casper

CASPER (Context-Aware Scheme for Paired-End Read) is state-of-the art merging tool in terms of accuracy and robustness. 
Using this sophisticated merging method, we could get elongated reads from the forward and reverse reads.
<br><br>

### Main Algorithm
CASPER uses quality scores and k-mer frequency for merging. <br>
When the difference between the quality scores of mismathcing base
is significant, CASPER relies on the quality scores for correction.
If not, CASPER instead examines k-mer-based contexts around the mismatch
and makes a statistical decision.

<p>

### Usage

Use -j option, if Jellyfish is not installed

=============================================================================
<br>
Usage   : ./casper forward.fastq reverse.fastq -j [OPTIONS]

    [MANDATORY]
     Input forward side FASTQ file first.
     Input reverse side FASTQ file following the forward file.

    [OPTIONS]
     -t  <int>   The number of threads for parallel proessing
                 (default=96 up to maximum number of system limit)

     -k  <int>   The size of k-mers used to represent contexts around
                 mismatching bases.                   (default=17)

     -d  <int>   Threshold for difference of quality-scores
                 Context-based mismatch resolution starts if quality scores
                 differ less than 'd'.
                 Smaller value indicates more trust to quality scores than k-mer context.
                 (default=19)

     -g  <float> Threshold for mismatch ratio of best overlap region
                 CASPER gives up merging if the mismatch ratio in the overlap
                 is greater than 'g' and leaves the two reads unmerged.
                 If all the reads have overlap then set 'g' as default or higher.
                 Or if you want sensitive for not merging(TN) then set 'g' as 
                 lower than default. (0.27 or lower is recommended)
                 (default=0.5)

     -w  <int>   The minimum length (in bp) of the overlap between forward
                 and reverse reads.               (default=10bp)

     -o  <str>   Prefix of output          (default=casper)
                 By default, 'casper.fastq' <- merged output is generated.

     -j          Internal naive k-mer counting method is used instead of Jellyfish.
                 By default (without this option), Jellyfish (for k-mer counting)
                 is used to speed up.

     -l          CASPER can generate the unmerged output file.
                 prefix_for_left.fastq, prefix_rev_left.fastq for forward, reverse 
                 individually.

     -h          Help for usage information

     -v          Version information

      *          CASPER do not need PHRED offset. Either PHRED+64 or PHRED+33 is OK.
                 Only the difference between two quality scores instead of absolute
                 value is used.

    [Examples]
      case1: using Jellyfish, output prefix is out, k-mer=19, threads=6,
      $ casper forward.fastq reverse.fastq -o out -k 19 -t 6
      <br>
      case2: without Jellyfish, give up threshold=0.27
      $ casper forward.fastq reverse.fastq -j -g 0.27
=============================================================================
