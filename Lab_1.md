Лабораторна робота № 1. Завантаження та зчитування даних.
================

1.  За допомогою download.file() завантажте любий excel файл з порталу
    <http://data.gov.ua> та зчитайте його (xls, xlsx – бінарні формати,
    тому встановить mode = “wb”. Виведіть перші 6 строк отриманого
    фрейму даних.

<!-- end list -->

``` r
library(xlsx)
Sys.setlocale(locale = "ukrainian")
```

    ## [1] "LC_COLLATE=Ukrainian_Ukraine.1251;LC_CTYPE=Ukrainian_Ukraine.1251;LC_MONETARY=Ukrainian_Ukraine.1251;LC_NUMERIC=C;LC_TIME=Ukrainian_Ukraine.1251"

``` r
download.file("https://data.gov.ua/dataset/8f34acc1-5853-46bf-80eb-ea40a55292fe/resource/810b8f57-67d5-4917-b3c2-236fcf94b020/download/energy_bal_year_plan_29_03_2019.xlsx", "d_1.xlsx", "auto", TRUE, "wb")
d_1 <- read.xlsx("d_1.xlsx", sheetIndex = 1, header = TRUE, encoding = "UTF-8")
head(d_1, n = 6)
```

    ##   approval_date forecast_year forecast_month component sub_component value
    ## 1    2017-12-26          2018              1         1           ТЕС  5550
    ## 2    2017-12-26          2018              1         1           ТЕЦ  1678
    ## 3    2017-12-26          2018              1         1           ГЕС   787
    ## 4    2017-12-26          2018              1         1          ГАЕС   155
    ## 5    2017-12-26          2018              1         1           АЕС  7121
    ## 6    2017-12-26          2018              1         1  блок-станції   144

2.  За допомогою download.file() завантажте файл
    getdata\_data\_ss06hid.csv за посиланням
    <https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv>
    та завантажте дані в R. Code book, що пояснює значення змінних
    знаходиться за посиланням
    <https://www.dropbox.com/s/dijv0rlwo4mryv5/PUMSDataDict06.pdf?dl=0>
    Необхідно знайти, скільки property мають value
$1000000+

<!-- end list -->

``` r
download.file("https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv","d_2.csv", "auto", TRUE,"wb")
d_2 <- read.csv("d_2.csv")
sum(d_2$VAL == 24, na.rm = TRUE)
```

    ## [1] 53

3.  Зчитайте xml файл за посиланням
    <http://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Frestaurants.xml>
    Скільки ресторанів мають zipcode 21231?

<!-- end list -->

``` r
library(XML)
download.file("http://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Frestaurants.xml","d_3.xml", "auto", TRUE,"wb")
d_3 <- xmlRoot(xmlTreeParse("d_3.xml", useInternalNodes = TRUE))
sum(xpathSApply(d_3, "//zipcode", xmlValue) == 21231)
```

    ## [1] 127
