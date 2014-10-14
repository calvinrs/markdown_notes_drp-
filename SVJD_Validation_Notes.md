RW SVJD - Validation Current Runs
=================================

Runs
-----

We run 3 separate ESG runs to generate all of the Validation stats we require.


##General Validation Settings##

> We do not seem to update the correlation matrix in the default simulation file - this is using 2012 numbers. 
Should we be using up-to-date correlations?  

We use a single default economy, denominated in USD. We use the Ext2FBK model for nominal rates.


We setup the Volatility of the first EquityAssetFactor as an SVJD model, and calibrate it with the latest values. 
The Risk-Neutral parameters are left as their default values.  

Factors 2-6 are left at their default values - these rarely change.

We set up a ParentEquityAsset for each of the Equity Assets we have calibrated. These use the default asset as the base economy.



Validation 1
------------

We run the ESG over 360 monthly steps, for a 30y horizon. We use 20,000 trials in this run. Seed = 123.  


We calibrate, for each ParentEquityAsset:

+ The factor loadings
+ The SVJD Volatility - only MeanReversionLevel and Initial Value are set.

We output:

In outfile 1:

+ Cash TRI, for the dummy economy
+ Equity Asset TRI, for each Equity Asset
	+ On each trial subset complete event, we calculate the XS return for each TRI

In outfile 2:

+ Equity Asset Volatility, for each Equity Asset

We then calculate:
	
For each equity asset:
+ For all output timesteps (months [12,24,36,48,60,72,84,96,108,120,348,359,360])
	+ Return the standard deviation of the log of the XS returns / Sqrt(Time in years) *Annualised Vol?*
+ For months [12, 360]
	+ Return the correlation between the standard deviation of the log of the XS returns
		+ These are labelled 1y [12] and Unconditional [360]  
+ For month [12] (or "1y")
	+ Calculate Percentiles at N = {**some pretty granular range, around 0.01 ticks**}
		+ Percentiles of the *change* in XS Total Return (*XS Total Return[i]*)
	+ Append "Mean" and "Volatility" values to the percentiles
		+ "Mean" is the average return given by the the *change* in XS Total Return (*XS Total Return[i]*)
		+ "Volatility" is the Standard Deviation of the *log change* in XS Total Return (*ln(XS Total Return[i])*)
+ For month [360] (or "unconditional")
	+ Calculate Percentiles at N = {**some less granular range, around 0.1 ticks**}
		+ Percentiles of the *change* in XS Total Return (*XS Total Return[360] / XS Total Return [359]*)
	+ Append "Mean" and "Volatility" values to the percentiles
		+ "Mean" is the average return given by the the *change* in XS Total Return (*XS Total Return[360] / XS Total Return [359]*)
		+ "Volatility" is the Standard Deviation of the *log change* in XS Total Return (*ln(XS Total Return[360] / XS Total Return [359])*)
		
Validation 2
------------

The theme of this run is to calculate the **Short Term Validation Statistics**

We run the ESG over 12 monthly steps, for a 1y horizon. We use 20,000 trials in this run. Seed = 123.  

Output files created are **Identical** to the first validation.

> Flashback - this validation was originally one single run, but memory issues occurred when the number of equity assets increased, so we had to split the runs.

Calculate, for each equity asset:
+ At month[1], calculate:
	+ Mean - average of the *log* of the XS Total Return Index
	+ Volatility - standard deviation of the log of the XS return index / Sqrt(Time in years)
	+ Skew - Skew of the log of the XS return index 
	+ Kurtosis - Kurtosis of the log of the XS return index 
	+ 99.5 Percentile - Percentile of the *log change* in XS Total Return
	+ Standard Error at the 99.5 percentile
+ At month[12], calculate:
	+ Mean - average of the *log* of the XS Total Return Index
	+ Volatility - standard deviation of the log of the XS return index / Sqrt(Time in years)
	+ Skew - Skew of the log of the XS return index 
	+ Kurtosis - Kurtosis of the log of the XS return index 
	+ 99.5 Percentile - Percentile of the log of the XS return index 
	+ Standard Error at the 99.5 percentile
	+ Calculate the tail correlations between the percentiles at [95%, 99% 99.5%]
		+ Percentiles of the *change* in XS Total Return (*XS Total Return[12] / XS Total Return [11]*)
		+ Tail correlation is a normalised(?) count of the observed values outside of the percentiles chosen  
		
Validation 3
------------

The theme of this run is to get the one-month Unconditional Skew and Kurtosis of equity returns.

We run the ESG over 1 monthly step, for a 1m horizon. We use 5,000 trials in this run. Seed = 123.  

Again, output files created are **Identical** to the first validation.  

Calculate, for each equity asset:
+ At month[1], calculate:
	+ Skew - Skew of the log of the XS return index
	+ Kurtosis - Kurtosis of the log of the XS return index
	
> Is there any difference in these values and the 1m values calculated in run 2, other than the number of trials?