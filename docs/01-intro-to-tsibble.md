# 시계열 첫걸음, 관련 패키지의 이해 {#intro}

## `tsibble` 그리고 `fable` 패키지

시계열과 관련 된 R 패키지는 정말 많이 있다. 그 중에서 가장 유명한 패키지는 `forecast` 패키지이다. `fpp` 2판에서 메인 패키지로 활용되었던 `forecast` 패키지는 예측성능이 좋기로 유명하다. 하지만 `forecast`는 base 패키지를 기반으로 짜여져있는 패키지라서 `fpp` 3판에서 사용하게 될 패키지는 `tsibble`과 `fable` 패키지를 주로 사용하게 될 예정이다. 두 패키지 모두 `tidyverse`스러운 코딩과 잘 맞는 함수들을 지원하는 것이 특징이다.

<div class="rmdwarning">
<h3 id="주의하기">주의하기</h3>
<p><code>forecast</code> 패키지는 다른 언어를 사용하는 커뮤니티에도 알려질 정도로 잘 짜여진 패키지이다. 꼭 한번은 사용법을 알아둘 것을 추천한다. <a href="https://otexts.com/fppkr/"><code>fpp</code> 2판의 한글판</a>을 봐두는 것도 나쁘지 않는 선택이다.</p>
</div>

## `fpp3` 패키지

`fpp3` 패키지는 Hyndman 교수와 Athanasopoulos 교수가 지은 Forecasting: principles and practice 3판에 딸림 R 패키지라고 생각하면 된다. `fpp3` 패키지를 로드시키면 앞에서 말했던 `tsibble`과 `fable` 패키지가 같이 로드된다.


```r
library(fpp3)
```

## tsible 객체


```r
y <- tsibble(
  year = 2015:2019,
  observation = c(123, 39, 78, 52, 110),
  index = year
)
y
```

```
#> # A tsibble: 5 x 2 [1Y]
#>    year observation
#>   <int>       <dbl>
#> 1  2015         123
#> 2  2016          39
#> 3  2017          78
#> 4  2018          52
#> 5  2019         110
```

`tibble` 객체를 확장 시킨 `tidyverse`용 데이터 프레임. y변수를 선언할 때 어떤 열을 `index`로 할 것인지 선언해 줌. `[1Y]`라는 것을 보고 관찰값이 1년 단위 관측되었다는 것을 알 수 있음.

`tsibbledata` 패키지에 들어있는 `olympic_running` 데이터를 살펴보자.


```r
olympic_running
```

```
#> # A tsibble: 312 x 4 [4Y]
#> # Key:       Length, Sex [14]
#>     Year Length Sex    Time
#>    <int>  <int> <chr> <dbl>
#>  1  1896    100 men    12  
#>  2  1900    100 men    11  
#>  3  1904    100 men    11  
#>  4  1908    100 men    10.8
#>  5  1912    100 men    10.8
#>  6  1916    100 men    NA  
#>  7  1920    100 men    10.8
#>  8  1924    100 men    10.6
#>  9  1928    100 men    10.8
#> 10  1932    100 men    10.3
#> # ... with 302 more rows
```

위의 결과값을 보면 key 변수가 2개 (Length, Sex) 로 설정이 되어있고, `[14]`라는 표시가 보인다. 이것은 두 개의 변수가 갖는 unique한 값들로 조합 할 수 있는 경우의 수가 14가지 경우가 된다는 것을 의미한다.

다음을 살펴보자.


```r
olympic_running %>% 
  distinct(Length)
```

```
#> # A tibble: 7 x 1
#>   Length
#>    <int>
#> 1    100
#> 2    200
#> 3    400
#> 4    800
#> 5   1500
#> 6   5000
#> 7  10000
```

```r
olympic_running %>% 
  distinct(Sex)
```

```
#> # A tibble: 2 x 1
#>   Sex  
#>   <chr>
#> 1 men  
#> 2 women
```

## csv 파일로부터 `tsibble` 객체 만들기 연습


```r
prison <- readr::read_csv("https://OTexts.com/fpp3/extrafiles/prison_population.csv")
head(prison)
```

```
#> # A tibble: 6 x 6
#>   Date       State Gender Legal     Indigenous Count
#>   <date>     <chr> <chr>  <chr>     <chr>      <dbl>
#> 1 2005-03-01 ACT   Female Remanded  ATSI           0
#> 2 2005-03-01 ACT   Female Remanded  Non-ATSI       2
#> 3 2005-03-01 ACT   Female Sentenced ATSI           0
#> 4 2005-03-01 ACT   Female Sentenced Non-ATSI       5
#> 5 2005-03-01 ACT   Male   Remanded  ATSI           7
#> 6 2005-03-01 ACT   Male   Remanded  Non-ATSI      58
```

`prison` 변수에는 csv 파일로부터 불러온 시계열 정보가 들어있다. 날짜정보가 들어있는 첫번째 열을 인덱스로, count 정보를 제외한 나머지 열들을 key로 설정하여 tsibble로 바꿔보도록 하자.


```r
prison %<>% 
  janitor::clean_names() %>% 
  mutate(quarter = yearquarter(date),
         .keep = "unused") %>% 
  as_tsibble(key = state:indigenous,
             index = quarter)
prison
```

```
#> # A tsibble: 3,072 x 6 [1Q]
#> # Key:       state, gender, legal, indigenous [64]
#>    state gender legal    indigenous count quarter
#>    <chr> <chr>  <chr>    <chr>      <dbl>   <qtr>
#>  1 ACT   Female Remanded ATSI           0 2005 Q1
#>  2 ACT   Female Remanded ATSI           1 2005 Q2
#>  3 ACT   Female Remanded ATSI           0 2005 Q3
#>  4 ACT   Female Remanded ATSI           0 2005 Q4
#>  5 ACT   Female Remanded ATSI           1 2006 Q1
#>  6 ACT   Female Remanded ATSI           1 2006 Q2
#>  7 ACT   Female Remanded ATSI           1 2006 Q3
#>  8 ACT   Female Remanded ATSI           0 2006 Q4
#>  9 ACT   Female Remanded ATSI           0 2007 Q1
#> 10 ACT   Female Remanded ATSI           1 2007 Q2
#> # ... with 3,062 more rows
```

분기별 데이터를 `yearquarter` 사용해서 설정해줬던 것 처럼 다음 표과 같이 여러 시간 간격을 설정할 수 있는 함수를 `tsibble` 패키지에서는 제공한다. 자신의 데이터에 맞는 시간 간격을 설정해서 사용하자.

<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>(\#tab:unnamed-chunk-8)tsibble 객체에 부여할 수 있는 시간 간격 종류들</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Frequency </th>
   <th style="text-align:left;"> Function </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Annual </td>
   <td style="text-align:left;"> start:end </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Quarterly </td>
   <td style="text-align:left;"> yearquarter() </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Monthly </td>
   <td style="text-align:left;"> yearmonth() </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Weekly </td>
   <td style="text-align:left;"> yearweek() </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Daily </td>
   <td style="text-align:left;"> as_date(), ymd() </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sub-daily </td>
   <td style="text-align:left;"> as_datetime(), ymd_hms() </td>
  </tr>
</tbody>
</table>



## Time plots

시계열에서 제일 기본적인 그래프이다. 시간에 따라서 우리가 보고싶은 반응 변수값을 그려준다. 앞에서 정의한 `prison` 시계열 데이터에서 특정 그룹의 시간에 따른 죄수 수의 변화를 그려보자.


```r
prison %>% 
  filter(state == "ACT", gender == "Female",
         legal =="Remanded", indigenous == "ATSI") %>% 
  autoplot(count) +
  labs(title = "시간에 따른 죄수 인구의 변화",
       y = "죄수 (명)",
       x = "시간 (분기)")
```

<img src="01-intro-to-tsibble_files/figure-html/unnamed-chunk-9-1.png" width="672" />

## 시계열 자료 구성 요소

* 추세 (Trend): 전반적인 방향성의 존재 유무
* 계절성 (Seasonal): 계절에 따른 반복유무 (고정 빈도)
* 주기성(cycle): 고정된 빈도가 아닌 증가, 감소 형태

## Seasonal plot

`vic_elec` 데이터는 호주의 빅토리아 주의 전력 수요량을 30분 단위로 기록한 데이터이다. 


```r
head(vic_elec)
```

```
#> # A tsibble: 6 x 5 [30m] <Australia/Melbourne>
#>   Time                Demand Temperature Date       Holiday
#>   <dttm>               <dbl>       <dbl> <date>     <lgl>  
#> 1 2012-01-01 00:00:00  4383.        21.4 2012-01-01 TRUE   
#> 2 2012-01-01 00:30:00  4263.        21.0 2012-01-01 TRUE   
#> 3 2012-01-01 01:00:00  4049.        20.7 2012-01-01 TRUE   
#> 4 2012-01-01 01:30:00  3878.        20.6 2012-01-01 TRUE   
#> 5 2012-01-01 02:00:00  4036.        20.4 2012-01-01 TRUE   
#> 6 2012-01-01 02:30:00  3866.        20.2 2012-01-01 TRUE
```

이 자료에서 하루를 기준으로 전력 수요가 어떠한 패턴으로 움직이는지 보고싶을 때 계절성 그래프 (Seasonal plot)가 유용함.


```r
vic_elec %>% 
  janitor::clean_names() %>% 
  filter(year(time) == 2012) %>% 
  gg_season(demand, period = "day") +
  theme(legend.position = "none") +
  labs(y="MWh", title="Electricity demand: Victoria")
```

<div class="figure">
<img src="01-intro-to-tsibble_files/figure-html/unnamed-chunk-11-1.png" alt="하루 기준 전력 수요량의 변화 패턴" width="672" />
<p class="caption">(\#fig:unnamed-chunk-11)하루 기준 전력 수요량의 변화 패턴</p>
</div>

한 주를 기준으로 그려보면 새로운 패턴을 발견할 수 있다.


```r
vic_elec %>% 
  janitor::clean_names() %>% 
  filter(year(time) == 2012) %>% 
  gg_season(demand, period = "week") +
  theme(legend.position = "none") +
  labs(y="MWh", title="Electricity demand: Victoria")
```

<div class="figure">
<img src="01-intro-to-tsibble_files/figure-html/unnamed-chunk-12-1.png" alt="한주 기준 전력 수요량의 변화 패턴" width="672" />
<p class="caption">(\#fig:unnamed-chunk-12)한주 기준 전력 수요량의 변화 패턴</p>
</div>

<div class="rmdnote">
<h3 id="혼자-학습하기">혼자 학습하기</h3>
<p>연도를 기준으로 계절성 그래프를 그려보고, 계절에 따른 전력 수요량 패턴을 해석해보자.</p>
</div>

## Scatter plots

전력 수요량 데이터의 경우 전력 수요량을 나타내는 `demand` 변수가 존재하고, 이에 대응하는 `temperature` 변수가 존재한다. 두 개의 변수를 독립적으로 시계열 데이터로 보고 시간 그래프를 그려보면 다음과 같을 것이다.


```r
vic_elec %>% 
  janitor::clean_names() %>% 
  filter(year(time) == 2012) %>% 
   autoplot(demand) +
  labs(y = "GW",
       title = "Half-hourly electricity demand: Victoria")
```

<img src="01-intro-to-tsibble_files/figure-html/unnamed-chunk-13-1.png" width="672" />



```r
vic_elec %>% 
  janitor::clean_names() %>% 
  filter(year(time) == 2012) %>% 
   autoplot(temperature) +
  labs(y = "Degrees Celsius",
       title = "Half-hourly electricity demand: Victoria")
```

<img src="01-intro-to-tsibble_files/figure-html/unnamed-chunk-14-1.png" width="672" />

하지만, 위의 두 그래프를 시간 변수를 무시한 채 산점도를 그려보면 어떻게 될까? 다음과 같이 산점도를 그릴 경우 온도와 전력 수요량 간의 새로운 패턴을 발견 할 수 있다.


```r
vic_elec %>% 
  janitor::clean_names() %>% 
  filter(year(time) == 2012) %>% 
  ggplot(aes(x = temperature, y = demand)) +
  geom_point() +
  labs(x = "Temperature (degrees Celsius)",
       y = "Electricity demand (GW)")
```

<img src="01-intro-to-tsibble_files/figure-html/unnamed-chunk-15-1.png" width="672" />

전력수요량의 경우 온도가 낮은 경우와 높은 경우 증가하는 2차 곡선 형태의 패턴을 보인다. 이것은 겨울철 난방과 여름철 냉방을 위한 전력 수요에 기인한 것으로 보인다.


```r
prison %>%
  group_by(state) %>%
  summarise(count = sum(count)) %>% 
  ggplot(aes(x = quarter, y = count)) +
  geom_line() +
  facet_grid(vars(state), scales = "free_y") +
  labs(title = "Australian prison population by states",
       y= "population")
```

<img src="01-intro-to-tsibble_files/figure-html/unnamed-chunk-16-1.png" width="672" />

dfsdfasdf
