import React, { useState, useEffect } from 'react';
import './App.css';

const WINNING_COMBINATIONS = [
  [0, 1, 2], [3, 4, 5], [6, 7, 8], // rows
  [0, 3, 6], [1, 4, 7], [2, 5, 8], // columns
  [0, 4, 8], [2, 4, 6] // diagonals
];

const App = () => {
  const [board, setBoard] = useState(Array(9).fill(null));
  const [isXNext, setIsXNext] = useState(true);
  const [darkMode, setDarkMode] = useState(false);
  const winner = calculateWinner(board);

  useEffect(() => {
    if (!isXNext && !winner) {
      const timeout = setTimeout(() => {
        botMove();
      }, 500);
      return () => clearTimeout(timeout);
    }
  }, [isXNext, board, winner]);

  const handleClick = (index) => {
    if (board[index] || winner || !isXNext) return;

    const newBoard = [...board];
    newBoard[index] = 'X';
    setBoard(newBoard);
    setIsXNext(false);
  };

  const botMove = () => {
    const availableSpaces = board
      .map((val, idx) => (val === null ? idx : null))
      .filter((idx) => idx !== null);

    if (availableSpaces.length === 0) return;

    const move = availableSpaces[Math.floor(Math.random() * availableSpaces.length)];
    const newBoard = [...board];
    newBoard[move] = 'O';
    setBoard(newBoard);
    setIsXNext(true);
  };

  const resetGame = () => {
    setBoard(Array(9).fill(null));
    setIsXNext(true);
  };

  const toggleDarkMode = () => setDarkMode(!darkMode);

  return (
    <div className={`game-container ${darkMode ? 'dark' : ''}`}>
      <h1>Tic-Tac-Toe</h1>
      <div className="status">
        {winner
          ? `🏆 Winner: ${winner}`
          : board.every((cell) => cell !== null)
          ? '🤝 It\'s a draw!'
          : `Next: ${isXNext ? 'You (X)' : 'Bot (O)'}`}
      </div>
      <div className="board">
        {board.map((cell, index) => (
          <div
            key={index}
            className={`square ${cell}`}
            onClick={() => handleClick(index)}
          >
            {cell}
          </div>
        ))}
      </div>
      <button className="reset-btn" onClick={resetGame}>Start New Game</button>
      <button className="dark-mode-btn" onClick={toggleDarkMode}>
        {darkMode ? 'Light Mode' : 'Dark Mode'}
      </button>
    </div>
  );
};

function calculateWinner(board) {
  for (let [a, b, c] of WINNING_COMBINATIONS) {
    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a];
    }
  }
  return null;
}

export default App;
