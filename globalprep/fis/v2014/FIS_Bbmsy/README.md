Generating B_bmsy values using R script
=====================


Primary data
====================
Hex outputs > .RData.

I believe these were generated using the following scripts:
cmsy_mpi_ohi_originalPrior_added0s_Aug162014.R
cmsy_mpi_ohi_originalPrior_no0s_Aug162014.R
cmsy_mpi_ohi_uniformPrior_no0s_Aug162014.R
cmsy_mpi_ohi_v0_FinalBio2CT_uniformPrior2_OHIrun.R is the script from Kristin.  Should basically be the same as ours, but her's outputs some additional variables that we do not need. I believe the other similarly named files were the scripts used to run our data and scenarios on the super computer to generate the Hex outputs.

stock_resil_06cutoff.... files used to assign b/bmsy scores per stock based on regions resilience scores

compareCMSYmodels.R --- used to take 5 year running mean


R files
=====================
* B_bmsyDataTest.R: includes code to run parallel processing on
Neptune to calculate b/bmsy and many different scenarios to
test whether I could duplicate Kristin's results and to test how different versions of the model perform. Sources the various cmsy_... models to calculate b/bmsy (see: for description):

cmsy_constrained 
cmsy_constrained_res 
cmsy_relaxed 
cmsy_uniform 

In general, this accesses data from the "raw" file and saves to the "output" file 

Description
===========
A few notes: 
* Code does not support the use of resilience as an r prior, so the default prior is used. This is how the code was run before.
* make sure that the years are sorted for each species before you run
* Also, we never actually used the geometric mean. It was always the arithmetic mean. I double checked this in the code. Not sure where that confusion came in, but in the WG Coilin had argued for the arithmetic mean over the geometric mean, so that's what we went with. I did modify the code for Hex to output the geometric mean and median so you guys could explore those options. You'll see that in the output as extra columns. I did not output the upper and lower CI bounds at this point and for the WG we are still using the arithmetic mean. One of Andy's concerns was that the B/Bmsy's were a bit 'pessimistic'...so using the geometric mean would likely result in lower B/Bmsys, so probably arithmetic mean is better?


Raw data
==================
* cmsy.ohi.df_Jul292014.csv = b/bmsy data generated by Kristin using the new priors

* OHICatchHistoryCMSY_added0s_07_21_2014.csv = Catch history provided by Katie for b/bmsy model.  Trailing zeros are added and NA's are replaced with zeroes.

Notes for preparing data for CMSY analysis
===========================================
Data should look like this for CMSY scripts:

stock_id            res           ct   yr
Ablennes hians_51   NA            27.05 1985
Ablennes hians_51   NA            38.18 1990
Ablennes hians_51   NA            54.10 1991
...

Analysis is done at the FAO scale (id following species name)

1.  select species (taxonkey>=600000)
2. create unique stock_id by pasting taxon name and fao region
3. sum catch for duplicated stock from the same FAO region/year (especially if data is reported at SAUP level)
4. limit to stock with 10 years of non-zero/NA reported catch
5. order by stock_id and year


