<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<title>CHILL-U Assistant</title>
<style>
  body {
    font-family: 'Poppins', sans-serif;
    margin: 0;
  }

  /* Tombol melayang */
  #chatbot-toggle {
    position: fixed;
    bottom: 20px;
    right: 20px;
    background: #4a90e2;
    color: white;
    width: 60px;
    height: 60px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 30px;
    cursor: pointer;
    box-shadow: 0 4px 8px rgba(0,0,0,0.3);
    z-index: 9999;
    transition: transform 0.3s;
  }
  #chatbot-toggle:hover {
    transform: scale(1.1);
  }

  /* Kotak chatbot */
  #chatbot-container {
    position: fixed;
    bottom: 90px;
    right: 20px;
    width: 360px;
    height: 500px;
    background: #f8f8f8;
    color: #333;
    border-radius: 12px;
    box-shadow: 0 0 10px rgba(0,0,0,0.25);
    display: none;
    flex-direction: column;
    overflow: hidden;
    z-index: 9998;
  }

  /* Header chatbot */
  #chatbot-header {
    background: #4a90e2;
    color: white;
    padding: 12px;
    text-align: center;
    font-weight: 600;
  }

  /* Area chat */
  #chatbot-messages {
    flex: 1;
    padding: 10px;
    overflow-y: auto;
    background: #fff;
  }

  .msg-user, .msg-bot {
    margin: 8px 0;
    padding: 10px;
    border-radius: 10px;
    max-width: 80%;
  }

  .msg-user {
    background: #dce6ff;
    margin-left: auto;
  }

  .msg-bot {
    background: #f1f1f1;
  }

  /* Input area */
  #chatbot-input-area {
    display: flex;
    border-top: 1px solid #ccc;
  }

  #chatbot-input {
    flex: 1;
    padding: 10px;
    border: none;
    outline: none;
  }

  #chatbot-send {
    background: #4a90e2;
    color: white;
    border: none;
    padding: 10px 16px;
    cursor: pointer;
  }
</style>
</head>
<body>

<!-- Tombol melayang -->
<div id="chatbot-toggle">🤖</div>

<!-- Kotak chatbot -->
<div id="chatbot-container">
  <div id="chatbot-header">CHILL-U Assistant</div>
  <div id="chatbot-messages"></div>
  <div id="chatbot-input-area">
    <input id="chatbot-input" type="text" placeholder="Tanyakan apa saja..." />
    <button id="chatbot-send">Kirim</button>
  </div>
</div>

<script>
  const toggleBtn = document.getElementById('chatbot-toggle');
  const container = document.getElementById('chatbot-container');
  const sendBtn = document.getElementById('chatbot-send');
  const input = document.getElementById('chatbot-input');
  const messages = document.getElementById('chatbot-messages');

  // Buka/tutup chatbot
  toggleBtn.onclick = () => {
    container.style.display = container.style.display === 'flex' ? 'none' : 'flex';
  };

  // Kirim pesan
  sendBtn.onclick = sendMessage;
  input.addEventListener('keypress', e => {
    if (e.key === 'Enter') sendMessage();
  });

  function sendMessage() {
    const text = input.value.trim();
    if (!text) return;
    appendMessage('user', text);
    input.value = '';

    setTimeout(() => {
      appendMessage('bot', getBotReply(text));
      messages.scrollTop = messages.scrollHeight;
    }, 400);
  }

  function appendMessage(sender, text) {
    const div = document.createElement('div');
    div.className = sender === 'user' ? 'msg-user' : 'msg-bot';
    div.textContent = text;
    messages.appendChild(div);
  }

  // Jawaban dasar
  function getBotReply(q) {
    const t = q.toLowerCase();
    if (t.includes('atom')) return 'Atom adalah partikel terkecil penyusun materi.';
    if (t.includes('molekul')) return 'Molekul adalah gabungan dua atau lebih atom.';
    if (t.includes('halo') || t.includes('hai')) return 'Halo! Aku CHILL-U, siap bantu kamu belajar kimia! ⚛️';
    if (t.includes('terima kasih')) return 'Sama-sama! 😄';
    return 'Wah, menarik! Tapi aku belum punya jawaban detailnya untuk itu.';
  }
</script>

</body>
</html>
