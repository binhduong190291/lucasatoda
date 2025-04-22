import React, { useState, useEffect } from 'react';
import './App.css';

const App = () => {
  const [board, setBoard] = useState(Array(9).fill(null));
  const [isPlayerTurn, setIsPlayerTurn] = useState(true);
  const winner = checkWinner(board);

  useEffect(() => {
    if (!isPlayerTurn && !winner) {
      const botTimer = setTimeout(() => {
        botMove();
      }, 600);
      return () => clearTimeout(botTimer);
    }
  }, [isPlayerTurn, board, winner]);

  const handleClick = (index) => {
    if (board[index] || winner || !isPlayerTurn) return;
    const newBoard = [...board];
    newBoard[index] = 'X';
    setBoard(newBoard);
    setIsPlayerTurn(false);
  };

  const botMove = () => {
    const index = board.findIndex((val) => val === null);
    if (index !== -1) {
      const newBoard = [...board];
      newBoard[index] = 'O';
      setBoard(newBoard);
      setIsPlayerTurn(true);
    }
  };

  const restart = () => {
    setBoard(Array(9).fill(null));
    setIsPlayerTurn(true);
  };

  return (
    <div className="app">
      <h1>Caro vs Bot</h1>
      <p className="status">
        {winner
          ? `🎉 Winner: ${winner}`
          : board.every((v) => v !== null)
          ? "🤝 It's a draw!"
          : `🔄 Turn: ${isPlayerTurn ? 'Player (X)' : 'Bot (O)'}`}
      </p>
      <div className="board">
        {board.map((val, idx) => (
          <div
            key={idx}
            className={`cell ${val}`}
            onClick={() => handleClick(idx)}
          >
            {val}
          </div>
        ))}
      </div>
      <button onClick={restart} className="btn">Restart</button>
    </div>
  );
};

const checkWinner = (b) => {
  const lines = [
    [0,1,2],[3,4,5],[6,7,8],
    [0,3,6],[1,4,7],[2,5,8],
    [0,4,8],[2,4,6]
  ];
  for (let [a,b2,c] of lines) {
    if (b[a] && b[a] === b[b2] && b[a] === b[c]) {
      return b[a];
    }
  }
  return null;
};

export default App;
