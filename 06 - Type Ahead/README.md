# 06. Type Ahead

### :pencil: NOTE
### 1. CSS
- outline: 0;
- background: linear-gradient(top top/bottom, ...);
- transform: perspective() rotateX(); -> 직사각형을 마름모 모양으로 변경


### 2. fetch
`fetch API`는 ES6의 비동기 통신 방법이다. 

```javascript
fetch(url[, {
    // Request 설정 옵션
}]).then(function(response){
    // Code
}).catch(function(error){
    // Error
});
```

#### 2.1 Body.json()
`fetch`는 데이터가 아니라 `promise`를 리턴한다. 우리가 원하는 값은 JSON형식의 데이터이므로 요청(Request)의 응답(response) 텍스트를 JSON으로 바꾸는 `json()` 메소드를 사용한다. 

```javascript
const endpoint = url;
const cities = [];

fetch(endpoint) 
    .then(blob => blob.json())
    .then(data => cities.push(...data));
```

위의 코드에서 첫 번째 리턴값 blob(`promise`)이 어떤 데이터를 갖고 있는지 바로 알 수 없다. 이 때 raw data가 JSON타입이 되어야하므로 `Json.parse(blob)`이 아닌 `blob.json()`을 해야함을 주의한다. 

> ✔️ blob의 __proto__을 콘솔에서 확인해보면 `json()` 메소드가 존재한다.   
> 💬 [자막 참고] the raw data that it is in into JSON... *(무슨 말인지 모르겠고요..??)*   
     

그렇게 얻은 두 번째 리턴값은 우리가 원하는 JSON형식의 데이터이다. 이 때 `cities.push(data)`를 하면 cities 배열의 요소로 data 배열이 중첩되므로, `전개 구문(spread syntax)`을 이용해 data 배열의 요소를 각각의 인자로 분리시켜 cities 배열에 추가한다. 

         



### 3. 정규표현식
- `\b`: 단어의 경계(문자와 공백 사이의 문자)를 의미한다.
- `\B`: 문자와 공백 사이가 아닌 문자를 의미한다. 
- `?`: 앞의 문자와 0 또는 1회 반복되는 것을 의미한다. 
- `?=`: 전방 탐색 패턴 구문  
    - 전방 탐색 패턴은 일치하는 영역을 발견해도 그 값을 반환하지 않는(소비하지 않는) 패턴을 의미한다.

아래는 세 자리마다 콤마를 찍는 정규표현식 예제이다. (_모르겠다!!_)

```javascript
x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
```

 

---
### 👀 POINT
1. 검색바의 값이 변경되거나, 키보드 타이핑이 발생하면 `displayMatches`함수를 실행한다.
2. `displayMatches`함수는 `findMatches`함수를 호출해 입력한 값과 매칭되는 배열을 찾는다. 
3. 그리고 그 배열을 `<li></li>` 형식의 문자열로 리턴하고, 결과적으로 하나의 문자열로 합쳐 `innerHTML`을 사용해 `ul`태그의 의 자식 html로 대체한다. ㄴ