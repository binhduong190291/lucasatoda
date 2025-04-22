* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Arial', sans-serif;
  background-color: #f4f4f4;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

.game-container {
  text-align: center;
  width: 90vw;
  max-width: 400px;
  padding: 20px;
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
}

h1 {
  font-size: 32px;
  margin-bottom: 20px;
}

.status {
  font-size: 18px;
  margin-bottom: 20px;
  font-weight: bold;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  margin-bottom: 20px;
}

.square {
  width: 100%;
  height: 100px;
  background-color: #fff;
  border: 2px solid #ddd;
  font-size: 50px;
  font-weight: bold;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: transform 0.3s ease;
}

.square:hover {
  transform: scale(1.1);
}

.square.X {
  color: #f56a79;
}

.square.O {
  color: #4da8da;
}

.reset-btn {
  padding: 10px 20px;
  background-color: #4da8da;
  color: white;
  border: none;
  font-size: 16px;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s;
  margin-top: 20px;
}

.reset-btn:hover {
  background-color: #4a90e2;
}
