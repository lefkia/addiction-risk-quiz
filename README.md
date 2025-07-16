import webbrowser
from http.server import SimpleHTTPRequestHandler, HTTPServer

html_content = '''
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>What’s Your Risk? - Addiction Quiz</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 700px; margin: 2em auto; padding: 1em; background: #f5f5f5; }
    h1 { text-align: center; }
    .question { background: white; padding: 1em; margin-bottom: 1em; border-radius: 5px; border: 1px solid #ccc; }
    .results { font-size: 1.2em; background: #e6ffe6; padding: 1em; margin-top: 2em; border-radius: 5px; }
    button { padding: 10px 20px; font-size: 1em; display: block; margin: 2em auto; background: #007BFF; color: white; border: none; border-radius: 5px; cursor: pointer; }
    button:hover { background: #0056b3; }
  </style>
</head>
<body>
  <h1>What’s Your Risk? A Genetic & Environmental Addiction Quiz</h1>
  <form id="quiz">
    <!-- Questions -->
    {questions}
    <button type="button" onclick="showResults()">Submit</button>
  </form>
  <div class="results" id="results"></div>
  <script>
    function showResults() {
      let score = 0;
      for (let i = 1; i <= 10; i++) {
        const options = document.getElementsByName("q" + i);
        let answered = false;
        for (let opt of options) {
          if (opt.checked) {
            score += parseInt(opt.value);
            answered = true;
          }
        }
        if (!answered) {
          alert("Please answer question " + i);
          return;
        }
      }
      const percent = Math.round((score / 20) * 100);
      let risk = percent <= 25 ? 'Low' : percent <= 60 ? 'Moderate' : 'High';
      document.getElementById('results').innerHTML =
        `Your risk score is <strong>${percent}% (${risk} risk)</strong>.<br>` +
        (risk === 'High'
          ? '⚠️ You may benefit from additional support and professional resources.'
          : risk === 'Moderate'
          ? '⚠️ You have some risk factors. Awareness and healthy habits are key.'
          : '✅ You have a relatively low risk. Keep up the good choices!');
    }
  </script>
</body>
</html>
'''

questions = [
  ("Do any of your close relatives (parents or siblings) have a history of addiction (alcohol, drugs, gambling)?",
   ["A. Yes, multiple family members", "B. One family member", "C. No"]),
  ("Have you been diagnosed with or experienced symptoms of anxiety, depression, or ADHD?",
   ["A. Yes, diagnosed", "B. Experienced symptoms but not diagnosed", "C. No"]),
  ("How often do you act on impulse or take risks without much planning?",
   ["A. Often", "B. Sometimes", "C. Rarely"]),
  ("Have you experienced traumatic events in your early life (e.g., abuse, neglect, loss)?",
   ["A. Yes", "B. Somewhat", "C. No"]),
  ("What is your current stress level?",
   ["A. High – I often feel overwhelmed", "B. Moderate – I manage but feel it", "C. Low – I rarely feel stressed"]),
  ("Do your friends or peers regularly use drugs, alcohol, or vape?",
   ["A. Yes, often", "B. Occasionally", "C. No"]),
  ("How involved were your parents/guardians while you were growing up?",
   ["A. Not involved or neglectful", "B. Somewhat involved", "C. Very involved and supportive"]),
  ("How accessible are drugs, alcohol, or addictive substances in your daily life?",
   ["A. Very accessible", "B. Somewhat accessible", "C. Not easily accessible"]),
  ("How often do you feel emotionally disconnected or lonely?",
   ["A. Frequently", "B. Sometimes", "C. Rarely"]),
  ("Do you use healthy coping mechanisms like journaling, sports, or talking to someone when stressed?",
   ["A. Rarely", "B. Occasionally", "C. Frequently"]),
]

q_html = ""
for idx, (question, answers) in enumerate(questions, 1):
    q_html += f'<div class="question"><strong>{idx}. {question}</strong><br>'
    for v, ans in enumerate(answers[::-1]):  # C=0, B=1, A=2
        q_html += f'<label><input type="radio" name="q{idx}" value="{v}"> {ans}</label><br>'
    q_html += '</div>'

final_html = html_content.format(questions=q_html)
Path("quiz.html").write_text(final_html)

# Serve locally for preview
class Handler(SimpleHTTPRequestHandler):
    def log_message(self, format, *args):
        return

PORT = 8000
print(f"Serving on http://localhost:{PORT}/quiz.html")
webbrowser.open(f"http://localhost:{PORT}/quiz.html")
HTTPServer(("localhost", PORT), Handler).serve_forever()
