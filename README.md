import React, { useState, useEffect } from 'react';
import './App.css';

function App() {
  const [board, setBoard] = useState(Array(9).fill(null));
  const [xTurn, setXTurn] = useState(true);
  const winner = checkWinner(board);

  useEffect(() => {
    if (!xTurn && !winner) {
      const delay = setTimeout(() => botMove(), 400);
      return () => clearTimeout(delay);
    }
  }, [xTurn, board, winner]);

  const handleClick = (i) => {
    if (board[i] || !xTurn || winner) return;
    const next = [...board];
    next[i] = 'X';
    setBoard(next);
    setXTurn(false);
  };

  const botMove = () => {
    let move = chooseSmartMove(board);
    if (move !== null) {
      const next = [...board];
      next[move] = 'O';
      setBoard(next);
      setXTurn(true);
    }
  };

  const chooseSmartMove = (b) => {
    // Priority: center > corners > random
    if (!b[4]) return 4;
    const corners = [0, 2, 6, 8].filter(i => !b[i]);
    if (corners.length > 0) return corners[Math.floor(Math.random() * corners.length)];
    const empty = b.map((val, i) => val === null ? i : null).filter(i => i !== null);
    return empty.length > 0 ? empty[Math.floor(Math.random() * empty.length)] : null;
  };

  const reset = () => {
    setBoard(Array(9).fill(null));
    setXTurn(true);
  };

  return (
    <div className="wrapper">
      <h1>Caro Game: X vs Bot</h1>
      <div className="info">
        {winner
          ? `🏆 Winner: ${winner}`
          : board.every(c => c) ? '🤝 It’s a draw!' : `👉 ${xTurn ? 'Your Turn (X)' : 'Bot is thinking...'}`}
      </div>
      <div className="board">
        {board.map((val, i) => (
          <div key={i} className="cell" onClick={() => handleClick(i)}>
            {val}
          </div>
        ))}
      </div>
      <button className="restart" onClick={reset}>Play Again</button>
    </div>
  );
}

function checkWinner(b) {
  const lines = [
    [0,1,2], [3,4,5], [6,7,8],
    [0,3,6], [1,4,7], [2,5,8],
    [0,4,8], [2,4,6]
  ];
  for (let [a,b2,c] of lines) {
    if (b[a] && b[a] === b[b2] && b[a] === b[c]) return b[a];
  }
  return null;
}

export default App;
