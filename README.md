# quiz-bas-canada2
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Dates évènements Bas-Canada</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f9f9f9;
      padding: 40px;
      max-width: 900px;
      margin: auto;
    }

    h1 {
      text-align: center;
      color: #2c3e50;
    }

    .question {
      background: #fff;
      padding: 15px 20px;
      margin: 12px 0;
      border-radius: 8px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }

    .question-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
    }

    select {
      padding: 6px;
      font-size: 16px;
      margin-left: 10px;
    }

    .feedback {
      margin-top: 8px;
      font-weight: bold;
    }

    .correct {
      color: green;
    }

    .incorrect {
      color: red;
    }

    button {
      display: inline-block;
      margin: 20px 10px 10px 10px;
      padding: 12px 24px;
      background-color: #3498db;
      color: white;
      font-size: 16px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
    }

    button:hover {
      background-color: #2980b9;
    }

    #result {
      text-align: center;
      font-size: 20px;
      font-weight: bold;
      color: #2c3e50;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h1>Dates évènements Bas-Canada</h1>

  <div id="quiz-container"></div>

  <div style="text-align:center;">
    <button onclick="checkAnswers()">Finir</button>
    <button onclick="location.reload()">Réinitialiser</button>
  </div>

  <div id="result"></div>

  <script>
    const questionsData = [
      { event: "Arrivée de la première banque au Bas-Canada (la Banque de Montréal)", answer: "1817" },
      { event: "Arrivée de la première université (McGill)", answer: "1821" },
      { event: "Projet d’Union des deux Canadas proposés par Ramsay", answer: "1822" },
      { event: "Début de la crise agricole", answer: "1830" },
      { event: "Émeute à Montréal entre partisans patriotes et partisans britanniques", answer: "1832" },
      { event: "Première épidémie de choléra à Montréal", answer: "1832" },
      { event: "Envoi des 92 Résolutions à Londres", answer: "1834" },
      { event: "Arrivée des 10 Résolutions de Russell", answer: "1837" },
      { event: "Batailles armées / Rébellions armées des patriotes", answer: "1837" },
      { event: "Défaite des patriotes / Suspension de la constitution", answer: "1838" },
      { event: "Rapport Durham", answer: "1839" },
      { event: "Arrivée de l’Acte d’Union", answer: "1840" }
    ];

    const allDates = Array.from(new Set(questionsData.map(q => q.answer)));

    function shuffle(array) {
      const newArray = array.slice();
      for (let i = newArray.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [newArray[i], newArray[j]] = [newArray[j], newArray[i]];
      }
      return newArray;
    }

    const shuffledQuestions = shuffle(questionsData);
    const container = document.getElementById("quiz-container");

    shuffledQuestions.forEach((q, index) => {
      const div = document.createElement("div");
      div.className = "question";

      const shuffledDates = shuffle(allDates); // shuffle dates for each question

      div.innerHTML = `
        <div class="question-header">
          <span>${index + 1}. ${q.event}</span>
          <select>
            <option value="">--Choisir--</option>
            ${shuffledDates.map(date => `<option value="${date}">${date}</option>`).join("")}
          </select>
        </div>
        <div class="feedback"></div>
      `;
      container.appendChild(div);
    });

    function checkAnswers() {
      const questions = document.querySelectorAll('.question');
      let score = 0;
      let completed = true;

      questions.forEach((q, i) => {
        const select = q.querySelector('select');
        const feedback = q.querySelector('.feedback');
        const userAnswer = select.value;
        const correct = shuffledQuestions[i].answer;

        if (userAnswer === "") {
          completed = false;
          feedback.textContent = "Veuillez choisir une réponse.";
          feedback.className = "feedback incorrect";
        } else if (userAnswer === correct) {
          score++;
          feedback.textContent = "✅ Bonne réponse";
          feedback.className = "feedback correct";
        } else {
          feedback.textContent = `❌ Mauvaise réponse (Bonne date : ${correct})`;
          feedback.className = "feedback incorrect";
        }
      });

      const result = document.getElementById("result");
      if (!completed) {
        result.style.color = "red";
        result.textContent = "Veuillez répondre à toutes les questions.";
      } else {
        result.style.color = "#2c3e50";
        result.textContent = `Vous avez ${score} bonne(s) réponse(s) sur ${questions.length}.`;
      }
    }
  </script>
</body>
</html>
