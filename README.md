# Data-Wrangling-Exercise-1-Basic-Data-Manipulation
Data Wrangling Exercise 1: Basic Data Manipulation

#loading dplyr & tidyr

library("dplyr", lib.loc="~/R/win-library/3.5")
library("tidyr", lib.loc="~/R/win-library/3.5")

#loading dataset

library(readxl)
refine_original <- read_excel("~/Springboard/refine_original.xlsx")
View(refine_original)

# Cleaning up company names

refine_original$company <- gsub(pattern = "Phillips|phillips|phlips|phllips|phillps|phillipS|fillips", replacement = "philips", x = refine_original$company)
refine_original$company <- gsub(pattern = "ak zo|akz0|Akzo|AKZO", replacement = "akzo", x=refine_original$company)
refine_original$company <- gsub(pattern = "Van Houten|van Houten", replacement = "van houten", x=refine_original$company)
refine_original$company <- gsub(pattern = "Unilever|unilver", replacement = "unilever", x=refine_original$company)

# Seperating the "Product code / number" column into "product_code" and "product_number" columns.

refine_original <- separate(refine_original, col = "Product code / number", into = c("product_code" , "product_number"), sep = "-")

# Translating the product code into products.

prd_codes <- c("p" = "smartphone", "v" = "tv", "x" = "saptop", "q" = "tablet")
refine_original$product_category <- prd_codes[refine_original$product_code]

# Concatenates the address into 1 column.

refine_original <- mutate(refine_original, full_address = paste(address,city,country, sep = ","))

# Create 1 binary column for each company.

refine_original <- mutate(refine_original, company_philips = ifelse(company == "philips", 1, 0))
refine_original <- mutate(refine_original, company_akzo = ifelse(company == "akzo", 1, 0))
refine_original <- mutate(refine_original, company_van_houten = ifelse(company == "van houten", 1, 0))
refine_original <- mutate(refine_original, company_unilever = ifelse(company == "unilever", 1, 0))

# Do the same as above for product categories.

refine_original <- mutate(refine_original, product_smartphone = ifelse(product_category == "smartphone",1,0))
refine_original <- mutate(refine_original, product_tv = ifelse(product_category == "tv",1,0))
refine_original <- mutate(refine_original, product_laptop = ifelse(product_category == "laptop",1,0))
refine_original <- mutate(refine_original, product_tablet = ifelse(product_category == "tablet",1,0))

## export the cleaned dataset 
write.csv(refine_original, "refine_clean.csv")
