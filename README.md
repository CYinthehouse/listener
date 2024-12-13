<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Korean Voice Correction</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      margin: 0;
      padding: 20px;
    }
    button {
      font-size: 18px;
      padding: 10px 20px;
      margin: 10px;
      cursor: pointer;
    }
    #output {
      margin-top: 20px;
      font-size: 18px;
      color: #333;
    }
  </style>
</head>
<body>

  <h1>Korean Voice Correction</h1>
  <p>Hold the button, speak a sentence in Korean, and get feedback!</p>

  <button id="record-button">🎤 Hold to Speak</button>
  <div id="output">Transcription and corrections will appear here...</div>

  <script>
    let recognition;
    const output = document.getElementById("output");
    const recordButton = document.getElementById("record-button");

    // Check for browser support
    if ('webkitSpeechRecognition' in window || 'SpeechRecognition' in window) {
      recognition = new (window.webkitSpeechRecognition || window.SpeechRecognition)();
      recognition.lang = 'ko-KR'; // Korean Language
      recognition.interimResults = false; // Only final result
      recognition.continuous = false;

      let isRecording = false;

      // Start Recording
      recordButton.addEventListener("mousedown", () => {
        output.textContent = "Listening...";
        recognition.start();
        isRecording = true;
      });

      // Stop Recording
      recordButton.addEventListener("mouseup", () => {
        if (isRecording) {
          recognition.stop();
          isRecording = false;
        }
      });

      // Process Results
      recognition.onresult = async (event) => {
        const userTranscript = event.results[0][0].transcript;
        output.textContent = `You said: "${userTranscript}"\nProcessing...`;

        // Send transcript to GitHub Actions API (replace URL with your GitHub Actions backend)
        try {
          const response = await fetch("YOUR_BACKEND_API_URL", {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ transcript: userTranscript })
          });
          const data = await response.json();

          // Display the correction
          output.textContent = data.correctedText || "Error processing response.";
        } catch (error) {
          output.textContent = "An error occurred. Please try again later.";
        }
      };

      recognition.onerror = () => {
        output.textContent = "Error occurred while recognizing speech.";
      };
    } else {
      output.textContent = "Speech recognition not supported in this browser.";
    }
  </script>
</body>
</html>
