# ChatGPT-4でオセロゲームを作る

## 結果

オセロゲームは作れた。
AIに手を決めてもらう実装の追加を試みたところで断念

![image](https://user-images.githubusercontent.com/11986901/226084674-6e18b066-23fd-4e09-8661-16ace1001472.png)

## Chat内容



![image](https://user-images.githubusercontent.com/11986901/226082350-67d2cadc-57a4-4b41-ab96-ac6e74b3b166.png)

![image](https://user-images.githubusercontent.com/11986901/226082374-4663b9ba-d9b0-4860-8057-fa48bd3a9b03.png)


```javascript
class Othello {
  constructor() {
    this.board = this.createBoard();
    this.currentPlayer = "black";
  }

  createBoard() {
    const board = Array.from({ length: 8 }, () => Array(8).fill(null));
    board[3][3] = "white";
    board[3][4] = "black";
    board[4][3] = "black";
    board[4][4] = "white";
    return board;
  }

  switchPlayer() {
    this.currentPlayer = this.currentPlayer === "black" ? "white" : "black";
  }

  isValidMove(x, y, player) {
    if (this.board[x][y] !== null) return false;

    const directions = [
      [-1, 0],
      [1, 0],
      [0, -1],
      [0, 1],
      [-1, -1],
      [-1, 1],
      [1, -1],
      [1, 1],
    ];

    for (const [dx, dy] of directions) {
      let nx = x + dx;
      let ny = y + dy;
      let foundOpponent = false;

      while (nx >= 0 && nx < 8 && ny >= 0 && ny < 8) {
        const cell = this.board[nx][ny];

        if (cell === null) break;
        if (cell === player) {
          if (foundOpponent) return true;
          break;
        }

        foundOpponent = true;
        nx += dx;
        ny += dy;
      }
    }

    return false;
  }

  makeMove(x, y, player) {
    if (!this.isValidMove(x, y, player)) return false;

    const directions = [
      [-1, 0],
      [1, 0],
      [0, -1],
      [0, 1],
      [-1, -1],
      [-1, 1],
      [1, -1],
      [1, 1],
    ];

    this.board[x][y] = player;

    for (const [dx, dy] of directions) {
      let nx = x + dx;
      let ny = y + dy;
      let cellsToFlip = [];

      while (nx >= 0 && nx < 8 && ny >= 0 && ny < 8) {
        const cell = this.board[nx][ny];

        if (cell === null) break;
        if (cell === player) {
          cellsToFlip.forEach(([fx, fy]) => {
            this.board[fx][fy] = player;
          });
          break;
        }

        cellsToFlip.push([nx, ny]);
        nx += dx;
        ny += dy;
      }
    }

    this.switchPlayer();
    return true;
  }
}

```

![image](https://user-images.githubusercontent.com/11986901/226082421-bc5a672e-d407-4742-833c-dcabcbc9576b.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Othello Game</title>
    <style>
        canvas {
            display: block;
            margin: 0 auto;
            border: 1px solid black;
        }
    </style>
</head>
<body>
    <canvas id="board" width="320" height="320"></canvas>
    <script>
        // ここに先程の Othello クラスのコードを追加

        const canvas = document.getElementById('board');
        const ctx = canvas.getContext('2d');
        const cellSize = 40;
        const game = new Othello();

        canvas.addEventListener('click', (e) => {
            const rect = canvas.getBoundingClientRect();
            const x = Math.floor((e.clientX - rect.left) / cellSize);
            const y = Math.floor((e.clientY - rect.top) / cellSize);

            game.makeMove(x, y, game.currentPlayer);
            drawBoard();
        });

        function drawBoard() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draw grid lines
            ctx.strokeStyle = 'black';
            for (let i = 0; i <= 8; i++) {
                ctx.beginPath();
                ctx.moveTo(i * cellSize, 0);
                ctx.lineTo(i * cellSize, canvas.height);
                ctx.stroke();

                ctx.beginPath();
                ctx.moveTo(0, i * cellSize);
                ctx.lineTo(canvas.width, i * cellSize);
                ctx.stroke();
            }

            // Draw stones
            for (let x = 0; x < 8; x++) {
                for (let y = 0; y < 8; y++) {
                    const cell = game.board[x][y];
                    if (cell !== null) {
                        ctx.fillStyle = cell;
                        ctx.beginPath();
                        ctx.arc(
                            x * cellSize + cellSize / 2,
                            y * cellSize + cellSize / 2,
                            cellSize * 0.4,
                            0,
                            2 * Math.PI
                        );
                        ctx.fill();
                    }
                }
            }
        }

        drawBoard();
    </script>
</body>
</html>
```

![image](https://user-images.githubusercontent.com/11986901/226082453-d7a18fbb-1364-4a92-8294-172c66fb61e5.png)

![image](https://user-images.githubusercontent.com/11986901/226082476-b4bb9bf3-e61f-4a70-a6d4-0ba13d6961ea.png)


```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Othello Game</title>
    <style>
        canvas {
            display: block;
            margin: 0 auto;
            border: 1px solid black;
        }
    </style>
</head>
<body>
    <canvas id="board" width="320" height="320"></canvas>
    <script>
        // 以下に Othello クラスのコードを追加
        class Othello {
          constructor() {
            this.board = this.createBoard();
            this.currentPlayer = "black";
          }

          createBoard() {
            const board = Array.from({ length: 8 }, () => Array(8).fill(null));
            board[3][3] = "white";
            board[3][4] = "black";
            board[4][3] = "black";
            board[4][4] = "white";
            return board;
          }

          switchPlayer() {
            this.currentPlayer = this.currentPlayer === "black" ? "white" : "black";
          }

          isValidMove(x, y, player) {
            if (this.board[x][y] !== null) return false;

            const directions = [
              [-1, 0],
              [1, 0],
              [0, -1],
              [0, 1],
              [-1, -1],
              [-1, 1],
              [1, -1],
              [1, 1],
            ];

            for (const [dx, dy] of directions) {
              let nx = x + dx;
              let ny = y + dy;
              let foundOpponent = false;

              while (nx >= 0 && nx < 8 && ny >= 0 && ny < 8) {
                const cell = this.board[nx][ny];

                if (cell === null) break;
                if (cell === player) {
                  if (foundOpponent) return true;
                  break;
                }

                foundOpponent = true;
                nx += dx;
                ny += dy;
              }
            }

            return false;
          }

          makeMove(x, y, player) {
            if (!this.isValidMove(x, y, player)) return false;

            const directions = [
              [-1, 0],
              [1, 0],
              [0, -1],
              [0, 1],
              [-1, -1],
              [-1, 1],
              [1, -1],
              [1, 1],
            ];

            this.board[x][y] = player;

            for (const [dx, dy] of directions) {
              let nx = x + dx;
              let ny = y + dy;
              let cellsToFlip = [];

              while (nx >= 0 && nx < 8 && ny >= 0 && ny < 8) {
                const

```

![image](https://user-images.githubusercontent.com/11986901/226082565-44351acc-977b-40af-a770-87103d727aee.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Othello Game</title>
    <style>
        canvas {
            display: block;
            margin: 0 auto;
            border: 1px solid black;
        }
    </style>
</head>
<body>
    <canvas id="board" width="320" height="320"></canvas>
    <script>
        // 以下に Othello クラスのコードを追加
        class Othello {
          // ... Othello クラスのコードをここに貼り付け ...

        const canvas = document.getElementById('board');
        const ctx = canvas.getContext('2d');
        const cellSize = 40;
        const game = new Othello();

        canvas.addEventListener('click', (e) => {
            const rect = canvas.getBoundingClientRect();
            const x = Math.floor((e.clientX - rect.left) / cellSize);
            const y = Math.floor((e.clientY - rect.top) / cellSize);

            game.makeMove(x, y, game.currentPlayer);
            drawBoard();
        });

        function drawBoard() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draw stones
            for (let x = 0; x < 8; x++) {
                for (let y = 0; y < 8; y++) {
                    const cell = game.board[x][y];
                    if (cell !== null) {
                        ctx.fillStyle = cell;
                        ctx.beginPath();
                        ctx.arc(
                            x * cellSize + cellSize / 2,
                            y * cellSize + cellSize / 2,
                            cellSize * 0.4,
                            0,
                            2 * Math.PI
                        );
                        ctx.fill();
                    }
                }
            }

            // Draw grid lines
            ctx.strokeStyle = 'black';
            for (let i = 0; i <= 8; i++) {
                ctx.beginPath();
                ctx.moveTo(i * cellSize, 0);
                ctx.lineTo(i * cellSize, canvas.height);
                ctx.stroke();

                ctx.beginPath();
                ctx.moveTo(0, i * cellSize);
                ctx.lineTo(canvas.width, i * cellSize);
                ctx.stroke();
            }
        }

        drawBoard();
    </script>
</body>
</html>

```

![image](https://user-images.githubusercontent.com/11986901/226082605-bd323a91-76e7-4f8f-add1-0609ae90899d.png)

![image](https://user-images.githubusercontent.com/11986901/226082610-09873e10-9607-4abd-bc03-3633e3a0548c.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Othello Game</title>
    <style>
        canvas {
            display: block;
            margin: 0 auto;
            border: 1px solid black;
        }
    </style>
</head>
<body>
    <canvas id="board" width="320" height="320"></canvas>
    <script>
        // 以下に Othello クラスのコードを追加
        class Othello {
          // ... Othello クラスのコードをここに貼り付け ...

        const canvas = document.getElementById('board');
        const ctx = canvas.getContext('2d');
        const cellSize = 40;
        const game = new Othello();

        canvas.addEventListener('click', (e) => {
            const rect = canvas.getBoundingClientRect();
            const x = Math.floor((e.clientX - rect.left) / cellSize);
            const y = Math.floor((e.clientY - rect.top) / cellSize);

            game.makeMove(x, y, game.currentPlayer);
            drawBoard();
        });

        function drawBoard() {
            // Clear canvas and set background color to green
            ctx.fillStyle = "green";
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            // Draw stones
            for (let x = 0; x < 8; x++) {
                for (let y = 0; y < 8; y++) {
                    const cell = game.board[x][y];
                    if (cell !== null) {
                        ctx.fillStyle = cell;
                        ctx.beginPath();
                        ctx.arc(
                            x * cellSize + cellSize / 2,
                            y * cellSize + cellSize / 2,
                            cellSize * 0.4,
                            0,
                            2 * Math.PI
                        );
                        ctx.fill();
                    }
                }
            }

            // Draw grid lines
            ctx.strokeStyle = 'black';
            for (let i = 0; i <= 8; i++) {
                ctx.beginPath();
                ctx.moveTo(i * cellSize, 0);
                ctx.lineTo(i * cellSize, canvas.height);
                ctx.stroke();

                ctx.beginPath();
                ctx.moveTo(0, i * cellSize);
                ctx.lineTo(canvas.width, i * cellSize);
                ctx.stroke();
            }
        }

        drawBoard();
    </script>
</body>
</html>

```

![image](https://user-images.githubusercontent.com/11986901/226082629-b5f22624-d2ca-4db2-8853-17fb69c98a3a.png)

![image](https://user-images.githubusercontent.com/11986901/226082641-7e943068-90a0-45c7-a039-1b96896adb93.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Othello Game</title>
    <style>
        canvas {
            display: block;
            margin: 0 auto;
            border: 1px solid black;
        }
    </style>
</head>
<body>
    <canvas id="board" width="320" height="320"></canvas>
    <script>
        // 以下に Othello クラスのコードを追加
        class Othello {
          // ... Othello クラスのコードをここに貼り付け ...

        const canvas = document.getElementById('board');
        const ctx = canvas.getContext('2d');
        const cellSize = 40;
        const game = new Othello();

        function chooseBestMove(game) {
          const validMoves = [];

          for (let x = 0; x < 8; x++) {
            for (let y = 0; y < 8; y++) {
              if (game.isValidMove(x, y, game.currentPlayer)) {
                validMoves.push({ x, y });
              }
            }
          }

          // Randomly select a valid move
          if (validMoves.length > 0) {
            const randomIndex = Math.floor(Math.random() * validMoves.length);
            return validMoves[randomIndex];
          }

          return null;
        }

        function isGameOver(game) {
          for (let x = 0; x < 8; x++) {
            for (let y = 0; y < 8; y++) {
              if (game.isValidMove(x, y, game.currentPlayer)) {
                return false;
              }
            }
          }
          return true;
        }

        canvas.addEventListener('click', (e) => {
          if (isGameOver(game)) {
            return;
          }

          const rect = canvas.getBoundingClientRect();
          const x = Math.floor((e.clientX - rect.left) / cellSize);
          const y = Math.floor((e.clientY - rect.top) / cellSize);

          if (game.makeMove(x, y, game.currentPlayer)) {
            drawBoard();
            game.switchPlayer();

            if (!isGameOver(game)) {
              const aiMove = chooseBestMove(game);
              if (aiMove !== null) {
                game.makeMove(aiMove.x, aiMove.y, game.currentPlayer);
                drawBoard();
              }
            }
            game.switchPlayer();
          }
        });

        // ... drawBoard() function ...

        drawBoard();
    </script>
</body>
</html>

```

![image](https://user-images.githubusercontent.com/11986901/226082669-adb6ec1b-c046-43f0-98a9-4f4b5940bab7.png)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Othello Game</title>
    <style>
        canvas {
            display: block;
            margin: 0 auto;
            border: 1px solid black;
        }
    </style>
</head>
<body>
    <canvas id="board" width="320" height="320"></canvas>
    <script>
        // 以下に Othello クラスのコードを追加
        class Othello {
          // ... Othello クラスのコードをここに貼り付け ...

        const canvas = document.getElementById('board');
        const ctx = canvas.getContext('2d');
        const cellSize = 40;
        const game = new Othello();

        function chooseBestMove(game) {
          // ... chooseBestMove function ...

        }

        function isGameOver(game) {
          // ... isGameOver function ...

        }

        canvas.addEventListener('click', (e) => {
          // ... click event listener ...

          if (game.makeMove(x, y, game.currentPlayer)) {
            drawBoard();
            game.switchPlayer();

            if (!isGameOver(game)) {
              const aiMove = chooseBestMove(game);
              if (aiMove !== null) {
                game.makeMove(aiMove.x, aiMove.y, game.currentPlayer);
                drawBoard();
              }
            }
            game.switchPlayer();
          }
        });

        function drawBoard() {
          // ... drawBoard function ...

        }

        drawBoard();
    </script>
</body>
</html>

```

![image](https://user-images.githubusercontent.com/11986901/226083412-d6522391-dbd6-4ffb-a6b8-b5ae16d686bf.png)

![image](https://user-images.githubusercontent.com/11986901/226083415-e54233da-52b5-483f-a835-9471044fdbe8.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Othello Game</title>
    <style>
        canvas {
            display: block;
            margin: 0 auto;
            border: 1px solid black;
        }
    </style>
</head>
<body>
    <canvas id="board" width="320" height="320"></canvas>
    <script>
        // 以下に Othello クラスのコードを追加
        class Othello {
          // ... Othello クラスのコードをここに貼り付け ...

        const canvas = document.getElementById('board');
        const ctx = canvas.getContext('2d');
        const cellSize = 40;
        const game = new Othello();

        function chooseBestMove(game) {
          const validMoves = [];

          for (let x = 0; x < 8; x++) {
            for (let y = 0; y < 8; y++) {
              if (game.isValidMove(x, y, game.currentPlayer)) {
                validMoves.push({ x, y });
              }
            }
          }

          // Randomly select a valid move
          if (validMoves.length > 0) {
            const randomIndex = Math.floor(Math.random() * validMoves.length);
            return validMoves[randomIndex];
          }

          return null;
        }

        function isGameOver(game) {
          for (let x = 0; x < 8; x++) {
            for (let y = 0; y < 8; y++) {
              if (game.isValidMove(x, y, game.currentPlayer)) {
                return false;
              }
            }
          }
          return true;
        }

        canvas.addEventListener('click', (e) => {
          if (isGameOver(game)) {
            return;
          }

          const rect = canvas.getBoundingClientRect();
          const x = Math.floor((e.clientX - rect.left) / cellSize);
          const y = Math.floor((e.clientY - rect.top) / cellSize);

          if (game.makeMove(x, y, game.currentPlayer)) {
            drawBoard();
            game.switchPlayer();

            if (!isGameOver(game)) {
              const aiMove = chooseBestMove(game);
              if (aiMove !== null) {
                game.makeMove(aiMove.x, aiMove.y, game.currentPlayer);
                drawBoard();
              }
            }
            game.switchPlayer();
          }
        });

        function drawBoard() {
            // Clear canvas and set background color to green
            ctx.fillStyle = "green";
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            // Draw stones
            for (let x = 0; x < 8; x++) {
                for (let y = 0; y < 8; y++) {
                    const cell = game.board[x][y];
                    if (cell !== null) {
                        ctx.fillStyle = cell;
                        ctx.beginPath();
                        ctx.arc(
                            x * cellSize + cellSize / 2,
                            y * cellSize + cellSize / 2,
                            cellSize * 0.4,
                            0,
                            2 * Math.PI
                        );
                        ctx.fill();
                    }
                }

```

![image](https://user-images.githubusercontent.com/11986901/226083425-a8dd982b-4f17-4239-997e-032b9b54ca23.png)

```html
            // Draw grid lines
            ctx.strokeStyle = 'black';
            for (let i = 0; i <= 8; i++) {
                ctx.beginPath();
                ctx.moveTo(i * cellSize, 0);
                ctx.lineTo(i * cellSize, canvas.height);
                ctx.stroke();

                ctx.beginPath();
                ctx.moveTo(0, i * cellSize);
                ctx.lineTo(canvas.width, i * cellSize);
                ctx.stroke();
            }
        }

        drawBoard();
    </script>
</body>
</html>

![image](https://user-images.githubusercontent.com/11986901/226083444-01313fb8-9752-456c-9dff-584e6fa25964.png)

あきた

```






