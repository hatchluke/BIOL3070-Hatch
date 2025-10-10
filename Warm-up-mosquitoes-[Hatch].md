Warm-up Mini-Report: Mosquito Blood Hosts in Salt Lake City, Utah
================
Luke Hatch
2025-10-10

- [ABSTRACT](#abstract)
- [BACKGROUND](#background)
  - [Question](#question)
  - [Hypothesis](#hypothesis)
  - [Prediction](#prediction)
- [METHODS](#methods)
  - [1st Analysis (Barplot)](#1st-analysis-barplot)
  - [2nd Analysis (Logistic/Linear
    Regressions)](#2nd-analysis-logisticlinear-regressions)
- [DISCUSSION](#discussion)
  - [Interpretation of Analysis 1](#interpretation-of-analysis-1)
  - [Interpretation of Analysis 2](#interpretation-of-analysis-2)
- [CONCLUSION](#conclusion)
- [REFERENCES](#references)

# ABSTRACT

This study investigated the role of different species as West Nile Virus
reservoirs in the Salt Lake City area. The proposed question was: Which
bird species are the highest amplifying hosts of WNV within Salt Lake
City? To determine this, mosquito bloodmeals were collected from various
areas known to contain or not contain WNV. These bloodmeals were then
amplified through PCR, sequenced, and BLASTed to determine from which
organisms they came from. Results showed a positive and significant
correlation between WNV presence and house finch bloodmeals. Based on
this data, it was determined where house finches are present, there is a
higher likelihood of finding WNV. Additionally, the more mosquitos
consume bloodmeals from house finches, the more likely the spread of
WNV.

# BACKGROUND

The West Nile Virus (WNV) system is the process in which WNV remains and
spreads throughout an environment. Simply put, it is the cycle of the
virus that maintains its existence. For example, WNV may be present in a
bird, which is then bitten by a mosquito. This transfers the virus to
the mosquito, a vector. The mosquito can then feed on another bird or
mammal, spreading WNV to a new host. WNV can be fatal in many animals,
especially horses (Dr. Greg White). Thus, it is important to understand
the movement of this virus through mosquitos and prevent further spread.
One way to study this system is through mosquito bloodmeals. Mosquito
bloodmeals are studied through extracting the DNA from the blood in
mosquitos (from hosts they’ve fed from). This DNA is ran through PCR,
where specific sections of the host DNA are amplified for analysis in
sequencing. The sequenced DNA can then be matched with known DNA
sequences to determine what host species the bloodmeal came from. This
process can be seen in the research of Komar et al. (2003) as they
examined American birds with the New York 1999 strain of WNV. Birds tend
to have high viremia, meaning they hold higher concentrations of the
virus in their blood. This makes them great hosts for WNV and it’s
spread from mosquito bloodmeals. Komar et al. (2003) examined 25 bird
species after infection to see their viremia levels and survival rates
of WNV. One species in particular, the House Finch, is a very common
sight in Salt Lake City and showed high WNV reservoir competence in
their research. This is also demonstrated in the viremia bar plot below,
where house finches tend to “hold onto” the virus longer than other
birds. With these findings, we could expect to see higher levels of WNV
present in mosquitos’ house finch bloodmeals in our research.

``` r
# Manually transcribe duration (mean, lo, hi) from the last table column
duration <- data.frame(
  Bird = c("Canada Goose","Mallard", 
           "American Kestrel","Northern Bobwhite",
           "Japanese Quail","Ring-necked Pheasant",
           "American Coot","Killdeer",
           "Ring-billed Gull","Mourning Dove",
           "Rock Dove","Monk Parakeet",
           "Budgerigar","Great Horned Owl",
           "Northern Flicker","Blue Jay",
           "Black-billed Magpie","American Crow",
           "Fish Crow","American Robin",
           "European Starling","Red-winged Blackbird",
           "Common Grackle","House Finch","House Sparrow"),
  mean = c(4.0,4.0,4.5,4.0,1.3,3.7,4.0,4.5,5.5,3.7,3.2,2.7,1.7,6.0,4.0,
           4.0,5.0,3.8,5.0,4.5,3.2,3.0,3.3,6.0,4.5),
  lo   = c(3,4,4,3,0,3,4,4,4,3,3,1,0,6,3,
           3,5,3,4,4,3,3,3,5,2),
  hi   = c(5,4,5,5,4,4,4,5,7,4,4,4,4,6,5,
           5,5,5,7,5,4,3,4,7,6)
)

# Choose some colors
cols <- c(rainbow(30)[c(10:29,1:5)])  # rainbow colors

# horizontal barplot
par(mar=c(5,12,2,2))  # wider left margin for names
bp <- barplot(duration$mean, horiz=TRUE, names.arg=duration$Bird,
              las=1, col=cols, xlab="Days of detectable viremia", xlim=c(0,7))

# add error bars
arrows(duration$lo, bp, duration$hi, bp,
       angle=90, code=3, length=0.05, col="black", xpd=TRUE)
```

<img src="Warm-up-mosquitoes-[Hatch]_files/figure-gfm/viremia-1.png" style="display: block; margin: auto auto auto 0;" />

## Question

Which bird species are the highest amplifying hosts of WNV within Salt
Lake City?

## Hypothesis

House finches are acting as important reservoirs for WNV in Salt Lake
City.

## Prediction

Due to research showing higher viremia in house finches (common species
in SLC area), locations known to have WNV will have higher bloodmeal
counts from house finches and sparrows than other bird species in the
same area. It would also be expected to see higher bloodmeal counts
overall in WNV areas compared to non-WNV areas.

# METHODS

Mosquitos were trapped in various areas known to either have or not have
the WNV present. They were trapped using either a Gravid or CO2 trap.
Following the collection of mosquitos, the mosquitos were smashed/ground
up to extract the DNA from their bloodmeals. The DNA was amplified and
sequenced to compare with known DNA sequences in order to identify what
hosts the mosquitos were feeding on. After identifying each species, the
data was placed in a file with the DNA sequences and associated species.
This data was then analyzed to answer the research question and test for
significance. This was done in R-studio (see analysis 1 and 2).

## 1st Analysis (Barplot)

The code chunk below aggregates the number of host counts per location
and produces a bar plot with bloodmeal count on the x-axis and each
species on the y-axis. The bar plot shows mosquito bloodmeals between
areas with known WNV-negative pools and areas with known WNV-positive
pools among different bird species. The bar plot provides a visual
comparison to quickly distinguish which bird species had the highest
bloodmeal counts in each area.

``` r
## import counts_matrix: data.frame with column 'loc_positives' (0/1) and host columns 'host_*'
counts_matrix <- read.csv("./bloodmeal_plusWNV_for_BIOL3070.csv")

## 1) Identify host columns
host_cols <- grep("^host_", names(counts_matrix), value = TRUE)

if (length(host_cols) == 0) {
  stop("No columns matching '^host_' were found in counts_matrix.")
}

## 2) Ensure loc_positives is present and has both levels 0 and 1 where possible
counts_matrix$loc_positives <- factor(counts_matrix$loc_positives, levels = c(0, 1))

## 3) Aggregate host counts by loc_positives
agg <- stats::aggregate(
  counts_matrix[, host_cols, drop = FALSE],
  by = list(loc_positives = counts_matrix$loc_positives),
  FUN = function(x) sum(as.numeric(x), na.rm = TRUE)
)

## make sure both rows exist; if one is missing, add a zero row
need_levels <- setdiff(levels(counts_matrix$loc_positives), as.character(agg$loc_positives))
if (length(need_levels)) {
  zero_row <- as.list(rep(0, length(host_cols)))
  names(zero_row) <- host_cols
  for (lv in need_levels) {
    agg <- rbind(agg, c(lv, zero_row))
  }
  ## restore proper type
  agg$loc_positives <- factor(agg$loc_positives, levels = c("0","1"))
  ## coerce numeric host cols (they may have become character after rbind)
  for (hc in host_cols) agg[[hc]] <- as.numeric(agg[[hc]])
  agg <- agg[order(agg$loc_positives), , drop = FALSE]
}

## 4) Decide species order (overall abundance, descending)
overall <- colSums(agg[, host_cols, drop = FALSE], na.rm = TRUE)
host_order <- names(sort(overall, decreasing = TRUE))
species_labels <- rev(sub("^host_", "", host_order))  # nicer labels

## 5) Build count vectors for each panel in the SAME order
counts0 <- rev(as.numeric(agg[agg$loc_positives == 0, host_order, drop = TRUE]))
counts1 <- rev(as.numeric(agg[agg$loc_positives == 1, host_order, drop = TRUE]))

## 6) Colors: reuse your existing 'cols' if it exists and is long enough; otherwise generate
if (exists("cols") && length(cols) >= length(host_order)) {
  species_colors <- setNames(cols[seq_along(host_order)], species_labels)
} else {
  species_colors <- setNames(rainbow(length(host_order) + 10)[seq_along(host_order)], species_labels)
}

## 7) Shared x-limit for comparability
xmax <- max(c(counts0, counts1), na.rm = TRUE)
xmax <- if (is.finite(xmax)) xmax else 1
xlim_use <- c(0, xmax * 1.08)

## 8) Plot: two horizontal barplots with identical order and colors
op <- par(mfrow = c(1, 2),
          mar = c(4, 12, 3, 2),  # big left margin for species names
          xaxs = "i")           # a bit tighter axis padding

## Panel A: No WNV detected (loc_positives = 0)
barplot(height = counts0,
        names.arg = species_labels, 
        cex.names = .5,
        cex.axis = .5,
        col = rev(unname(species_colors[species_labels])),
        horiz = TRUE,
        las = 1,
        xlab = "Bloodmeal counts",
        main = "Locations WNV (-)",
        xlim = xlim_use)

## Panel B: WNV detected (loc_positives = 1)
barplot(height = counts1,
        names.arg = species_labels, 
        cex.names = .5,
        cex.axis = .5,
        col = rev(unname(species_colors[species_labels])),
        horiz = TRUE,
        las = 1,
        xlab = "Bloodmeal counts",
        main = "Locations WNV (+)",
        xlim = xlim_use)
```

![](Warm-up-mosquitoes-%5BHatch%5D_files/figure-gfm/first-analysis-1.png)<!-- -->

``` r
par(op)

## Keep the colors mapping for reuse elsewhere
host_species_colors <- species_colors
```

## 2nd Analysis (Logistic/Linear Regressions)

This analysis determines whether it is simply the presence or the number
of house finch bloodmeals that can predict whether a site has WNV +
pools (binary) or a higher WNV positivity rate (numeric). Essentially,
this statistical test allows the formal evaluation of the relationships
visualized above in the barplots and determines whether the house finch
is influencing whether WNV is or is not found in an area.

``` r
# second-analysis-or-plot, glm with house finch alone against binary +/_
glm1 <- glm(loc_positives ~ host_House_finch,
            data = counts_matrix,
            family = binomial)
summary(glm1)
```

    ## 
    ## Call:
    ## glm(formula = loc_positives ~ host_House_finch, family = binomial, 
    ##     data = counts_matrix)
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error z value Pr(>|z|)  
    ## (Intercept)       -0.1709     0.1053  -1.622   0.1047  
    ## host_House_finch   0.3468     0.1586   2.187   0.0287 *
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 546.67  on 394  degrees of freedom
    ## Residual deviance: 539.69  on 393  degrees of freedom
    ## AIC: 543.69
    ## 
    ## Number of Fisher Scoring iterations: 4

``` r
#glm with house-finch alone against positivity rate
glm2 <- glm(loc_rate ~ host_House_finch,
            data = counts_matrix)
summary(glm2)
```

    ## 
    ## Call:
    ## glm(formula = loc_rate ~ host_House_finch, data = counts_matrix)
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)      0.054861   0.006755   8.122 6.07e-15 ***
    ## host_House_finch 0.027479   0.006662   4.125 4.54e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for gaussian family taken to be 0.01689032)
    ## 
    ##     Null deviance: 6.8915  on 392  degrees of freedom
    ## Residual deviance: 6.6041  on 391  degrees of freedom
    ##   (2 observations deleted due to missingness)
    ## AIC: -484.56
    ## 
    ## Number of Fisher Scoring iterations: 2

# DISCUSSION

## Interpretation of Analysis 1

In analysis 1, results in the barplots show higher bloodmeal counts in
both the + and - WNV areas for the house finch, house sparrow, and
american robin. In both the + and - areas, these birds are in the same
respective order from highest to lower bloodmeal count. These results
from analysis 1 support the hypothesis (house finches are important
reservoirs in SLC) on the surface/visual level, but the second analysis
clarifies significance into the numerical role of house finches
specifically.

## Interpretation of Analysis 2

The second analysis was ran in two parts. The first section of the code
chunk was evaluating the data from the house finch for any positive test
of WNV at the locations. This produced a statistically significant
result (0.0287). After this was determined to be significant, additional
analysis was done to determine relativity of each positive test at each
location. This was also statistically significant (4.54e-05), meaning
there was a higher likelihood of WNV presence where house finches were
found. This supports our hypothesis of house finches acting as important
reservoirs.

# CONCLUSION

Overall, the results support the hypothesis - house finches showed
significance in acting as reservoirs for WNV. This was shown by the data
organized in the bar plot demonstrating higher bloodmeals in both
positive and negative areas for WNV in house finches. The second
analysis delved deeper into the house finch specifically and tested for
numerical significance. It can be concluded that as the number of house
finch bloodmeals increases, as does the likelihood of finding WNV.
Similarly, the more house finch meals mosquitos have, the more likely
the spread - and therefore amplification - of WNV in SLC.

# REFERENCES

1.  Komar, N., Langevin, S., Hinten, S., Nemeth, N., Edwards, E.,
    Hettler, D., Davis, B., Bowen, R., & Bunning, M. (2003).
    Experimental infection of North American birds with the New York
    1999 strain of West Nile virus. Emerging Infectious Diseases, 9(3),
    311–322. <https://doi.org/10.3201/eid0903.020628>

2.  ChatGPT. OpenAI, version Jan 2025. Used as a reference for functions
    such as plot() and to correct syntax errors. Accessed 2025-10-10.
