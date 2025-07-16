<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Addiction Risk Quiz</title>
  <style>
    body { font-family: Arial; max-width: 600px; margin: 2em auto; }
    .question { margin-bottom: 1.5em; }
    .results { font-size: 1.2em; margin-top: 2em; }
  </style>
</head>
<body>
  <h1>Addiction Risk Quiz</h1>
  <form id="quiz">
    <!-- Questions 1 to 10 -->
    ${Array.from({ length: 10 }, (_, i) => `
      <div class="question">
        <strong>${i+1}.</strong> ${[
          "Do any of your close relatives have a history of addiction?",
          "Have you been diagnosed with or experienced symptoms of anxiety, depression, or ADHD?",
          "How often do you act on impulse or take risks?",
          "Have you experienced traumatic events early in life?",
          "What is your current stress level?",
          "Do your friends or peers regularly use addictive substances?",
          "How involved were your parents/guardians growing up?",
          "How accessible are addictive substances in your life?",
          "How often do you feel emotionally disconnected or lonely?",
          "Do you use healthy coping mechanisms when stressed?"
        ][i]}
        <br>
        <label><input type="radio" name="q${i+1}" value="2"> A. Often / Yes</label><br>
        <label><input type="radio" name="q${i+1}" value="1"> B. Sometimes / Occasionally</label><br>
        <label><input type="radio" name="q${i+1}" value="0"> C. Rarely / No</label>
      </div>`).join('')}
    <button type="button" onclick="showResults()">Submit</button>
  </form>

  <div class="results" id="results"></div>

  <script>
    function showResults() {
      const form = document.getElementById('quiz');
      let score = 0;
      for (let i = 1; i <= 10; i++) {
        const v = form['q'+i].value;
        if (v === '') return alert('Please answer question '+i);
        score += parseInt(v);
      }
      const pct = Math.round((score / 20) * 100);
      let risk = pct <= 25 ? 'Low' : pct <= 60 ? 'Moderate' : 'High';
      document.getElementById('results').innerHTML = 
        `Your risk score is <strong>${pct}% (${risk} risk)</strong>.<br>` +
        (risk === 'High'
          ? 'You may benefit from stronger support and resources.'
          : risk === 'Moderate'
          ? 'You have some risk factors—stay mindful and practice coping strategies.'
          : 'You have relatively low risk—but healthy habits still matter!');
    }
  </script>
</body>
</html>
  <img width="468" height="652" alt="image" src="https://github.com/user-attachments/assets/ae84ad16-1062-406c-9145-1a115d62aea3" />
