# HyPrColoc

`hyprcoloc` is a function used to identify clusters of colocalized
traits and candidate causal SNPs in genomic regions

## Usage

``` r
hyprcoloc(
  effect.est,
  effect.se,
  binary.outcomes = rep(0, dim(effect.est)[2]),
  trait.subset = c(1:dim(effect.est)[2]),
  trait.names = c(1:dim(effect.est)[2]),
  snp.id = c(1:dim(effect.est)[1]),
  ld.matrix = diag(1, dim(effect.est)[1], dim(effect.est)[1]),
  trait.cor = diag(1, dim(effect.est)[2], dim(effect.est)[2]),
  sample.overlap = matrix(rep(1, dim(effect.est)[2]^2), nrow = dim(effect.est)[2]),
  bb.alg = TRUE,
  bb.selection = "regional",
  reg.steps = 1,
  reg.thresh = "default",
  align.thresh = "default",
  prior.1 = 1e-04,
  prior.c = 0.02,
  prior.12 = NULL,
  sensitivity = FALSE,
  sense.1 = 1,
  sense.2 = 2,
  uniform.priors = FALSE,
  ind.traits = FALSE,
  snpscores = FALSE
)
```

## Arguments

- effect.est:

  matrix of snp regression coefficients (i.e. regression beta values) in
  the genomic region

- effect.se:

  matrix of standard errors associated with the beta values

- binary.outcomes:

  a binary vector of dimension the number of traits: 1 represents a
  binary trait 0 otherwise

- trait.subset:

  vector of trait names (or number) from the full trait list: used for
  trageted colocalization analysis in a region

- trait.names:

  vector of trait names corresponding to the columns in the effect.est
  matrix

- snp.id:

  vector of SNP IDs

- ld.matrix:

  LD matrix

- trait.cor:

  matrix of pairwise correlations between traits

- sample.overlap:

  matrix of pairwise sample overlap between traits

- bb.alg:

  branch and bound algorithm: TRUE, employ BB algorithm; FALSE, do not

- bb.selection:

  branch and bound algorithm type, e.g. regional or alignment selection

- reg.steps:

  regional step paramter

- reg.thresh:

  threshold probability beyond which traits are believed to share a
  regional association signal

- align.thresh:

  threshold probability beyond which traits are believed to align at a
  single causal variant

- prior.1:

  prior probability of a SNP being associated with one trait

- prior.c:

  conditional colocalization prior: probability of a SNP being
  associated with an additional trait given that the SNP is associated
  with at least 1 other trait

- prior.12:

  COLOC prior p12: prior probability of a SNP being associated with any
  two traits

- sensitivity:

  perform senstivity analysis

- sense.1:

  first sensitivity analysis

- sense.2:

  second sensitivity analysis

- uniform.priors:

  uniform priors

- ind.traits:

  are the traits independent or to be treated as independent

- snpscores:

  output estimated posterior probability explained each SNP

## Value

A data.frame of HyPrColoc results: each row is a cluster of colocalized
traits or is coded NA (if no colocalization is identified)

If snpscores = TRUE: additionally returns a list of posterior
probability explained by each SNPs and for each cluster of colocalized
traits identified

## Author

Christopher Foley <chris.neal.foley@gmail.com> & James Staley
<jrstaley95@gmail.com>

## Examples

``` r
# Regression coefficients and standard errors from ten GWAS studies
# (Traits 1-5, 6-8 & 9-10 are the clusters of colocalized traits)
betas <- hyprcoloc::test.betas
head(betas)
#>                      T1           T2            T3           T4           T5
#> rs6694014    0.02791630 -0.030353270 -0.0006508550 -0.015820079  0.029344113
#> rs11206477  -0.01333565 -0.007925434 -0.0220791782 -0.024533654 -0.006170044
#> rs978479     0.02789307 -0.031569213  0.0013910408 -0.016449156  0.030562284
#> rs6684892    0.01109516 -0.035811867 -0.0041582437 -0.001093336  0.021029899
#> rs149881092 -0.02058398  0.033028644  0.0732322501 -0.062564186  0.031519527
#> rs2081705    0.02804342 -0.031658194 -0.0006283532 -0.017953736  0.029719314
#>                      T6          T7          T8            T9         T10
#> rs6694014   -0.03739309 -0.05015619 -0.03963766 -0.0497666615 -0.01565694
#> rs11206477   0.08915364  0.08788543  0.07824464 -0.0364999498 -0.07140001
#> rs978479    -0.03667044 -0.04993080 -0.03999140 -0.0485026083 -0.01742117
#> rs6684892   -0.04342024 -0.05462565 -0.02610322 -0.0516383902 -0.01373675
#> rs149881092  0.03339484  0.08794777  0.06747322  0.0002877561 -0.01701753
#> rs2081705   -0.03798465 -0.05052849 -0.04150411 -0.0498085699 -0.01767236
ses <- hyprcoloc::test.ses
head(ses)
#>                     T1         T2         T3         T4         T5         T6
#> rs6694014   0.01640805 0.01601499 0.01606328 0.01595009 0.01629387 0.01602093
#> rs11206477  0.01522992 0.01486595 0.01490668 0.01480196 0.01512465 0.01484631
#> rs978479    0.01642066 0.01602706 0.01607561 0.01596228 0.01630616 0.01603341
#> rs6684892   0.01738306 0.01696379 0.01701563 0.01689661 0.01726146 0.01696989
#> rs149881092 0.03917473 0.03823671 0.03833955 0.03807307 0.03890204 0.03825441
#> rs2081705   0.01643768 0.01604368 0.01609229 0.01597868 0.01632324 0.01604975
#>                     T7         T8         T9        T10
#> rs6694014   0.01618062 0.01625153 0.01639785 0.01626775
#> rs11206477  0.01499870 0.01506721 0.01522146 0.01508189
#> rs978479    0.01619313 0.01626394 0.01641083 0.01628007
#> rs6684892   0.01713951 0.01721824 0.01737041 0.01723253
#> rs149881092 0.03863522 0.03880163 0.03916327 0.03883610
#> rs2081705   0.01620976 0.01628044 0.01642748 0.01629694

# Trait names and SNP IDs
traits <- paste0("T", 1:10)
rsid <- rownames(betas)

# Colocalisation analyses
results <- hyprcoloc(betas, ses, trait.names = traits, snp.id = rsid)
```
