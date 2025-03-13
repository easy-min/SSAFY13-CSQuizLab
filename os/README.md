당신은 아래와 같은 형식의 HTML 퀴즈 템플릿을 사용하여, 1번부터 20번까지의 문제를 자동으로 생성하는 스크립트를 만들어야 합니다.  
각 문제는 다음과 같은 정보로 구성됩니다:
- 문제 번호 및 문제 내용
- 4개의 선택지 (각 번호와 텍스트)
- 정답 번호 (예: "정답: ②")
- 해설 (예: "해설: 운영체제는 ...")

아래는 원본 HTML 템플릿 예시입니다:

<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>컴퓨터 구조와 운영체제 week2 01</title>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;700&display=swap" rel="stylesheet">
  <style>
    /* CSS 스타일 그대로 유지 */
    body { font-family: 'Noto Sans KR', sans-serif; margin: 20px; padding: 20px; }
    .question { margin-bottom: 15px; font-weight: bold; }
    .options { margin-bottom: 10px; }
    .result { margin-top: 20px; font-weight: bold; }
    button { margin-top: 10px; padding: 12px 20px; font-size: 18px; cursor: pointer; background-color: #007BFF; color: white; border: none; border-radius: 5px; transition: background-color 0.3s; }
    button:hover { background-color: #91a743; }
    table { width: 100%; border-collapse: collapse; margin-top: 20px; box-shadow: 0px 2px 10px rgba(0,0,0,0.1); }
    th, td { border: 1px solid black; text-align: center; padding: 12px; }
    .correct { color: green; }
    .incorrect { color: red; }
    #explanation { font-family: 'Noto Sans KR', sans-serif; font-size: 16px; line-height: 1.6; color: #333; background: #f8f9fa; padding: 15px; border-radius: 5px; margin-top: 20px; box-shadow: 0px 2px 10px rgba(0,0,0,0.1); }
    @media (max-width: 600px) { body { padding: 10px; } button { width: 100%; } }
  </style>
</head>
<body>
  <h1>컴퓨터구조와 운영체제 2주차 TEST 02 (25/03/06)</h1>
  <form id="quizForm">
    <!-- 이름 입력 -->
    <label for="userName"><b>이름 입력:</b></label>
    <input type="text" id="userName" placeholder="이름을 입력하세요" required>
    <br><br>
    <!-- 문제 영역: 아래 형식으로 1번부터 20번까지 문제를 추가 -->
    <!-- 예시 -->
    <div class="question">1. [문제 1의 내용]</div>
    <div class="options">
      <label><input type="radio" name="q1" value="1"> ① [선택지1]</label><br>
      <label><input type="radio" name="q1" value="2"> ② [선택지2]</label><br>
      <label><input type="radio" name="q1" value="3"> ③ [선택지3]</label><br>
      <label><input type="radio" name="q1" value="4"> ④ [선택지4]</label><br>
    </div>
    <!-- 문제 2 ~ 20도 동일한 패턴 -->
    ...
    <button type="button" id="submitButton" onclick="checkAnswers()">제출하기</button>
  </form>
  <div id="result"></div>
  <div id="submissionTime" style="margin-top: 10px"></div>
  <div id="score"></div>
  <div id="explanation"></div>
  <button type="button" id="downloadExplanation" style="display: none;" onclick="downloadExplanation()">해설 다운로드</button>
  <script>
    function checkAnswers() {
      // 정답과 해설 데이터를 객체 형태로 정의 (예: answers = { "q1": 정답번호, "q2": 정답번호, ... })
      // 해설 데이터도 explanations = { "q1": "[해설]", "q2": "[해설]", ... } 형태로 저장.
      // 사용자가 선택한 답안과 비교해서 결과 테이블 및 점수, 해설 영역을 동적으로 생성.
      // 문제별 정답과 해설은 미리 제공한 데이터로 대체.
      // (원본 코드 참고)
    }
    function downloadExplanation() {
      // 해설 다운로드 기능 구현 (원본 코드 참고)
    }
  </script>
</body>
</html>

이제 내가 제공하는 문제 목록(문제, 선택지, 정답, 해설)을 입력받으면, 위 템플릿의 각 문제 부분을 1번부터 20번까지 자동으로 생성하는 스크립트 혹은 템플릿을 만들어주세요.

예시 입력(문제 1~20):
----------------------------------------------------------
문제 1:
운영체제는 실행할 프로그램이 원활히 동작하도록 지원하는 중요한 시스템 소프트웨어이다. 이에 대한 설명으로 적절하지 않은 것을 고르시오.
① 운영체제는 프로그램이 실행되는 동안 필요한 자원을 할당하고 해제하는 역할을 한다.
② 운영체제는 사용자가 실행한 프로그램을 직접 실행하여 처리하는 역할을 한다.
③ 운영체제는 프로세스 관리, 메모리 관리, 파일 시스템 관리 등의 기능을 수행한다.
④ 운영체제는 하드웨어와 소프트웨어 사이에서 인터페이스 역할을 수행한다.
정답: ②
해설: 운영체제는 사용자가 실행한 프로그램을 직접 실행하지 않는다. 운영체제는 프로그램이 실행될 수 있도록 환경을 제공하고, 프로세스를 관리하며, 필요한 자원을 할당하는 역할을 한다. 프로그램의 실제 실행은 CPU에서 이루어지며, 운영체제는 이를 관리한다.
----------------------------------------------------------
(문제 2~20도 같은 방식으로 제공됨)

당신은 이 입력 데이터를 받아서, 위 HTML 템플릿의 해당 부분(문제 1~20, 선택지, 정답, 해설)이 자동으로 바뀌도록 전체 HTML 코드를 생성해야 합니다.

최종 산출물은 완성된 HTML 코드로 출력되어야 하며, 문제 데이터만 변경된 버전이어야 합니다.

