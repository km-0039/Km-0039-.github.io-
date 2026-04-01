# Km-0039-.github.io-
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<title>学習型AI</title>
<style>
  body { font-family: sans-serif; margin: 20px; }
  #chat { border: 1px solid #ccc; padding: 10px; height: 300px; overflow-y: auto; }
  input { width: 80%; padding: 5px; }
  button { padding: 5px; }
  .user { color: blue; }
  .ai { color: green; }
</style>
</head>
<body>

<h2>学習型AI</h2>
<div id="chat"></div>
<input id="userInput" placeholder="メッセージを入力" />
<button onclick="sendMessage()">送信</button>
<button onclick="clearMemory()">記憶リセット</button>

<script>
let memory = JSON.parse(localStorage.getItem('memory') || '[]');

function displayMessage(who, text) {
  const chat = document.getElementById('chat');
  const div = document.createElement('div');
  div.className = who;
  div.textContent = `${who === 'user' ? '俺' : 'AI'}: ${text}`;
  chat.appendChild(div);
  chat.scrollTop = chat.scrollHeight;
}

function learn(text) {
  const words = text.split(/\s+/);
  memory.push(...words);
  localStorage.setItem('memory', JSON.stringify(memory));
}

function generateReply() {
  if (memory.length === 0) return '...';
  const count = Math.min(memory.length, Math.floor(Math.random() * 4) + 2);
  const words = [];
  for (let i = 0; i < count; i++) {
    words.push(memory[Math.floor(Math.random() * memory.length)]);
  }
  return words.join('');
}

function sendMessage() {
  const input = document.getElementById('userInput');
  const text = input.value.trim();
  if (!text) return;
  displayMessage('user', text);
  learn(text);
  const reply = generateReply();
  displayMessage('ai', reply);
  input.value = '';
}

function clearMemory() {
  memory = [];
  localStorage.removeItem('memory');
  displayMessage('ai', '記憶をリセットしました');
}
</script>

</body>
</html>
