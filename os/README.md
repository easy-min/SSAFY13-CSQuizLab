당신은 HTML 기반 퀴즈 페이지를 자동 생성하는 스크립트를 작성해야 합니다.
최종 산출물은 다음 두 가지 기능을 포함한 완성된 HTML 코드여야 합니다.

1. **텍스트 기반 문제 생성 기능**  
   - 사용자가 1번부터 20번까지의 문제 데이터(문제 내용, 선택지 4개, 정답, 해설)를 아래와 같은 형식으로 제공하면,
   - 기존 HTML 템플릿 내의 해당 부분(문제, 선택지, 정답, 해설)을 자동으로 교체하여 최종 HTML 코드를 생성합니다.
   - HTML 템플릿에는 "이름 입력" 필드, 문제 영역(.question, .options), 제출 후 채점 및 해설(해설 다운로드 버튼 포함) 기능이 포함되어야 합니다.
   
   **입력 데이터 예시:**
   ----------------------------------------------------------
   문제 1:
   운영체제는 실행할 프로그램이 원활히 동작하도록 지원하는 중요한 시스템 소프트웨어이다. 이에 대한 설명으로 적절하지 않은 것을 고르시오.
   ① 운영체제는 프로그램이 실행되는 동안 필요한 자원을 할당하고 해제하는 역할을 한다.
   ② 운영체제는 사용자가 실행한 프로그램을 직접 실행하여 처리하는 역할을 한다.
   ③ 운영체제는 프로세스 관리, 메모리 관리, 파일 시스템 관리 등의 기능을 수행한다.
   ④ 운영체제는 하드웨어와 소프트웨어 사이에서 인터페이스 역할을 수행한다.
   정답: ②
   해설: 운영체제는 사용자가 실행한 프로그램을 직접 실행하지 않는다. 운영체제는 프로그램이 실행될 수 있도록 환경을 제공하고, 프로세스를 관리하며, 필요한 자원을 할당하는 역할을 한다.
   ----------------------------------------------------------
   (문제 2~20도 동일한 형식으로 제공됨)

2. **이미지 기반 변형 문제 생성 기능**  
   - 사용자가 이미지 파일을 업로드하면, 해당 이미지에서 OCR(문자인식)을 통해 문제, 선택지, 정답, 해설 데이터를 추출합니다.
   - 추출된 데이터를 기반으로 원본 문제와 유사하되, 예를 들어 단어를 약간 변경(예: "실행" → "구동")하거나 선택지 순서를 약간 섞고, 해설에 추가 설명을 덧붙이는 등 변형 문제를 생성합니다.
   - 생성된 변형 문제 역시 위 HTML 템플릿의 문제 영역에 맞춰 구성되며, 제출 후 채점, 해설 출력, 해설 다운로드 기능이 동일하게 동작해야 합니다.

**HTML 템플릿 기본 구조 (예시):**
--------------------------------------------------------------------------------
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>컴퓨터 구조와 운영체제 Quiz</title>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;700&display=swap" rel="stylesheet">
  <style>
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
    <!-- 문제 영역: 1번부터 20번까지 반복 -->
    <div class="question">1. [문제 1 내용]</div>
    <div class="options">
      <label><input type="radio" name="q1" value="1"> ① [선택지1]</label><br>
      <label><input type="radio" name="q1" value="2"> ② [선택지2]</label><br>
      <label><input type="radio" name="q1" value="3"> ③ [선택지3]</label><br>
      <label><input type="radio" name="q1" value="4"> ④ [선택지4]</label><br>
    </div>
    <!-- 문제 2~20 동일 패턴 -->
    ...
    <button type="button" id="submitButton" onclick="checkAnswers()">제출하기</button>
  </form>
  <div id="result"></div>
  <div id="submissionTime" style="margin-top: 10px"></div>
  <div id="score"></div>
  <div id="explanation"></div>
  <button type="button" id="downloadExplanation" style="display: none;" onclick="downloadExplanation()">해설 다운로드</button>
  <script>
    // checkAnswers 함수는 사용자가 선택한 답안과 정답 데이터를 비교하여 점수, 결과 테이블, 해설을 출력합니다.
    function checkAnswers() {
      let submitButton = document.getElementById("submitButton");
      if (submitButton.disabled) return;
      submitButton.disabled = true;
      submitButton.style.cursor = "not-allowed";
      let userName = document.getElementById("userName").value.trim();
      if (userName === "") {
        alert("이름을 입력하세요!");
        submitButton.disabled = false;
        submitButton.style.cursor = "pointer";
        return;
      }
      // 정답 및 해설 데이터는 입력된 문제 데이터의 정답/해설 부분을 반영하여 자동 생성되어야 합니다.
      const answers = { /* 예: q1: "2", q2: "4", ... */ };
      const explanations = { /* 예: q1: "[해설 내용]", q2: "[해설 내용]", ... */ };
      let correctCount = 0;
      let incorrectExplanations = "";
      let explanationText = `<h3>${userName}님, 틀린 문제 해설</h3><ul>`;
      let resultTable = `<table>
          <tr>
            <th>문제 번호</th>
            <th>제출한 답</th>
            <th>정답</th>
          </tr>`;
      for (let key in answers) {
        let userAnswerElem = document.querySelector(`input[name="${key}"]:checked`);
        let userAnswer = userAnswerElem ? userAnswerElem.value : "미선택";
        let rowClass = userAnswer === answers[key] ? "correct" : "incorrect";
        resultTable += `<tr class="${rowClass}">
            <td>${key.replace("q", "")}</td>
            <td>${userAnswer}</td>
            <td>${answers[key]}</td>
          </tr>`;
        if (userAnswer === answers[key]) {
          correctCount++;
        } else {
          explanationText += `<li>${explanations[key]}</li>`;
          incorrectExplanations += `문제 ${key.replace("q", "")}: ${explanations[key]}\n\n`;
        }
      }
      resultTable += `</table>`;
      document.getElementById("result").innerHTML = resultTable;
      let now = new Date();
      document.getElementById("submissionTime").innerHTML = `<h3>제출 시간: ${now.toLocaleTimeString()}</h3>`;
      document.getElementById("score").innerHTML = `<h2>${userName}님, 당신의 점수는 ${correctCount} / 20입니다.</h2>`;
      explanationText += "</ul>";
      document.getElementById("explanation").innerHTML = incorrectExplanations.trim() ? explanationText : "<h3>모든 문제를 맞췄습니다! 🎉</h3>";
      if (incorrectExplanations.trim()) {
        let downloadBtn = document.getElementById("downloadExplanation");
        downloadBtn.style.display = "block";
        downloadBtn.setAttribute("data-explanation", incorrectExplanations);
      }
    }
    function downloadExplanation() {
      let explanationContent = document.getElementById("downloadExplanation").getAttribute("data-explanation");
      let blob = new Blob([explanationContent], { type: "text/plain" });
      let link = document.createElement("a");
      link.href = URL.createObjectURL(blob);
      link.download = "틀린_문제_해설.txt";
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    }
    
    /* 추가 기능: 이미지 파일에서 텍스트 추출 및 변형 문제 생성 */
    // 1. 사용자가 이미지 파일을 업로드하면 OCR을 통해 문제 데이터를 추출합니다.
    // 2. 추출된 텍스트 데이터를 기반으로, 원본 문제를 약간 변형(예: 단어 변경, 선택지 순서 섞기, 해설 보충 등)합니다.
    // 3. 변형된 문제 데이터를 HTML 템플릿의 문제 영역에 적용하여 동일한 퀴즈 페이지를 생성합니다.
    //
    // 이 기능을 구현하기 위해:
    // - 예를 들어 Tesseract.js와 같은 OCR 라이브러리를 활용하여 이미지에서 텍스트를 추출하고,
    // - 추출된 텍스트를 파싱하여 문제, 선택지, 정답, 해설 데이터를 분리합니다.
    // - 이후, 데이터 변형 로직을 추가하여 원본과 유사하지만 약간 다른 변형 문제를 생성합니다.
    //
    // 구현 예시 (개략적 pseudocode):
    /*
    function processImageFile(file) {
      Tesseract.recognize(file, 'kor').then(({ data: { text } }) => {
        // text에서 문제, 선택지, 정답, 해설 파싱 (정해진 구분자를 사용)
        // 예: "문제 1:", "정답:", "해설:" 등의 키워드 활용
        let parsedData = parseTextToProblems(text);
        // parsedData에 기반하여 변형 문제 생성 (예: 단어 변경, 순서 섞기)
        let transformedData = transformProblems(parsedData);
        // transformedData를 이용해 HTML 템플릿 내 문제 영역 업데이트
        updateQuizHTML(transformedData);
      });
    }
    */
    // 사용자가 파일을 업로드하면 processImageFile 함수를 호출하도록 파일 입력 이벤트를 추가할 수 있습니다.
  </script>
</body>
</html>
--------------------------------------------------------------------------------

**요약:**
- 이 프롬프트는 두 가지 상황(텍스트 기반 문제 생성, 이미지에서 추출 후 변형 문제 생성)에 대해 HTML 퀴즈 페이지를 자동 생성하는 스크립트를 요구합니다.
- HTML 템플릿에는 문제, 선택지, 정답, 해설, 제출 후 채점, 해설 다운로드 기능이 포함되어야 합니다.
- 이미지 파일의 경우, OCR을 통해 텍스트를 추출하고, 추출한 데이터를 약간 변형하여 동일한 HTML 구조에 반영합니다.

이 프롬프트를 기반으로 코드를 작성하면, 사용자가 직접 문제 데이터를 입력하거나 이미지 파일을 업로드할 때 자동으로 완성된 HTML 퀴즈 페이지를 생성할 수 있습니다.
