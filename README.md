<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <title>早餐選擇聊天機器人</title>
    <style>
        body {
            font-family: Arial, sans-serif;
        }
        #chatArea {
            width: 100%;
            height: 300px;
            border: 1px solid #ccc;
            padding: 10px;
            overflow-y: scroll;
            margin-bottom: 10px;
            white-space: pre-wrap;
        }
        #userInput {
            width: 80%;
            padding: 10px;
        }
        button {
            padding: 10px;
        }
    </style>
</head>
<body>
    <h1>早餐選擇聊天機器人</h1>
    <div id="chatArea"></div>
    <input type="text" id="userInput" placeholder="輸入你的消息...">
    <button onclick="sendMessage()">發送</button>

    <script>
        const chineseBreakfast = [
            "三明治", "稀飯", "豆漿油條", "炒麵", "煎餅果子", "肉夾饃", "燒餅", "包子", "油條", "雞蛋灌餅",
            "蔥油餅", "牛奶", "豆漿", "豆腐腦", "餛飩", "餃子", "煎包", "燒賣", "粽子", "腸粉",
            "拉麵", "米粉", "餛飩麵", "雞蛋餅", "飯團", "烤麵包", "果醬吐司", "蛋糕", "芝士蛋糕", "曲奇餅"
        ];

        const westernBreakfast = [
            "馬芬", "鬆餅", "法式吐司", "漢堡", "熱狗", "披薩", "牛排", "炸雞", "炸魚", "薯條",
            "沙拉", "雞蛋", "炒飯", "炒菜", "蒸菜", "豆腐", "滷蛋", "鹵肉飯", "肉鬆飯", "章魚燒",
            "日式咖哩", "韓式烤肉", "壽司", "生魚片", "烤鰻魚", "龍蝦", "大閘蟹", "水煮魚", "涼拌菜", "泡菜"
        ];

        let step = 0;
        let currentBreakfastList = [];

        function getRandomItem(items) {
            return items[Math.floor(Math.random() * items.length)];
        }

        function sendMessage() {
            const userInput = document.getElementById('userInput').value;
            document.getElementById('userInput').value = '';
            const chatArea = document.getElementById('chatArea');
            chatArea.innerHTML += `<p><strong>你:</strong> ${userInput}</p>`;

            let replyMessage = '';

            if (step === 0 && (userInput === "你好" || userInput === "早安")) {
                replyMessage = '親愛的主人您早～早上要吃什麼？請選擇中式或西式，或者輸入“隨機”讓我幫您選擇。';
                step = 1;
            } else if (step === 1 && userInput === "中式") {
                currentBreakfastList = chineseBreakfast;
                replyMessage = '請選擇以下中式早餐，或者輸入“隨機”讓我幫您選擇：\n' + currentBreakfastList.join('，');
                step = 2;
            } else if (step === 1 && userInput === "西式") {
                currentBreakfastList = westernBreakfast;
                replyMessage = '請選擇以下西式早餐，或者輸入“隨機”讓我幫您選擇：\n' + currentBreakfastList.join('，');
                step = 2;
            } else if (step === 1 && userInput === "隨機") {
                currentBreakfastList = chineseBreakfast.concat(westernBreakfast);
                const randomBreakfast = getRandomItem(currentBreakfastList);
                replyMessage = `我幫您選擇了${randomBreakfast}。`;
                step = 0;
            } else if (step === 2 && userInput === "隨機") {
                const randomBreakfast = getRandomItem(currentBreakfastList);
                replyMessage = `我幫您選擇了${randomBreakfast}。`;
                step = 0;
            } else if (step === 2 && currentBreakfastList.includes(userInput)) {
                replyMessage = `好的，主人，您的早餐是${userInput}。`;
                step = 0;
            } else {
                replyMessage = '對不起，主人，我不明白您的意思。請選擇中式或西式，或者輸入“隨機”。';
                step = 1;
            }

            chatArea.innerHTML += `<p><strong>機器人:</strong> ${replyMessage}</p>`;
            chatArea.scrollTop = chatArea.scrollHeight;
        }
    </script>
</body>
</html>
