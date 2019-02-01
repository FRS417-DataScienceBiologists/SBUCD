---
title: "lab1_hw_SBUCD"
output: 
  html_document: 
    keep_md: yes
---




```r
5 - 3 * 2
```

```
## [1] -1
```

```r
8 / 2 ** 2
```

```
## [1] 2
```
## These results did not surprise me. However, it is interesting to note that r already has an order of operations.


```r
(5 - 3) * 2
```

```
## [1] 4
```

```r
(8 / 2) ** 2
```

```
## [1] 16
```

```r
blackjack <- c(140, -20, 70, -120, 240)
roulette <- c(60, 50, 120, -300, 10)
```


```r
days <- c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday")
```


```r
names(blackjack) <- days
names(roulette) <- days
```


```r
total_blackjack <- sum(blackjack)
total_blackjack
```

```
## [1] 310
```


```r
total_roulette <- sum(roulette)
total_roulette
```

```
## [1] -60
```


```r
sum(blackjack[1] + roulette[1])
```

```
## [1] 200
```

```r
sum(blackjack[2] + roulette[2])
```

```
## [1] 30
```

```r
sum(blackjack[3] + roulette[3])
```

```
## [1] 190
```

```r
sum(blackjack[4] + roulette[4])
```

```
## [1] -420
```

```r
sum(blackjack[5] + roulette[5])
```

```
## [1] 250
```


```r
daily_total <- c(200, 30, 190, -420, 250)
```


```r
total_week <- cbind(blackjack, roulette, daily_total)
total_week
```

```
##           blackjack roulette daily_total
## Monday          140       60         200
## Tuesday         -20       50          30
## Wednesday        70      120         190
## Thursday       -120     -300        -420
## Friday          240       10         250
```
## Thursday seems unlucky


```r
if (total_blackjack > total_roulette) {
  print ("stick to blackjack")  
}
```

```
## [1] "stick to blackjack"
```

```r
if (total_roulette > total_blackjack)
  {
  print ("stick to roulette")
}
```
