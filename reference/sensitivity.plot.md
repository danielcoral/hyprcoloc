# Sensitivity plot

`sensitivity.plot` is a function which repeatedly calls the hyprcoloc
function to compute a similarity matrix which illustrates how strongly
clustered/colocalized pairs of traits are across different input
thresholds and priors

## Usage

``` r
sensitivity.plot(
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
  reg.thresh = c(0.6, 0.7, 0.8, 0.9),
  align.thresh = c(0.6, 0.7, 0.8, 0.9),
  prior.1 = 1e-04,
  prior.c = c(0.02, 0.01, 0.005),
  prior.12 = NULL,
  uniform.priors = FALSE,
  ind.traits = TRUE,
  equal.thresholds = FALSE,
  similarity.matrix = FALSE
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

  a vector of regional probability thresholds

- align.thresh:

  a vector of alignment probability thresholds

- prior.1:

  prior probability of a SNP being associated with one trait

- prior.c:

  vector of conditional colocalization priors: where prior.c is the
  probability of a SNP being associated with an additional trait given
  that the SNP is associated with at least 1 other trait

- prior.12:

  COLOC prior p12: prior probability of a SNP being associated with any
  two traits

- uniform.priors:

  uniform priors

- ind.traits:

  are the traits independent or to be treated as independent

- equal.thresholds:

  fix the regional and alignment thresholds to be equal

- similarity.matrix:

  To be viewed as a similarity matrix. Default FALSE.

## Author

Christopher Foley <chris.neal.foley@gmail.com> & James Staley
<jrstaley95@gmail.com>
