import React, { useState, useEffect } from 'react';
import './App.css';

const BOARD_SIZE = 3;

function App() {
  const [board, setBoard] = useState(Array(9).fill(null));
  const [xIsNext, setXIsNext] = useState(true);
  const winner = calculateWinner(board);

  useEffect(() => {
    if (!xIsNext && !winner) {
      const timer = setTimeout(() => {
        makeBotMove();
      }, 400);
      return () => clearTimeout(timer);
    }
  }, [xIsNext, board, winner]);

  const handleClick = (i) => {
    if (board[i] || winner || !xIsNext) return;

    const next = board.slice();
    next[i] = 'X';
    setBoard(next);
    setXIsNext(false);
  };

  const makeBotMove = () => {
    const move = getBestMove(board);
    if (move !== null) {
      const next = board.slice();
      next[move] = 'O';
      setBoard(next);
      setXIsNext(true);
    }
  };

  const getBestMove = (board) => {
    // Block if X can win
    for (let i = 0; i < board.length; i++) {
      if (!board[i]) {
        const test = board.slice();
        test[i] = 'X';
        if (calculateWinner(test) === 'X') return i;
      }
    }
    // Random fallback
    const empty = board.map((v, i) => (v ? null : i)).filter((i) => i !== null);
    return empty.length > 0 ? empty[Math.floor(Math.random() * empty.length)] : null;
  };

  const renderSquare = (i) => (
    <div className={`cell ${board[i]}`} onClick={() => handleClick(i)} key={i}>
      {board[i]}
    </div>
  );

  const resetGame = () => {
    setBoard(Array(9).fill(null));
    setXIsNext(true);
  };

  return (
    <div className="container">
      <h2 className="title">React Caro Bot</h2>
      <div className="status">
        {winner
          ? `Winner: ${winner}`
          : board.every((x) => x !== null)
          ? 'It’s a draw!'
          : `Next: ${xIsNext ? 'You (X)' : 'Bot (O)'}`}
      </div>
      <div className="grid">
        {board.map((_, i) => renderSquare(i))}
      </div>
      <button className="reset" onClick={resetGame}>Restart</button>
    </div>
  );
}

function calculateWinner(b) {
  const lines = [
    [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
    [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
    [0, 4, 8], [2, 4, 6]             // Diagonals
  ];
  for (let [a, b2, c] of lines) {
    if (b[a] && b[a] === b[b2] && b[a] === b[c]) {
      return b[a];
    }
  }
  return null;
}

export default App;
