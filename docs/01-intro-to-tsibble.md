# 시계열 첫걸음, 관련 패키지의 이해 {#intro}

## `tsibble` 그리고 `fable` 패키지

시계열과 관련 된 R 패키지는 정말 많이 있다. 그 중에서 가장 유명한 패키지는 `forecast` 패키지이다. `fpp` 2판에서 메인 패키지로 활용되었던 `forecast` 패키지는 예측성능이 좋기로 유명하다. 하지만 `forecast`는 base 패키지를 기반으로 짜여져있는 패키지라서 `fpp` 3판에서 사용하게 될 패키지는 `tsibble`과 `fable` 패키지를 주로 사용하게 될 예정이다. 두 패키지 모두 `tidyverse`스러운 코딩과 잘 맞는 함수들을 지원하는 것이 특징이다.

<div class="rmdwarning">
<h3 id="주의하기">주의하기</h3>
<p><code>forecast</code> 패키지는 다른 언어를 사용하는 커뮤니티에도 알려질 정도로 잘 짜여진 패키지이다. 꼭 한번은 사용법을 알아둘 것을 추천한다. <a href="https://otexts.com/fppkr/"><code>fpp</code> 2판의 한글판</a>을 봐두는 것도 나쁘지 않는 선택이다.</p>
</div>

## `fpp3` 패키지

`fpp3` 패키지는 Hyndman 교수와 Athanasopoulos 교수가 지은 Forecasting: principles and practice 3판에 딸림 R 패키지라고 생각하면 된다. `fpp3` 패키지를 로드시키면 앞에서 말했던 `tsibble`과 `fable` 패키지가 같이 로드되는 것을 알 수 있다.


```r
library(fpp3)
```

