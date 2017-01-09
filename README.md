# Web_scraping_with_R
     library(rvest)
 Loading required package: xml2
 
    html <- read_html("http://www.imdb.com/title/tt1490017/")
    cast <- html_nodes(html, "#titleCast .itemprop")
    length(cast)
[1] 30

     cast[1:2]
   {xml_nodeset (2)}
    
     [1] <td class="itemprop" itemprop="actor" itemscope="" itemtype="http://schema.org/Person" ...
     [2] <span class="itemprop" itemprop="name">Will Arnett</span>
     #> {xml_nodeset (2)}
     #> [1] <td class="itemprop" itemprop="actor" itemscope="" itemtype="http:// ...
    > #> [2] <span class="itemprop" itemprop="name">Will Arnett</span>
    > cast <- html_nodes(html, "#titleCast span.itemprop")
    > length(cast)
    [1] 15
    > #> [1] 15
    > html_text(cast)
    [1] "Will Arnett"     "Elizabeth Banks" "Craig Berry"     "Alison Brie"    
    [5] "David Burrows"   "Anthony Daniels" "Charlie Day"     "Amanda Farinos" 
    [9] "Keith Ferguson"  "Will Ferrell"    "Will Forte"      "Dave Franco"    
    [13] "Morgan Freeman"  "Todd Hansen"     "Jonah Hill"     
    > #>  [1] "Will Arnett"     "Elizabeth Banks" "Craig Berry"
    > #>  [4] "Alison Brie"     "David Burrows"   "Anthony Daniels"
    > #>  [7] "Charlie Day"     "Amanda Farinos"  "Keith Ferguson"
    > #> [10] "Will Ferrell"    "Will Forte"      "Dave Franco"
    > #> [13] "Morgan Freeman"  "Todd Hansen"     "Jonah Hill"
    > lego_movie <- html("http://www.imdb.com/title/tt1490017/")
    Warning message:
    'html' is deprecated.
    Use 'read_html' instead.
    See help("Deprecated") 
    > lego_movie %>%
    + html_node("strong span") %>%
    + html_text() %>%
    + as.numeric()
    [1] 7.8
    > lego_movie %>%
    + html_nodes("#titleCast .itemprop span") %>%
    + html_text()
 
     [1] "Will Arnett"     "Elizabeth Banks" "Craig Berry"     "Alison Brie"    
     [5] "David Burrows"   "Anthony Daniels" "Charlie Day"     "Amanda Farinos" 
     [9] "Keith Ferguson"  "Will Ferrell"    "Will Forte"      "Dave Franco"    
    [13] "Morgan Freeman"  "Todd Hansen"     "Jonah Hill"     
    
    lego_movie %>%
    + html_nodes("table") %>%
    + .[[3]] %>%
    + html_table()
                                   X1                                           X2
    1                   Amazon Affiliates                            Amazon Affiliates
    2 Amazon VideoWatch Movies &TV Online Prime VideoUnlimited Streamingof Movies & TV
                                        X3                                     X4
    1                        Amazon Affiliates                      Amazon Affiliates
    2 Amazon GermanyBuy Movies onDVD & Blu-ray Amazon ItalyBuy Movies onDVD & Blu-ray
                                       X5                                    X6
    1                       Amazon Affiliates                     Amazon Affiliates
    2 Amazon FranceBuy Movies onDVD & Blu-ray Amazon IndiaBuy Movie andTV Show DVDs
                          X7                         X8
    1          Amazon Affiliates          Amazon Affiliates
    2 DPReviewDigitalPhotography AudibleDownloadAudio Books
