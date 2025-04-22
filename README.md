import React, { useState, useEffect } from 'react';
import './App.css';

const App = () => {
  const [board, setBoard] = useState(Array(9).fill(null));
  const [isXNext, setIsXNext] = useState(true);
  const [winner, setWinner] = useState(null);
  const [gameOver, setGameOver] = useState(false);

  useEffect(() => {
    if (!isXNext && !gameOver) {
      const timeoutId = setTimeout(() => {
        botMove();
      }, 800);
      return () => clearTimeout(timeoutId);
    }
  }, [isXNext, board, gameOver]);

  const handleClick = (index) => {
    if (board[index] || gameOver) return;

    const newBoard = [...board];
    newBoard[index] = isXNext ? 'X' : 'O';
    setBoard(newBoard);
    setIsXNext(!isXNext);

    const winner = calculateWinner(newBoard);
    if (winner) {
      setWinner(winner);
      setGameOver(true);
    } else if (newBoard.every(cell => cell !== null)) {
      setGameOver(true);
    }
  };

  const botMove = () => {
    const emptyCells = board.map((val, idx) => val === null ? idx : null).filter(idx => idx !== null);
    const randomMove = emptyCells[Math.floor(Math.random() * emptyCells.length)];
    handleClick(randomMove);
  };

  const calculateWinner = (board) => {
    const lines = [
      [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
      [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
      [0, 4, 8], [2, 4, 6]             // Diagonals
    ];
    for (let [a, b, c] of lines) {
      if (board[a] && board[a] === board[b] && board[a] === board[c]) {
        return board[a];
      }
    }
    return null;
  };

  const resetGame = () => {
    setBoard(Array(9).fill(null));
    setIsXNext(true);
    setWinner(null);
    setGameOver(false);
  };

  return (
    <div className="game-container">
      <h1>Tic-Tac-Toe</h1>
      <div className="status">
        {winner
          ? `🎉 ${winner} wins!`
          : gameOver
          ? 'It\'s a draw!'
          : `Next: ${isXNext ? 'Player (X)' : 'Bot (O)'}`}
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
    </div>
  );
};

export default App;
