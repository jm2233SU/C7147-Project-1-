# C7147-Project-1-
# Identifying Mutations in NGS Data

## Title & Purpose

This program compares sequencing reads from fission yeastS. pombe, position by position, with a 200-nt reference to find mismatches. I wanted to see whether a genotoxic agent changes how often mutations happen, and where.

## How to run the program

Open Andrea_C7147_Project_1_assessment.ipynb in Google Colab and click Runtime, then Run all. Run the cells in order, since the first ones download the data.

## Inputs and outputs

Reference.txt is the 200-nt reference, and reads.csv has one read per row with its read_id, sample, group and sequence. Analyse_reads returns a dictionary for each read with its status, errors and mutations.  summarise_samples prints two tables per group and returns a dictionary per sample. Compare_positions prints one line per top position with the group means, the difference (treated minus control), n, and t and p from a Welch's t-test.

## Code design

I built the groups inside the main loop of  summarise_samples . Each new sample goes into a groups dictionary the first time it appears, so each group prints separately, in the order its samples show up. A separate function would have meant looping over the samples twice.

I counted mismatch positions with a dictionary rather than a list. Summarise_samples never sees the reference, so it doesn't know the read length, and a list of zeros would need that. With  .get(position, 0) , only positions with a mismatch are stored, which in the controls is just 53 to 72 of the 200.

I also used small helper functions.  get_top_positions  and  calculate_percentage  from Step 4 are reused in Step 5, and  get_sample_percentages  and  get_mean  stop the control and treated code being written twice.

## Testing

In test_summary_5, s5 has valid_reads: 0, so  compare_positions leaves it out, and the treatment n is 2, not 3.  get_top_positions({5: 2, 1: 2, 9: 4, 7: 2}) returns  [9, 1, 5], so the earlier position wins a tie. The Step 4 test copies the analysed reads, runs  summarise_samples , then uses assert to check the originals haven't changed.

