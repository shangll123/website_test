---
layout: default
title: Pubmed search in RISmed
parent: Academic
nav_order: 4
---

When using the RISmed package to search count of literatures of many combinations of terms in a loop, the below error may show on the screen:

```
Error in file(con, "r") : 
  cannot open the connection to 'https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&term=Schizophrenia[Title/Abstract][Title/Abstract]+AND+artery+aorta[Title/Abstract][Title/Abstract]&retmax=1000&tool=RISmed&email=s.a.kovalchik@gmail.com'
In addition: Warning message:
In file(con, "r") :
  cannot open URL 'https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&term=Schizophrenia[Title/Abstract][Title/Abstract]+AND+artery+aorta[Title/Abstract][Title/Abstract]&retmax=1000&tool=RISmed&email=s.a.kovalchik@gmail.com': HTTP status was '429 Unknown Error'
```

To solve this problem, we just need to add one line in the R code: "Sys.sleep(0.1)". Just slow down a little bit and give the searching engine some time.


For example:
```
library(RISmed)
```

The traits we want to search:
```
traits = c(
"Schizophrenia[Title/Abstract]",
"Bipolar disorder[Title/Abstract]",
"(Bipolar disorder Schizophrenia[Title/Abstract] OR Bipolar disorder[Title/Abstract] OR Schizophrenia[Title/Abstract])",
"Depressive symptoms[Title/Abstract] OR Depression[Title/Abstract]",
"(Alzheimer's disease[Title/Abstract] OR Alzheimer[Title/Abstract])",
"(Inflammatory bowel[Title/Abstract] OR IBD[Title/Abstract])",
"(Ulcerative colitis[Title/Abstract] OR Ulcerative[Title/Abstract])",
"(Crohn's disease[Title/Abstract] OR Crohn[Title/Abstract])")
```

The cell types we want to search:
```
celltype = c(
"astrocytes[Title/Abstract]",
"endothelial[Title/Abstract]",
"GABAergic[Title/Abstract]",
"microglia[Title/Abstract]",
"neuronal stem cells[Title/Abstract]",
"oligodendrocytes[Title/Abstract]",
"oligodendrocyte precursor cells[Title/Abstract]",
"Unclassified[Title/Abstract]",
"pyramidal neurons[Title/Abstract]",
"granule neurons[Title/Abstract]",
"glutamatergic neurons[Title/Abstract]"
)
```

Search the combination of cell types and traits in loop:
```
results_all_cell = matrix(0,8,11)
for(i in 1:8){
	print(i)
	for(j in 1:11){
		topic <- paste0(traits[i]," AND ",celltype[j])
		r <- QueryCount(EUtilsSummary(topic, db= "pubmed"))
		results_all_cell[i,j] = r
		Sys.sleep(0.1)
}
}

```
