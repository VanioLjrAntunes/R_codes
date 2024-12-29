# Who am I? <br>
![image](https://github.com/VanioLjrAntunes/R_codes/assets/155832432/13948857-e42c-4422-98a1-5d0db968801c)

<br>
Hi, I'm Vanio Antunes, a 4th medical student and former tutor at meta-analysis academy.<br>
My hobby is to create codes to make our work of producing meta-analysis easier.<br>
I created this repository in order to share them. I hope it may be useful for you. <br>
If you wish to follow my work here you have a <a href="https://l.instagram.com/?u=https%3A%2F%2Flinktr.ee%2Fvanio.antunes&e=AT0ZSXkXk3YTMmRxD2gIDd5L2Iqzhm4f_ghS8m0-L3Sf0IiD2zVsI-yi7JoqaKxxpfg6Bq1dqVvmrMswRk2FTZs78uJOymPfxH48xbev3vIOxRQLsoLR2Ds">link tree with my social medias.</a>


```{r}

custom_metaprop <- function(x) {
  meta::metaprop(x,
         event=event,
         n=n,
         method = "Inverse",
         method.tau = "DL",
         sm = "PAS",
         studlab = studlab,
         subgroup = subgroup)
}

custom_forest <- function(x) {
  meta::forest(x,
       leftcols = c("studlab", "event", "n", "w.random", "effect", "ci"),
       leftlabs = c("Author", "Event", "Total", "Weight", "Proportion", "95% CI"),
               sortvar = studlab,
       ff.xlab = "bold", fs.xlab = 12,
       pscale = 100,
       pooled.events = T,
       xlab = unique(data$smlab)[1],
       xlim = c(0,100),
       rightcols = FALSE,
       pooled.totals = TRUE,  # Ensure this is set to TRUE
       digits.pval.Q = 3, digits = 2,
       just = "center",
       random = TRUE, common = FALSE,
       fs.heading = 12, fs.study = 12, fs.hetstat = 10,
       colgap = "5mm", colgap.forest = "5mm",
       col.square = "darkblue", col.square.lines = "black", 
       col.diamond = "maroon", col.diamond.lines = "black",
       print.Q = TRUE, print.pval.Q = TRUE, print.tau.ci = TRUE,
       overall.hetstat=T,
       subgroup = T,
       col.subgroup = "black",
       print.subgroup.name = F,
       test.effect.subgroup = F,
       overall = T,
       test.subgroup = T)
}


```


# Code 1 - Extracting text made data directly from R <br>
> This code aims to make it easier the process of writing. Normally we need to go to our forest plots and extract our data from there. With this code you will have a basic pre-made text with your results.
## First Step: Prepare your data
>### 1. Create your xlsx file with your data like in the image below
>    ![image](https://github.com/VanioAntunesMD/R_codes/assets/155832432/49d6ac3a-9a0f-414a-a936-36191235d626)
>    Where:
>    a. Author = Last name of the first author of study <br>
>    b. Year = Year that the study was published<br>
>    c. mean.e = mean value of your intervention<br>
>    d. sd.e = standard deviation value of your intervention<br>
>    e. n.e = sample size of your intervention<br>
>    f. mean.e = mean value of your control<br>
>    g. sd.e = standard deviation value of your control<br>
>    h. n.e = sample size of your control<br>
>    i. outcome = name of your outcome<br>
>    j conotation = the meaning of the outcome. If it's good then "positive" and if bad then "negative"<br>
>    k. intervention = name of your intervention group<br>
>    l. control = name of your control group<br>
>    m. measure = measurement unity (if it's continuous)<br>

## Second Step: Load your packages
  ```r
package_list <- c("readxl", "meta", "tcltk", "dmetar", "metabias", "metafor", "dplyr", "xlsx")
for (package in package_list) {
  if (!require(package, character.only = TRUE)) {
    # If not installed, install the package
    install.packages(package, dependencies = TRUE)
    
    # Check the installation path
    if (length(grep(package, .libPaths(), fixed = TRUE)) == 0) {
      warning(paste("Error: Package", package, "not installed properly."))
    } else {
      # Try loading the installed package
      library(package, character.only = TRUE)
      message(paste("Package", package, "installed and loaded successfully."))
    }
  } else {
    message(paste("Package", package, "is already installed and loaded."))
  }
}
```

```
## Third Step: Load your data and select your working directory
```r
mwd <- tk_choose.dir()
myfile <- tk_choose.files()

  sheet_names <- excel_sheets(myfile)
  
  ma <- lapply(sheet_names, function(x) {                    # Read all sheets as lists
    as.data.frame(read_excel(myfile, sheet = x)) } )    # Convert your uploded dataset into a data 
  
  names(ma) <- sheet_names
```
> ### It will apear a new window behind R Studio. Browse through your computer and find the your folder
![image](https://github.com/VanioLjrAntunes/R_codes/assets/155832432/8592c4ec-aa6c-44c2-8863-0d0ca795255f)
<br>
> ### It will apear another window. Selec the file with your data
![image](https://github.com/VanioLjrAntunes/R_codes/assets/155832432/b3f1d7c7-ba56-41d7-8ce4-76581a3738b6)
<br>
