import React, { useState, useEffect } from 'react';
import './App.css';

const App = () => {
  const [grid, setGrid] = useState(Array(9).fill(null));
  const [isXTurn, setIsXTurn] = useState(true);
  const winner = getWinner(grid);

  useEffect(() => {
    if (!isXTurn && !winner) {
      const delay = setTimeout(() => {
        makeBotMove();
      }, 300);
      return () => clearTimeout(delay);
    }
  }, [isXTurn, grid, winner]);

  const handleClick = (idx) => {
    if (grid[idx] || winner || !isXTurn) return;
    const updated = [...grid];
    updated[idx] = 'X';
    setGrid(updated);
    setIsXTurn(false);
  };

  const makeBotMove = () => {
    const available = grid
      .map((val, i) => (val === null ? i : null))
      .filter((i) => i !== null);
    if (available.length === 0) return;
    const randomIdx = available[Math.floor(Math.random() * available.length)];
    const updated = [...grid];
    updated[randomIdx] = 'O';
    setGrid(updated);
    setIsXTurn(true);
  };

  const restart = () => {
    setGrid(Array(9).fill(null));
    setIsXTurn(true);
  };

  return (
    <div className="wrapper">
      <h1>🎮 Retro Caro</h1>
      <p className="info">
        {winner
          ? `🏆 Winner: ${winner}`
          : grid.every((x) => x !== null)
          ? '🤝 It\'s a draw!'
          : `⏳ Turn: ${isXTurn ? 'You (X)' : 'Bot (O)'}`}
      </p>
      <div className="grid">
        {grid.map((cell, idx) => (
          <div key={idx} className="cell" onClick={() => handleClick(idx)}>
            {cell}
          </div>
        ))}
      </div>
      <button className="restart" onClick={restart}>🔄 New Game</button>
    </div>
  );
};

const getWinner = (grid) => {
  const wins = [
    [0,1,2],[3,4,5],[6,7,8], // Rows
    [0,3,6],[1,4,7],[2,5,8], // Columns
    [0,4,8],[2,4,6]          // Diagonals
  ];
  for (const [a, b, c] of wins) {
    if (grid[a] && grid[a] === grid[b] && grid[a] === grid[c]) {
      return grid[a];
    }
  }
  return null;
};

export default App;
