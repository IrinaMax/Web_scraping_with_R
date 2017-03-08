# Web_scraping_with_R
![screenshot 2017-03-07zillow](https://cloud.githubusercontent.com/assets/16123495/23693755/eb8efa78-038a-11e7-93eb-ad4c59ad928b.png)

####YOU NEED TO KNOW HTML AND CSS FOR THIS PROJECT,AND WILL ALSO NEED TO KNOW THE PIPE OPERATOR IN R 
      library(rvest)
      library(tidyr)
      #page <- read_html("https://www.zillow.com/homes/Mountain-View-CA_rb/")
      page <- read_html("https://www.zillow.com/homes/for_sale/Mountain-View-CA/fsba,fsbo,fore,cmsn_lt/house_type/32999_rid/globalrelevanceex_sort/37.496516,-121.906528,37.329628,-122.256718_rect/11_zm/0_mmm/")
      #//*[(@id = "list-results")]
       houses <- page %>%
       html_nodes(".photo-cards li article")
>      houses
     {xml_nodeset (15)}
      [1] <article data-photocount="24" data-audiencetesteventlabel="" data-grouped="" data-latitude="373750 ...
      [2] <article data-photocount="30" data-audiencetesteventlabel="" data-grouped="" data-latitude="374123 ...
      [3] <article data-photocount="26" data-audiencetesteventlabel="" data-grouped="" data-latitude="373969 ...
      [4] <article data-photocount="15" data-audiencetesteventlabel="" data-grouped="" data-latitude="373912 ...
      [5] <article data-photocount="28" data-audiencetesteventlabel="" data-grouped="" data-latitude="374024 ...
      [6] <article data-photocount="15" data-audiencetesteventlabel="" data-grouped="" data-latitude="373991 ...
      [7] <article data-photocount="36" data-audiencetesteventlabel="" data-grouped="" data-latitude="374032 ...
      [8] <article data-photocount="35" data-audiencetesteventlabel="" data-grouped="" data-latitude="374045 ...
      [9] <article data-photocount="39" data-audiencetesteventlabel="" data-grouped="" data-latitude="373754 ...
     [10] <article data-photocount="28" data-audiencetesteventlabel="" data-grouped="" data-latitude="373832 ...
     [11] <article data-photocount="16" data-audiencetesteventlabel="" data-grouped="" data-latitude="374031 ...
     [12] <article data-photocount="22" data-audiencetesteventlabel="" data-grouped="" data-latitude="374055 ...
     [13] <article data-photocount="5" data-audiencetesteventlabel="" data-grouped="" data-latitude="3739736 ...
     [14] <article data-photocount="4" data-audiencetesteventlabel="" data-grouped="" data-latitude="3737595 ...
     [15] <article data-photocount="30" data-audiencetesteventlabel="" data-grouped="" data-latitude="374086 ...
      # we can look the stucture of the houses : str(houses)
      # here is other way to scap all in one time
      h1 <-html_nodes(page, ".photo-cards li article ") %>% map(xml_attrs) %>% map_df(~as.list(.))
      names(h1)
      [1] "data-photocount"             "data-audiencetesteventlabel" "data-grouped"               
      [4] "data-latitude"               "data-longitude"              "id"                         
      [7] "data-pgapt"                  "data-zpid"                   "class"                      
     [10] "data-sgapt"                 
      #or you can go the other method
      z_id <- houses %>% html_attr("id")
      z_id
      [1] "zpid_80131400"   "zpid_19512928"   "zpid_19516153"   "zpid_122249030"  "zpid_2095316847"
      [6] "zpid_19517376"   "zpid_19511665"   "zpid_19512189"   "zpid_63056778"   "zpid_59682876"  
     [11] "zpid_19511234"   "zpid_19511573"   "zpid_19513431"   "zpid_19536397"   "zpid_19508370"  
      address <- houses %>%
              + html_node(".zsg-photo-card-address") %>%
              + html_text()
      address
     [1] "1585 Bonita Ave, Mountain View, CA"            "710 Telford Ave, Mountain View, CA"           
     [3] "285 Santa Rosa Ave, Mountain View, CA"         "215 Shumway Ln, Mountain View, CA"            
     [5] "188 College Ave, Mountain View, CA"            "119 Flynn Ave APT A, Mountain View, CA"       
     [7] "513 Burgoyne St, Mountain View, CA"            "271 Sierra Vista Ave APT 6, Mountain View, CA"
     [9] "1569 Glen Una Ct, Mountain View, CA"           "174 Centre St, Mountain View, CA"             
    [11] "381 Farley St, Mountain View, CA"              "823 Burgoyne St, Mountain View, CA"           
    [13] "211 Elmwood St, Mountain View, CA"             "219 Carmelita Dr, Mountain View, CA"          
    [15] "374 Fay Way, Mountain View, CA"               
     price <- houses %>%
     html_node(".zsg-photo-card-price") %>%
                                       + html_text() %>%
                                       + readr::parse_number()
Let's see the price and all other parameters                                      
                                       
      price
      [1] 1498000 1698000 1999999 2098000 1299000  649000 2398000 1025000 2200000 1690000 1650000 2500000
      [13]  830000 2050000 1799000
      params <- houses %>%
                      + html_node(".zsg-photo-card-info") %>%
                      + html_text() %>%
                      + strsplit("\u00b7")
      params
[[1]]
[1] "3 bds "      " 2 ba "      " 1,178 sqft"

[[2]]
[1] "4 bds "      " 3 ba "      " 1,859 sqft"

[[3]]
[1] "4 bds "      " -- ba "     " 1,428 sqft"

[[4]]
[1] "4 bds "      " 3 ba "      " 1,976 sqft"

[[5]]
[1] "3 bds "   " 3 ba "   " -- sqft"

[[6]]
[1] "2 bds "    " 1 ba "    " 858 sqft"

[[7]]
[1] "4 bds "      " 3 ba "      " 1,925 sqft"

[[8]]
[1] "2 bds "      " 3 ba "      " 1,147 sqft"

[[9]]
[1] "5 bds "      " 3 ba "      " 2,530 sqft"

[[10]]
[1] "3 bds "      " 3 ba "      " 1,663 sqft"

[[11]]
[1] "5 bds "      " 2 ba "      " 1,813 sqft"

[[12]]
[1] "3 bds "      " 3 ba "      " 4,574 sqft"

[[13]]
[1] "2 bds "    " 1 ba "    " 792 sqft"

[[14]]
[1] "2 bds "      " 2 ba "      " 1,008 sqft"

[[15]]
[1] "3 bds "      " 2 ba "      " 1,628 sqft"

> length(params)
[1] 15
> beds <- params %>% purrr::map_chr(1) %>% readr::parse_number()
> beds
 [1] 3 4 4 4 3 2 4 2 5 3 5 3 2 2 3
> baths <- params %>% purrr::map_chr(2) %>% readr::parse_number()
Warning: 1 parsing failure.
row col expected actual
  3  -- a number      -

> baths
 [1]  2  3 NA  3  3  1  3  3  3  3  2  3  1  2  2
attr(,"problems")
# A tibble: 1 × 4
    row   col expected actual
  <int> <int>    <chr>  <chr>
1     3    NA a number      -
> house_area <- params %>% purrr::map_chr(3) %>% readr::parse_number()
Warning: 1 parsing failure.
row col expected actual
  5  -- a number      -

> house_area
 [1] 1178 1859 1428 1976   NA  858 1925 1147 2530 1663 1813 4574  792 1008 1628
attr(,"problems")
# A tibble: 1 × 4
    row   col expected actual
  <int> <int>    <chr>  <chr>
1     5    NA a number      -
> df_h <- data_frame(row.names(name), z_id, address, price, params, beds, baths, house_area)
Error in row.names(name) : object 'name' not found
> df_hauses <- data_frame(z_id, address, price, params, beds, baths, house_area)
> df_hauses
# A tibble: 15 × 7
              z_id                                       address   price    params  beds baths house_area
             <chr>                                         <chr>   <dbl>    <list> <dbl> <dbl>      <dbl>
1    zpid_80131400            1585 Bonita Ave, Mountain View, CA 1498000 <chr [3]>     3     2       1178
2    zpid_19512928            710 Telford Ave, Mountain View, CA 1698000 <chr [3]>     4     3       1859
3    zpid_19516153         285 Santa Rosa Ave, Mountain View, CA 1999999 <chr [3]>     4    NA       1428
4   zpid_122249030             215 Shumway Ln, Mountain View, CA 2098000 <chr [3]>     4     3       1976
5  zpid_2095316847            188 College Ave, Mountain View, CA 1299000 <chr [3]>     3     3         NA
6    zpid_19517376        119 Flynn Ave APT A, Mountain View, CA  649000 <chr [3]>     2     1        858
7    zpid_19511665            513 Burgoyne St, Mountain View, CA 2398000 <chr [3]>     4     3       1925
8    zpid_19512189 271 Sierra Vista Ave APT 6, Mountain View, CA 1025000 <chr [3]>     2     3       1147
9    zpid_63056778           1569 Glen Una Ct, Mountain View, CA 2200000 <chr [3]>     5     3       2530
10   zpid_59682876              174 Centre St, Mountain View, CA 1690000 <chr [3]>     3     3       1663
11   zpid_19511234              381 Farley St, Mountain View, CA 1650000 <chr [3]>     5     2       1813
12   zpid_19511573            823 Burgoyne St, Mountain View, CA 2500000 <chr [3]>     3     3       4574
13   zpid_19513431             211 Elmwood St, Mountain View, CA  830000 <chr [3]>     2     1        792
14   zpid_19536397           219 Carmelita Dr, Mountain View, CA 2050000 <chr [3]>     2     2       1008
15   zpid_19508370                374 Fay Way, Mountain View, CA 1799000 <chr [3]>     3     2       1628

# and here is the other way which will constract data tibble, but it will look quite different
df_tibble <- html_nodes(h, ".photo-cards li article") %>% map(xml_attrs) %>% map_df(~as.list(.))
df_tibble
df_tibble$`data-audiencetesteventlabel` <-NULL
df_tibble$`data-grouped` <- NULL
# A tibble: 15 × 8
   `data-photocount` `data-latitude` `data-longitude`              id `data-pgapt` `data-zpid`                                                        class
               <chr>           <chr>            <chr>           <chr>        <chr>       <chr>                                                        <chr>
1                 24        37375049       -122080032   zpid_80131400      ForSale    80131400 zsg-photo-card photo-card zsg-aspect-ratio type-not-favorite
2                 30        37412380       -122086470   zpid_19512928      ForSale    19512928 zsg-photo-card photo-card zsg-aspect-ratio type-not-favorite
3                 26        37396960       -122075823   zpid_19516153      ForSale    19516153 zsg-photo-card photo-card zsg-aspect-ratio type-not-favorite
4                 15        37391296       -122073303  zpid_122249030      ForSale   122249030 zsg-photo-card photo-card zsg-aspect-ratio type-not-favorite
5                 28        37402423       -122100440 zpid_2095316847      ForSale  2095316847 zsg-photo-card photo-card zsg-aspect-ratio type-not-favorite
6                 15        37399127       -122062732   zpid_19517376      ForSale    19517376 zsg-photo-card photo-card zsg-aspect-ratio type-not-favorite
7                 36        37403282       -122086409   zpid_19511665      ForSale    19511665 zsg-photo-card photo-card zsg-aspect-ratio type-not-favorite
8                 35        37404571       -122090996   zpid_19512189      ForSale    19512189 zsg-photo-card photo-card zsg-aspect-ratio type-not-favorite
9                 39        37375417       -122075789   zpid_63056778      ForSale    63056778 zsg-photo-card photo-card zsg-aspect-ratio type-not-favorite
10                28        37383233       -122075095   zpid_59682876      ForSale    59682876 zsg-photo-card photo-card zsg-aspect-ratio type-not-favorite
11                16        37403195       -122089318   zpid_19511234      ForSale    19511234 zsg-photo-card photo-card zsg-aspect-ratio type-not-favorite
12                22        37405589       -122085771   zpid_19511573      ForSale    19511573 zsg-photo-card photo-card zsg-aspect-ratio type-not-favorite
13                 5        37397369       -122079322   zpid_19513431      ForSale    19513431 zsg-photo-card photo-card zsg-aspect-ratio type-not-favorite
14                 4        37375957       -122073802   zpid_19536397      ForSale    19536397 zsg-photo-card photo-card zsg-aspect-ratio type-not-favorite
15                30        37408672       -122095376   zpid_19508370      ForSale    19508370 zsg-photo-card photo-card zsg-aspect-ratio type-not-favorite
# ... with 1 more variables: `data-sgapt` <chr>
