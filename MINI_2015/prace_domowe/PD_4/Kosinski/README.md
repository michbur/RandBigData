# 4 praca domowa
Marcin Kosinski  
9 kwietnia 2015  




```r
# Napisać funkcję przyjmująca za argumenty markę, model, rodzaj paliwa,
# moc silnika, rok produkcji, przebieg
# a jako wynik zwracającą minimum, maksimum i kwartyle dla cen aut
# pasujących do argumentów.
# Jeżeli argument jest wskazany to należy zawęzić się tylko do aut z tym
# argumentem (np Model='Toyota'), jeżeli argument nie jest podany to nie
# należy filtrować po tym kryterium.
# 
# Wewnątrz funkcji należy wykorzystać na ile to możliwe funkcje
# dplyrowe.


library(PogromcyDanych)
library(dplyr)


zrob4praceDomowa <- function( marka = NULL,
                              model = NULL,
                              RodzajPaliwa = NULL,
                              moc = NULL,
                              RokProdukcji = NULL,
                              Przebieg = NULL){
   stopifnot( is.null( marka ) | is.character( marka ) )
   stopifnot( is.null( model ) | is.character( model ) )
   stopifnot( is.null( RodzajPaliwa ) | is.character( RodzajPaliwa ) )
   stopifnot( is.null( moc ) | is.numeric( moc ) )
   stopifnot( is.null( RokProdukcji ) | is.numeric( RokProdukcji ) )
   stopifnot( is.null( Przebieg ) | is.numeric( Przebieg ) )
   
   if( !is.null(marka) ){
      auta2012 <- auta2012 %>% 
         filter( Marka %in% marka )
   }
   if( !is.null(model) ){
      auta2012 <- auta2012 %>% 
         filter( Model %in% model )
   }
   if( !is.null(RodzajPaliwa) ){
      auta2012 <- auta2012 %>% 
         filter( Rodzaj.paliwa %in% RodzajPaliwa )
   }
   if( !is.null(moc) ){
      auta2012 <- auta2012 %>% 
         filter( kW == moc )
   }
   if( !is.null(RokProdukcji) ){
      auta2012 <- auta2012 %>% 
         filter( Rok.produkcji == RokProdukcji )
   }
   if( !is.null(Przebieg) ){
      auta2012 <- auta2012 %>% 
         filter( Przebieg.w.km == Przebieg )
   }
   
   
   auta2012 %>% 
      summarise( minCena = min(Cena.w.PLN),
                 maxCena = max(Cena.w.PLN),
                 q1 = quantile(Cena.w.PLN, 0.25),
                 q2 = quantile(Cena.w.PLN, 0.5),
                 q3 = quantile(Cena.w.PLN, 0.75)
                 ) %>% 
      select( minCena, maxCena, q1, q2, q3)
   
}


zrob4praceDomowa()
```

```
  minCena  maxCena    q1    q2      q3
1     400 11111111 10900 19900 37470.9
```

```r
zrob4praceDomowa( marka = "Aixam")
```

```
  minCena maxCena    q1    q2    q3
1    3000   53900 13475 24600 42683
```

```r
zrob4praceDomowa( marka = "Aixam", RokProdukcji = 2012)
```

```
  minCena maxCena    q1    q2      q3
1   43820   45365 44470 45120 45242.5
```

