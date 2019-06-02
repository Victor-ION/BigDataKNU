Лабораторна робота № 3. Зчитування даних з WEB.
================

В цій лабораторній роботі необхідно зчитати WEB сторінку з сайту
IMDB.com зі 100 фільмами 2017 року виходу за посиланням
«<http://www.imdb.com/search/title?count=100&release_date=2017,2017&title_type=feature>».
Необхідно створити data.frame «movies» з наступними даними: номер фільму
(rank\_data), назва фільму (title\_data), тривалість (runtime\_data).
Для виконання лабораторної рекомендується використати бібліотеку
«rvest». CSS селектори для зчитування необхідних даних: rank\_data:
«.text-primary», title\_data: «.lister-item-header a», runtime\_data:
«.text-muted .runtime». Для зчитування url використовується функція
read\_html, для зчитування даних по CSS селектору – html\_nodes і для
перетворення зчитаних html даних в текст - html\_text. Рекомендується
перетворити rank\_data та runtime\_data з тексту в числові значення. При
формуванні дата фрейму функцією data.frame рекомендується використати
параметр «stringsAsFactors = FALSE». В результаті виконання
лабораторної роботи необхідно відповісти на запитання нижче.

``` r
library(xml2)
library(rvest)

html = read_html("http://www.imdb.com/search/title?count=100&release_date=2017,2017&title_type=feature")
rank_data <- as.numeric(html_text(html_nodes(html, '.text-primary')))
title_data <- html_text(html_nodes(html, '.lister-item-header a'))

# Firstly, we need to treat empty runtime cases. Current approach is to get list of outer objects and check for each case whether we have runtime or not, for the last case we should add NA to the result list, not just skip it:)
content_data <- html_nodes(html, '.lister-item-content')
runtime_data_raw <- character(0)
for (content_node in content_data) {
  extracted = html_text(html_node(content_node, '.text-muted .runtime'))
  if (length(extracted) == 0) {
    runtime_data_raw <- c(runtime_data_raw, NA)
  } else {
    runtime_data_raw <- c(runtime_data_raw, extracted)
  }
}
# old approach - not working for empty runtime cases
#runtime_data_raw <- html_text(html_nodes(html, '.text-muted .runtime'))

runtime_data <- as.numeric(gsub(" min", "", runtime_data_raw)) 

movies <- data.frame(Rank = rank_data, Title = title_data, Runtime = runtime_data, stringsAsFactors = FALSE)
movies
```

    ##     Rank                                         Title Runtime
    ## 1      1                                   John Wick 2     122
    ## 2      2                                 Thor Ragnarok     130
    ## 3      3                                            Ça     135
    ## 4      4                   The Upside - Seconde Chance     126
    ## 5      5                                       Get Out     104
    ## 6      6                        Spider-Man: Homecoming     133
    ## 7      7                           La Belle et la Bête     129
    ## 8      8             Les gardiens de la galaxie Vol. 2     136
    ## 9      9                     Baywatch: Alerte à Malibu     116
    ## 10    10                          The Greatest Showman     105
    ## 11    11                             Blade Runner 2049     164
    ## 12    12                                  Wonder Woman     141
    ## 13    13                                   Baby Driver     113
    ## 14    14                                Justice League     120
    ## 15    15                                 Power Rangers     124
    ## 16    16   Star Wars: Episode VIII - Les derniers Jedi     152
    ## 17    17                                         Logan     137
    ## 18    18                                 Atomic Blonde     115
    ## 19    19             Jumanji: Bienvenue dans la jungle     119
    ## 20    20                                     Dunkerque     106
    ## 21    21                          Call Me by Your Name     132
    ## 22    22    3 Billboards: Les panneaux de la vengeance     115
    ## 23    23                            Kong: Skull Island     118
    ## 24    24                                     Good Time     101
    ## 25    25                             La forme de l'eau     123
    ## 26    26                      Kingsman: Le cercle d'or     141
    ## 27    27                             Mes vies de chien     100
    ## 28    28 Pirates des Caraïbes: la Vengeance de Salazar     129
    ## 29    29                              Fast & Furious 8     136
    ## 30    30                                          Coco     105
    ## 31    31                               Alien: Covenant     122
    ## 32    32                   Mektoub, My Love: Canto Uno     181
    ## 33    33         Le roi Arthur: La légende d'Excalibur     126
    ## 34    34                                        Mother     121
    ## 35    35                                   Shot Caller     121
    ## 36    36                          Le bonhomme de neige     119
    ## 37    37                                      The Wife      99
    ## 38    38                 Transformers: The Last Knight     154
    ## 39    39                                La Tour sombre      95
    ## 40    40                                        Wonder     113
    ## 41    41                                    Wind River     107
    ## 42    42                                     Lady Bird      94
    ## 43    43                                      La momie     110
    ## 44    44                  Le crime de l'Orient-Express     114
    ## 45    45                                         Mercy     108
    ## 46    46                              xXx: Reactivated     107
    ## 47    47        Valérian et la Cité des Mille Planètes     137
    ## 48    48                                        Bright     117
    ## 49    49                            Ghost in the Shell     107
    ## 50    50                                        Jessie     103
    ## 51    51                                    Moi, Tonya     120
    ## 52    52                                    The Circle     110
    ## 53    53                               Happy Birthdead      96
    ## 54    54                                      Papillon     133
    ## 55    55                     Mise à mort du cerf sacré     121
    ## 56    56                                  Le grand jeu     140
    ## 57    57                                    Section 99     132
    ## 58    58                                      Hostiles     134
    ## 59    59                                 Seven Sisters     123
    ## 60    60                                 Unicorn Store      92
    ## 61    61                                Le 12ème Homme     135
    ## 62    62      Vegas Academy: Coup de Poker pour la Fac      88
    ## 63    63                                 # Pire soirée     101
    ## 64    64                               A Beautiful Day      89
    ## 65    65                                          Mary     101
    ## 66    66                                   Logan Lucky     118
    ## 67    67               Annabelle 2: la création du mal     109
    ## 68    68                            Les heures sombres     125
    ## 69    69                               Pentagon Papers     116
    ## 70    70                  Barry Seal: American Traffic     115
    ## 71    71                            Hitman & Bodyguard     118
    ## 72    72                           The Disaster Artist     104
    ## 73    73                Cinquante nuances plus sombres     118
    ## 74    74                        Life: Origine inconnue     104
    ## 75    75                                       Revenge     108
    ## 76    76                                   To the Bone     107
    ## 77    77                              My Friend Dahmer     107
    ## 78    78                         The Hatton Garden Job      93
    ## 79    79                                   Death House      95
    ## 80    80                             American Assassin     112
    ## 81    81                                La Baby-Sitter      85
    ## 82    82                                Phantom Thread     130
    ## 83    83                                      Geostorm     109
    ## 84    84                               The Current War     105
    ## 85    85                                     Le Rituel      94
    ## 86    86             La planète des singes: Suprématie     140
    ## 87    87                            La mort de Staline     107
    ## 88    88                        Tout l'argent du monde     132
    ## 89    89                                     Ferdinand     108
    ## 90    90                                 Désobéissance     114
    ## 91    91                                          Okja     120
    ## 92    92                          Voice from the Stone      94
    ## 93    93                                          1922     102
    ## 94    94                               Pitch Perfect 3      93
    ## 95    95                                     Valentine      NA
    ## 96    96                                    Les proies      93
    ## 97    97                                  Line of fire     134
    ## 98    98                Sur le chemin de la rédemption     113
    ## 99    99                                 The Foreigner     113
    ## 100  100                                   The Endless     111

1.  Виведіть перші 6 назв фільмів дата фрейму.

<!-- end list -->

``` r
movies[1:6, "Title"]
```

    ## [1] "John Wick 2"                 "Thor Ragnarok"              
    ## [3] "Ça"                          "The Upside - Seconde Chance"
    ## [5] "Get Out"                     "Spider-Man: Homecoming"

2.  Виведіть всі назви фільмів с тривалістю більше 120 хв.

<!-- end list -->

``` r
movies[which(movies$Runtime > 120), "Title"]
```

    ##  [1] "John Wick 2"                                  
    ##  [2] "Thor Ragnarok"                                
    ##  [3] "Ça"                                           
    ##  [4] "The Upside - Seconde Chance"                  
    ##  [5] "Spider-Man: Homecoming"                       
    ##  [6] "La Belle et la Bête"                          
    ##  [7] "Les gardiens de la galaxie Vol. 2"            
    ##  [8] "Blade Runner 2049"                            
    ##  [9] "Wonder Woman"                                 
    ## [10] "Power Rangers"                                
    ## [11] "Star Wars: Episode VIII - Les derniers Jedi"  
    ## [12] "Logan"                                        
    ## [13] "Call Me by Your Name"                         
    ## [14] "La forme de l'eau"                            
    ## [15] "Kingsman: Le cercle d'or"                     
    ## [16] "Pirates des Caraïbes: la Vengeance de Salazar"
    ## [17] "Fast & Furious 8"                             
    ## [18] "Alien: Covenant"                              
    ## [19] "Mektoub, My Love: Canto Uno"                  
    ## [20] "Le roi Arthur: La légende d'Excalibur"        
    ## [21] "Mother"                                       
    ## [22] "Shot Caller"                                  
    ## [23] "Transformers: The Last Knight"                
    ## [24] "Valérian et la Cité des Mille Planètes"       
    ## [25] "Papillon"                                     
    ## [26] "Mise à mort du cerf sacré"                    
    ## [27] "Le grand jeu"                                 
    ## [28] "Section 99"                                   
    ## [29] "Hostiles"                                     
    ## [30] "Seven Sisters"                                
    ## [31] "Le 12ème Homme"                               
    ## [32] "Les heures sombres"                           
    ## [33] "Phantom Thread"                               
    ## [34] "La planète des singes: Suprématie"            
    ## [35] "Tout l'argent du monde"                       
    ## [36] "Line of fire"

3.  Скільки фільмів мають тривалість менше 100 хв.

<!-- end list -->

``` r
nrow(movies[which(movies$Runtime < 100),])
```

    ## [1] 14
