# 요구사항

- 메뉴 생성 후 배부관리 바로가기 버튼 생성
    - 배부목록으로 바로 가게 함
- 수신문서 서류 적합성 검토
    - 적합성 버튼 생성
    - '적합성체크'버튼은 '접수대기'상태의 문서에서만 노출될 수 있도록 처리
- 적합성 팝업 생성 후 저장 삭제
- 적합성 체크 팝업 체크 컬럼은 검토여부 컬럼으로 대신해서 필요없음
- 삭제 버튼은 해당 교신문서의 적합성 체크 항목에 관한 사항을 교신문서 적합성체크 테이블에 있는 것을 다 삭제하는 기능임.

```jsx
corrPopup.jsp 에서 작업
'적합성체크' 버튼 추가 - 클릭 시, 아래 3페이지 팝업 호출
(적합성 체크 완료시 이상 항목이 없을 경우, 배부 버튼 / 이상 항목 있을 경우, 반려 버튼 노출)
```

**나중에 진행해야 하는 사항**

- 

# 해야 할 일

- 

**진행과정**

```jsx
< 상위 프레임 변수 가지고 오기 >
1. menu.select("tree_89"); 
-> 실패
- 셀렉트만 되지 아래 토글이 나오지 않는다.

2. 
$('button[data-dhx-id=tree_98]').click();
$('button[data-dhx-id=tree_3513]').click();

parent.$('button[data-dhx-id=tree_98]').click();
parent.$('button[data-dhx-id=tree_3506]').click();
일단 클릭하는 것에는 성공 하지만 두개 동시에 실행하면 되지 않는다. 

parent.$('button[data-dhx-id=tree_98]').click(function(){
    $('button[data-dhx-id=tree_3506]').click();
});
-> 실패
실행 되지 않았다. 

var button = parent.$('button[data-dhx-id=tree_98]')[0];
//button.dispatchEvent(new Event('click')); // 가상의 click 이벤트 발생
parent.$('button[data-dhx-id=tree_98]').click();

// 버튼에 이벤트 핸들러 등록
button.addEventListener('click', function() {
  // 버튼 클릭 후 실행될 로직 작성
  console.log("버튼 클릭 후 다른 함수 실행");
  parent.$('button[data-dhx-id=tree_3506]').click();
});
-> 실패
콘솔은 잘 찍히는데 아직 토글이 나오지 않아서 아래 클릭이 안되었다. 

3. 상위 탭바를 가져와서 넣어준다.
-> 실패
상단의 탭바를 가져올 수 없었다.  
여기서 중요한 점은 함수는 잘 호출하는데 왜 변수는 못 가져오냐는 것이다.
이것은 변수의 범위와 관련된 것이다. 지금 let 으로 선언된 tabbar는 하위 프레임에서 사용할 수가 없다.
var로 선언하거나 const 로  선언하여야 전역 번수로 사용이 가능하다. 

parent.tabbar.addTab({
	id: "tree_3513"
	,tab: "수신관리대장"
	,menuSeq: "3513"
	,menuUrl: "/dom/document/receiveList"
});

parent.tabbar.setActive("tree_3513"); // 탭 활성화

https://www.freecodecamp.org/korean/news/var-let-constyi-caijeomeun/
https://yokina.tistory.com/43

==>> 결국 이사님이 공통으로 생성해주시기로 함.
```
