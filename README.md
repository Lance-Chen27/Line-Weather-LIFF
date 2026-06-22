<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>即時天氣看板</title>
    <script src="https://static.line-scdn.net/liff/edge/2/sdk.js"></script>
    <style>
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background: linear-gradient(180deg, #3a7bd5, #3a6073); color: white; height: 100vh; margin: 0; display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center; }
        .card { background: rgba(255, 255, 255, 0.2); padding: 30px; border-radius: 24px; backdrop-filter: blur(10px); width: 85%; max-width: 320px; box-shadow: 0 8px 32px rgba(0,0,0,0.2); border: 1px solid rgba(255,255,255,0.1); }
        .btn { background: #ffffff; color: #3a7bd5; border: none; padding: 14px 28px; border-radius: 50px; font-size: 16px; font-weight: bold; cursor: pointer; margin-top: 20px; width: 100%; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }
        #result { margin-top: 25px; font-size: 1rem; line-height: 1.6; }
    </style>
</head>
<body>

    <div class="card">
        <h2 id="greet">🌤 即時天氣查詢</h2>
        <div id="result">歡迎使用！請點擊下方按鈕允許定位，即可查詢您所在位置的即時天氣。</div>
        <button class="btn" onclick="startLocation()">開始定位查詢</button>
    </div>

    <script>
        const MY_LIFF_ID = '2010465206-8uZZX4hM'; 

        liff.init({ liffId: MY_LIFF_ID })
            .then(() => {
                if (!liff.isLoggedIn()) { liff.login(); }
                else {
                    liff.getProfile().then(p => { document.getElementById('greet').innerText = `🌤 嗨，${p.displayName}`; });
                }
            });

        function startLocation() {
            document.getElementById('result').innerText = '獲取 GPS 定位中...';
            // 優先使用手機瀏覽器原生定位，最穩定不卡權限
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(
                    (pos) => {
                        const lat = pos.coords.latitude;
                        const lon = pos.coords.longitude;
                        
                        // 🎯 成功拿到 GPS！先顯示在畫面上，這就是我們要丟給天氣 API 的燃料
                        document.getElementById('result').innerHTML = `
                            ✅ 定位成功！<br><br>
                            緯度：${lat.toFixed(4)}<br>
                            經度：${lon.toFixed(4)}<br><br>
                            <span style="color:#ffeb3b;">[下一階段：準備將此座標傳送至後端換取天氣資訊]</span>
                        `;
                    },
                    (err) => { document.getElementById('result').innerText = '定位失敗，請確保手機已開啟定位功能。'; }
                );
            }
        }
    </script>
</body>
</html>
