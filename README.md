# Portfolio

[Continuous Integration](#continuous-integration)
/ [Code Examples](#code-examples)
/ [Shiny Apps](#shiny-apps) 
/ [Flask Apps](#flask-apps) 
/ [Software](#software) 
/ [Presentations](#presentations) 
/ [Workshops](#workshops)
/ [Visualizations](#visualizations)


This repository contains (links to) code and/or data products, as well as presentations I've developed and created in recent years.

## Continuous Integration

### Continuous Integration for data curation
The "weekly oil bulletin"-database holds fule prices for European countries. It is generated through a CI-runner that executes code every week to generate download links from scraped meta data. The database is updated and relevant logs written to file for easy curation and error handling.  
**links:** [GitHub repository (weekly_oil_bulletin)](https://github.com/the-Hull/weekly_oil_bulletin) 


<img src="https://github.com/the-hull/weekly_oil_bulletin/raw/main/fig/wob_germany.png?raw=true" width="600" align="middle" />


### Continuous Integration for report generation

This code snippet, as part of a automated, report-generation workflow, uses `git2r` to push results (parameterized reports) from a hosted instance of `R` on `TravisCI` back to a GitHub repository with encrypted authentification tokens.  
**links:** [GitHub repository (task automation)](https://github.com/the-Hull/02_task_automation/) (see [this presentation for more](https://hydrosoc.github.io/rhydro_EGU19/presentations/06_task_automation/06_task_automation.html#1))

## Code Examples

### Data processing, statisitical modelling and visualization 

* **Processing, modelling and visualizing hydrological data in R**:  
This interactive document is an example of a reproducible analyses in `R`, using web APIs to obtain data, reading in non-standard tables with custom functions, reshaping and applying statistical models to infer best predictors of extreme river flows across catchments in the UK. The analyses relies heavily on the `tidyverse`, especially `purrr` for mapping custom (nested) functions to list-columns.  
**links**: [Document](https://aglhurley.rbind.io/ext_post/01_rhydro_aglhurley_proc-mod-viz) // [Rmarkdown (raw)](https://github.com/the-Hull/rHydro_2018/blob/master/presentation/01_rHydro_AGLHurley_proc-mod-viz.Rmd) - [![DOI](https://zenodo.org/badge/123999066.svg)](https://zenodo.org/badge/latestdoi/123999066)


```r
uk_nest <- models %>% 
  
  # use custom function and add id column
  map_df(apply_model, uk_nested, .id = "id_model") %>% 
  
  # carry over pertinent columns
  select(id_model, country, model) %>% 
  
  # add model coefficients and performance metrics to data frame.
  mutate(coefficients = map(model, tidy),
         performance = map(model, glance)) %>% 
  select(-model)

# see results  
uk_nest %>% head(20)

```
<img src="/doc/img/code_example_rhydro.png" width="600" align="middle" />

### Reproducible analyses applying `R` package development principles as *research compendium*

* **World Bank Population Data analyses**  
The brief analyses featured in an elaborate [blog post](https://the-hull.github.io/reproducibility_compendium/analyses/00_write-up.html) for my personal website on reproducibility approaches. In this case, the structure and methods for `R` package development were applied to allow attaching custom functions and data with documentation to anybody looking to explore or reproduce the analyses (or entire blog post). It heavily relies on `usethis`, the `tidyverse` as well as `rlang` non-standard evaluation to facilitate use with piping and `dplyr`. The source for the function `wb_change_percent` used below, including documentation, can be found [here](https://github.com/the-Hull/reproducibility_compendium/blob/master/R/wb_calc_change_percent.R).

```r
#load custom library containing research compendium
library(WBanalyses) 
library(dplyr)
library(ggplot2)

# calculate pop change for last 10 years in data set.
max_year <- max(pop_data$year)
start_year <- max_year - 10


# ?wb_change_percent for the documentation

# add "region" in ... before country_name to keep column in analyses
pop_change <- pop_data %>%
    WBanalyses::wb_change_percent(start_year = start_year,
                                  end_year = max_year,
                                  value_col = population,
                                  year_col = year,
                                  outname = "population_perc_change",
                                  region,
                                  country_name)


```
<img src="/doc/img/code_example_reproducible.png" width="600" align="middle" />


## Shiny Apps

* [**Birmingham Institute of Forest Research** (BIFoR)](https://www.birmingham.ac.uk/research/activity/bifor/index.aspx) Data Expliration Platform:   
**link**: [https://aglhurley.shinyapps.io/bifor/](https://aglhurley.shinyapps.io/bifor/)  
This in-devolopment `shiny` app is being built for the institute's staff as an exploration and data download portal, as well as for outreach and educational purposes. 
It was demonstrated at an outreach and education exchange workshop with members from UK higher education institutions. Ultimately, this app will feature live data, once the necessary infrastructure has been installed on-site.

<img src="/doc/img/shiny_app_bifor.png" width="600" align="middle" />


## Flask Apps

* **NexTrip ✈** Travel Destination Recommender:   
**link**: [http://aglhurley.pythonanywhere.com/](http://aglhurley.pythonanywhere.com/)
Mock-project showcasing devlopment of back-end, front-end and machine learning approaches in `python` using `flask`, `scikit-learn` and data visualization through `plotly` and `matplotlib`.


## Software

### Authored:

* **remis**:  
`R` package for Interacting with the UNFCCC Data Interface API. 
The package allows querying and searching meta-data to post requests for all greenhouse-gas emissions data reported by ANNEX-I and Non-Annex-I countries to the UNFCCC.  
**links**: [GitHub](https://github.com/the-Hull/remis) // [package site](https://the-hull.github.io/remis/index.html)

<img src="https://the-hull.github.io/remis/articles/api-use_files/figure-html/fig-transportation-1.png" width="600" align="middle" />


* **datacleanr**:  
`R` package for Interactive and Reproducible Data Cleaning. 
datacleanr is developed using the shiny package, and relies on informative summaries, visual cues and interactive data selection and annotation. All data-altering operations are documented, and converted to valid R code (reproducible recipe), that can be copied, sent to an active RStudio script, or saved to disk.
**links**: [GitHub](https://github.com/the-Hull/datacleanr) // [package site](https://the-hull.github.io/datacleanr/index.html)

<img src="/doc/img/datacleanr_readme_cosore.gif" width="600" align="middle" />

* **lib2bib**:  
`R` package to acknowledge other `R` packages. 
It allows to search for and cite all packages used in a file, directory or `RStudio` project via calls to `library()`, `require()` or using the `::` operator notation. It includes a shiny interface to select specific packages and save them to a text or `.bib` file. 
Future improvements will include the ability to supply a bibliography style file, and copying the output to the clipboard.  
**links**: [GitHub](https://github.com/the-Hull/lib2bib) // [package site](https://the-hull.github.io/lib2bib/index.html)

<img src="/doc/img/lib_interactive.png" width="600" align="middle" />


* **ROAR**:  
`R` package for Reproducible and Organized Assimilation Routines (*in development*). 
`ROAR` assists in assimilating heterogeneous data sources into a pre-defined format, using templated scripts. These scripts are pre-populated through a "meta table" that contains information on every data set (origin, time zone, units, table format, etc.).
**links**: [GitHub](https://github.com/the-Hull/ROAR) 

### Contributed:

* **TREX**:  
Assimilate, process and analyse thermal dissipation sap flow data `R`. 
`TREX` allows to assimilate, process and analyse sap flow data obtained with the thermal dissipation method (TDM). The package includes functions for gap filling time-series data, detecting outliers, calculating data-processing uncertainties and generating uniform data output and visualisation. The package is designed to deal with large quantities of data and apply commonly used data-processing methods.
**links**: [GitHub](https://github.com/the-Hull/TREX) //  [CRAN](https://cran.r-project.org/web/packages/TREX/index.html) // [accompanying publication (*Methods in Ecology and Evolution*)](https://doi.org/10.1111/2041-210X.13524)

* **RAPTOR**:  
Row and Position Tracheid Organizer in `R`.
`R` package for wood anatomical analyses, allowing to pre-process, filter and extract cell properties from wood thinsections, significantly simplifying the process of extracting information for further processing.
My role in this package was to refactor and re-structure code to ensure compliance with CRAN requirements and best-practices.  
**links**: [GitHub](https://github.com/the-Hull/raptor) //  [CRAN](https://cran.r-project.org/web/packages/RAPTOR/index.html) // [accompanying publication (*Dendrochronologia*)](https://www.sciencedirect.com/science/article/pii/S1125786517301236)


## Presentations

* **Introduction to R - project set-up, data management and coding** (2015). 
This presentation was delivered to the physical geography department during a dedicated seminar at University of Birmingham. The focus was set presenting approaches to ensure projects were set up in a coherent, machine-readable and reproducible manner.  
**links:** [slides (PDF)](/doc/presentations/Introduction-to-R.pdf)

* **Intro to project management with git, GitHub and RStudio** (2018).  
This presentation was delivered to the water science research group during a dedicated seminar at University of Birmingham and included best practices in version controlling, project set-up, reproducible workflows and live coding sessions.  
**links:** [slides (react.js)](https://the-hull.github.io/intro_projects/#1) // [Rmarkdown (raw)](https://github.com/the-Hull/intro_projects/blob/master/04_presentation/intro_project_management.Rmd)

* **Processing, modelling and vizualising hydrological data in R** (2018).  
Delivered at the European Geophysical Union General Assembly, this presentation was directed at hydrologists interested in streamlining national-scale analyses with tiydverse methods.  
**links:** [document (html)](https://aglhurley.rbind.io/ext_post/01_rhydro_aglhurley_proc-mod-viz) // [Rmarkdown (raw)](https://github.com/the-Hull/rHydro_2018/blob/master/presentation/01_rHydro_AGLHurley_proc-mod-viz.Rmd) // [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.2553998.svg)](https://doi.org/10.5281/zenodo.2553998)

* **Staying up-to date: automating tasks from downloading data to reporting** (2019).  
Delivered at the European Geophysical Union General Assembly, this presentation was directed at geo-scientists looking to automate data aquisition, reporting and visualization, relying on `Rmarkdown`, `GitHub` and `TravisCI`.  
**links:** [slides (react.js)](https://hydrosoc.github.io/rhydro_EGU19/presentations/06_task_automation/06_task_automation.html#1) // [Rmarkdown (raw)](https://github.com/hydrosoc/rhydro_EGU19/blob/master/presentations/06_task_automation/06_task_automation.Rmd)

* **Obtaining, cleaning and visualizing hydrological data with R** (2019).  
Delivered at the European Geophysical Union General Assembly, this presentation showcased a suite of open-source software (`R` packages) that can facilitate typical workflows while enhancing reproducibility, including downloading, processing and visualizing results.  
**links**: [slides (react.js)](https://hydrosoc.github.io/rhydro_EGU19/presentations/03_get_clean_viz/03_get-clean-viz.html#1) // [Rmarkdown (raw)](https://github.com/hydrosoc/rhydro_EGU19/blob/master/presentations/03_get_clean_viz/03_get-clean-viz.Rmd)

<img src="/doc/img/presentation_viz.png" width="600" align="middle" />


## Workshops

**Using R in Hydrology** <img src="doc/img/rhydro_logo_alt.png" align="right" height = 150px, width = 150px />  
This short course, delivered at the European Geophysical Union General Assembly, has been run in association with the [Young Hydrologic Society](https://younghs.com/) since 2017. 
I've contributed in 2018 and took over convener's duties in 2019, organizing speakers, materials (syllabus and archiving); I also developed a consistent theme and logo based on `xaringan` and the common `R` hex-logo.  
The strong community focus driving this workshop led to a collaborative effort in authoring a discussion paper, titled ["*Using R in hydrology: a review of recent developments and future directions*" (Slater *et al.*, 2019)](https://doi.org/10.5194/hess-2019-50) in *Hydrology and Earth System Sciences*.


**2018 Workshop**: [GitHub repository](https://github.com/hydrosoc/rhydro_EGU18) - [![DOI](https://zenodo.org/badge/116503839.svg)](https://zenodo.org/badge/latestdoi/116503839)

**2019 Workshop**: [GitHub repository](https://github.com/hydrosoc/rhydro_EGU19) - [![DOI](https://zenodo.org/badge/169320856.svg)](https://zenodo.org/badge/latestdoi/169320856)

---

**Workshops on tree physiology and time series analyses**
Within the context of the Dendro-ecology and Eco-physiology (DEEP)-Toolbox, I participated the devlopment and the delivery of several workshops starting in 2021. See the [DEEP-Website](https://deep-tools.netlify.app/) for additional information.

<img src="https://d33wubrfki0l68.cloudfront.net/00889003a9c308968743de9471c6e2ac16ef266a/077e5/talk/nadef-2022-cpappas-rpeters-ahurley/featured_hu0d18b8b4e7255a4d80a6c16d37b758fa_78264_720x0_resize_q90_lanczos.jpg" width="600" align="middle" />

**2020 Workshop**: [Materials](https://deep-tools.netlify.app/docs-workshops/esa-workshop2020/) //  [GitHub repository](https://github.com/deep-org/workshop_data/tree/master/esa-workshop2020)

## Visualizations

### Spatial / Scientific


<a href="https://raw.githubusercontent.com/the-Hull/code_portfolio/master/doc/img/gallery/00_URSA_P19.png" target="_blank">
<img src="/doc/img/gallery/00_URSA_P19.png" width = 400px > </a>

<br>

<a href="https://raw.githubusercontent.com/the-Hull/code_portfolio/master/doc/img/gallery/01_berlin_transport_overview.png" width = 300px height = 400px target="_blank">
<img src="doc/img/gallery/01_berlin_transport_overview.png" width = 300px > </a> 

<br>

<a href="https://raw.githubusercontent.com/the-Hull/code_portfolio/master/doc/img/gallery/09_map_patch_ndvi_scaling.png" width = 350px height = 250px target="_blank">
<img src="doc/img/gallery/09_map_patch_ndvi_scaling.png" width = 300px > </a>

<br>

<a href="https://raw.githubusercontent.com/the-Hull/code_portfolio/master/doc/img/gallery/07_et_pred_performance.png" width = 300px height = 350px target="_blank">
<img src="/doc/img/gallery/07_et_pred_performance.png" width = 300px > </a>
</p>

<br>

<a href="https://raw.githubusercontent.com/the-Hull/code_portfolio/master/doc/img/gallery/07_hysteresis_precip_res.png" width = 300px height = 300px target="_blank">
<img src="/doc/img/gallery/07_hysteresis_precip_res.png" width = 300px > </a>

<br>

<a href="https://raw.githubusercontent.com/the-Hull/code_portfolio/master/doc/img/gallery/RWI_x_y__ASP_WSPR_HSB_HSM.png" width = 200px height = 500 px target="_blank">
<img src="/doc/img/gallery/RWI_x_y__ASP_WSPR_HSB_HSM.png"  height = 400 > </a>

