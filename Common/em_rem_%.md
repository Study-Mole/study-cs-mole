# EM, REM, 퍼센트 차이

EM, REM, 퍼센트는 CSS에서 자주 사용되는 상대 단위로, 반응형 웹을 구현하는데 중요한 역할을 한다.

### 상대 단위란?

고정되지 않고 특정 기준에 따라서 유동적으로 바뀔 수 있는 길이를 나타내는 단위이다.<br/> `EM`, `REM`, `%`를 포함해 `vw`, `vh` 등이 있다.

## EM vs REM

EM과 REM은 CSS의 `font-size` 속성값에 비례하여 결정되는 상대 단위이다.

예를들어 `font-size: 16px`인 경우 브라우저는 다음과 같이 계산된다.

- `0.5em = 16px * 0.5 = 8px`
- `1em = 16px * 1 = 16px`
- `2em = 16px * 2 = 32px`

<br/>

**EM, REM의 차이점은 어느 요소의 `font-size`를 기준으로 둘 것인지에 있다.**
<br/> 
EM의 경우 해당 단위가 사용되고 있는 요소를, REM은 root,즉 최상위 요소의 `font-size`를 기준으로 한다.


- `EM` : 부모 요소의 폰트 크기를 기준으로 한다.
- `REM` : 루트 요소인 `html`의 폰트 크기를 기준으로 한다.

예를 들어, 다음과 같이 `font-size: 20px`으로 설정된 html 요소가 있다고 하자.

```css
html {
  font-size: 20px;
}
```

그리고 아래와 같이 `div`가 스타일링 된다면 이 요소의 너비는 `200px`이 된다. REM은 루트 요소의 폰트 크기를 기준으로 하기 때문이다.

```css
div {
  font-size: 16px;
  width: 10rem; /* 200px */
}
```

### 둘 중 어떤걸 사용할까?

많은 가이드들이 rem을 우선 사용하도록 권고하고 있다.
<br/>

EM은 컴포넌트의 상대적인 크기 조정이 필요할 떄 유용하게 사용되지만, 부모 요소의 크기에 영향을 받으므로 요소들이 중첩될수록 EM으로 스타일링하는 것이 더 복잡해질 수 있기 때문이다.

## 퍼센트

퍼센트는 부모 요소의 크기에 대한 상대적인 비율을 나타낸다. <br/>(= 부모 요소 크기에 비례하여 요소의 크기를 정의한다.)

특히, 너비와 높이를 지정할 때 유용한데 예를들어 부모 요소의 너비가 100px일 때, 자식 요소의 너비를 50%로 설정하면 자식 요소의 너비는 50px이 된다.

그러나, 퍼센트 단위만으로는 모든 디자인 요구사항을 충족시키기 어렵기 때문에, REM, EM과 함께 사용되는 경우가 많다.

[참고]

- [CSS 상대 단위 - em과 rem](https://www.daleseo.com/css-em-rem/)
- [CSS 단위의 이해: REM, EM, 퍼센트 차이점](https://f-lab.kr/insight/understanding-css-units?gad_source=1&gclid=CjwKCAiA74G9BhAEEiwA8kNfpVjocUesDZdSztjbxc0CxqqmYnwuIP5fhCotGC51IyuNxitZK_qemBoCbnYQAvD_BwE)
