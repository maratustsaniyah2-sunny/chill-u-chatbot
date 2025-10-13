[chatbot.html](https://github.com/user-attachments/files/22890966/chatbot.html)
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<title>🤖 CHILL-U Universe Chat</title>
<style>
  body {
    margin: 0;
    font-family: 'Segoe UI', sans-serif;
  }

  /* === Tombol robot biru melayang === */
  #chatToggle {
    position: fixed;
    bottom: 25px;
    right: 25px;
    width: 65px;
    height: 65px;
    border-radius: 50%;
    background: radial-gradient(circle at 30% 30%, #6b8bff, #2f4cff);
    border: none;
    cursor: pointer;
    font-size: 32px;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 0 20px rgba(91,124,255,0.8);
    animation: float 3s ease-in-out infinite, pulse 2s ease-in-out infinite;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    z-index: 1000;
  }

  #chatToggle:hover {
    transform: scale(1.15);
    box-shadow: 0 0 25px rgba(91,124,255,1);
  }

  /* === Animasi melayang dan berdenyut === */
  @keyframes float {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-6px); }
  }
  @keyframes pulse {
    0%, 100% { box-shadow: 0 0 15px rgba(91,124,255,0.8); }
    50% { box-shadow: 0 0 30px rgba(91,124,255,1); }
  }

  /* === Balon teks ajakan === */
  #chatHint {
    position: fixed;
    bottom: 105px;
    right: 95px;
    background: white;
    color: #2f4cff;
    padding: 10px 14px;
    border-radius: 12px;
    box-shadow: 0 4px 14px rgba(0,0,0,0.2);
    font-size: 14px;
    font-weight: 500;
    opacity: 0;
    transform: translateY(10px);
    animation: hintPop 4s ease-in-out infinite;
    z-index: 999;
  }

  @keyframes hintPop {
    0%, 70%, 100% { opacity: 0; transform: translateY(10px); }
    10%, 40% { opacity: 1; transform: translateY(0); }
  }

  /* === Kotak chat === */
  .chat-box {
    position: fixed;
    bottom: 95px;
    right: 25px;
    width: 340px;
    height: 460px;
    display: none;
    flex-direction: column;
    background: linear-gradient(145deg, #f9f9f9, #e5e5e5);
    border-radius: 16px;
    box-shadow: 0 10px 25px rgba(0,0,0,0.3);
    overflow: hidden;
    z-index: 999;
    animation: slideUp 0.4s ease;
  }

  @keyframes slideUp {
    from { transform: translateY(30px); opacity: 0; }
    to { transform: translateY(0); opacity: 1; }
  }

  .chat-header {
    background: #5b7cff;
    color: white;
    text-align: center;
    padding: 12px;
    font-weight: 600;
    letter-spacing: 0.5px;
  }

  .messages {
    flex: 1;
    padding: 14px;
    overflow-y: auto;
    background: #ffffff;
    color: #333;
    font-size: 14px;
  }

  .user, .bot {
    padding: 10px;
    margin: 8px 0;
    border-radius: 10px;
    line-height: 1.4;
  }

  .user {
    background: #dbe2ff;
    text-align: right;
    color: #1e2a78;
  }

  .bot {
    background: #f0f0f0;
    color: #333;
  }

  .input-area {
    display: flex;
    gap: 8px;
    padding: 10px;
    background: #efefef;
    border-top: 1px solid #ddd;
  }

  input {
    flex: 1;
    padding: 8px 10px;
    border-radius: 8px;
    border: 1px solid #ccc;
    outline: none;
    font-size: 14px;
  }

  button#sendBtn {
    padding: 8px 14px;
    border: none;
    border-radius: 8px;
    background: #5b7cff;
    color: white;
    cursor: pointer;
    font-weight: 600;
  }

  button#sendBtn:hover {
    background: #6e8fff;
  }
</style>
</head>
<body>

<!-- Balon teks ajakan -->
<div id="chatHint">Butuh bantuan? Tanya CHILL-U 🤖</div>

<!-- Tombol ikon robot -->
<button id="chatToggle">🤖</button>

<!-- Kotak chat -->
<div class="chat-box" id="chatBox">
  <div class="chat-header">CHILL-U Asisten Kimia</div>
  <div class="messages" id="chatMessages"></div>
  <div class="input-area">
    <input type="text" id="chatInput" placeholder="Tanyakan tentang kimia...">
    <button id="sendBtn">Kirim</button>
  </div>
</div>

<script>
  const chatBox = document.getElementById("chatBox");
  const chatToggle = document.getElementById("chatToggle");
  const chatHint = document.getElementById("chatHint");
  const chatMessages = document.getElementById("chatMessages");
  const chatInput = document.getElementById("chatInput");
  const sendBtn = document.getElementById("sendBtn");

  // Toggle muncul/tutup chat
  chatToggle.onclick = () => {
    const visible = chatBox.style.display === "flex";
    chatBox.style.display = visible ? "none" : "flex";
    chatHint.style.display = visible ? "block" : "none";
  };

  // Kirim pesan
  function sendMessage() {
    const userText = chatInput.value.trim();
    if (!userText) return;
    appendMessage("user", userText);
    chatInput.value = "";

    setTimeout(async () => {
      const botReply = await getBotReply(userText);
      appendMessage("bot", botReply);
      chatMessages.scrollTop = chatMessages.scrollHeight;
    }, 400);
  }

  // Tampilkan pesan
  function appendMessage(sender, text) {
    const msg = document.createElement("div");
    msg.className = sender;
    msg.textContent = text;
    chatMessages.appendChild(msg);
  }

  // Balasan bot sederhana
  async function getBotReply(question) {
    const q = question.toLowerCase();
    if (q.includes("atom")) return "Atom adalah partikel terkecil penyusun materi, terdiri dari proton, neutron, dan elektron.";
    if (q.includes("molekul")) return "Molekul adalah gabungan dua atau lebih atom yang terikat secara kimia.";
    if (q.includes("asam") && q.includes("basa")) return "Asam menghasilkan ion H⁺ dan basa menghasilkan ion OH⁻ dalam larutan.";
    if (q.includes("reaksi kimia")) return "Reaksi kimia melibatkan perubahan susunan atom untuk membentuk zat baru.";
    if (q.includes("katalis")) return "Katalis mempercepat reaksi tanpa ikut bereaksi secara permanen.";
    if (q.includes("larutan")) return "Larutan adalah campuran homogen antara zat pelarut dan zat terlarut.";
    if (q.includes("hidrokarbon")) return "Hidrokarbon terdiri dari unsur karbon (C) dan hidrogen (H). Contohnya metana (CH₄).";
    if (q.includes("halo") || q.includes("hai")) return "Halo! Aku CHILL-U, siap bantu kamu belajar kimia! 😊";
    if (q.includes("terima kasih")) return "Sama-sama! Senang bisa membantu 😄";
    return "Hmm... menarik! Tapi aku belum punya jawaban detail untuk itu. Coba tanyakan hal tentang atom, reaksi, atau katalis 😉";
  }

  sendBtn.onclick = sendMessage;
  chatInput.addEventListener("keypress", e => {
    if (e.key === "Enter") sendMessage();
  });
</script>

</body>
</html>
