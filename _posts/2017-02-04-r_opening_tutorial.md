---
layout: post
title: "A function for reading fixed-width survey data into R"
---

In this post I introduce a function that will take survey data associated with a codebook and render that data as a dataframe in `R`. This function fills a need for the surprisingly large volume of data originally devised in an earlier format like `.dta`, `.spss`, `.sav`, and `.por` that is not usable by many of the common `read()` functions in `R` due to not having any metadata directly associated with the fixed-width data itself.

That said, the vast majority of data we as social scientists work with going forward is now associated with a canned function or library somewhere in R. Others have taken the time to fulfill the vast majority of our data reading needs. First, I talk through and provide some examples of the most common versions of those functions. Much of the data that I use includes snippets of surveys and voterfiles so I do not provide those data files directly here, but I invite the reader to follow along with their own data.

``` r
library(foreign) # .dta, .spss, .por, .sav
library(readxl) # .xls, .xlsx
library(readstata13) # .stata13
library(readODS) # .ods
```

And most of these functions are fairly straightforward.

``` r
readxl::read_excel('field_paper_data_1.xlsx')
```

    ## # A tibble: 5 x 2
    ##   favorite_department_lunch    IQ
    ##                       <chr> <dbl>
    ## 1               Clementines   102
    ## 2               Clementines   104
    ## 3       Feast From the East    36
    ## 4       Feast From the East    39
    ## 5               Clementines   102

(this `readxl` example is an inside joke here at UCLA's political science department)

``` r
foreign::read.dta('PBC_pres_returns.dta')[1:5,]
```

    ##   year state   county countyid vote fipscode    dempct
    ## 1 2004    GA  APPLING       10 1848    13001 0.2901099
    ## 2 2004    GA ATKINSON       30  799    13003 0.3234818
    ## 3 2004    GA    BACON       50  930    13005 0.2452532
    ## 4 2004    GA    BAKER       70  936    13007 0.5306122
    ## 5 2004    GA  BALDWIN       90 6775    13009 0.4653159

``` r
readstata13::read.dta13('cars.dta')[1:5,1:5]
```

    ##            make price mpg rep78 headroom
    ## 1   AMC Concord  4099  22     3      2.5
    ## 2     AMC Pacer  4749  17     3      3.0
    ## 3    AMC Spirit  3799  22    NA      3.0
    ## 4 Buick Century  4816  20     3      4.5
    ## 5 Buick Electra  7827  15     4      4.0

``` r
read.csv('PBC_pres_returns.csv', stringsAsFactors = F)[1:5,]
```

    ##   state   dem08   rep08   dem12   rep12
    ## 1    AL  813479 1266546  795696 1255925
    ## 2    AK  123594  193841  122640  164676
    ## 3    AZ 1034707 1230111 1025232 1233654
    ## 4    AR  422310  638017  394409  647744
    ## 5    CA 8274473 5011781 7854285 4839958

``` r
load('PBC_pres_returns.RData') # 'Load' automatically creates environment variables
pres[1:5,]
```

    ##   year state   county countyid vote fipscode    dempct
    ## 1 2004    GA  APPLING       10 1848    13001 0.2901099
    ## 2 2004    GA ATKINSON       30  799    13003 0.3234818
    ## 3 2004    GA    BACON       50  930    13005 0.2452532
    ## 4 2004    GA    BAKER       70  936    13007 0.5306122
    ## 5 2004    GA  BALDWIN       90 6775    13009 0.4653159

``` r
read.spss('GSS2012.sav', to.data.frame = T)[1:5,1:5] # How did I know this data was SPSS?
```

    ##   year id      wtss vpsu vstrat
    ## 1 2012  1 2.6219629    1   3001
    ## 2 2012  2 3.4959505    1   3001
    ## 3 2012  3 1.7479752    1   3001
    ## 4 2012  4 1.2356944    1   3001
    ## 5 2012  5 0.8739876    1   3001

How did I know which function to use? Sometimes its obvious --- look at the file extension in the data you're working with! `.dta` corresponds to `read.dta`, `.csv` corresponds to `read.csv`, and so on. But when its not obvious, either open the flie in a text editor or use `readLines()` to see what you're dealing with. The ability to open a piece of data and figure out what original data structure its based on is not a skill that came naturally to me, but it develops over time.

``` r
readLines('PBC_pres_returns.csv', warn = F)[1:5]
```

    ## [1] "state,dem08,rep08,dem12,rep12"     
    ## [2] "AL,813479,1266546,795696,1255925"  
    ## [3] "AK,123594,193841,122640,164676"    
    ## [4] "AZ,1034707,1230111,1025232,1233654"
    ## [5] "AR,422310,638017,394409,647744"

``` r
readLines('PBC_pres_returns.dta', warn = F)[1:5]
```

    ## [1] "n\002\001"       ""                "\x87\xd2?\xd4\a" "\x82\xe3?\xd4\a"
    ## [5] "3"

Note that here, for example, it would be really hard to look at this string of gobbledygoop and figure out what type of data we're looking at. Usually the data we want to work with isn't a total nightmare. Except when it is. Lets say I downloaded the [LACSS Community Survey data and codebook](http://sda.library.ucla.edu/sdaweb/analysis/?dataset=lcum).

``` r
readLines('data.txt')[1:2]
```

    ## Warning in readLines("data.txt"): incomplete final line found on 'data.txt'

    ## [1] "19920000119920          3 1 64   206921950  1        1 90     322                      2  2  2                    2                                               44  2   101 1                                 3636                          2                                                               33   250            1            50  3     1 1 631599 2          1        5      90405 11  155                                                                        2                                1 22  3 5                    5645               1                                                             50    50 50 50                                                                                                                               3                                                                                                                                                                                                                                                                                                                                                                                                                                           2                                                                                                 2                                                                                   223343      23332423                                   50 35 50 502                                                                                                  135171 605 2243          3 602431  60370 1222332212247142331 40 40333333                  225135   1 43524454451423332223                     1400"
    ## [2] "19920000319920          2 1 49   204922016  1        5 90     522                                                                                                 23  4   951 3                                 3524                2                                                                         54                  51255        40260     1 1 311254 3          4        1 8    91789 11  555                                                                                                         3 23  3 3                    2527               1                                                             40    50 80 60                                                                                                                               2                                                                                                                                   4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             544442      44344442      3                            60 50 30  02                                                                                                  11    43211    2448      2 252511  60371 1221631111141534222 20 20534533                  415115   1 5161326345          222242223334242      1000"

``` r
try(read.spss('data.txt')[1:2,]) # Returns an error that Markdown doesn't bother to show
```

Data that looks like this --- no header, long strings of numbers interspersed with large strings of white space --- are usually fixed-width format files, or ASCII files.

Really, almost all data we work with is just ASCII data wrapped in window dressing --- window dressing that some clever developer has figure out how to translate into R. This data does not have such window dressing. It does, however, have a codebook. Note that if it does not, life would be REAL rough. That data has 1,674 columns!

Let's take a look at this code book, which the user can find on the LACSS website. If we scroll around, at a certain point, we see a list of what look like variable names, followed by individual characters. These are the data that many kinds of software, specifically SPSS and STATA, use to figure out how the fixed-width files will be read.

Here, the variable names are stored in three columns separated by a few spaces, or tabs. For example, the first value is `CASEID 1-9`, which tells us that the first variable is called `CASEID`, and its values fill fields 1 through 9 of those fixed-width fields. That is, in every row of the table, the first nine numbers and/or characters will refer to that observation's `CASEID`. And so on.

Look how many variables there are!

``` r
readLines("data_spss.txt")[10:381]
```

    ##   [1] " /1           CASEID 1-9              YEAR 10-13             OVERSAMP 14"
    ##   [2] "            RND1_4 15-16            RND2_4 17-18             SPLIT94A 19"
    ##   [3] "             SPLIT94B 20               RND3_4 21          RACESHRT 22-23"
    ##   [4] "              CON2 24-25           FNLCODE 26-27      INVWRNUM 28-30 (A)"
    ##   [5] "       FNLDATE 31-38 (A)      INVWTIME 39-42 (A)             ORDRFLAG 43"
    ##   [6] "          SAMPREPL 44-45          SPLIT94C 46-53                  SEX 54"
    ##   [7] "          RACELONG 55-57              ETHDEFS 58           ETHIDEN 59-60"
    ##   [8] "           ETHCOMM 61-62             LA_LAST5 63             GOVCONF2 64"
    ##   [9] "              COPCONF 65             CONFLICT 66             CONFLCT2 67"
    ##  [10] "             CONFLCT3 68             ETHRELFU 69             ETHGRPLA 70"
    ##  [11] "             ETHGRPGV 71             ETHGRPEC 72             DIVERTAX 73"
    ##  [12] "              LARIOTS 74             LARIOTS2 75             LARIOTS3 76"
    ##  [13] "            IMECON 77-79           IMUNEMP 80-82           IMUNITE 83-85"
    ##  [14] "           IMHITAX 86-88            IMCULT 89-91           IMWRKHD 92-94"
    ##  [15] "           IMECONA 95-97         IMUNEMPA 98-100        IMUNITEA 101-103"
    ##  [16] "        IMHITAXA 104-106         IMCULTA 107-109        IMWRKHDA 110-112"
    ##  [17] "         IMECONH 113-115        IMUNEMPH 116-118        IMUNITEH 119-121"
    ##  [18] "        IMHITAXH 122-124         IMCULTH 125-127        IMWRKHDH 128-130"
    ##  [19] "            IMSERVCS 131             PERMITS 132            KIDSCITS 133"
    ##  [20] "            IMMTOTAL 134            BORDERS$ 135             DEPORT$ 136"
    ##  [21] "            REFUGEES 137            CITZNSHP 138              IDCARD 139"
    ##  [22] "            HELPBLKS 140            HELPBLK2 141            HELPBLK3 142"
    ##  [23] "             JOB2MIN 143            HIREBLKB 144            HIREBLK2 145"
    ##  [24] "            HIREBLK3 146            HIREASNB 147            HIREASN2 148"
    ##  [25] "            HIREASN3 149            HIREHISB 150            HIREHIS2 151"
    ##  [26] "            HIREHIS3 152            BILINGED 153            TEACHEN2 154"
    ##  [27] "             BALLOTS 155            OFFCLENG 156             ETHLDRS 157"
    ##  [28] "             ETHCONG 158            ETHTCHRS 159            ETHHSTRY 160"
    ##  [29] "            ETHTCHR2 161            SLAVERY2 162            WORKHARD 163"
    ##  [30] "            BLAMSELF 164             PROTEST 165            RESPAUTH 166"
    ##  [31] "            EQLCHANC 167              EQLTRT 168             STREETS 169"
    ##  [32] "        PIDFILTR 170-172            PIDSTRNG 173             PIDLEAN 174"
    ##  [33] "            IDEOFLTR 175            IDEOSTRL 176            IDEOSTRC 177"
    ##  [34] "             IDEOLN2 178            GOVTSIZE 179             FINPAST 180"
    ##  [35] "            FINPAST2 181            FINPAST3 182             FINFUTR 183"
    ##  [36] "            FINFUTR2 184            FINFUTR3 185             LAECNFU 186"
    ##  [37] "            LAECNFU2 187            LAECNFU3 188             PROUD2A 189"
    ##  [38] "         AMERISS 190-192            AMERISSC 193            AMERISSE 194"
    ##  [39] "        AMERISSG 195-197            AMERISSI 198            AMERISSJ 199"
    ##  [40] "            AMERDIV1 200            AMERDIV2 201            AMERDIV3 202"
    ##  [41] "             MELTPOT 203            MELTPOT2 204            MELTPOT3 205"
    ##  [42] "            AMERIDEN 206            SEPARAT3 207            SEPARATA 208"
    ##  [43] "               POORW 209               POORB 210               POORA 211"
    ##  [44] "               POORH 212              POORLI 213              POORII 214"
    ##  [45] "               LAZYW 215               LAZYB 216               LAZYA 217"
    ##  [46] "               LAZYH 218              LAZYLI 219              LAZYII 220"
    ##  [47] "            GOVATTNW 221            GOVATTNB 222        GOVATTNH 223-225"
    ##  [48] "        GOVATTNA 226-228            SRIRSHB1 229            SRDMNDB1 230"
    ##  [49] "        SRIRISHI 231-233        SRDEMNDI 234-236        SRIRISHA 237-239"
    ##  [50] "        SRDEMNDA 240-242        SRIRISHH 243-245        SRDEMNDH 246-248"
    ##  [51] "         IMECONR 249-251          IMTAXR 252-254         IMCRIMR 255-257"
    ##  [52] "        IMECONRA 258-260         IMTAXRA 261-263        IMCRIMRA 264-266"
    ##  [53] "        IMECONRH 267-269         IMTAXRH 270-272        IMCRIMRH 273-275"
    ##  [54] "         IMECONG 276-278          IMTAXG 279-281         IMCRIMG 282-284"
    ##  [55] "        IMECONGA 285-287         IMTAXGA 288-290        IMCRIMGA 291-293"
    ##  [56] "        IMECONGH 294-296         IMTAXGH 297-299        IMCRIMGH 300-302"
    ##  [57] "            BTTRCHNC 303            GETAHEAD 304            EXPDISCR 305"
    ##  [58] "            EXPDISCG 306            ANCINTRO 307        ANCSTRY1 308-310"
    ##  [59] "        ANCSTRY2 311-313        ANCSTRY3 314-316        ANCSTRY4 317-319"
    ##  [60] "        ANCCLOSE 320-322             BORNUSA 323         BORNIN2 324-326"
    ##  [61] "            YRSLIVUS 327             STAYUSA 328             CITIZEN 329"
    ##  [62] "             VOTEREG 330        PBRNUS2B 331-332             SPKLANG 333"
    ##  [63] "            SPKLANG2 334            LEAVEUSA 335             AGE 336-337"
    ##  [64] "        RELIGLNG 338-340        ATTEND45 341-342            BIBLEINT 343"
    ##  [65] "        EDUCGRAD 344-345              EDUCHS 346             EDUCGED 347"
    ##  [66] "             EDUCUNI 348        EDUCUNI2 349-350            EDUCDGR2 351"
    ##  [67] "             WORKING 352           OCCUP 353-355             GOVTEMP 356"
    ##  [68] "            JOBWRRY2 357             INCOME1 358             INCOME2 359"
    ##  [69] "             INCOME3 360             INCOME4 361             INCOME5 362"
    ##  [70] "             INCOME6 363             INCOME7 364            WALKBLOK 365"
    ##  [71] "            WALKMILE 366        HOODETH2 367-368            HOODETH3 369"
    ##  [72] "            HOODETH4 370          ETHPCT 371-376             HOODCHG 377"
    ##  [73] "            HOODCHG2 378            HOODCHG3 379             NONENG1 380"
    ##  [74] "             NONENG2 381            ETHRELAX 382             ETHDATE 383"
    ##  [75] "         ZIPCODE 384-388                THAK 389                INTC 390"
    ##  [76] "                 IN1 391                IN10 392                 IN2 393"
    ##  [77] "                 IN6 394                 IN8 395                 IN9 396"
    ##  [78] "        AMRCODE1 397-399        PCTHOODA 400-402        PCTHOODB 403-405"
    ##  [79] "        PCTHOODH 406-408        PCTHOODW 409-411        PCTHOODO 412-414"
    ##  [80] "          RND1_5 415-416          RND2_5 417-418          RND3_5 419-420"
    ##  [81] "            RND4 421-422            RND5 423-424            CON3 425 (A)"
    ##  [82] "            CON4 426 (A)            CON5 427 (A)                CON6 428"
    ##  [83] "        SPLIT95D 429-436        SPLIT95A 437-444                CLIK 445"
    ##  [84] "        ETHIDEN2 446-447        ORDERFLG 448 (A)            SPLIT95C 449"
    ##  [85] "            SPLIT95B 450            CONFLCT4 451            EXPDISCB 452"
    ##  [86] "            EXPDISCW 453            EXPDISCH 454            EXPDISCA 455"
    ##  [87] "            EXPDISCL 456            EXPDISCI 457            BCJOBAPP 458"
    ##  [88] "            P187KNOW 459            P187VOTE 460            P187VOT2 461"
    ##  [89] "             P187FAV 462             P187OPP 463            P209PREF 464"
    ##  [90] "             P209FAV 465             P209OPP 466            WELFELIM 467"
    ##  [91] "            WELF2KID 468             IMCRIME 469            HIREBKD2 470"
    ##  [92] "            HIREWOMB 471            HIREWOM2 472            HIREWOM3 473"
    ##  [93] "             FEDCONB 474            FEDCONB2 475            FEDCONB3 476"
    ##  [94] "         FEDCONW 477-479            FEDCONW2 480            FEDCONW3 481"
    ##  [95] "         FEDCONH 482-484            FEDCONH2 485            FEDCONH3 486"
    ##  [96] "             AAINFER 487            AAUNQUAL 488             AAEFCTW 489"
    ##  [97] "             AAEFCTB 490         AAEFCTH 491-493        AAEFCTWM 494-496"
    ##  [98] "            TEACHNG1 497            SPANKING 498             SUCCESS 499"
    ##  [99] "            SEXPREMA 500             SEXPORN 501            DEFENS$2 502"
    ## [100] "            CITIES$2 503             CRIME$2 504            ENVRMT$2 505"
    ## [101] "             BLACK$2 506            WELFAR$2 507            SOCSEC$2 508"
    ## [102] "            PRISON$2 509             SOCDOM2 510            SOCDM13A 511"
    ## [103] "            PIDSHORT 512            AMERISSB 513             AMERISC 514"
    ## [104] "            AMERISSD 515             AMERISE 516            AMERISSF 517"
    ## [105] "         AMERISG 518-520            AMERISSH 521             AMERISI 522"
    ## [106] "             AMERISJ 523            AMERISSK 524            STEINTRO 525"
    ## [107] "              POORHI 526               VIOLA 527               VIOLH 528"
    ## [108] "               VIOLW 529               VIOLB 530              INTELA 531"
    ## [109] "              INTELH 532              INTELW 533              INTELB 534"
    ## [110] "            ATTNINTR 535        GOVATTNI 536-538             SRINTRO 539"
    ## [111] "            IMCONSQW 540            IMCONSQB 541            IMCONSQH 542"
    ## [112] "            IMCONSQA 543            WRKHRDB1 544            SUCCSSB1 545"
    ## [113] "             EQLTRTB 546            EQLCHNCB 547        PBORNUS2 548-549"
    ## [114] "             OWNRENT 550         INVWMIN 551-554        AMRCODE2 555-556"
    ## [115] "            AMRCODE3 557            AMRCODE4 558            AMRCODE5 559"
    ## [116] "         PIDLONG 560-561             SPLIT96 562                RN1A 563"
    ## [117] "                RN2A 564                RN3A 565                RN4A 566"
    ## [118] "                RN5A 567                RN6A 568                RN1B 569"
    ## [119] "                RN2B 570                RN3B 571                RN4B 572"
    ## [120] "                RN5B 573                RN6B 574            RN1C 575-576"
    ## [121] "            RN2C 577-578            RN3C 579-580            RN4C 581-582"
    ## [122] "            RN5C 583-584            RN6C 585-586            ETHIMPT2 587"
    ## [123] "             ETHFEEL 588             ETHOTHR 589             EQOPPTY 590"
    ## [124] "               MERIT 591            GOVTPOOR 592            GOVTHLTH 593"
    ## [125] "             GOVTTAX 594            GOVTGRPS 595            GOVT2BIG 596"
    ## [126] "             COPFEAR 597              LIBCON 598            ABORTION 599"
    ## [127] "             HOMOSEX 600            THMINTRO 601        THMWOMEN 602-604"
    ## [128] "         THMPOOR 605-607          THMGOP 608-610        THMBLACK 611-613"
    ## [129] "         THMDEMS 614-616         THMHISP 617-619        THMWHITE 620-622"
    ## [130] "        THMASIAN 623-625          THMUSC 626-628         THMUCLA 629-631"
    ## [131] "             SOCDOM9 632            SOCDOM10 633            SOCDOM11 634"
    ## [132] "            SOCDOM12 635             SOCDOM7 636            SOCDOM1A 637"
    ## [133] "            SOCDOM2A 638            SOCDOM3A 639             SOCDOM8 640"
    ## [134] "            SOCDOM13 641            SOCDOM14 642            SOCDOM4A 643"
    ## [135] "            SOCDOM5A 644            SOCDOM6A 645            SOCDOM15 646"
    ## [136] "            SOCDOM16 647            RGCJOBSB 648            RGCLPOLB 649"
    ## [137] "            RGCHOUSB 650            RGCECONB 651            EXPLDISB 652"
    ## [138] "            EXPLABLB 653            EXPLEDUB 654            EXPLMOTB 655"
    ## [139] "            AFFRMACT 656             AACOLL1 657             AAEXPLN 658"
    ## [140] "             INSTRUC 659            AACOLL1A 660            AACOLL1B 661"
    ## [141] "             AAINTRO 662             AAQUOTA 663            AAMEMBER 664"
    ## [142] "             AATIBRK 665             AATRAIN 666              AAQUAL 667"
    ## [143] "              AAPREF 668            AAINTRO2 669            AAQUOTA2 670"
    ## [144] "            AAMEMBR2 671            AATIBRK2 672            AATRAIN2 673"
    ## [145] "             AAQUAL2 674             AAPREF2 675            WRKHRDB2 676"
    ## [146] "            SUCCESSB 677            SRDEMNDB 678            SRIRISHB 679"
    ## [147] "            SEPARAT1 680            SEPARAT2 681            AAINTRO3 682"
    ## [148] "              AAEQOP 683            AAREVDIS 684            AAUNFAIR 685"
    ## [149] "            AADVRSTY 686            AACONFLT 687            AAGRPWRS 688"
    ## [150] "             AACOLL2 689            AACOLL2A 690            AACOLL2B 691"
    ## [151] "         AA4JOBW 692-694          AA4EDW 695-697         AA4CONW 698-700"
    ## [152] "         AA4JOBB 701-703          AA4EDB 704-706         AA4CONB 707-709"
    ## [153] "         AA4JOBP 710-712          AA4EDP 713-715        PRVOTE96 716-717"
    ## [154] "        PRLEAN96 718-719            LIVELA93 720             MVOTE93 721"
    ## [155] "             MPREF93 722            MARRIED6 723            HAVEKIDS 724"
    ## [156] "            KIDSAGE1 725            KIDSAGE2 726            KIDSAGE3 727"
    ## [157] "             MEXICAN 728               ASIAN 729                EDUC 730"
    ## [158] "        YEARBORN 731-734        FUNDMNT2 735-736             ATTEND2 737"
    ## [159] "             ATTEND6 738            WORKING1 739             LAIDOFF 740"
    ## [160] "            JOBWORRY 741        YRCAMEUS 742-745                TNUS 746"
    ## [161] "        YRCAMESC 747-750            PBRNUS2A 751         HHTOTAL 752-753"
    ## [162] "              PHONES 754            PHONENUM 755                RELA 756"
    ## [163] "            RELIGSHT 757            REL2 758-759                RN7B 760"
    ## [164] "                RN8B 761                RN9B 762            RN7C 763-764"
    ## [165] "            RN8C 765-766            RN9C 767-768            R10C 769-770"
    ## [166] "             READMAG 771             READPAP 772            NATLNEWS 773"
    ## [167] "            LOCLNEWS 774            VIOLNEWS 775          BORNIN 776-778"
    ## [168] "            NATNALTY 779        NATNLTY2 780-782            PSBORNUS 783"
    ## [169] "        ANCSTRYB 784-786        ANCSTRYC 787-789            ANCSTRYD 790"
    ## [170] "        ANCSTRYE 791-793             CLOSETH 794            CLOSETH2 795"
    ## [171] "            CLOSRACE 796            IDENTIFY 797             ETHIMPT 798"
    ## [172] "            THINKETH 799             ATTEND7 800            FUNDMENT 801"
    ## [173] "            CLOSRELG 802               CLASS 803              CLASS2 804"
    ## [174] "            CLOSCLAS 805        IDLG 806-817 (A)            IDEOSTRG 818"
    ## [175] "            IDEOLEAN 819            GOVTROLE 820            GOVROLE2 821"
    ## [176] "        IMPTPRB1 822-823        IMPTPRB2 824-825        IMPTPRB3 826-827"
    ## [177] "            GOVTCONF 828            SLFIDEN1 829            SLFIDEN2 830"
    ## [178] "            SLFIDEN3 831            SLFIDEN4 832            SLFIDEN5 833"
    ## [179] "            SLFIDEN6 834            SLFIDEN7 835                RANM 836"
    ## [180] "             GRPINFL 837             GRPHELD 838             GRPREPS 839"
    ## [181] "            GRPSATTN 840            GRPSPRES 841            GRPSPROG 842"
    ## [182] "            GRPSCAND 843            SPNINTRO 844            PRISONS$ 845"
    ## [183] "             POLICE$ 846            SCHOOLS$ 847             SOCSEC$ 848"
    ## [184] "            ENVIRMT$ 849               POOR$ 850              BLACK$ 851"
    ## [185] "            WELFARE$ 852            DEFENSE$ 853            AMERISS2 854"
    ## [186] "            AMERETH1 855            LOVEUSA7 856            USFLAG1A 857"
    ## [187] "            COPABUSE 858        THMSMOKE 859-861            RNM2 862-863"
    ## [188] "            CNTC 864-865        THMAMERS 866-868          THMUSA 869-871"
    ## [189] "        THMBORNS 872-874        THMRCLAS 875-877         THMRETH 878-880"
    ## [190] "         THMRREL 881-883            AMTDISCB 884            GVATTNWB 885"
    ## [191] "            CIVRPUSH 886             BLKWELF 887            GOVTEQOP 888"
    ## [192] "            EXPLINTR 889            EXPLABLM 890            EXPLJOBM 891"
    ## [193] "            CRIMEVIC 892            RANDVIOL 893            CRIMINT1 894"
    ## [194] "              NOJOBS 895            NOMORALS 896            NOSCHOOL 897"
    ## [195] "            NOFAMILY 898            CRIMINT2 899            DEATHPEN 900"
    ## [196] "            REDPOVTY 901            THRSTRKS 902            JOBTRAIN 903"
    ## [197] "              PROUD2 904            DESERVB2 905            REALCHNG 906"
    ## [198] "            GVATTNB2 907            TRYHARDB 908             SLAVERY 909"
    ## [199] "              DUMMY1 910             PROP209 911            MARRIED7 912"
    ## [200] "         EDUCYRS 913-914            EDUCDEGR 915          INCOME 916-917"
    ## [201] "            ENG@HOME 918             HOODETH 919            SKINCLRA 920"
    ## [202] "               RNDA1 921               RNDA2 922               RNDA3 923"
    ## [203] "               RNDA4 924           RNDB1 925-926           RNDB2 927-928"
    ## [204] "           RNDB3 929-930           RNDB4 931-932           RNDB5 933-934"
    ## [205] "           RNDB6 935-936           RNDB7 937-938           RNDB8 939-940"
    ## [206] "             SPLIT98 941            CONFLCT5 942             AA4FIRM 943"
    ## [207] "            P227KNOW 944            P227PREF 945            TEACHENG 946"
    ## [208] "            KNOWPRIM 947            PRIMPREF 948            POLPRTY1 949"
    ## [209] "            POLPRTY2 950            POLPRTY3 951            POLPRTY4 952"
    ## [210] "            POLPRTY5 953            AMERCORP 954            REPSROLE 955"
    ## [211] "            INTGROUP 956             USFLAG1 957             LOVEUSA 958"
    ## [212] "               PROUD 959            USNOGOOD 960             USFLAG2 961"
    ## [213] "            BETRPLAC 962            PARTYREG 963             REGLEAN 964"
    ## [214] "            HOWPRIMV 965            REPPRIMV 966            DEMPRIMV 967"
    ## [215] "            COMPLAIN 968             BLKPUSH 969            DESERVEB 970"
    ## [216] "            WORKHRDB 971             DEMANDB 972            DENYDISB 973"
    ## [217] "             DEMANDH 974            AMTDISCH 975            DESERVEH 976"
    ## [218] "             PIDSTRR 977             PIDSTRD 978            GOVINSUR 979"
    ## [219] "             GOVINS2 980            KIDCARE$ 981             EDMATH$ 982"
    ## [220] "             EDARTS$ 983             EDSOSC$ 984            EDFLANG$ 985"
    ## [221] "            EDBILNG$ 986            RACETENB 987             BLKECON 988"
    ## [222] "             SOCDOM1 989             SOCDOM3 990             SOCDOM4 991"
    ## [223] "             SOCDOM5 992             SOCDOM6 993             USAEQOP 994"
    ## [224] "            AAVSDISC 995        AA4JOBWW 996-998        AA4EDWW 999-1001"
    ## [225] "      AA4JOBBW 1002-1004       AA4EDBW 1005-1007      AA4JOBBM 1008-1010"
    ## [226] "       AA4EDBM 1011-1013      PCTGRADS 1014-1016      PCTNOJOB 1017-1019"
    ## [227] "      PCT2JOBS 1020-1022       PCTWGUN 1023-1025      PCTCRREC 1026-1028"
    ## [228] "      PCTUNWED 1029-1031      PCTDONAT 1032-1034      PCTBLACK 1035-1037"
    ## [229] "      PCTWHITE 1038-1040       PCTHISP 1041-1043      PCTCRIMB 1044-1046"
    ## [230] "      PCTCRIMW 1047-1049      PCTCRIMH 1050-1052      PCTDOCSB 1053-1055"
    ## [231] "      PCTDOCSW 1056-1058      PCTDOCSH 1059-1061      PCTWELFB 1062-1064"
    ## [232] "      PCTWELFW 1065-1067      PCTWELFH 1068-1070      PCTPOLSB 1071-1073"
    ## [233] "      PCTPOLSW 1074-1076      PCTPOLSH 1077-1079      PCTGANGB 1080-1082"
    ## [234] "      PCTGANGW 1083-1085      PCTGANGH 1086-1088             ATTEND 1089"
    ## [235] "            MARRIED 1090           THINKSEX 1091            IDENSEX 1092"
    ## [236] "              RNDA5 1093              RNDA6 1094              RNDC1 1095"
    ## [237] "              RNDC2 1096              RNDC3 1097              RNDC4 1098"
    ## [238] "              RNDD1 1099              RNDD2 1100              RNDD3 1101"
    ## [239] "              RNDD4 1102           SPLIT99A 1103           SPLIT99B 1104"
    ## [240] "           SPLIT99C 1105           SPLIT99D 1106            RNDGOV1 1107"
    ## [241] "            RNDGOV2 1108            RNDGOV3 1109           FINPSTYR 1110"
    ## [242] "           LAECNPST 1111             BCPERJ 1112           BCAFFAIR 1113"
    ## [243] "            BCFAVUN 1114           STRFAVUN 1115           BCAPPDIS 1116"
    ## [244] "           IMPEACH1 1117           CENSURE1 1118           IMPEACH2 1119"
    ## [245] "           CENSURE2 1120           HIREBLKC 1121           HIREBLKD 1122"
    ## [246] "           HIREBLKE 1123           HIREHSB2 1124           ASUREQOP 1125"
    ## [247] "           ASURECON 1126           RACEPROB 1127           RACEINTR 1128"
    ## [248] "  RACEQUAL 1129-1153 (A)           AMERETH2 1154           TRYHRDW1 1155"
    ## [249] "           AMTDISW1 1156           DESERVW1 1157           GVNORWW1 1158"
    ## [250] "           GVNORBW1 1159           GVNORBM1 1160           PARTYRG2 1161"
    ## [251] "           GVNORWW2 1162           GVNORBW2 1163           GVNORBM2 1164"
    ## [252] "        THMLIB 1165-1167        THMCON 1168-1170           TRYHRDW2 1171"
    ## [253] "           AMTDISW2 1172           DESERVW2 1173           GVNORWW3 1174"
    ## [254] "           GVNORBW3 1175           GVNORBM3 1176           TRYHARDH 1177"
    ## [255] "           DESERVH2 1178           HICAWORK 1179           HICACRIM 1180"
    ## [256] "           HICAEDUC 1181           HICAAMER 1182           USAEQOP1 1183"
    ## [257] "           HOODFEEL 1184       COMMUTE 1185-1187           SPLIT00A 1188"
    ## [258] "           SPLIT00B 1189           RACEOPEN 1190      RACEQUL0 1191-1193"
    ## [259] "      ETHTHINK 1194-1195           GOVTEQJB 1196           GOVTDISC 1197"
    ## [260] "           SPCLTRTB 1198            MENBTTR 1199           MENBTTR2 1200"
    ## [261] "           GOVCOMPJ 1201           GOVCOMPB 1202           UNDERREP 1203"
    ## [262] "           EQOPIMPT 1204           PRISONBM 1205           PRISONWM 1206"
    ## [263] "           PRISONBF 1207           PRISONWF 1208           PRISNTRM 1209"
    ## [264] "            COPAUTH 1210           COPAUTH2 1211           COPBRUTL 1212"
    ## [265] "           COPFEAR2 1213           RAMPKNOW 1214           RAMPFEEL 1215"
    ## [266] "            RAMPPUN 1216           RAMPVIC1 1217      RAMPVIC2 1218-1219"
    ## [267] "           RAMPPOL1 1220      RAMPPOL2 1221-1222           OVRCOMEB 1223"
    ## [268] "           AMTDISCW 1224        THMFEM 1225-1227            HEALTH$ 1228"
    ## [269] "           HIGHWAY$ 1229      EDUCYRS2 1230-1231           SKINCOLR 1232"
    ## [270] "           SPLIT01A 1233           SPLIT01B 1234           SPLIT01C 1235"
    ## [271] "           SPLIT01D 1236           SPLIT01E 1237           SPLIT01F 1238"
    ## [272] "           GOVCMPB1 1239           GOVCMPB2 1240           TRYGOALB 1241"
    ## [273] "           SUCCEEDB 1242           AMBTIONB 1243           WILLWRKB 1244"
    ## [274] "           SAMEFATE 1245            BTRCHNC 1246           DEATHPN2 1247"
    ## [275] "           THRSTRK1 1248           HISPECON 1249           SRIRSHH1 1250"
    ## [276] "           DISCRIMH 1251           WORKHRD1 1252           AMBITION 1253"
    ## [277] "           WILLWORK 1254            TRYGOAL 1255           SUCCESS1 1256"
    ## [278] "            SUCCEED 1257            ETHCON2 1258      ETHCON2A 1259-1260"
    ## [279] "           ETHCON3A 1261           ETHCON5I 1262           ETHCON5A 1263"
    ## [280] "           ETHCON5B 1264           ETHCON5C 1265           ETHCON5D 1266"
    ## [281] "           ETHCON6A 1267           ETHCON6B 1268           ETHCON6C 1269"
    ## [282] "           ETHCON6D 1270           ETHCON7A 1271           ETHCON7B 1272"
    ## [283] "        THMENV 1273-1275           OFFCLNG2 1276               QVER 1277"
    ## [284] "              SPLTS 1278            AMNESTY 1279  RACELNGB 1280-1330 (A)"
    ## [285] "             PROUD3 1331           RACETENH 1332              SEP11 1333"
    ## [286] "            TALIBAN 1334              PEARL 1335             TERROR 1336"
    ## [287] "           SECURITY 1337            LIFECHG 1338             FUTURE 1339"
    ## [288] "            MONITOR 1340           MILITACT 1341           STRNGACT 1342"
    ## [289] "            INTLLAW 1343              REACT 1344           ARABSYMP 1345"
    ## [290] "           TARGARAB 1346           SINGLOUT 1347              KABUL 1348"
    ## [291] "           RUMSFELD 1349      THMPALES 1350-1352       THMISRL 1353-1355"
    ## [292] "      THMMUSLM 1356-1358      THMARABS 1359-1361            HMSXMIL 1362"
    ## [293] "           SOMEBTR1 1363           SOMEBTR2 1364           AMBTION3 1365"
    ## [294] "           SOCDOM17 1366           SOCDOM18 1367           SOCDOM19 1368"
    ## [295] "           SOCDOM5B 1369           SOCDM12A 1370           SOCDOM20 1371"
    ## [296] "           SOCDOM21 1372           LARIOT3A 1373           LARIOT2A 1374"
    ## [297] "           NOSCHOL2 1375           NOMORLS2 1376            NOJOBS2 1377"
    ## [298] "           EXPLEFTB 1378           EXPLDSB2 1379           EXPLBLB2 1380"
    ## [299] "           EXPLDUB2 1381           EXPLMTB2 1382           EXPLENGB 1383"
    ## [300] "           EXPLDISH 1384           EXPLABLH 1385           EXPLEDUH 1386"
    ## [301] "           EXPLMOTH 1387           EXPLENGH 1388           HOODCG2A 1389"
    ## [302] "           RKINGVER 1390           NOJUSTSB 1391           NOJUSTSH 1392"
    ## [303] "            REBUILD 1393           REBUILD2 1394           REBUILD3 1395"
    ## [304] "           REBUILD5 1396             DONATE 1397             GOTGUN 1398"
    ## [305] "            GOTGUN2 1399            GOTGUN3 1400           ETHRLFU1 1401"
    ## [306] "            RESRCEH 1402           RESRCEH2 1403            RESRCEB 1404"
    ## [307] "           RESRCEB2 1405            RESRCEA 1406           RESRCEA2 1407"
    ## [308] "            RESRCEW 1408           RESRCEW2 1409            CCSPEND 1410"
    ## [309] "           CCSPEND2 1411             MINEF1 1412             MINEF2 1413"
    ## [310] "           POLPWRB1 1414           POLPWRB2 1415            POLPWRH 1416"
    ## [311] "       THMBSNS 1417-1419        THMPOL 1420-1422      THMHSPGP 1423-1425"
    ## [312] "      THMBLKGP 1426-1428           IDEOSTG2 1429            PVOTE92 1430"
    ## [313] "           PVOTE92A 1431            LEVOTEA 1432            LEVOTEB 1433"
    ## [314] "            FROMGRP 1434            LGOVINF 1435           LGOVCNCL 1436"
    ## [315] "                NMC 1437           NM1 1438-1452           NM2 1453-1467"
    ## [316] "           NM3 1468-1482               CLS1 1483               CLS2 1484"
    ## [317] "               CLS3 1485               NM10 1486               NM11 1487"
    ## [318] "          NM12 1488-1489          NM13 1490-1491               NM14 1492"
    ## [319] "               NM15 1493               N15A 1494               N15B 1495"
    ## [320] "               N15C 1496               N15D 1497               NM16 1498"
    ## [321] "               NM20 1499               NM21 1500          NM22 1501-1502"
    ## [322] "          NM23 1503-1504               NM24 1505               NM25 1506"
    ## [323] "               N25A 1507               N25B 1508               N25C 1509"
    ## [324] "               N25D 1510               NM26 1511               NM30 1512"
    ## [325] "               NM31 1513          NM32 1514-1515          NM33 1516-1517"
    ## [326] "               NM34 1518               NN35 1519               N35A 1520"
    ## [327] "               N35B 1521               N35C 1522               N35D 1523"
    ## [328] "               NM36 1524               NM41 1525               DMDF 1526"
    ## [329] "               DMEX 1527            HOUSING 1528           MARRIED3 1529"
    ## [330] "           LIVEWITH 1530      BORNINST 1531-1533      INDUSTRY 1534-1536"
    ## [331] "              UNION 1537           PWORKING 1538           RGCJOBSA 1539"
    ## [332] "           RGCLPOLA 1540           RGCHOUSA 1541           RGCECONA 1542"
    ## [333] "           RGCJBSB2 1543           RGCLPLB2 1544           RGCHOSB2 1545"
    ## [334] "           RGCECNB2 1546           RGCJOBSH 1547           RGCLPOLH 1548"
    ## [335] "           RGCHOUSH 1549           RGCECONH 1550         CON2A 1551-1552"
    ## [336] "            INCOME8 1553       INCOME9 1554-1556              ETHOP 1557"
    ## [337] "             SOCDLT 1558            PHONES2 1559             COUNTY 1560"
    ## [338] "           PIDLEAN2 1561      COUNTYCD 1562-1566               RNDM 1567"
    ## [339] "          DM12 1568-1569           HEALTH$2 1570           EDUCSPD$ 1571"
    ## [340] "             POOR$2 1572            PUBSCHL 1573           SATISWRK 1574"
    ## [341] "           SATISNWK 1575           SATISFAM 1576           SATISFRN 1577"
    ## [342] "           SATISHLT 1578           SATISHSE 1579            EXPMCPB 1580"
    ## [343] "           IMENGCUL 1581           MELTPOT4 1582           RGCLPOLI 1583"
    ## [344] "           IMECONR2 1584           GOVCONFF 1585           GOVCONFS 1586"
    ## [345] "           ABORTIN2 1587      THMHSPML 1588-1590      THMBLKML 1591-1593"
    ## [346] "           CNTCTNWB 1594           CNTCTNWA 1595           CNTCTNWH 1596"
    ## [347] "           CNTCTMWB 1597           CNTCTMWA 1598           CNTCTMWH 1599"
    ## [348] "           CNTCTNBW 1600           CNTCTNBA 1601           CNTCTNBH 1602"
    ## [349] "           CNTCTMBW 1603           CNTCTMBA 1604           CNTCTMBH 1605"
    ## [350] "           CNTCTNAB 1606           CNTCTNAW 1607           CNTCTNAH 1608"
    ## [351] "           CNTCTMAB 1609           CNTCTMAW 1610           CNTCTMAH 1611"
    ## [352] "           CNTCTNHB 1612           CNTCTNHA 1613           CNTCTNHW 1614"
    ## [353] "           CNTCTMHB 1615           CNTCTMHA 1616           CNTCTMHW 1617"
    ## [354] "           CRIMVIC2 1618           DEATHPN3 1619           MAJPURCH 1620"
    ## [355] "           GRPALONE 1621           WRKETHNC 1622           WRKETHCH 1623"
    ## [356] "            WRKCHOW 1624            WRKCHGD 1625            WRKLANG 1626"
    ## [357] "           INCOME2A 1627           INCOME3A 1628           INCOME6A 1629"
    ## [358] "           SLFSPRTW 1630           SLFSPRTB 1631           SLFSPRTA 1632"
    ## [359] "           SLFSPRTH 1633           GETALNGW 1634           GETALNGB 1635"
    ## [360] "           GETALNGA 1636           GETALNGH 1637                IN5 1638"
    ## [361] "                IN7 1639            BILING2 1640           BORDRS$2 1641"
    ## [362] "            HIREIMM 1642           IMMTOTL2 1643            ENGREPL 1644"
    ## [363] "           ENGADPTA 1645            STRNWKA 1646            ASNWELF 1647"
    ## [364] "           GOVATNA2 1648             AA4EDM 1649            JOBTRNB 1650"
    ## [365] "            JOBTRNH 1651            JOBTRNA 1652           HREBLKB2 1653"
    ## [366] "           HREHISB2 1654           HREASNB2 1655            AA4EDB2 1656"
    ## [367] "             AA4EDH 1657             AA4EDA 1658            AAUNFRB 1659"
    ## [368] "             AAOPPB 1660            AAUNQLB 1661           AACOMPTV 1662"
    ## [369] "           BLKWELF2 1663           HISADPEN 1664           HISWRKFC 1665"
    ## [370] "           HISPWELF 1666           GOVATNH2 1667           POLCHIEF 1668"
    ## [371] "           LARIOTS4 1669               B4RK 1670             INTETH 1671"
    ## [372] "               EXC1 1672               EXC2 1673"

Trying to fill these in by hand would be very onerous, to say the least. With the data that you're working with, find these lines in the associated codebook. If you're not sure where they are, just find the path to the codebook and the below function will try to guess.

The following function takes four arguments: `data_filename`, the path to the data file, `codebook_filename`, the path to the codebook file, `startline`, the (optional) line of code on which the list of variable locations starts, and `endline`, the (optional line of code on which the list of variable locations ends.)

``` r
open_with_codebook <- function(data_filename = NA, codebook_filename = NA, startline = NA, endline = NA){
  require(tidyr)
  
  x <- readLines(codebook_filename)
  
  if(is.na(startline) | is.na(endline)){
    startline <- which(grepl('/1', x))[1]
    endline <- which(grepl("^.\\s+[.]", x))[1]
    
    print(paste0("Start and end values for variable positions not entered. Guessing that they start at line ",startline," and end at line ", endline, "."))
  } else {
    print(paste0("Start and end variable positions based on lines ", startline, " through ", endline, " of codebook file ", codebook_filename))
  }
  
  nam <- x[startline:endline]
  nam[1] <- gsub('^.*/[A-Za-z0-9]', '', nam[1])
  nam[length(nam)] <- gsub('^.*/[A-Za-z0-9]', '', nam[length(nam)])
  
  nam <- nam %>%
    lapply(function(x) trimws(gsub('\\s{2}', ' ', x))) %>%
    unlist() %>%
    strsplit('\\s{2}') %>%
    unlist() %>%
    .[. != ''] %>%
    trimws() %>%
    gsub('*.[(].*', '', .)
  
  x2 = lapply(nam, function(x) strsplit(x, '\\s|-')[[1]])
  
  cb_mat <- matrix(ncol = 4, nrow = length(x2))
  cb_mat[,1] <- unlist(lapply(x2, function(x) x[1]))
  cb_mat[,2] <- unlist(lapply(x2, function(x) x[2]))
  cb_mat[,3] <- unlist(lapply(x2, function(x) ifelse(length(x) == 3, x[3], x[2])))
  cb_mat[,4] <- as.numeric(cb_mat[,3]) - as.numeric(cb_mat[,2]) + 1
  cb_mat = cb_mat[order(as.numeric(cb_mat[,2])),]
  
  the_widths = as.numeric(cb_mat[,4])
  var_names = cb_mat[,1]
  
  d = read.fwf(data_filename, widths = the_widths, col.names = var_names)
  
  return(d)
}

dat <- open_with_codebook(data_filename = 'data.txt', codebook_filename = 'data_spss.txt', startline = 10, endline = 381)
```

    ## Loading required package: tidyr

    ## [1] "Start and end variable positions based on lines 10 through 381 of codebook file data_spss.txt"

``` r
dat[1:5, 1:5]
```

    ##      CASEID YEAR OVERSAMP RND1_4 RND2_4
    ## 1 199200001 1992        0     NA     NA
    ## 2 199200003 1992        0     NA     NA
    ## 3 199200008 1992        0     NA     NA
    ## 4 199200015 1992        0     NA     NA
    ## 5 199200021 1992        0     NA     NA

I use a truncated version of the LACSS raw data for this example. It will probably take awhile to run on very large surveys, but no longer than the canned `R` functions. As far as I can tell, this function works fine on about any such data that I can find. Let me know if it works for you, or doesn't!
