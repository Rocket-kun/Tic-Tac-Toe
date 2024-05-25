<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic Tac Toe</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #333;
            margin: 0;
            font-family: Arial, sans-serif;
        }

        .container {
            text-align: center;
        }

        h1 {
            color: #fff;
        }

        .board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            gap: 10px;
            margin: 20px auto;
        }

        .cell {
            width: 100px;
            height: 100px;
            background-color: #fff;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 40px;
            cursor: pointer;
        }

        .cell.x::before {
            content: 'X';
            color: #ff6347;
        }

        .cell.o::before {
            content: 'O';
            color: #4682b4;
        }

        .winning-message, .selection-message {
            display: none;
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0.8);
            color: #fff;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            font-size: 40px;
        }

        .winning-message button, .selection-message button {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 20px;
            cursor: pointer;
        }

        .selection-message button {
            margin: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Tic Tac Toe</h1>
        <div class="board" id="board">
            <div class="cell" data-cell></div>
            <div class="cell" data-cell></div>
            <div class="cell" data-cell></div>
            <div class="cell" data-cell></div>
            <div class="cell" data-cell></div>
            <div class="cell" data-cell></div>
            <div class="cell" data-cell></div>
            <div class="cell" data-cell></div>
            <div class="cell" data-cell></div>
        </div>
        <div class="winning-message" id="winningMessage">
            <div data-winning-message-text></div>
            <button id="restartButton">Restart</button>
        </div>
        <div class="selection-message" id="selectionMessage">
            <div>Select your mark</div>
            <button id="xButton">X</button>
            <button id="oButton">O</button>
        </div>
    </div>
    <script>
        const X_CLASS = 'x';
        const O_CLASS = 'o';
        const WINNING_COMBINATIONS = [
            [0, 1, 2],
            [3, 4, 5],
            [6, 7, 8],
            [0, 3, 6],
            [1, 4, 7],
            [2, 5, 8],
            [0, 4, 8],
            [2, 4, 6]
        ];
        const cellElements = document.querySelectorAll('[data-cell]');
        const board = document.getElementById('board');
        const winningMessageElement = document.getElementById('winningMessage');
        const winningMessageTextElement = document.querySelector('[data-winning-message-text]');
        const restartButton = document.getElementById('restartButton');
        const selectionMessageElement = document.getElementById('selectionMessage');
        const xButton = document.getElementById('xButton');
        const oButton = document.getElementById('oButton');
        let oTurn;
        let playerClass;

        startGame();

        restartButton.addEventListener('click', startGame);
        xButton.addEventListener('click', () => selectMark(X_CLASS));
        oButton.addEventListener('click', () => selectMark(O_CLASS));

        function startGame() {
            oTurn = false;
            playerClass = null;
            cellElements.forEach(cell => {
                cell.classList.remove(X_CLASS);
                cell.classList.remove(O_CLASS);
                cell.removeEventListener('click', handleClick);
            });
            setBoardHoverClass();
            winningMessageElement.style.display = 'none';
            selectionMessageElement.style.display = 'flex';
        }

        function selectMark(selectedClass) {
            playerClass = selectedClass;
            cellElements.forEach(cell => {
                cell.addEventListener('click', handleClick, { once: true });
            });
            selectionMessageElement.style.display = 'none';
        }

        function handleClick(e) {
            const cell = e.target;
            const currentClass = oTurn ? (playerClass === X_CLASS ? O_CLASS : X_CLASS) : playerClass;
            placeMark(cell, currentClass);
            if (checkWin(currentClass)) {
                endGame(currentClass);
            } else if (isDraw()) {
                endGame(null);
            } else {
                swapTurns();
                setBoardHoverClass();
            }
        }

        function endGame(winnerClass) {
            if (winnerClass === X_CLASS) {
                winningMessageTextElement.innerText = "X's Wins!";
            } else if (winnerClass === O_CLASS) {
                winningMessageTextElement.innerText = "O's Wins!";
            } else {
                winningMessageTextElement.innerText = 'Draw!';
            }
            winningMessageElement.style.display = 'flex';
        }

        function isDraw() {
            return [...cellElements].every(cell => {
                return cell.classList.contains(X_CLASS) || cell.classList.contains(O_CLASS);
            });
        }

        function placeMark(cell, currentClass) {
            cell.classList.add(currentClass);
        }

        function swapTurns() {
            oTurn = !oTurn;
        }

        function setBoardHoverClass() {
            board.classList.remove(X_CLASS);
            board.classList.remove(O_CLASS);
            if (oTurn) {
                board.classList.add(playerClass === X_CLASS ? O_CLASS : X_CLASS);
            } else {
                board.classList.add(playerClass);
            }
        }

        function checkWin(currentClass) {
            return WINNING_COMBINATIONS.some(combination => {
                return combination.every(index => {
                    return cellElements[index].classList.contains(currentClass);
                });
            });
        }
    </script>
</body>
</html>
