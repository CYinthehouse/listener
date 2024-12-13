<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Korean Voice Feedback</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      margin: 20px;
    }
    h1 {
      color: #333;
    }
    button {
      background-color: #007bff;
      color: white;
      padding: 10px 20px;
      border: none;
      border-radius: 5px;
      font-size: 16px;
      cursor: pointer;
      margin: 10px;
    }
    button:hover {
      background-color: #0056b3;
    }
    #transcription {
      margin: 20px auto;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 5px;
      width: 80%;
      max-width: 600px;
      min-height: 50px;
      background-color: #f9f9f9;
    }
    #feedback {
      margin-top: 20px;
      font-size: 18px;
      color: #28a745;
    }
  </style>
</head>
<body>

  <h1>Korean Speaking Practice</h1>
  <p>Hold the button to record your voice in Korean.</p>
  
  <button id="recordButton">Hold to Speak</button>
  <div id="transcription">Your speech will appear here...</div>
  <button id="sendButton">Submit for Feedback</button>

  <div id="feedback">Feedback will appear here...</div>

  <script>
    const recordButton = document.getElementById('recordButton');
    const transcriptionDiv = document.getElementById('transcription');
    const sendButton = document.getElementById('sendButton');
    const feedbackDiv = document.getElementById('feedback');

    let recognition;
    let isRecording = false;

    // Initialize Speech Recognition
    if ('webkitSpeechRecognition' in window || 'SpeechRecognition' in window) {
      recognition = new (window.webkitSpeechRecognition || window.SpeechRecognition)();
      recognition.lang = 'ko-KR'; // Korean language
      recognition.interimResults = true;

      recognition.onresult = (event) => {
        const transcript = event.results[event.results.length - 1][0].transcript;
        transcriptionDiv.textContent = transcript;
      };

      recognition.onend = () => {
        if (isRecording) {
          recognition.start(); // Restart if still holding the button
        }
      };
    } else {
      transcriptionDiv.textContent = "Speech recognition not supported in this browser.";
    }

    // Start recording on button press
    recordButton.addEventListener('mousedown', () => {
      if (recognition) {
        isRecording = true;
        recognition.start();
        transcriptionDiv.textContent = "Listening...";
      }
    });

    // Stop recording on button release
    recordButton.addEventListener('mouseup', () => {
      isRecording = false;
      if (recognition) recognition.stop();
    });

    // Send transcript to GitHub Actions (Trigger Workflow)
    sendButton.addEventListener('click', async () => {
      const transcript = transcriptionDiv.textContent.trim();
      if (!transcript || transcript === "Listening...") {
        feedbackDiv.textContent = "Please say something first!";
        return;
      }

      feedbackDiv.textContent = "Sending for feedback...";

      try {
        const response = await fetch('[https://api.github.com/repos/CYinthehouse/listener/dispatches](https://us-central1-kortut.cloudfunctions.net/api)', {
          method: 'POST',
          headers: {
            'Accept': 'application/vnd.github.v3+json',
            'Authorization': '' // Replace with a Personal Access Token
          },
          body: JSON.stringify({
            event_type: "api-request", // Event trigger name
            client_payload: { transcript: transcript }
          })
        });

        if (response.ok) {
          feedbackDiv.textContent = "Feedback is being processed. Please check your GitHub Actions logs.";
        } else {
          feedbackDiv.textContent = "Failed to send transcript. Please try again.";
          console.error("Error:", response.statusText);
        }
      } catch (error) {
        feedbackDiv.textContent = "An error occurred. Please try again.";
        console.error("Error:", error);
      }
    });
  </script>
</body>
</html>
