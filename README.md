# How to deploy app with Fleek
## Step 1: Creat a Simple Caro Game
### If you don't want to creat a new, you can start at Step 3!

```
npx create-react-app caro-on-fleek && cd caro-on-fleek
```
```
npm start
```
### Now press `Ctrl + C` to stop
### You can ask ChatGPT to edit files `App.js` & `App.css` to become a new game!
### Edit `App.js` file:
```
rm src/App.js && nano src/App.js
```
#### Paste code below to `App.js`

```
import React, { useState, useEffect } from 'react';
import './App.css';

const BOARD_SIZE = 3;

function App() {
  const [squares, setSquares] = useState(Array(BOARD_SIZE * BOARD_SIZE).fill(null));
  const [xIsNext, setXIsNext] = useState(true); // true = Player's turn
  const winner = calculateWinner(squares);

  // Khi d?n lu?t máy, t? d?ng choi
  useEffect(() => {
    if (!xIsNext && !winner) {
      const timer = setTimeout(() => {
        makeRandomMove();
      }, 500); // Delay nh? cho t? nhiên
      return () => clearTimeout(timer);
    }
  }, [xIsNext, squares, winner]);

  const handleClick = (index) => {
    if (squares[index] || winner || !xIsNext) return;

    const nextSquares = squares.slice();
    nextSquares[index] = 'X';
    setSquares(nextSquares);
    setXIsNext(false);
  };

  const makeRandomMove = () => {
    const emptyIndices = squares
      .map((val, idx) => (val === null ? idx : null))
      .filter((val) => val !== null);

    if (emptyIndices.length === 0) return;

    const randomIndex = emptyIndices[Math.floor(Math.random() * emptyIndices.length)];
    const nextSquares = squares.slice();
    nextSquares[randomIndex] = 'O';
    setSquares(nextSquares);
    setXIsNext(true);
  };

  const renderSquare = (index) => (
    <div
      key={index}
      className={`square ${squares[index] === 'X' ? 'x' : squares[index] === 'O' ? 'o' : ''}`}
      onClick={() => handleClick(index)}
    >
      {squares[index]}
    </div>
  );

  const renderBoard = () => {
    const board = [];
    for (let row = 0; row < BOARD_SIZE; row++) {
      const cols = [];
      for (let col = 0; col < BOARD_SIZE; col++) {
        const index = row * BOARD_SIZE + col;
        cols.push(renderSquare(index));
      }
      board.push(
        <div className="board-row" key={row}>
          {cols}
        </div>
      );
    }
    return board;
  };

  const handleReset = () => {
    setSquares(Array(BOARD_SIZE * BOARD_SIZE).fill(null));
    setXIsNext(true);
  };

  return (
    <div className="game">
      <div className="status">
        {winner
          ? `Winner: ${winner}`
          : squares.every((s) => s !== null)
          ? 'Draw!'
          : `Turn: ${xIsNext ? 'You (X)' : 'Computer (O)'}`}
      </div>
      {renderBoard()}
      <button className="reset" onClick={handleReset}>Reset Game</button>
    </div>
  );
}

// Function to check for a winner
function calculateWinner(squares) {
  const lines = [
    [0, 1, 2], // rows
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6], // columns
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8], // diagonals
    [2, 4, 6],
  ];
  for (let [a, b, c] of lines) {
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}

export default App;
```

### Edit `App.css` file:
```
rm src/App.css && nano src/App.css
```
#### Paste code below to `App.css`

```
.game {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 50px;
  font-family: sans-serif;
}

.board-row {
  display: flex;
}

.square {
  width: 60px;
  height: 60px;
  font-size: 30px;
  background-color: #fff;
  border: 1px solid #ddd;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.x {
  color: red;
}

.o {
  color: green;
}

.status {
  margin-bottom: 10px;
  font-size: 18px;
  font-weight: bold;
}

.reset {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #ddd;
  border: none;
  cursor: pointer;
  font-size: 16px;
}
```
## Step 2: Push your app to Github
### Create a New Repository on GitHub
Creat with name `caro-on-fleek`

### Initialize Git in Your Project (On your VPS)
```
git init
```
```
git add .
```
```
git commit -m "Initial commit"
```
### Connect Your GitHub Repository to Your Local Project
```
git remote add origin https://github.com/USERNAME/caro-on-fleek.git
```
### Push Your Code to GitHub
#### Check branch
```
git branch
```
If you see `master`, use this:
```
git branch -m master main
```
#### Then run
```
git push -u origin main
```
#### Enter User name & PAT token for pass
### If you use codespace run
```
git remote set-url origin https://GITHUB_USERNAME:YOUR_TOKEN@github.com/GITHUB_USERNAME/caro-on-fleek.git
```
#### Then run
```
git push -u origin main
```

## Step 3: Creat a fleek account
### Singup with your Email ✅
### Install Fleek CLI
```
npm install -g @fleek-platform/cli
```
```
fleek version
```
### Login
```
fleek login
```
Confirm on your Fleek account !
`✅ Success! You are now logged in to the Fleek Platform.`

### Fork my repository
My app: https://github.com/ToanBm/caro-on-fleek
### Clone the repository
```
git clone https://github.com/YOUR-GITHUB-USER/caro-on-fleek.git
```
```
cd caro-on-fleek
npm i
npm run build
```

## Step 4: Deploy app on Fleek
### Visit Fleek dashboard > Add new > Deploy my site
### Connect github > choose repo > deploy

## ********************* Thank you ***********************


