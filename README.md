# INHA_NET_ZERO_HACKATHON
AWS에서 주관하는 INHA SW NET-Zero 해커톤에서 사용한 서버 코드입니다. Spring Boot를 활용하여 "탄소중립실현" 모바일 애플리케이션의 프로토타입을 구현하였습니다.
## 👨‍🏫 프로젝트 소개
"저탄소 에너지 사용 점수를 통한 녹색소비 솔루션"을 주제로 한 안드로이드 애플리케이션입니다. 소비자들의 저탄소 소비를 독려하기 위해 에너지소비효율을 비교/분석하는 플랫폼을 제작해보았습니다. 

## ⏲️ 개발 기간 
- 2023.07.13(목) ~ 2023.07.15(토)
- 탄소중립의 이해, DevOps 특강
- 아이디어 노트 작성
- 코딩테스트
- 아이디어 발표
- 해커톤
- 발표평가
  
## 🧑‍🤝‍🧑 개발자 소개 
- **변현섭** : 팀장, Server 개발자
- **강민교** : 데이터 분석
- **곽수연** : 데이터 분석
- **양원철** : Android 개발자
- **김가영** : UX UI Designer
  
![개발자 소개](https://github.com/gmlstjq123/INHA_NET_ZERO_HACKATHON/blob/hello_there-12/%EA%B0%9C%EB%B0%9C%EC%9E%90%20%EC%86%8C%EA%B0%9C.png)

## 💻 개발환경
- **Version** : Java 17
- **IDE** : IntelliJ
- **Framework** : SpringBoot 2.7.11
- **ORM** : JPA

## ⚙️ 기술 스택
- **Server** : AWS EC2
- **DataBase** : AWS RDS, Datagrip, JPQL, ERD AqueryTool
- **WS/WAS** : Nginx, Tomcat
- **OCR** : AWS Textract, AWS S3
- **아이디어 회의** : Slack, Zoom, Notion

## 📝 프로젝트 아키텍쳐
![프로젝트 아키텍쳐](https://github.com/gmlstjq123/INHA_NET_ZERO_HACKATHON/blob/hello_there-12/%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%20%EC%95%84%ED%82%A4%ED%85%8D%EC%B3%90.png)

## 📌 주요 기능
- 가전기기 등록
  - 공공데이터포털에서 제공하는 가전기기의 업체명, 모델명, 소비전력량, 에너지 효율등급, 시간당 이산화탄소 배출량를 csv 파일로 가져온다.
  - 가격필드를 추가한 후 데이터 크롤링을 이용해 가격 정보를 csv파일에 입력한다.
  - csv파일을 Json으로 변환하여 DB에 입력한다.
  - 전기냉장고, 전기냉방기, 김치냉장고, 전기세탁기 품목에 한정하여 진행하였다.
- 가전기기 검색
   - 텍스트로 모델명을 검색하면 텍스트와 일치하는 모델에 대한 정보가 사용자에게 반환된다. Like 연산자를 활용하여 모델명이 부분일치하는 경우도 반환할 수 있게 하였다.
   - 에너지소비효율 등급을 사진으로 찍으면 좌표값을 이용하여 모델명을 추출하여 해당 모델에 대한 정보를 사용자에게 반환한다.
- 품목 별 랭킹 조회
    - 기존의 에너지효율 등급과 탄소배출량 등을 고려하여 매긴 자체점수를 이용해 가전제품의 랭킹을 사용자에게 보여준다.
    - 기존의 에너지효율 등급만으로는 동일한 등급 간의 비교가 어려웠고, 얼마만큼의 이득이 생기는지가 수치적으로 드러나지 않는다는 단점이 존재했다. 이러한 점을 개선하기 위해 자체 점수 제도를 도입하였다.
    - 또한 등급간의 비율도 가전기기의 종류마다 제각각이었기 때문에 인공지능을 활용해 공평하게 등급을 구분하였다.
      
## 프론트엔드 기술 구현 과정
**1. DOM 조작과 이벤트 처리**
  - `DOMContentLoaded`: HTML 문서가 완전히 로드된 후 코드를 실행하며 페이지 초기화 작업을 수행
  - 다양한 이벤트(`click`, `mousemove`, `mouseup`, `keydown`, `wheel` 등)를 감지하여 페이지 내에서 발생하는 상호작용을 처리
  - 버튼 클릭 시 오버레이 및 글로우 효과를 토글하거나 해제하며, F8, F9 키를 통해 웹캠 캔버스와 가상 마우스 기능을 토글

**2. WebGL을 통한 그래픽 처리**
  - WebGL을 사용해 사용자의 웹캠 스트림을 `<canvas>`에 렌더링하고, 텍스처로 웹캠 영상을 처리하여 실시간 그래픽을 구현
  - 정점 셰이더와 프래그먼트 셰이더를 작성해 웹캠 프레임의 텍스처 좌표를 계산하고 색상 처리 작업 수행
  - 크로마 키 기능을 통해 프래그먼트 셰이더에서 특정 색상을 감지하고 해당 부분을 투명하게 처리하는 방식 사용

**3. 미디어 처리 (Navigator API 및 MediaPipe)**
  - `getUserMedia API`를 사용해 웹캠에서 실시간 비디오 스트림을 가져와 비디오 요소와 WebGL 캔버스에 연결
  - **MediaPipe Selfie Segmentation**을 사용해 실시간으로 사람과 배경을 구분하고 배경을 투명하게 처리
  - 웹캠 프레임에서 사람을 인식하고 배경을 투명하게 만들어 `<canvas>`에 렌더링

**4. 로컬 스토리지를 활용한 상태 동기화**
  - **localStorage**를 사용해 페이지 간 상태 동기화 및 저장
  - 마우스 위치, 오버레이 및 글로우 상태를 저장하고 이를 다른 페이지나 iframe 간에 동기화
  - 웹캠 재생 시간, 위치, 크기 등의 상태를 저장해 페이지를 새로고침하거나 다른 페이지에서도 상태를 유지

**5. BroadcastChannel을 통한 페이지 간 통신**
  - **BroadcastChannel API**를 사용해 여러 페이지 또는 iframe 간에 메시지를 전송하고 리다이렉션 동기화
  - 페이지 간 특정 버튼 클릭 시 다른 페이지에도 동일한 동작을 수행하도록 동기화
  - 여러 페이지에서 공통된 리다이렉션 상태를 유지하여 일관성 있는 사용자 경험 제공

**6. 동기화된 마우스 움직임과 가상 커서**
  - 가상의 마우스를 표시하고 페이지 간 마우스 움직임을 동기화
  - 마우스가 움직일 때, 로컬 스토리지와 페이지 간 메시지를 통해 다른 페이지에서도 동일한 위치에 가상 마우스를 표시
  - F9 키를 통해 가상 마우스 기능을 켜거나 끌 수 있음

**7. 웹캠 동기화 및 컨트롤**
  - 웹캠 스트림 재생 시간, 위치, 크기 상태를 동기화하여 페이지 간 동일한 웹캠 상태 유지
  - 웹캠 스트림을 드래그 앤 드롭하여 위치를 변경하고, 확대/축소 기능을 지원
  - 페이지 간 웹캠 재생 상태를 동기화하여 일시정지나 재생이 다른 페이지에도 적용

**8. 드래그 앤 드롭과 확대/축소**
  - 웹캠 스트림을 클릭하여 드래그로 위치를 변경할 수 있으며, 마우스 휠을 사용해 비디오 크기를 조절
  - 드래그 및 확대/축소 시 로컬 스토리지를 통해 상태를 저장하고 다른 페이지에서도 동일한 설정을 유지

## ✒️ 백엔드 기술 구현 과정
- API 상세설명 : <https://velog.io/@gmlstjq123/INHA-SW-NET-Zero-%EA%B3%B5%EB%8F%99%ED%95%B4%EC%BB%A4%ED%86%A4-Server-%EC%BD%94%EB%93%9C#3-%ED%92%88%EB%AA%A9%EB%B3%84-%EB%9E%AD%ED%82%B9-%EC%A1%B0%ED%9A%8C>


- API 명세서 : <https://makeus-challenge.notion.site/API-ecafb2a8fb8c427c9e78abf6120d674b>
