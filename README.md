import React, { useState, useEffect } from 'react';
import './App.css';

const WIN_COMBOS = [
  [0,1,2],[3,4,5],[6,7,8], // rows
  [0,3,6],[1,4,7],[2,5,8], // cols
  [0,4,8],[2,4,6]          // diags
];

function App() {
  const [cells, setCells] = useState(Array(9).fill(null));
  const [isPlayerTurn, setIsPlayerTurn] = useState(true);
  const winner = getWinner(cells);

  useEffect(() => {
    if (!isPlayerTurn && !winner) {
      const timeout = setTimeout(botMove, 300);
      return () => clearTimeout(timeout);
    }
  }, [isPlayerTurn, cells, winner]);

  const handleClick = (index) => {
    if (cells[index] || !isPlayerTurn || winner) return;

    const updated = [...cells];
    updated[index] = 'X';
    setCells(updated);
    setIsPlayerTurn(false);
  };

  const botMove = () => {
    const move = smartBot(cells);
    if (move === null) return;
    const updated = [...cells];
    updated[move] = 'O';
    setCells(updated);
    setIsPlayerTurn(true);
  };

  const smartBot = (b) => {
    // Try to win
    for (let i = 0; i < 9; i++) {
      if (!b[i]) {
        const copy = [...b];
        copy[i] = 'O';
        if (getWinner(copy) === 'O') return i;
      }
    }
    // Try to block
    for (let i = 0; i < 9; i++) {
      if (!b[i]) {
        const copy = [...b];
        copy[i] = 'X';
        if (getWinner(copy) === 'X') return i;
      }
    }
    // Else pick center, corner, or any
    const choices = [4,0,2,6,8,1,3,5,7].filter(i => !b[i]);
    return choices.length ? choices[0] : null;
  };

  const reset = () => {
    setCells(Array(9).fill(null));
    setIsPlayerTurn(true);
  };

  return (
    <div className="main">
      <h2>🔷 Caro Game: X (You) vs O (Bot)</h2>
      <div className="status">
        {winner
          ? `🎉 ${winner} wins!`
          : cells.every(cell => cell) ? '😐 Draw!' : `Turn: ${isPlayerTurn ? 'You (X)' : 'Bot (O)'}`}
      </div>
      <div className="board">
        {cells.map((val, idx) => (
          <div key={idx} className="square" onClick={() => handleClick(idx)}>
            {val}
          </div>
        ))}
      </div>
      <button className="btn" onClick={reset}>🔁 Restart</button>
    </div>
  );
}

function getWinner(cells) {
  for (let [a, b, c] of WIN_COMBOS) {
    if (cells[a] && cells[a] === cells[b] && cells[a] === cells[c]) {
      return cells[a];
    }
  }
  return null;
}

export default App;
