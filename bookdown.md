---
title: "시계열 공략집 with R"
author: "[슬기로운통계생활](https://www.youtube.com/c/statisticsplaybook)"
date: "2022-01-15"
site: bookdown::bookdown_site
output: bookdown::gitbook
documentclass: book
bibliography: [reference.bib]
biblio-style: apalike
link-citations: yes
image: "./image/deeplearning-playbook.png"
github-repo: statisticsplaybook/r-torch-playbook
description: "복잡한 시계열 공부, 차근차근 공부해서 정복해보시죠! 본격 R 시계열 공략집"
---

# 들어가며 {.unnumbered}

Placeholder


## 공략집 사용법 {.unnumbered}
## 설치하기 {.unnumbered}
## 기본 패키지 {.unnumbered}

<!--chapter:end:index.Rmd-->


# 시계열 첫걸음, 관련 패키지의 이해 {#intro}

Placeholder


## `tsibble` 그리고 `fable` 패키지
## `fpp3` 패키지
## tsible 객체
## csv 파일로부터 `tsibble` 객체 만들기 연습
## Time plots
## 시계열 자료 구성 요소
## Seasonal plot
## Scatter plots

<!--chapter:end:01-intro-to-tsibble.Rmd-->

# 시계열 첫걸음, 관련 패키지의 이해 {#chapter3}

## `tsibble` 그리고 `fable` 패키지

시계열과 관련 된 R 패키지는 정말 많이 있다. 그 중에서 가장 유명한 패키지는 `forecast` 패키지이다. `fpp` 2판에서 메인 패키지로 활용되었던 `forecast` 패키지는 예측성능이 좋기로 유명하다. 하지만 `forecast`는 base 패키지를 기반으로 짜여져있는 패키지라서 `fpp` 3판에서 사용하게 될 패키지는 `tsibble`과 `fable` 패키지를 주로 사용하게 될 예정이다. 두 패키지 모두 `tidyverse`스러운 코딩과 잘 맞는 함수들을 지원하는 것이 특징이다.

<div class="rmdwarning">
<h3 id="주의하기">주의하기</h3>
<p><code>forecast</code> 패키지는 다른 언어를 사용하는 커뮤니티에도 알려질 정도로 잘 짜여진 패키지이다. 꼭 한번은 사용법을 알아둘 것을 추천한다. <a href="https://otexts.com/fppkr/"><code>fpp</code> 2판의 한글판</a>을 봐두는 것도 나쁘지 않는 선택이다.</p>
</div>

Chapter 3 (tested by Jongjin)

<!--chapter:end:03-Chapter3.Rmd-->


# References {-}


<div>Custom block icons made by <a href="https://www.freepik.com" title="Freepik">Freepik</a> and <a href="https://www.flaticon.com/authors/mavadee" title="mavadee">mavadee</a> from <a href="https://www.flaticon.com/" title="Flaticon">www.flaticon.com</a></div>


<!--chapter:end:references.Rmd-->

