<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Korean Speech Tutor</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      margin: 0;
      padding: 0;
      background-color: #f9f9f9;
    }
    h1 {
      color: #333;
      margin: 20px;
    }
    #mic-button {
      cursor: pointer;
      width: 150px;
      height: 150px;
      background-size: cover;
      background-position: center;
      border: none;
      border-radius: 50%;
      background-color: #f0f0f0;
      background-image: url('https://cdn.shopify.com/s/files/1/0674/6747/7282/files/4_67840d70-6c42-465e-95a7-11d01bbc5c03.png?v=1730790629');
      transition: transform 0.1s;
    }
    #mic-button.active {
      transform: scale(0.9);
      background-image: url('https://cdn.shopify.com/s/files/1/0674/6747/7282/files/3_290befee-9dbc-4c8c-83a7-1cfbda29bac0.png?v=1730790629');
    }
    #transcription, #response {
      font-size: 1.1rem;
      color: #333;
      margin: 15px auto;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 5px;
      background-color: #f0f9ff;
      width: 90%;
      max-width: 600px;
      text-align: left;
    }
    .button-row {
      margin-top: 15px;
    }
    button {
      padding: 10px 15px;
      font-size: 1rem;
      margin: 5px;
      border: none;
      border-radius: 5px;
      color: white;
      background-color: #007bff;
      cursor: pointer;
    }
    button:hover {
      background-color: #0056b3;
    }
  </style>
</head>
<body>
  <h1>Korean Speech Tutor</h1>
  <p>Hold the button to speak in Korean, and let GPT correct your sentence!</p>

  <button id="mic-button"></button>
  <div id="transcription">Your sentence will appear here...</div>
  <div id="response">GPT's response will appear here...</div>
  <div class="button-row">
    <button onclick="clearContent()">Clear</button>
  </div>

  <script>
    const micButton = document.getElementById('mic-button');
    const transcriptionDiv = document.getElementById('transcription');
    const responseDiv = document.getElementById('response');

    let recognition;
    let isRecording = false;
    let transcriptText = "";

    // Check if browser supports Speech Recognition
    if ('webkitSpeechRecognition' in window || 'SpeechRecognition' in window) {
      recognition = new (window.webkitSpeechRecognition || window.SpeechRecognition)();
      recognition.lang = 'ko-KR'; // Korean language
      recognition.interimResults = false; // Final transcript only
      recognition.continuous = false;

      recognition.onresult = (event) => {
        transcriptText = event.results[0][0].transcript;
        transcriptionDiv.textContent = transcriptText;
        sendToBackend(transcriptText);
      };

      recognition.onend = () => {
        isRecording = false;
        micButton.classList.remove('active');
      };

      // Button listeners
      micButton.addEventListener('mousedown', startRecording);
      micButton.addEventListener('mouseup', stopRecording);
      micButton.addEventListener('touchstart', startRecording);
      micButton.addEventListener('touchend', stopRecording);

    } else {
      transcriptionDiv.textContent = "Speech Recognition is not supported in this browser.";
    }

    // Start Recording
    function startRecording(event) {
      event.preventDefault();
      if (!isRecording) {
        isRecording = true;
        micButton.classList.add('active');
        recognition.start();
      }
    }

    // Stop Recording
    function stopRecording(event) {
      event.preventDefault();
      if (isRecording) {
        isRecording = false;
        micButton.classList.remove('active');
        recognition.stop();
      }
    }

    // Send transcript to backend
    async function sendToBackend(transcript) {
      responseDiv.textContent = "Processing...";
      try {
        const response = await fetch('https://us-central1-kortut.cloudfunctions.net/api', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ text: transcript })
        });

        const result = await response.json();
        if (result.correct) {
          responseDiv.textContent = "잘했어요! 🎉";
        } else {
          responseDiv.textContent = `Correction: ${result.correction}\nReason: ${result.reason}`;
        }
      } catch (error) {
        console.error("Error:", error);
        responseDiv.textContent = "Something went wrong. Please try again.";
      }
    }

    // Clear Content
    function clearContent() {
      transcriptionDiv.textContent = "Your sentence will appear here...";
      responseDiv.textContent = "GPT's response will appear here...";
    }
  </script>
</body>
</html>
