RW SVJD - Validation Requirements
=================================

Purpose
-------

Given the analysis of what we do run during an SVJD equity validation, we can state here what our requirements are from the validation process.

Required Statistics
-------------------

We wish to look at the statistics of a number of ParentEquityAsset models. This is the same coverage as for the standard multi-year BHC files.  

> As we set the "Economy" parameter to a default economy, and do not use the actual underlying interest rate models, we are not *quite* using the same model as in the BHC.
We may consider the assets that we use for validation as dummy ParentEquityAssets, that happen to share some of the same parameters.
More specifically, they have the same factor loadings and volatility model calibrations.

As we are only interested in excess returns over cash, we can ignore the choice of NYC model that we use in validations. 
We can investigate whether the choice of NYC model can be changed to increase the run-times - i.e. benchmark E2FBK with Monthly LMM(+).

Excess returns are calculated and saved into memory at each "TrialSubsetEventComplete" event that the ESG run throws.  

This is defined as:  
`EquityXSTotalReturnIndex[iTimeStep, iTrial] = EquityTotalReturnIndex[iTimeStep, iTrial] / CashTotalReturnIndex[iTimeStep, iTrial]`

Note that the Cash TRI address will vary according to the Type of NYC model used. 
The Equity TRI address remains the same for all ParentEquityAssetModel choices - i.e. it will be identical for constant and SVJD sigma models.

We wish to return the same statistics for each/ for all ParentEquityAsset models.
This means that we are unconcerned with having the granularity to specify tests per model - it will suffice to define a list of models and a list of tests, and cross-join the two.
We will however wish to *retrieve* the results on an asset-by-asset basis.  

 
 



