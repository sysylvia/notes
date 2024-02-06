---
aliases: 
tags:
  - fleeting-note
  - SocAccFP-Kisumu
share: true
---

# SocAccFP-Kisumu-AnalysisPlanNotes

1. Data
	1. Data Collection
		1. Planned
		2. Deviations/Changes
			1. Possible addition of KHIS data on sample facilities {{Ask Dickens}}
			2. Sample selection for Women’s survey: 
				1. At baseline we visited sampled buildings in a random order (order of random draw). 
				2. At end line we drew 1 batch of 18 structures in each polygon, then drew a second batch of size $n_p$ where $n_p$ was a polygon specific number based on how many structures sampled at baseline were households with eligible women such that $$n_{p}= 1.2*misses$$ where misses =  the number of sampled units out of original 18 sampled in baseline were not houses or were households without eligible women. **{{need to confirm coefficient with Abby and Ben - It Sounds like they used some sort of batching instead of a fixed inflation factor}}** 
				3. Implications
					1. This means we may overshoot in some polygons, and these would likely tend to be more industrial. We may need to adjust for this in analyses by reweighing so that each polygon is equally represented (preferred) or randomly dropping excess observations in polygons with too many observations.  
					2. Side note: The above is assuming we want to weight each polygon equally in analysis. More policy relevant would be to account for the size of the population affected by quality in each facility. The most obvious is to use population in each polygon, but a) this may be unavailable and b) I think we can do better with less work - more thoughts on this below.
	2. Primary Outcomes
	3. Secondary Outcomes
2. Baseline Descriptives 
	1. Quantitative 
		1. Sample descriptive stats +  comparison between arms
			1. Women
			2. Provider
			3. SP
	2. Qualitative 
3. Raw descriptives of intervention impacts: Control v T1 v T2
	1. Graphical Analysis: 
		1. Density plots (continuous)
		2. Bar/Forest plots (binary)
	2. Unadjusted means: Pre, post, difference, diff-in-diff
4. Qualitative
5. Main Effects
	1. Traditional ANCOVA Specification: Outcomes on baseline + strata dummies, cluster se (hc2)
		1. Plain vanilla specification: $$Y_{ij,t} = \alpha + \beta_{t_{1}}*T_{1} + \beta_{t_{2}}*T_{2} + Y_{ij,-t} + \delta_{strata} + \varepsilon_{ij} $$
		2. OLS/Linear Probability Model 
			1. For health journal, will also want to estimate impacts on binary outcomes using logit and present average marginal effects + OR following analogous specification. Also may want to use multi-level model (cluster random effects) depending on outlet. This should not be qualitatively different from OLS.
		3. Covariate-Adjusted
			1. $$Y_{ij,t} = \alpha + \beta_{t_{1}}*T_{1} + \beta_{t_{2}}*T_{2} + Y_{ij,-t} + \textcolor{blue}{X_{dl}’\gamma} + \delta_{strata} + \varepsilon_{ij} $$
			2. $\textcolor{blue}{X_{dl}’\gamma}$ = covariates selected via double-lasso covariate selection
				1. Note: Dickens may be able to provide facility-level variables related to key outcomes from past years from KHIS - this could increase power substantially
		4. Inference
			1. Tests
				1. $$\beta_{t_{1}}=0$$
				2.  $$\beta_{t_{2}}=0$$
				3.  $$\beta_{t_{1}}=\beta_{t_{2}}$$
			2. Standard errors
				1. Clustered standard errors
				2. **question for discussion:** Cluster for the main specification may be polygon following study design, but also argument for MOH defined catchment area. My sense is that we’ll need to use the latter as a robustness check or as a piece of “policy relevant analysis” section below. This will require overlaying catchment areas on the map and re-assigning sampled HH. **{{Need input from Brian}}**
			3. Multiple Hypothesis P-values
				1. Q-values, adjust across tests$*$key outcomes
6. Mechanisms
	1. **To do: List different stages/mechanisms of impact following existing Theory of Change, then identify tests (impacts on secondary outcomes and heterogeneous analysis) corresponding to each
	2. Mechanism Hypotheses
	3. Estimating impacts on secondary outcomes
		1. Group secondary outcomes according to hypothesis
		2. Within each group, construct an index if there are multiple corresponding indicators 
			1. There are different options for creating indexes. GLS weighting ala Anderson 2008 is powerful and straight-forward...Sean has code. Last I looked there weren't any good papers comparing index construction approaches, but we should check this.
			2. Estimate effects on the index using main estimating equation (same as main effects)
			3. 
7. Extension/Robustness
	1. Policy-relevant effects accounting for affected population
		1. **Note:** This may be too much for a public health or family planning journal, but I do think this is an important methodological issue that we should present somewhere. I can work with a student on the team to do this - or if there is no interest, I can pull in one of my econ students.
		2. Problem
			1. The usual way we look at effects for health-facility level interventions in cluster RCTs is to just assume relevant cluster is the formal catchment area (or political admin area) of each facility. However:
				1. In this study, we were not able to sample based on official catchment area
				2. More generally, there is a problem with this assumption when people can freely choose their facility of service. This means that whom is affected by improvement in a given facility depends on who chooses to go to that facility
				3. Moreover, in this type of intervention, quality changes at a given facility (and nearby facilities) can affect who seeks care at the facility.
			2. This suggests that assuming official clusters in the analysis would likely be biased when looking at effects on population health outcomes for women: From a policy perspective, we care about the population affected, which is not strictly defined geographically, but depends on facility choice
			3. Formally enters in two ways:
				1. The baseline level of policy relevant quality is incorrect if people choose where to seek care
				2. The changes induced by the intervention don’t just affect women who were seeking care at the facility at baseline, but also those who decide to attend the facility rather than elsewhere or deciding to forego care.
			4. **Need to formalize more** but my intuition is that we’ll underestimate the policy relevant effect of the treatment using just polygons. Here is the start of a toy model:
				1. Assume 
					1. there are two facilities, one relatively bad (B) and one good (G), who have distinct official catchment areas (and polygons)
					2. For simplicity, assume they are on a straight road 
					3. Women care about two things: quality and travel distance:
						1. Utility from attending facility f for woman w: $$U_{f}^w = U(\theta_{f}, Dist_{f,w} | \gamma) $$ where $\gamma \in [0,1]$ is disutility from distance relative to quality.
						2. assume linear utility: $$U_{f} = \theta_f - \gamma*Dist_{f,w}$$
					4. At baseline: $$\theta_G > \theta_{B}$$
				2. Baseline (single period)
					1. Two women live on the road: woman 1 lives closer to B, woman 2 closer to G, ie $Dist_{B,1}<Dist_{G,1}$ and $Dist_{B,2}>Dist_{G,2}$
					2. Case 1
					3. Case 2
				3. Dynamic - induced switching
		3. Proposed Analysis
			1. **Note: Sean is thinking more about this**
			2. Estimate intervention effects in different ways - 2 "naïve" ways using traditional cluster assignments and proposed fixes. 
				1. **Note: I have some draft ideas below, but there may be some more sophisticated ways to do this. I'd like to reach out to [Desire Kedagni](https://econ.unc.edu/directory/desire-kedagni/) an econometrician in Econ and new CPC fellow to see if he has any interesting ideas**
				2. Potential Approaches (Any other ideas here?)
					1. Defining Clusters
						1. Naive 1: Using polygons as clusters
						2. Naive 2: Using official catchment areas as clusters
						3. Using overlapping radius or travel distance
						4. Mapped market networks based on Woman survey responses of facility choice
					2. Estimation
						1. Plain vanilla - just use new cluster definition and estimate treatment effects as normal
						2. Model-based approach: First estimate demand model to get elasticities, estimate intervention effects on switching, use model to weight treatment effects
						3. ML based approaches - Estimate CATEs on switching probability using Generalized Random Forest, use to adjust...
						4. Seem like there are many options here, need to consult literature 
					3. Inference
						1. May need to bootstrap or use some more flexible cluster standard error adjustment