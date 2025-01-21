# sysAgNPs <a href="https://github.com/xitingwang-ida/sysAgNPs"><img src="man/figures/logo.svg" align="right" height="139" /></a>

## Introduction

There is variation across AgNPs due to differences in characterization techniques and testing metrics employed in studies. To address this problem, we have developed a systematic evaluation framework called 'sysAgNPs'. Within this framework, Distribution Entropy (DE) is utilized to measure the uncertainty of  feature categories of AgNPs, Proclivity Entropy (PE) assesses the preference of these categories, and Combination Entropy (CE) quantifies the uncertainty of feature combinations of AgNPs. Additionally, a Markov chain model is employed to examine the relationships among the sub-features of AgNPs and to determine a Transition Score (TS) scoring standard that is based on steady-state probabilities. The 'sysAgNPs' framework provides metrics for evaluating AgNPs, which helps to unravel their complexity and facilitates effective comparisons among different AgNPs, thereby advancing the scientific research and application of these AgNPs.

## Installation of sysAgNPs package

```R
# Install sysAgNPs 
## 1. Install sysAgNPs from CRAN
install.packages("sysAgNPs")

## 2. Install sysAgNPs from GitHub
install.packages("devtools")
library(devtools)
devtools::install_github("xitingwang-ida/sysAgNPs")

## 3. Install sysAgNPs from Bitbucket
devtools::install_bitbucket("cindy-w/sysAgNPs")

# Load package
library(sysAgNPs)
```

## How to use sysAgNPs to evaluate AgNPs data
All the approaches of sysAgNPs are implemented as a software package that handles all the different steps of the framework creation process and makes it easy to evaluate AgNPs experimental data by adjusting a few parameters.

### Build Transition Score criteria

```R
# Import AgNPs dataset to build Transition Score criteria
data(dataset)

# Select the columns to be discretized
var_dis <- c("Synthesis methods", "pH", "Temperature (℃)", "Zeta potential (mV)","Size (nm)", "Shape", "Applications")

# Convert categorical variables into discrete variables
dis_data <- sys_discretize(dataset, var_dis)

# Build markov chain and calculate transition matrix
tran_matrix <- sys_tran(dis_data)

# Obtain the relationship between the number of iterations and tolerance
tol_iter <- sys_steady(dis_data, tran_matrix)

# Loop "n_iter" times to count the results of each iteration. The final steady-state result is reached when the number of iterations is "n_iter"

iter_prob <- sys_iter(dataset, 6, var_dis)

# Export the Transition Score criteria
TS_criteria <- sys_eval_cri(dataset, 6, var_dis)
# Save Transition Score criteria
rio::export(TS_criteria,"inst/extdata/TS_criteria.xlsx")
```

### Evaluate the experimental data of AgNPs

```R
# Import and evaluate the experimental data
data(dataset)
users_data <- dataset

# Calculate the Distribution Entropy
DE <- sys_DE(users_data)

# Calculate the Proclivity Entropy
PE <- sys_PE(users_data)

# Calculate the Combination Entropy
CE <- sys_CE(users_data, dataset)

# Transition score
T_S <- sys_TS(users_data, dataset, 6, var_dis)

# sysAgNPs score of AgNPs (DE, PE, CE, TS)
sysAgNPs_score <- data.frame(DE = DE$H_pB,
                             PE = PE$H_pK,
                             CE = CE$H_pE,
                             TS = T_S$TS)

# Save sysAgNPs score results
rio::export(sysAgNPs_score, "inst/extdata/sysAgNPs_score.xlsx")

# Line and radar plots of sysAgNPs score                            
sysAgNPs_line_radar_1 <- sys_line_radar(sysAgNPs_score, 1)
# sysAgNP1
sysAgNPs_line_radar_1
```
![](man/figures/sysAgNP1.png)<!-- -->

## Session information
```R
sessionInfo()
# R version 4.4.1 (2024-06-14 ucrt)
# Platform: x86_64-w64-mingw32/x64
# Running under: Windows 11 x64 (build 22631)
# 
# Matrix products: default
# 
# 
# locale:
# [1] LC_COLLATE=Chinese (Simplified)_China.utf8 
# [2] LC_CTYPE=Chinese (Simplified)_China.utf8   
# [3] LC_MONETARY=Chinese (Simplified)_China.utf8
# [4] LC_NUMERIC=C                               
# [5] LC_TIME=Chinese (Simplified)_China.utf8    
# 
# time zone: Asia/Shanghai
# tzcode source: internal
# 
# attached base packages:
# [1] stats     graphics  grDevices utils     datasets  methods   base     
# 
# other attached packages:
# [1] sysAgNPs_1.0.0
# 
# loaded via a namespace (and not attached):
#  [1] gtable_0.3.5       ggplot2_3.5.1      htmlwidgets_1.6.4  devtools_2.4.5    
#  [5] remotes_2.5.0      processx_3.8.4     rstatix_0.7.2      lattice_0.22-6    
#  [9] callr_3.7.6        vctrs_0.6.5        tools_4.4.1        ps_1.7.7          
# [13] generics_0.1.3     tibble_3.2.1       fansi_1.0.6        R.oo_1.26.0       
# [17] pkgconfig_2.0.3    Matrix_1.7-0       RColorBrewer_1.1-3 desc_1.4.3        
# [21] lifecycle_1.0.4    farver_2.1.2       compiler_4.4.1     stringr_1.5.1     
# [25] munsell_0.5.1      carData_3.0-5      httpuv_1.6.15      htmltools_0.5.8.1 
# [29] usethis_2.2.3      yaml_2.3.9         pkgdown_2.1.0      crayon_1.5.3      
# [33] later_1.3.2        pillar_1.9.0       car_3.1-2          ggpubr_0.6.0      
# [37] urlchecker_1.0.1   tidyr_1.3.1        R.utils_2.12.3     ellipsis_0.3.2    
# [41] cachem_1.1.0       sessioninfo_1.2.2  abind_1.4-5        mime_0.12         
# [45] tidyselect_1.2.1   digest_0.6.36      stringi_1.8.4      dplyr_1.1.4       
# [49] purrr_1.0.2        labeling_0.4.3     rio_1.1.1          forcats_1.0.0     
# [53] rprojroot_2.0.4    fastmap_1.2.0      grid_4.4.1         colorspace_2.1-0  
# [57] expm_0.999-9       cli_3.6.3          magrittr_2.0.3     patchwork_1.2.0   
# [61] pkgbuild_1.4.4     utf8_1.2.4         broom_1.0.6        withr_3.0.0       
# [65] scales_1.3.0       promises_1.3.0     backports_1.5.0    writexl_1.5.0     
# [69] ggsignif_0.6.4     R.methodsS3_1.8.2  memoise_2.0.1      shiny_1.8.1.1     
# [73] miniUI_0.1.1.1     profvis_0.3.8      rlang_1.1.4        Rcpp_1.0.13       
# [77] xtable_1.8-4       glue_1.7.0         pkgload_1.4.0      rstudioapi_0.16.0 
# [81] R6_2.5.1           fs_1.6.4  
```
