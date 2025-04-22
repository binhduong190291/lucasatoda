import React, { useState, useEffect } from 'react';
import './App.css';

const App = () => {
  const [cells, setCells] = useState(Array(9).fill(null));
  const [isXTurn, setIsXTurn] = useState(true);
  const winner = getWinner(cells);

  useEffect(() => {
    if (!isXTurn && !winner) {
      const botDelay = setTimeout(() => {
        botPlay();
      }, 500);
      return () => clearTimeout(botDelay);
    }
  }, [isXTurn, cells, winner]);

  const handleClick = (i) => {
    if (cells[i] || winner || !isXTurn) return;
    const next = [...cells];
    next[i] = 'X';
    setCells(next);
    setIsXTurn(false);
  };

  const botPlay = () => {
    const emptyIndices = cells
      .map((val, idx) => (val === null ? idx : null))
      .filter((v) => v !== null);

    if (emptyIndices.length === 0) return;
    const choice = emptyIndices[Math.floor(Math.random() * emptyIndices.length)];

    const newBoard = [...cells];
    newBoard[choice] = 'O';
    setCells(newBoard);
    setIsXTurn(true);
  };

  const reset = () => {
    setCells(Array(9).fill(null));
    setIsXTurn(true);
  };

  return (
    <div className="main">
      <h2>Tic Tac Toe</h2>
      <p className="status">
        {winner
          ? `🏆 Winner: ${winner}`
          : cells.every((c) => c !== null)
          ? '🤝 Draw!'
          : `👉 ${isXTurn ? 'Your turn (X)' : 'Bot thinking...'}`}
      </p>
      <div className="board">
        {cells.map((val, i) => (
          <div key={i} className="cell" onClick={() => handleClick(i)}>
            {val}
          </div>
        ))}
      </div>
      <button onClick={reset} className="reset-btn">Restart</button>
    </div>
  );
};

const getWinner = (cells) => {
  const winLines = [
    [0,1,2],[3,4,5],[6,7,8],
    [0,3,6],[1,4,7],[2,5,8],
    [0,4,8],[2,4,6]
  ];
  for (let [a,b,c] of winLines) {
    if (cells[a] && cells[a] === cells[b] && cells[a] === cells[c]) {
      return cells[a];
    }
  }
  return null;
};

export default App;
