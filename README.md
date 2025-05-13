<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Photo Strip Booth</title>
  <style>
    body { font-family: sans-serif; text-align: center; background: #111; color: white; }
    video, canvas { display: block; margin: 10px auto; }
    #strip { background: white; padding: 10px; }
    button { padding: 10px 20px; font-size: 16px; margin-top: 10px; }
  </style>
</head>
<body>
  <h1>Photo Strip Booth</h1>
  <video id="video" width="320" height="240" autoplay></video>
  <button onclick="startBooth()">Start Photo Booth</button>
  <canvas id="strip" width="320" height="750"></canvas>
  <a id="download" style="display: block; margin-top: 10px;" download="photo_strip.png">Download Photo Strip</a>

  <script>
    const video = document.getElementById('video');
    const canvas = document.getElementById('strip');
    const ctx = canvas.getContext('2d');
    const downloadLink = document.getElementById('download');

    // Access webcam
    navigator.mediaDevices.getUserMedia({ video: true })
      .then(stream => { video.srcObject = stream; });

    function takePhoto(yOffset) {
      ctx.drawImage(video, 0, yOffset, 320, 240);
    }

    async function startBooth() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      for (let i = 0; i < 3; i++) {
        await new Promise(res => setTimeout(res, 2000)); // 2 sec countdown
        takePhoto(i * 250);
      }
      const dataURL = canvas.toDataURL('image/png');
      downloadLink.href = dataURL;
    }
  </script>
</body>
</html>
