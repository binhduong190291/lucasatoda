import React, { useState, useEffect } from 'react';
import './App.css';

const WINNING_COMBINATIONS = [
  [0, 1, 2], [3, 4, 5], [6, 7, 8], // rows
  [0, 3, 6], [1, 4, 7], [2, 5, 8], // columns
  [0, 4, 8], [2, 4, 6]             // diagonals
];

const App = () => {
  const [board, setBoard] = useState(Array(9).fill(null));
  const [isXNext, setIsXNext] = useState(true);
  const [winner, setWinner] = useState(null);
  const [gameOver, setGameOver] = useState(false);
  const [moveHistory, setMoveHistory] = useState([]);
  const [botThinking, setBotThinking] = useState(false);

  useEffect(() => {
    if (!isXNext && !gameOver) {
      setBotThinking(true);
      setTimeout(() => {
        botMove();
        setBotThinking(false);
      }, 1000);
    }
  }, [isXNext, board, gameOver]);

  const handleClick = (index) => {
    if (board[index] || gameOver) return;

    const newBoard = [...board];
    newBoard[index] = 'X';
    setBoard(newBoard);
    setMoveHistory([...moveHistory, index]);
    setIsXNext(false);

    const winner = calculateWinner(newBoard);
    if (winner) {
      setWinner(winner);
      setGameOver(true);
    }
  };

  const botMove = () => {
    const bestMove = getBestMove(board);
    const newBoard = [...board];
    newBoard[bestMove] = 'O';
    setBoard(newBoard);
    setMoveHistory([...moveHistory, bestMove]);
    setIsXNext(true);

    const winner = calculateWinner(newBoard);
    if (winner) {
      setWinner(winner);
      setGameOver(true);
    }
  };

  const getBestMove = (board) => {
    const availableSpaces = board
      .map((val, idx) => (val === null ? idx : null))
      .filter((idx) => idx !== null);

    return availableSpaces[Math.floor(Math.random() * availableSpaces.length)];
  };

  const calculateWinner = (board) => {
    for (let [a, b, c] of WINNING_COMBINATIONS) {
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
    setMoveHistory([]);
  };

  return (
    <div className="game-container">
      <h1>Tic-Tac-Toe</h1>
      <div className="status">
        {winner
          ? `🎉 ${winner} wins!`
          : gameOver
          ? '😞 It\'s a draw!'
          : `Next: ${isXNext ? 'You (X)' : 'Bot (O)'}`}
      </div>
      <div className="board">
        {board.map((cell, index) => (
          <div
            key={index}
            className={`square ${cell} ${gameOver && winner === cell ? 'winner' : ''}`}
            onClick={() => handleClick(index)}
          >
            {cell}
          </div>
        ))}
      </div>
      <button className="reset-btn" onClick={resetGame}>Start New Game</button>
      {botThinking && <div className="bot-thinking">Bot is thinking...</div>}
    </div>
  );
};

export default App;
