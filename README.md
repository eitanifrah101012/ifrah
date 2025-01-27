<!DOCTYPE html>
<html lang="he">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>משחק זיכרון עם שאלות</title>
    <style>
        body {
            margin: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            background-color: #282c34;
            color: white;
            font-family: Arial, sans-serif;
            text-align: center;
        }
        .grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }
        .button {
            width: 100px;
            height: 100px;
            font-size: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            background-color: lightgray;
            transition: background-color 0.5s, transform 0.5s;
            text-align: center;
        }
        .button.wrong {
            background-color: red;
            animation: shake 0.5s;
        }
        @keyframes shake {
            0% { transform: translateX(0); }
            25% { transform: translateX(-5px); }
            50% { transform: translateX(5px); }
            75% { transform: translateX(-5px); }
            100% { transform: translateX(0); }
        }
        #timer {
            margin-bottom: 20px;
        }
    </style>
</head>
<body>
    <h1>משחק זיכרון עם שאלות</h1>
    <p>זמן: <span id="timer">0</span> שניות</p>
    <div id="grid" class="grid"></div>

    <script type="module">
        // Import Firebase
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.2.0/firebase-app.js";
        import { getFirestore, collection, addDoc } from "https://www.gstatic.com/firebasejs/11.2.0/firebase-firestore.js";

        // Firebase Configuration
        const firebaseConfig = {
            apiKey: "AIzaSyCBKOpZlAUhJvDfmL-pI2RYJ1k8YEzvi-4",
            authDomain: "eitan-e234c.firebaseapp.com",
            projectId: "eitan-e234c",
            storageBucket: "eitan-e234c.appspot.com",
            messagingSenderId: "999186555821",
            appId: "1:999186555821:web:dfaf703883f1cc80a53724",
            measurementId: "G-ZEKZRCJLML"
        };

        // Initialize Firebase
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        // Game Variables
        const questions = [
            { text: "היעוד של תחנת החלל שלנו הוא:", pair: "אסיפת פסולת מהחלל" },
            { text: "השלימו את המשפט: בתוך תחנת החלל שלנו יש...", pair: "(צי של חלליות (קטנות" },
            { text: "מי התחנת החלל הסינית?", pair: "Tiangong" },
            { text: "מי התחנת Gateway לתחנת הירח?", pair: "Gateway" },
            { text: "מי תחנת החלל העתידית?", pair: "StarLab" },
            { text: "מי התחנת החלל המפורסמת הרוסית?", pair: "Mir" },
            { text: "מי תחנת החלל האמריקאית הראשונה?", pair: "SkyLab" },
            { text: "מי תחנת החלל הראשונה?", pair: "Salyut" }
        ];
        const answers = [
            { text: "אסיפת פסולת מהחלל", pair: "אסיפת פסולת מהחלל" },
            { text: "(צי של חלליות (קטנות", pair: "(צי של חלליות (קטנות" },
            { text: "Tiangong", pair: "Tiangong" },
            { text: "Gateway", pair: "Gateway" },
            { text: "StarLab", pair: "StarLab" },
            { text: "Mir", pair: "Mir" },
            { text: "SkyLab", pair: "SkyLab" },
            { text: "Salyut", pair: "Salyut" }
        ];
        const gridChoices = [...questions, ...answers].sort(() => Math.random() - 0.5);
        const grid = document.getElementById('grid');
        const timerElement = document.getElementById('timer');
        let first = true;
        let previousButton = null;
        let matchesCount = 0;
        let startTime = new Date();

        // Timer Function
        function updateTimer() {
            let currentTime = new Date();
            let elapsedTime = Math.floor((currentTime - startTime) / 1000);
            timerElement.textContent = elapsedTime;
        }
        setInterval(updateTimer, 1000);

        function click(button, item) {
            if (button.textContent !== '') return;

            button.textContent = item.text;

            if (first) {
                previousButton = button;
                first = false;
            } else {
                if (previousButton.dataset.pair !== item.pair) {
                    previousButton.classList.add('wrong');
                    button.classList.add('wrong');
                    setTimeout(() => {
                        previousButton.textContent = '';
                        button.textContent = '';
                        previousButton.classList.remove('wrong');
                        button.classList.remove('wrong');
                    }, 500);
                } else {
                    matchesCount++;
                    previousButton.style.backgroundColor = 'lightgreen';
                    button.style.backgroundColor = 'lightgreen';
                }
                first = true;
            }

            // Check if Game is Over
            if (matchesCount === questions.length) {
                let time = timerElement.textContent;
                alert('כל הכבוד! סיימת בזמן של ' + time + ' שניות.');
            }
        }

        // Initialize Grid
        gridChoices.forEach((item) => {
            const button = document.createElement('div');
            button.className = 'button';
            button.dataset.pair = item.pair;
            button.onclick = () => click(button, item);
            grid.appendChild(button);
        });
    </script>
</body>
</html>


