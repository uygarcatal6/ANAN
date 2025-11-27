<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Web Evolution Quiz</title>
  <style>
    /*
      Base styling inspired by the original Yekuiz layout: a dark header with
      centered navigation links and light content area. Cards are used for
      the quiz category selection. The quiz itself borrows the pop‑up
      overlay and scoreboard behaviour from the prototype. All styles are
      kept simple and use only concepts covered in basic HTML/CSS labs.
    */
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }
    body {
      font-family: Arial, sans-serif;
      background: #f5f5f5;
      color: #333;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }
    header {
      background: #333;
      color: #fff;
      padding: 1em 0;
    }
    nav ul {
      list-style: none;
      display: flex;
      justify-content: center;
      align-items: center;
      margin: 0;
    }
    nav li {
      margin: 0 1em;
    }
    nav a {
      color: #fff;
      text-decoration: none;
      font-weight: bold;
    }
    nav a:hover {
      text-decoration: underline;
    }
    main {
      flex: 1;
      max-width: 900px;
      margin: 0 auto;
      padding: 1.5em;
    }
    section {
      display: none;
      padding: 1em 0;
    }
    section.active {
      display: block;
    }
    /* Home section styling */
    #home {
      text-align: center;
    }
    #home h1 {
      font-size: 2em;
      margin-bottom: 0.5em;
    }
    #home p {
      font-size: 1.1em;
      margin-bottom: 1.5em;
    }
    /* Category cards */
    #quizlist .categories {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 1em;
      margin-top: 1em;
    }
    .quiz-card {
      background: #fff;
      border: 1px solid #ccc;
      border-radius: 6px;
      width: 160px;
      padding: 1em;
      cursor: pointer;
      text-align: center;
      transition: background 0.3s;
    }
    .quiz-card:hover {
      background: #eaeaea;
    }
    /* Quiz container */
    #quiz-container {
      background: #fff;
      border: 1px solid #ccc;
      border-radius: 8px;
      padding: 1.5em;
      max-width: 600px;
      margin: 0 auto;
      box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
    }
    #quiz-container img {
      width: 100%;
      height: auto;
      border-radius: 4px;
      margin-bottom: 1em;
    }
    #quiz-container .question-text {
      font-size: 1.1rem;
      font-weight: bold;
      margin-bottom: 0.5em;
    }
    #answers {
      list-style: none;
      padding: 0;
      margin: 0;
    }
    #answers li {
      margin-bottom: 0.5em;
    }
    #answers button {
      width: 100%;
      padding: 0.75em;
      border: 1px solid #888;
      border-radius: 4px;
      background-color: #eee;
      cursor: pointer;
      transition: background-color 0.3s;
      font-size: 0.95rem;
    }
    #answers button:hover {
      background-color: #e0e0e0;
    }
    #answers .correct-selected {
      background-color: #1e7e34;
      color: #fff;
    }
    #answers .wrong-selected {
      background-color: #dc3545;
      color: #fff;
    }
    #scoreboard {
      margin-top: 1em;
      padding: 0.5em;
      background: #f7f7f7;
      border: 1px solid #ddd;
      border-radius: 4px;
      text-align: center;
      font-size: 0.9rem;
    }
    #quiz-controls {
      margin-top: 0.5em;
      display: flex;
      justify-content: space-between;
      gap: 0.5em;
    }
    #quiz-controls button {
      flex: 1;
      padding: 0.6em;
      border: none;
      border-radius: 4px;
      color: #fff;
      cursor: pointer;
      font-size: 0.9rem;
    }
    #reset-button { background-color: #dc3545; }
    #skip-button { background-color: #6c757d; }
    #next-button { background-color: #007bff; }
    #quiz-controls button:hover {
      opacity: 0.9;
    }
    #result {
      margin-top: 1em;
      font-size: 1rem;
      text-align: center;
      display: none;
    }
    #result span.correct {
      color: #1e7e34;
      font-weight: bold;
    }
    #result span.wrong {
      color: #dc3545;
      font-weight: bold;
    }
    #result span.unanswered {
      color: #6c757d;
      font-weight: bold;
    }
    /* Overlay pop‑up */
    #overlay {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0, 0, 0, 0.6);
      display: none;
      justify-content: center;
      align-items: center;
      z-index: 1000;
    }
    #overlay .overlay-content {
      background: #fff;
      padding: 1.5em;
      border-radius: 8px;
      text-align: center;
      width: 90%;
      max-width: 400px;
    }
    #overlay .overlay-content h3 {
      margin-top: 0;
      margin-bottom: 0.5em;
    }
    #overlay .overlay-content p {
      margin-bottom: 1em;
    }
    #overlay .overlay-content button {
      padding: 0.6em 1.2em;
      background-color: #007bff;
      color: #fff;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-size: 0.9rem;
    }
    #overlay .overlay-content button:hover {
      opacity: 0.9;
    }
    /* About section styling */
    #about {
      max-width: 700px;
      margin: 0 auto;
    }
  </style>
</head>
<body>
  <header>
    <nav>
      <ul>
        <li><a href="#" onclick="showSection('home')">Home</a></li>
        <li><a href="#" onclick="showSection('quizlist')">Take the Quiz</a></li>
        <li><a href="#" onclick="showSection('about')">About Us</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <!-- Home Section -->
    <section id="home" class="active">
      <h1>Welcome to the Web Evolution Quiz</h1>
      <p>Test your knowledge of the evolution of the web. Choose a quiz category below to get started!</p>
    </section>

    <!-- Quiz Selection Section -->
    <section id="quizlist">
      <h2>Select a Quiz Category</h2>
      <div class="categories">
        <div class="quiz-card" onclick="startQuiz('web1')">
          <h3>Web 1.0</h3>
          <p>The early static web</p>
        </div>
        <div class="quiz-card" onclick="startQuiz('web2')">
          <h3>Web 2.0</h3>
          <p>The social web</p>
        </div>
        <div class="quiz-card" onclick="startQuiz('web3')">
          <h3>Web 3.0</h3>
          <p>The semantic/intelligent web</p>
        </div>
        <div class="quiz-card" onclick="startQuiz('web4')">
          <h3>Web 4.0</h3>
          <p>The futuristic web</p>
        </div>
      </div>
    </section>

    <!-- Quiz Section (dynamic) -->
    <section id="quiz">
      <h2 id="quiz-title"></h2>
      <div id="quiz-container">
        <img id="question-image" src="" alt="Question image">
        <div id="question-text" class="question-text"></div>
        <ul id="answers"></ul>
        <div id="scoreboard">Correct: <span id="correct-count">0</span> | Wrong: <span id="wrong-count">0</span> | Unanswered: <span id="unanswered-count">0</span></div>
        <div id="quiz-controls">
          <button id="reset-button" onclick="resetQuiz()">Reset</button>
          <button id="skip-button" onclick="skipQuestion()">Skip</button>
        </div>
        <div id="result"></div>
      </div>
    </section>

    <!-- About Section -->
    <section id="about">
      <h2>About Us</h2>
      <p>This quiz application is a project for an introductory web development course. It demonstrates basic HTML structure, CSS styling, and JavaScript for interactivity. All content and logic are contained in one HTML file. No external libraries were used, only the concepts taught in the course are applied here.</p>
      <p>Created by: [Your Name]</p>
    </section>
  </main>

  <!-- Pop‑up Overlay -->
  <div id="overlay">
    <div class="overlay-content">
      <h3 id="overlay-title"></h3>
      <p id="overlay-message"></p>
      <button onclick="nextQuestion()">Next Question</button>
    </div>
  </div>

  <script>
    /*
      JavaScript for the quiz logic.  We maintain separate question sets for
      each web era.  Each question has a text, an image (using a base64
      placeholder by default), and an array of answer objects with a
      boolean indicating correctness.  The functions below handle
      navigation, showing questions, processing answers, displaying
      overlays, and tracking scores.
    */

    // Placeholder image used for all questions (can be replaced with your own)
    const placeholderImage = 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAZAAAAEsCAIAAABi1XKVAAAEYElEQVR4nO3awWqEMBRA0ZnSj/L/V/msLgQRnaHC0OqVc1YhuHir\n1572';

    // Question sets for each category; each has 5 questions as requested.
    const questionSets = {
      web1: [
        {
          text: 'Web 1.0 Q1: Lorem ipsum dolor sit amet, consectetur adipiscing?',
          image: placeholderImage,
          answers: [
            { text: 'Lorem ipsum dolor sit.', correct: true },
            { text: 'Consectetur adipiscing elit.', correct: false },
            { text: 'Sed do eiusmod tempor.', correct: false },
            { text: 'Incididunt ut labore et dolore.', correct: false }
          ]
        },
        {
          text: 'Web 1.0 Q2: Sed ut perspiciatis unde omnis iste natus?',
          image: placeholderImage,
          answers: [
            { text: 'At vero eos et accusamus.', correct: false },
            { text: 'Doloremque laudantium totam.', correct: false },
            { text: 'Totam rem aperiam.', correct: true },
            { text: 'Eaque ipsa quae ab illo.', correct: false }
          ]
        },
        {
          text: 'Web 1.0 Q3: But I must explain to you how all this mistaken idea?',
          image: placeholderImage,
          answers: [
            { text: 'Denouncing pleasure and praising pain.', correct: true },
            { text: 'Neque porro quisquam est.', correct: false },
            { text: 'Qui dolorem ipsum quia dolor.', correct: false },
            { text: 'Sit amet, consectetur.', correct: false }
          ]
        },
        {
          text: 'Web 1.0 Q4: On the other hand, we denounce with righteous indignation?',
          image: placeholderImage,
          answers: [
            { text: 'Beguiled and demoralized by pleasure.', correct: false },
            { text: 'By the charms of pleasure.', correct: false },
            { text: 'Indignation and dislike men.', correct: true },
            { text: 'Denouncing pleasure and pain.', correct: false }
          ]
        },
        {
          text: 'Web 1.0 Q5: At vero eos et accusamus et iusto odio dignissimos?',
          image: placeholderImage,
          answers: [
            { text: 'Qui blanditiis praesentium.', correct: false },
            { text: 'Voluptatum deleniti atque.', correct: false },
            { text: 'Corrupti quos dolores et quas.', correct: true },
            { text: 'Molestiae excepturi sint.', correct: false }
          ]
        }
      ],
      web2: [
        {
          text: 'Web 2.0 Q1: Lorem ipsum dolor sit amet?',
          image: placeholderImage,
          answers: [
            { text: 'A', correct: false },
            { text: 'B', correct: true },
            { text: 'C', correct: false },
            { text: 'D', correct: false }
          ]
        },
        {
          text: 'Web 2.0 Q2: Sed ut perspiciatis unde omnis?',
          image: placeholderImage,
          answers: [
            { text: 'A', correct: false },
            { text: 'B', correct: false },
            { text: 'C', correct: true },
            { text: 'D', correct: false }
          ]
        },
        {
          text: 'Web 2.0 Q3: But I must explain to you?',
          image: placeholderImage,
          answers: [
            { text: 'A', correct: false },
            { text: 'B', correct: false },
            { text: 'C', correct: true },
            { text: 'D', correct: false }
          ]
        },
        {
          text: 'Web 2.0 Q4: On the other hand, we denounce?',
          image: placeholderImage,
          answers: [
            { text: 'A', correct: false },
            { text: 'B', correct: true },
            { text: 'C', correct: false },
            { text: 'D', correct: false }
          ]
        },
        {
          text: 'Web 2.0 Q5: At vero eos et accusamus?',
          image: placeholderImage,
          answers: [
            { text: 'A', correct: true },
            { text: 'B', correct: false },
            { text: 'C', correct: false },
            { text: 'D', correct: false }
          ]
        }
      ],
      web3: [
        {
          text: 'Web 3.0 Q1: In a free hour, when our power of choice is untrammelled?',
          image: placeholderImage,
          answers: [
            { text: 'And when nothing prevents.', correct: false },
            { text: 'Our being able to do what we like best.', correct: false },
            { text: 'Every pleasure is to be welcomed.', correct: true },
            { text: 'Every pain avoided.', correct: false }
          ]
        },
        {
          text: 'Web 3.0 Q2: But in certain circumstances and owing to the claims of duty?',
          image: placeholderImage,
          answers: [
            { text: 'Ut et voluptates repudiandae.', correct: false },
            { text: 'Illum quo minus id quod maxime.', correct: false },
            { text: 'Placeat facere possimus.', correct: true },
            { text: 'Omnis voluptas assumenda est.', correct: false }
          ]
        },
        {
          text: 'Web 3.0 Q3: These cases are perfectly simple and easy to distinguish?',
          image: placeholderImage,
          answers: [
            { text: 'At vero eos et accusamus.', correct: false },
            { text: 'Et iusto odio dignissimos.', correct: false },
            { text: 'Ducimus qui blanditiis.', correct: false },
            { text: 'Praesentium voluptatum deleniti.', correct: true }
          ]
        },
        {
          text: 'Web 3.0 Q4: On the other hand, we denounce with righteous indignation?',
          image: placeholderImage,
          answers: [
            { text: 'The wise man therefore always holds.', correct: true },
            { text: 'In these matters to this principle.', correct: false },
            { text: 'He rejects pleasures to secure.', correct: false },
            { text: 'But because those who do not know.', correct: false }
          ]
        },
        {
          text: 'Web 3.0 Q5: Ut enim ad minima veniam, quis nostrum exercitationem?',
          image: placeholderImage,
          answers: [
            { text: 'Ullam corporis suscipit laboriosam.', correct: false },
            { text: 'Nisi ut aliquid ex ea commodi.', correct: true },
            { text: 'Consequatur autem.', correct: false },
            { text: 'Quis autem vel eum iure.', correct: false }
          ]
        }
      ],
      web4: [
        {
          text: 'Web 4.0 Q1: Nor again is there anyone who loves or pursues?',
          image: placeholderImage,
          answers: [
            { text: 'Or desires to obtain pain.', correct: false },
            { text: 'Of itself, because it is pain.', correct: false },
            { text: 'But occasionally circumstances occur.', correct: true },
            { text: 'Which toil and pain can procure.', correct: false }
          ]
        },
        {
          text: 'Web 4.0 Q2: In a free hour, when our power of choice is untrammelled?',
          image: placeholderImage,
          answers: [
            { text: 'And when nothing prevents.', correct: false },
            { text: 'Our being able to do what we like best.', correct: false },
            { text: 'Every pleasure is to be welcomed.', correct: true },
            { text: 'Every pain avoided.', correct: false }
          ]
        },
        {
          text: 'Web 4.0 Q3: But in certain circumstances and owing to the claims of duty?',
          image: placeholderImage,
          answers: [
            { text: 'Ut et voluptates repudiandae.', correct: false },
            { text: 'Illum quo minus id quod maxime.', correct: false },
            { text: 'Placeat facere possimus.', correct: true },
            { text: 'Omnis voluptas assumenda est.', correct: false }
          ]
        },
        {
          text: 'Web 4.0 Q4: These cases are perfectly simple and easy to distinguish?',
          image: placeholderImage,
          answers: [
            { text: 'At vero eos et accusamus.', correct: false },
            { text: 'Et iusto odio dignissimos.', correct: false },
            { text: 'Ducimus qui blanditiis.', correct: false },
            { text: 'Praesentium voluptatum deleniti.', correct: true }
          ]
        },
        {
          text: 'Web 4.0 Q5: Nor again is there anyone who loves or pursues?',
          image: placeholderImage,
          answers: [
            { text: 'Or desires to obtain pain.', correct: false },
            { text: 'Of itself, because it is pain.', correct: false },
            { text: 'But occasionally circumstances occur.', correct: true },
            { text: 'Which toil and pain can procure.', correct: false }
          ]
        }
      ]
    };

    // State variables
    let currentSet = [];
    let currentIndex = 0;
    let correctCount = 0;
    let wrongCount = 0;
    let unansweredCount = 0;
    let currentCategory = '';

    // Show the specified section and hide others
    function showSection(id) {
      const sections = document.querySelectorAll('main > section');
      sections.forEach(sec => sec.classList.remove('active'));
      const target = document.getElementById(id);
      if (target) target.classList.add('active');
    }

    // Start a quiz for a given category
    function startQuiz(category) {
      currentCategory = category;
      currentSet = questionSets[category];
      currentIndex = 0;
      correctCount = 0;
      wrongCount = 0;
      unansweredCount = 0;
      // Update quiz title
      document.getElementById('quiz-title').textContent = category.toUpperCase() + ' Quiz';
      // Reset scoreboard
      updateScoreboard();
      // Show first question
      showQuestion(currentIndex);
      // Show quiz section
      showSection('quiz');
      // Ensure question elements are visible
      document.getElementById('question-image').style.display = '';
      document.getElementById('question-text').style.display = '';
      document.getElementById('answers').style.display = '';
      document.getElementById('quiz-controls').style.display = 'flex';
    }

    // Render current question
    function showQuestion(index) {
      // If all questions answered, show results
      if (!currentSet || index >= currentSet.length) {
        showResults();
        return;
      }
      const question = currentSet[index];
      document.getElementById('question-image').src = question.image;
      document.getElementById('question-text').textContent = 'Q' + (index + 1) + '. ' + question.text;
      const answersList = document.getElementById('answers');
      answersList.innerHTML = '';
      question.answers.forEach((answer, i) => {
        const li = document.createElement('li');
        const btn = document.createElement('button');
        btn.textContent = answer.text;
        btn.onclick = function() {
          handleAnswer(btn, answer.correct);
        };
        li.appendChild(btn);
        answersList.appendChild(li);
      });
      // Hide result and overlay if visible
      document.getElementById('result').style.display = 'none';
      document.getElementById('overlay').style.display = 'none';
      // Re-enable buttons
      const qButtons = answersList.querySelectorAll('button');
      qButtons.forEach(btn => {
        btn.disabled = false;
        btn.parentElement.classList.remove('correct-selected', 'wrong-selected');
      });
    }

    // Handle answer selection
    function handleAnswer(button, isCorrect) {
      // Disable all answer buttons to avoid multiple clicks
      const allButtons = document.querySelectorAll('#answers button');
      allButtons.forEach(btn => btn.disabled = true);
      // Highlight selected answer
      const li = button.parentElement;
      if (isCorrect) {
        li.classList.add('correct-selected');
        correctCount++;
        showOverlay('Correct!', 'Well done! That is the right answer.');
      } else {
        li.classList.add('wrong-selected');
        wrongCount++;
        // Determine correct answer text for feedback
        const correctAnswer = currentSet[currentIndex].answers.find(a => a.correct).text;
        showOverlay('Wrong!', 'The correct answer was: ' + correctAnswer);
      }
      updateScoreboard();
    }

    // Display overlay with message
    function showOverlay(title, message) {
      document.getElementById('overlay-title').textContent = title;
      document.getElementById('overlay-message').textContent = message;
      document.getElementById('overlay').style.display = 'flex';
    }

    // Proceed to next question (called by overlay button)
    function nextQuestion() {
      document.getElementById('overlay').style.display = 'none';
      currentIndex++;
      if (currentIndex >= currentSet.length) {
        showResults();
      } else {
        showQuestion(currentIndex);
      }
    }

    // Skip current question: count as unanswered and move on
    function skipQuestion() {
      unansweredCount++;
      updateScoreboard();
      currentIndex++;
      if (currentIndex >= currentSet.length) {
        showResults();
      } else {
        showQuestion(currentIndex);
      }
    }

    // Reset the quiz (used by reset button)
    function resetQuiz() {
      currentIndex = 0;
      correctCount = 0;
      wrongCount = 0;
      unansweredCount = 0;
      updateScoreboard();
      showQuestion(currentIndex);
      // Ensure controls and question elements visible
      document.getElementById('question-image').style.display = '';
      document.getElementById('question-text').style.display = '';
      document.getElementById('answers').style.display = '';
      document.getElementById('quiz-controls').style.display = 'flex';
    }

    // Update scoreboard display
    function updateScoreboard() {
      document.getElementById('correct-count').textContent = correctCount;
      document.getElementById('wrong-count').textContent = wrongCount;
      document.getElementById('unanswered-count').textContent = unansweredCount;
    }

    // Show final results at end of quiz
    function showResults() {
      // Hide question elements
      document.getElementById('question-image').style.display = 'none';
      document.getElementById('question-text').style.display = 'none';
      document.getElementById('answers').style.display = 'none';
      document.getElementById('quiz-controls').style.display = 'none';
      // Show result summary
      const resultDiv = document.getElementById('result');
      resultDiv.innerHTML = 'Quiz finished! You answered <span class="correct">' + correctCount + '</span> correctly, <span class="wrong">' + wrongCount + '</span> incorrectly and <span class="unanswered">' + unansweredCount + '</span> unanswered.';
      resultDiv.style.display = 'block';
    }
  </script>
</body>
</html>
