<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🎃 万圣节礼物</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            margin: 0;
            padding: 0;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background: url('https://images.unsplash.com/photo-1546156018-efc496d24a9c?q=80&w=1000&auto=format') center/cover no-repeat;
            background-blend-mode: overlay;
            background-color: rgba(0, 0, 0, 0.7);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            overflow: hidden;
        }

        .gift-container {
            text-align: center;
            color: white;
            padding: 30px;
            border-radius: 20px;
            background: rgba(0, 0, 0, 0.6);
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.8);
            animation: float 3s ease-in-out infinite;
        }

        @keyframes float {
            0% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
            100% { transform: translateY(0px); }
        }

        .gift-icon {
            font-size: 60px;
            margin-bottom: 20px;
        }

        .gift-title {
            font-size: 26px;
            margin-bottom: 15px;
            text-shadow: 0 0 10px #ff6b6b;
        }

        .gift-btn {
            background: #ff6b6b;
            color: white;
            border: none;
            border-radius: 30px;
            font-size: 20px;
            padding: 12px 40px;
            margin-top: 20px;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(255, 107, 107, 0.6);
            transition: all 0.3s;
        }

        .gift-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 20px rgba(255, 107, 107, 0.8);
        }

        .blessing {
            position: fixed;
            width: 280px; /* 加宽弹窗 */
            min-height: 60px; /* 最小高度 */
            background: white;
            border-radius: 10px;
            display: flex;
            justify-content: center;
            align-items: center;
            text-align: center;
            padding: 12px;
            font-size: 15px; /* 调整字体大小 */
            box-shadow: 0 2px 8px rgba(0,0,0,0.2);
            z-index: 1000;
            cursor: pointer;
        }

        .special-blessing {
            width: 320px;
            min-height: 80px;
            font-size: 18px;
            font-weight: bold;
            background: #ffc1e9;
            color: #e91e63;
            box-shadow: 0 4px 15px rgba(233, 30, 99, 0.5);
        }

        .close-btn {
            position: absolute;
            top: 6px;
            right: 10px;
            font-size: 16px;
            color: #999;
            cursor: pointer;
        }

        .special-blessing .close-btn {
            color: #e91e63;
        }
    </style>
</head>
<body>
    <audio id="backgroundMusic" loop preload="auto">
        <source src="https://assets.mixkit.co/sfx/preview/mixkit-happy-halloween-1782.mp3" type="audio/mpeg">
        Your browser does not support the audio element.
    </audio>

    <div class="gift-container" id="giftScreen">
        <div class="gift-icon">🎁</div>
        <div class="gift-title">您有一份万圣节礼物</div>
        <div class="gift-message">确定要打开吗？</div>
        <button class="gift-btn" id="openBtn">打开礼物</button>
    </div>

    <script>
        const openBtn = document.getElementById('openBtn');
        const giftScreen = document.getElementById('giftScreen');
        const audio = document.getElementById('backgroundMusic');
        audio.volume = 0.3;

        const backgrounds = [
            '#ffc0cb', '#ffb6c1', '#ff69b4', '#ff1493', '#db7093',
            '#bc8f8f', '#c71585', '#ffa07a', '#ff7f50', '#ffa500',
            '#ffffe0', '#e6e6fa', '#d8bfd8', '#dda0dd', '#ee82ee'
        ];

        const normalBlessings = [
            '别太累了，偶尔痛点也好',
            '愿所有烦恼都消失',
            '记得好好护肤',
            '记得吃水果',
            '我想你了',
            '早点休息',
            '多喝水哦',
            '要相信自己噢',
            '外面冷，出门记得戴围巾',
            '护手霜记得涂，别让手干裂了',
            '三餐要按时吃，别饿肚子',
            '被子盖好，别踢被子',
            '多喝温水，别喝冰水',
            '水果要吃够，补充维生素',
            '脸洗干净，别偷懒不护肤',
            '每天都在想你',
            '天冷加衣，别逞强',
            '睡前泡个脚，睡得更香',
            '手机别玩太晚，眼睛会累',
            '想你了，小柱子',
            '记得吃早餐，一天才有精神',
            '嘴唇干裂了就涂润唇膏',
            '多吃苹果，平安又健康',
            '你在我心里',
            '晚上别刷手机到半夜',
            '护手霜放包里，随时用',
            '穿秋裤，别让妈妈担心',
            '保温杯带在身边，随时喝水',
            '水果切好放饭盒里，方便吃',
            '面膜记得敷，皮肤才水嫩',
            '出门戴帽子，耳朵别冻红',
            '回家先洗手，注意卫生',
            '别因为忙就不吃水果',
            '热水泡脚，促进血液循环',
            '护肤品别乱用，选适合自己的',
            '天冷别露脚踝，容易生病',
            '每天都要开心，我在想你',
            '水果和蔬菜都要吃哦',
            '护手霜是冬天必备',
            '别用冷水洗脸，刺激皮肤',
            '记得吃香蕉，补钾又通便',
            '晚上11点前要睡着',
            '围巾手套帽子都戴好',
            '多喝热水，不是敷衍，是关心',
            '水果要新鲜，别吃坏的',
            '好好照顾自己，我才放心',
            '皮肤干就用加湿器',
            '你要好好的，我一直想你',
            '雪天路滑，走路慢点',
            '火锅配朋友，冬天最幸福',
            '电热毯提前开，被窝暖烘烘',
            '脸蛋别冻裂，记得涂面霜',
            '冬天的阳光最珍贵，多晒太阳补钙',
            '热汤一碗，暖身又暖心',
            '期末考试前，穿红袜子求好运',
            '冬天的夜晚长，做个好梦到天亮',
            '围巾选红色，喜庆又保暖',
            '天冷别赖床，但可以多睡十分钟',
            '手套分你一只，一起牵手过冬',
            '冬天的星星特别亮，愿你梦想也闪亮',
            '热可可加棉花糖，甜到心里暖洋洋'
        ];

        const specialBlessing = '小柱子万圣节快乐！';
        let blessingElements = [];

        openBtn.addEventListener('click', () => {
            audio.play().catch(err => {
                console.log("音乐播放失败:", err);
            });

            giftScreen.style.display = 'none';

            const shuffled = [...normalBlessings].sort(() => 0.5 - Math.random());
            for (let i = 0; i < normalBlessings.length; i++) {
                setTimeout(() => createBlessing(shuffled[i], false), i * 150);
            }
            setTimeout(() => createBlessing(specialBlessing, true), normalBlessings.length * 150 + 500);
        });

        function createBlessing(text, isSpecial = false) {
            const div = document.createElement('div');
            div.className = isSpecial ? 'blessing special-blessing' : 'blessing';
            div.style.backgroundColor = isSpecial ? '' : backgrounds[Math.floor(Math.random() * backgrounds.length)];
            div.innerHTML = `<span class="close-btn">×</span><div>${text}</div>`;

            if (isSpecial) {
                div.style.left = '50%';
                div.style.top = '50%';
                div.style.transform = 'translate(-50%, -50%)';
            } else {
                const maxX = window.innerWidth - 280;
                const maxY = window.innerHeight - 60;
                div.style.left = `${Math.floor(Math.random() * maxX)}px`;
                div.style.top = `${Math.floor(Math.random() * maxY)}px`;
            }

            div.querySelector('.close-btn').addEventListener('click', () => div.remove());
            div.addEventListener('click', () => div.remove());
            document.body.appendChild(div);
            blessingElements.push(div);
        }
    </script>
</body>
</html>
