const express = require('express');
const bodyParser = require('body-parser');

const app = express();
const port = process.env.PORT || 3000;

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

const getRandomItem = (items) => {
    return items[Math.floor(Math.random() * items.length)];
};

app.use(bodyParser.json());

app.post('/chat', (req, res) => {
    const userMessage = req.body.message;
    let replyMessage = '';

    if (step === 0 && (userMessage === "你好" || userMessage === "早安")) {
        replyMessage = '親愛的主人您早～早上要吃什麼？請選擇中式或西式，或者輸入“隨機”讓我幫您選擇。';
        step = 1;
    } else if (step === 1 && userMessage === "中式") {
        currentBreakfastList = chineseBreakfast;
        replyMessage = '請選擇以下中式早餐，或者輸入“隨機”讓我幫您選擇：\n' + currentBreakfastList.join('，');
        step = 2;
    } else if (step === 1 && userMessage === "西式") {
        currentBreakfastList = westernBreakfast;
        replyMessage = '請選擇以下西式早餐，或者輸入“隨機”讓我幫您選擇：\n' + currentBreakfastList.join('，');
        step = 2;
    } else if (step === 1 && userMessage === "隨機") {
        currentBreakfastList = chineseBreakfast.concat(westernBreakfast);
        const randomBreakfast = getRandomItem(currentBreakfastList);
        replyMessage = `我幫您選擇了${randomBreakfast}。`;
        step = 0;
    } else if (step === 2 && userMessage === "隨機") {
        const randomBreakfast = getRandomItem(currentBreakfastList);
        replyMessage = `我幫您選擇了${randomBreakfast}。`;
        step = 0;
    } else if (step === 2 && currentBreakfastList.includes(userMessage)) {
        replyMessage = `好的，主人，您的早餐是${userMessage}。`;
        step = 0;
    } else {
        replyMessage = '對不起，主人，我不明白您的意思。請選擇中式或西式，或者輸入“隨機”。';
        step = 1;
    }

    res.json({ reply: replyMessage });
});

app.listen(port, () => {
    console.log(`Server is running on port ${port}`);
});
