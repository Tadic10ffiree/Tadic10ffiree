<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Anket Oluştur</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            margin: 0;
            padding: 0;
        }
        .container {
            max-width: 800px;
            margin: auto;
            padding: 20px;
        }
        .header {
            text-align: center;
            background-color: #0078d7;
            color: white;
            padding: 20px 0;
        }
        .form-group {
            margin-bottom: 20px;
        }
        label {
            display: block;
            margin-bottom: 10px;
        }
        input, textarea, button {
            width: 100%;
            padding: 10px;
            margin: 5px 0;
        }
        button {
            background-color: #0078d7;
            color: white;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #005a9e;
        }
        .link {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>Anket Oluştur</h1>
    </div>
    <div class="container">
        <div class="form-group">
            <label for="question">Soru:</label>
            <input type="text" id="question" placeholder="Sorunuzu yazın...">
        </div>
        <div class="form-group">
            <label for="options">Seçenekler (Her satıra bir seçenek yazın):</label>
            <textarea id="options" placeholder="Seçenekleri yazın..."></textarea>
        </div>
        <button onclick="addQuestion()">Soru Ekle</button>
        <button onclick="generateLink()">Anket Linki Oluştur</button>
        <div id="questionsContainer"></div>
        <div id="linkContainer" class="link" style="display:none;">
            <p>Anketinizi paylaşmak için bu linki kullanın:</p>
            <textarea id="generatedLink" readonly></textarea>
        </div>
    </div>

    <script>
        const questions = [];

        function addQuestion() {
            const questionText = document.getElementById('question').value.trim();
            const optionsText = document.getElementById('options').value.trim();

            if (!questionText || !optionsText) {
                alert('Lütfen bir soru ve seçenekler yazın.');
                return;
            }

            const options = optionsText.split('\n');
            questions.push({ question: questionText, options });

            document.getElementById('question').value = '';
            document.getElementById('options').value = '';

            displayQuestions();
        }

        function displayQuestions() {
            const container = document.getElementById('questionsContainer');
            container.innerHTML = '';
            questions.forEach((q, index) => {
                const div = document.createElement('div');
                div.className = 'form-group';
                div.innerHTML = `
                    <p><strong>${index + 1}. ${q.question}</strong></p>
                    <ul>
                        ${q.options.map(option => `<li>${option}</li>`).join('')}
                    </ul>
                `;
                container.appendChild(div);
            });
        }

        function generateLink() {
            if (questions.length === 0) {
                alert('Lütfen en az bir soru ekleyin.');
                return;
            }

            const baseUrl = location.origin + location.pathname;
            const queryString = encodeURIComponent(JSON.stringify(questions));
            const shareableLink = `${baseUrl}?data=${queryString}`;

            document.getElementById('linkContainer').style.display = 'block';
            document.getElementById('generatedLink').value = shareableLink;
        }
    </script>
</body>
</html>
