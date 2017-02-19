# Web_scraping_with_R
Web Scraping in general is almost always going to be unique to your personal use case, this is because every website is different, updates occur, and things can change.
If you don't know HTML or CSS, you may be able to use an auto-web-scrape tool, like import.io. Check it out, it will auto scrape and create a csv file for you.

      # Will also install dependencies
     install.packages('rvest')
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
    
 use a similar process to extract the cast, using html_nodes() to find all nodes that match the selector:   
    
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
    
The titles and authors of recent message board postings are stored in a the third table on the page. We can use html_node() and [[ ]] to find it, then coerce it to a data frame with html_table():

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
