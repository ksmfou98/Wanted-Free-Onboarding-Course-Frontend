# 원티드 프리 온보딩 코스 프론트 엔드

<br />

## [👉 배포 페이지 바로가기 👈](https://ecstatic-feynman-9b7ef9.netlify.app/)

<br />
<br />

## 필수 구현 사항

1. 상단 GNB(Global Nabigation Bar)
2. 슬라이드(또는 캐러샐이라고 불림) 영역
3. 반응형(Responsive Web) 구현
4. Github Repository 주소와 배포 링크 제출

<br />

## 구현 요구사항 분석

### 슬라이드(캐러샐) 기능

- 슬라이드 되어야할 이미지가 항상 페이지의 정중앙에 유지되도록 설정
- 슬라이드 이미지가 3~4초 마다 자동으로 슬라이드 되어야 함
- 이미지에 마우스 hover시에 이미지 슬라이드 멈춤.
- 이미지가 무한 루프로 연결되어 있음
- 브라우저 배율을 500%로 최대한 줄여도 이미지가 끊이지 않고 계속 보여야함
- 페이지를 새로 고침 할 때마다 나오는 이미지가 다름
  - 랜더링 할 때 이미지데이터 배열을 랜덤으로 수정
- 이미지를 반응형으로 수정했더니 화면을 줄일 때마다 이미지가 애니메이션이 생김
  - resize 될 동안에는 애니메이션과 화면 이동을 막아야함
- 슬라이드 이동 버튼을 빠르게 클릭하면 애니메이션이 엄청 빠르게 이동되는 현상이 생김
  - 원티드 페이지에서는 버튼 클릭시 0.6초 정도 버튼을 비활성화 시키는거 같음
  - 원티드 페이지 처럼 슬라이드 버튼 이동 클릭시 버튼을 0.6초 동안 비활성화 시키고 이후에 활성화 시키는 형태로 해야함

### 반응형 구현

- 이미지가 어느 픽셀에서든 항상 가운데 유지해야함

<br />

## 이슈 사항 && 해결 과정

### 슬라이드 되어야할 이미지가 항상 페이지의 정중앙에 유지되도록 설정

처음에는 이미지 리스트 박스를 `position: absolute`를 이용해서 가로로 정렬을 했었습니다. 그렇게 해서 이미지가 잘 나타나고 있어서 "이미지가 잘 나왔네!" 라고 생각했습니다. 그런데 브라우저 아래에 보니 엄청난 스크롤이 생기는걸 보고 스크롤을 넘겨보니 이미지 데이터가 다 노출되는 현상이 일어났습니다. 그렇게 상위 엘리먼트의 `overflow: hidden` 값을 주게 되니 이미지 박스 전부가 `hidden` 처리가 되어서 보이지 않았습니다.. `position: absolute`에 문제가 있기 때문에 `position`을 지우고 상위 엘리먼트에 `width: 100%`를 준 뒤 `width: 이미지 크기; margin: 0 auto`로 이미지 크기 만큼 width 값을 설정 후 가운데 정렬로 해결 하였습니다.

하지만 이렇게 됐을 때 **두번째 이슈**가 발생했습니다. 이미지의 첫번째 요소부터 가운데 정렬을 해버리니
아래 이미지의 빨간 엑스표시 부분이 빈 공백으로 나오게 되었습니다.
![고양이](https://i.ibb.co/W3T5W6M/2022-01-12-11-53-08.png)

이미지 데이터 리스트를 무한으로 나오게 설정하지 않았기 때문에 발생한 이슈였기 때문에 무한으로 연결하는 작업을 해주었습니다. 이미지를 무한으로 연결하기 위해서는 [...원본 이미지 리스트 복사본, ...원본 이미지 리스트, ...원본 이미지 리스트 복사본] 이렇게 배열을 생성해 놔야 자연스러운 연결이 됩니다. 그렇게 연결을 한 뒤 이미지의 최초 위치를 중앙으로 잡아주게 되면 이미지가 양 옆으로도 잘 나오게 됩니다. 이미지의 최초 위치를 중앙으로 잡은 로직은 아래에 설명하겠습니다.

<br >

### 무한 이미지 슬라이드(캐러샐) 기능

처음에 시작할 땐 무한 이미지 슬라이드를 너무 쉽게 생각했었습니다. 단순히 이미지가 슬라이드 1칸 될 때마다 첫번째 요소를 지우고 맨 뒤에다가 연결해주고 이런식으로 하면 될 줄 알았는데, 막상 해보니 첫번째 요소를 지우게 되면 인덱스가 하나씩 밀리기 때문에 이미지가 슬라이드 하지도 않았는데 앞으로 땡겨지는 현상이 일어났습니다. 그렇게 고민을 하다가 예전에 대학교 동기들과 마켓컬리 클론코딩을 했었던 기억이 났습니다. 그 때도 원티드 메인과 비슷한 무한 이미지 슬라이드가 있었는데, 그 당시에 했던 로직이 원본이미지 배열을 2개 복사해서 [...원본 이미지 리스트 복사본, ...원본 이미지 리스트, ...원본 이미지 리스트 복사본] 이렇게 만든 뒤에 가운데 원본 이미지 리스트 중앙에서 시작해서 오른쪽(왼쪽)으로 이동하다가 시작했던 이미지와 같은 이미지가 나오게 되면 그 다음으로 슬라이드 되는 것이 아니라 사용자 몰래 다시 시작했던 이미지 리스트로 바꿔치기 하는 로직입니다. 이렇게 하게 되면 이미지가 계속해서 무한으로 나오는거 처럼 보이게 됩니다.

여기서 또 해결해야할 과제가 생겼습니다. 그렇다면 이 [...원본 이미지 리스트 복사본, ...원본 이미지 리스트, ...원본 이미지 리스트 복사본] 배열에서 어떻게 가운데 위치부터 시작하게 할까? 였습니다. 위치를 바꿔주는 css인 `transform` 으로 랜더링 되자마자 가운데 위치로 이동시켜주는 방식으로 처음 구현을 하였지만 그렇게 되면 첫 랜더링 되자마자 시작 위치에서부터 가운데 위치까지 이동하는 모션이 보였습니다. 그래서 `transform` 처럼 움직이는게 아니라 위치를 잡아주는 `absolute`를 이용하기로 했습니다. 이미지 리스트 박스 아래에 `div` 태그를 하나 더 만든 뒤에 `position: absolute`를 주어서 시작 위치값을 `left: 슬라이드 될 아이템 width 값 곱하기 배열 가운데 인덱스` 이렇게 해서 시작 위치를 가운데로 잡아 주었습니다.

이제 시작 위치도 이미지 배열 중앙으로 잡아주었고 캐러샐 기능만 구현하면 되었습니다. 캐러샐 기능은 쉬워서 쉽게 구현하였는데, 구현하니 또 이슈가 발생했습니다. 이미지를 무한으로 연결시켜주는 로직 부분에서 사용자 몰래 이미지를 바꿔치기 한다고 했었는데, 이미지 슬라이드를 하다보면 바꿔치기 할 때 바꿔치는 장소로 슬라이드가 되돌아가는 모션이 보이게 됐습니다. 바꿔치는 순간에는 슬라이드 모션이 보이지 않게 바꿔야 하기 때문에 `const [isAnimation, setIsAnimation] = useState(true)` 라는 상태를 만들어서 이미지 위치를 바꿔치는 순간에만 `isAnimation`을 false로 만들어서 애니메이션 효과를 끄는것으로 해결되었습니다.

<br />

### 버튼을 클릭하지 않아도 자동으로 이미지 슬라이드 되게 설정

원티드 페이지를 보면 양 옆 버튼을 클릭하지 않아도 이미지가 3~4초 마다 자동으로 슬라이드가 되고 있습니다. 자동 슬라이드가 되는 것은 구현을 바로 하였지만, 여기서도 문제가 발생했습니다. 원티드 페이지를 보면 이미지에 마우스를 올려논 상태에선 자동으로 슬라이드가 되지 않고 있는데, 제가 그 부분은 처리를 안해줬어서 마우스를 이미지에 올려놓아도 계속해서 슬라이드가 되고 있었습니다. 그래서 `const [isFlowing, setIsFlowing] = useState(true)` 라는 상태를 만들어서 `isFlowing`이 `true`일때만 자동 슬라이드가 되도록 설정하고 이미지에 마우스를 올렸을 땐 `isFlowing`을 `false`로 만들어주었습니다.

<br />

### 반응형으로 화면이 줄어들 때 이미지 width 값이 동적으로 바뀌어야함

처음에는 반응형으로 구현할때 이미지가 특정 픽셀마다 줄어드는 것으로 생각했습니다. 예를 들어서 화면이 1200px 일 경우에는 이미지가 1060px 화면이 1060px 일땐 이미지가 760px 화면이 760px 일떈 이미지가 500px 이런식으러 되는 것으로 생각했습니다. 하지만 원티드 페이지를 잘 분석해보니 화면이 1200px 아래로 줄어들게 되면 이미지가 화면에 맞게 동적으로 계속 바뀌게 되어있어서 조금 당황했습니다. window.innerWidth 값을 이용하면 브라우저의 width 값을 받아올 수 있었는데, 원티드 페이지를 잘 분석해보니 현재 브라우저의 width 값 - 80px로 이미지의 width 값이 결정되고 있는걸 확인했습니다. 그래서 브라우저 width값이 1200px 아래로 내려가게 되면 동적으로 이미지 width 값을 설정하도록 구현하였습니다.

반응형으로 이미지 크기를 줄이는데 까지는 잘 성공했습니다. 하지만 또 이슈가 발생했는데요, 화면을 줄이는 와중에 이미지가 슬라이드가 되면서 슬라이드로 이동하는 거리와 이미지 크기가 달라지게 되어서 UI가 이상하게 나오는 현상이 발생했습니다. 원티드 페이지를 살펴보니 화면이 줄어들거나 늘어날 때는 이미지가 슬라이드가 되지 않고 있었습니다 그래서 저도 다음 코드와 같이 화면이 resize 될 동안에는 이미지 슬라이드가 안되도록 구현하였습니다.

```jsx
useEffect(() => {
  const handleResize = () => {
    // resize 되는 동안에는 애니매이션, 이동을 막아줌
    setIsAnimaion(false);
    setIsFlowing(false);
    // resize 가 끝난 0.5초 이후에 다시 활성화
    setTimeout(() => {
      setIsFlowing(true);
      setIsAnimaion(true);
    }, 500);
  };
  window.addEventListener("resize", handleResize);
  return () => {
    window.removeEventListener("resize", handleResize);
  };
}, []);
```

<br />

### 슬라이드 이동 버튼을 빠르게 클릭하면 애니메이션이 엄청 빠르게 이동되는 현상이 생김

테스트를 하던 와중 슬라이드 이동 버튼을 연속해서 빠르게 클릭하면 클릭 한 횟수만큼 애니매이션이 빠르게 움직이는 현상이 발생했습니다. 원티드 페이지는 이걸 어떻게 해결했는지 확인을 해보니 버튼을 클릭하면 0.6초 정도 버튼을 비활성화 시킨 후에 다시 활성화 시키는 방식으로 구현되어 있었습니다. 그래서 저도 아래 코드와 같이 0.6초 정도 비활성화 시키고 다시 활성화 시키는 방식으로 구현하였습니다.

```jsx
const onDisabledButton = () => {
  setIsDisabled(true);
  timer.current = setTimeout(() => {
    setIsDisabled(false);
  }, 600);
};

useEffect(() => {
  return () => {
    if (timer.current) {
      clearTimeout(timer.current);
    }
  };
}, [timer]);
```

<br />

## 해결하지 못한 과제

- 개발자 도구에서 메인 로고의 폰트를 도저히 못찾겠어서 최대한 비슷한 roboto 폰트를 이용했습니다 ..
