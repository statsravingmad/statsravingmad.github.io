---
title: (Auto) Document your Google Tag Manager implementation
author: manos_parzakonis
excerpt: Export your GTM configuration to a Google Sheet for easy reference
categories:
- measure
tags:
- documentation
- script
- R
- google tag manager

---

{% include toc title="Unique Title" icon="file-text" %}

## Intro
You might already noticed that I am a huge fan of documentation. The sweat spot I am a fool for is the automation, and this is what I covered in the [Create a Google Analytics implementation doc using R]() post. Today, we will generalize this into pulling the Google Tag Manager configuration using the GTM API and feeding it into a Google Sheet.

## Strategy


## Execution


``` r
library(googleAuthR);
library(googletagmanagerv1.auto)
library(googlesheets) ## Push it to a Google Sheet

## Set the desired scopes for our problem(s)
options(googleAuthR.scopes.selected = c('https://www.googleapis.com/auth/tagmanager.delete.containers',
    'https://www.googleapis.com/auth/tagmanager.edit.containers',
    'https://www.googleapis.com/auth/tagmanager.edit.containerversions',
    'https://www.googleapis.com/auth/tagmanager.manage.accounts',
    'https://www.googleapis.com/auth/tagmanager.manage.users',
    'https://www.googleapis.com/auth/tagmanager.publish',
    'https://www.googleapis.com/auth/tagmanager.readonly'))

## Now authenticate to Google
gar_auth(new_user = T)
## Auth with `googlesheets` as well (since this can be a different account)
gs_auth()
```


``` r
## Sample Ids
accountId <- 65691
containerId <- 71466
```


``` r
## Define functions for the `googletagmanagerv1` package
## but let's use them directly here
accounts.list <- function() {
    url <- "https://www.googleapis.com/tagmanager/v1/accounts"
    # tagmanager.accounts.list
    f <- gar_api_generator(url, "GET", data_parse_function = function(x) x)
    f()
    
}

tags.list <- function(accountId,containerId) {
  url <- paste0("https://www.googleapis.com/tagmanager/v1/accounts/",accountId,"/containers/",containerId,"/tags")
  # tagmanager.tags.list
  f <- gar_api_generator(url, "GET", data_parse_function = function(x) x)
  f()
}


triggers.list <- function(accountId,containerId) {
  url <- paste0("https://www.googleapis.com/tagmanager/v1/accounts/",accountId,"/containers/",containerId,"/triggers")
  # tagmanager.triggers.list
  f <- gar_api_generator(url, "GET", data_parse_function = function(x) x)
  f()
}


variables.list <- function(accountId,containerId) {
  url <- paste0("https://www.googleapis.com/tagmanager/v1/accounts/",accountId,"/containers/",containerId,"/variables")
  # tagmanager.variables.list
  f <- gar_api_generator(url, "GET", data_parse_function = function(x) x)
  f()
}
```

Now, we can use a for loop to get all containers under an account into a separate Google Sheet

``` r
time <- 3;
for (acc_Id in accountId){
  for (con_Id in containerId) {
    container.tags <- tags.list(acc_Id,con_Id)
    container.triggers <- triggers.list(acc_Id,con_Id)
    container.variables <- variables.list(acc_Id,con_Id)
    
    ## Parameter is a `JSON` (so in `R` this is a `list`), that we will not use for now
    # str(container.tags$tags$parameter)
    
    ## Objects of interest
    tags <- container.tags$tags
    triggers <- container.triggers$triggers
    variables <- container.variables$variables
    
    ## Register a new Google Sheet to pass the data
    gs_new(paste("(Auto) GTM Implementation - ",acc_Id,"_",con_Id), verbose = TRUE)
    s_id <- gs_title(paste("(Auto) GTM Implementation - ",acc_Id,"_", con_Id), verbose = TRUE)
    # gs_ws_new(ss=s_id, ws_title = "Tags")
    # gs_ws_new(ss=s_id, ws_title = "Triggers")
    # gs_ws_new(ss=s_id, ws_title = "Variables")
    yo.tags <- gs_ws_new(ss=s_id, ws_title="Tags" , input = tags[,1:10], trim = TRUE)
    yo.triggers <- gs_ws_new(ss=s_id, ws_title="Triggers", input = triggers, trim = TRUE)
    yo.variables <- gs_ws_new(ss=s_id, ws_title="Variables", input = variables, trim = TRUE)
    
    ## Share the spreadsheet via an email
    # require(mailR)
    # s_id$browser_url)
    
  }
  Sys.sleep(time) # This is used to cool off the API calls...
}
```
Hope this helps you!
