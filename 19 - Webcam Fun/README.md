# 19. Webcam Fun


### :pencil: NOTE
### 1. clearfix
HTML 문서 구조에서 부모 요소가 자식 요소를 감싸고 있을 때, 자식 요소에게 `float`속성을 적용하면 부모 요소가 자식 요소를 더이상 감싸지 않게 되어 높이값을 파악하지 못하게 된다. 이 때 부모 요소가 다시 자식 요소를 감쌀 수 있게 `float`을 초기화해주는 것이 필요한데 그걸 `clearfix`라고 한다.   
`clearfix`를 하는 방법엔 여러가지가 있지만 여기서는 가상요소 `::after`를 사용한다. 

> CSS3부터는 구분을 위해 가상 클래스는 싱글콜론, 가상 요소는 더블콜론을 사용한다. (!?)


photobooth의 자식인 photo가 `float:left` 스타일을 적용하기 때문에 아래와 같이 부모 요소인 phtobooth에 `clearfix`를 해준다. photobooth 뒤에 빈 block요소를 만들고 `float`을 초기화하겠다는 의미이다. 

```css
.photobooth:after {
	content: '';
	display: block;
	clear: both;
}
```



### 2. getVideo() 관련
`navigator.mediaDevices.getUserMedia()`로 요청한 미디어 스트림을 가져와서 `srcObject` 프로퍼티로 HTML 미디어 요소에 셋팅(?)한다. 

```javascript
// promise를 리턴
navigator.mediaDevices.getUserMedia({ video: true, audio: false })
	.then(localMediaStream => {
		video.srcObject = localMediaStream;
		video.play(); // 캠이 라이브로 실행된다!
	})
	.catch(err => {
		console.error(`Oh no!!!`, arr);
	});
```



### 3. canvas 관련
...



### 4. 자바스크립트로 요소 생성 후 DOM에 삽입
- `createElement(tagName)`: 지정한 태그의 HTML 요소를 만들어 반환한다. 
- `insertBefore(newNode, refNode)` : 두 번째 인자로 전달한 참조 노드 앞에 자식 노드를 삽입한다. 만약 참조 노드가 `null`이라면, 노드는 부모 노드의 맨 끝에 추가된다. 

```javascript
const link = document.createElement('a');
link.innerHTML = `<img src="${data}" alt="Handsome Man" />`;

// strip의 첫 번째 자식요소로 link를 삽입한다. 
strip.insertBefore(link, strip.firstChild);
```


### 5. a태그의 download속성
HTML5에서 추가된 속성으로 서버에 파일을 저장하지 않아도 손쉽게 다운로드를 할 수 있게 해준다. 다만 역시 IE에서는 지원이 되지 않는다. 
- `download`: 원본 파일명으로 저장 된다.
- `download="file name"`: 설정한 파일명으로 저장 된다. 

```html 
<!-- a태그의 경로와 이미지 경로를 동일하게 지정해준다 -->
<a href="image path" download>
	<img src="image path">
</a>
```



### 6. 로컬 서버
이번 예제를 실행하기 위해선 로컬 서버가 필요한데, 제공된 package.json 파일의 `browser-sync`라는 옵션이 로컬 서버를 대신한다. (아마도?) 아래 json파일의 코드는 `browser-sync`가 css, html, js파일을 서버에서 실행시킨다는 뜻이 아닐까. 
```json
"scripts": {
    "start": "browser-sync start --server --files \"*.css, *.html, *.js\""
},
"devDependencies": {
	"browser-sync": "^2.12.5 <2.23.2"
}
```
#### `browser-sync` 실행(?)하기 
1. node.js를 설치하고, 현재 경로에서 `npm install` 명령어로 npm을 설치한다. 그러면 node_modules라는 폴더가 생성된다. 
2. `npm start`로 npm을 실행한다.
3. `localhost`로 시작하는 로컬 주소가 제공되고, 그 주소로 예제가 열린다. 

> #### 127.0.0.1 / localhost 의 차이
> 
> 둘 다 내 PC에서 서버를 실행할 경우 내 IP주소와 동일한 곳을 가리키고, 서버를 실행한 그 기기에서만 접속이 가능하다는 공통점이 있지만 차이 또한 존재한다. 
>> **127.0.0.1**: 예약된 IP주소로 OS에서 가상으로 지원하며, 디바이스를 통과하지 않고 소프트웨어적으로 처리된다.    
>> **localhost**: 자신의 IP로 직접 접근하며, 디바이스 영역을 통과해 처리되므로 더 빠르고 시스템 자원을 덜 쓴다. localhost라는 호스트명은 바꿀 수도 있다. 
(_블로그마다 말이 달라..._)
>
> #### 192.168.0.x(사설 IP) / 공인 IP의 차이
>> **사설IP**: 같은 대역의 사설 IP를 할당받은 모든 기기에서 접속이 가능하다. 즉, 같은 와이파이에 연결되어있지 않으면 접속이 불가능하다.   
>> **공인IP**: 어디에서나 접속이 가능하다. 하지만 공인IP는 개수의 제한이 있으므로 모두 사용할 수 있는 것은 아니다. 


---
이번에 회사에서 프로그램이 셋팅되어 있는 노트북(본체)과 탭을 연동시키는 작업을 했었다. 🌟🌠   
그 때 노트북과 탭을 같은 와이파이에 연결하고, 노트북에서 와이파이에 물린 IP를 확인 후 탭의 브라우저에서 IP주소를 쳐 같은 프로그램에 접속할 수 있었다.    
당시에는 무슨 말인지도 모르고 그런가보다 했는데, 왜 와이파이가 바뀔 때마다 ipconfig로 IP를 다시 확인해 재접속을 했고, 병원에서는 왜 자꾸만 공인 IP라는 단어가 들려왔던 건지 이제서야 이해가 된다! 


---
이번 강의는 조금 듣다가... 우선 넘어가는 걸로.. 😂💨