<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HOK 国际版 - 官方绿色直充</title>
    <style>
        :root { --gold: #d4af37; --bg: #0a0a0a; --card: #161616; --green: #2ecc71; --red: #e74c3c; }
        body { background: var(--bg); color: #fff; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; margin: 0; padding: 15px; }
        .container { max-width: 480px; margin: auto; }
        
        /* 头部状态 */
        .header { background: linear-gradient(145deg, #1f1f1f, #000); padding: 20px; border-radius: 20px; border: 1px solid #333; text-align: center; margin-bottom: 20px; }
        .brand { font-size: 24px; font-weight: 800; color: var(--gold); letter-spacing: 1px; margin-bottom: 5px; }
        .status-box { padding: 8px 15px; border-radius: 50px; font-size: 13px; font-weight: bold; display: inline-block; margin: 10px 0; }
        
        /* 价格表样式 */
        .price-grid { display: grid; grid-template-columns: 1fr; gap: 12px; }
        .price-card { 
            background: var(--card); border-radius: 15px; padding: 15px; 
            display: flex; justify-content: space-between; align-items: center;
            border: 1px solid #222; transition: 0.3s;
        }
        .price-card:hover { border-color: var(--gold); transform: translateY(-2px); }
        .tokens { font-size: 18px; font-weight: bold; }
        .save-label { font-size: 11px; background: rgba(231, 76, 60, 0.1); color: var(--red); padding: 2px 8px; border-radius: 4px; margin-top: 4px; display: inline-block; }
        .price-amount { font-size: 20px; font-weight: 900; color: var(--gold); }
        
        /* 支付弹窗 */
        .modal { display: none; position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); 
                 background: #1c1c1c; padding: 25px; border-radius: 25px; z-index: 1000; width: 85%; max-width: 350px; text-align: center; border: 1px solid var(--gold); }
        .overlay { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.9); z-index: 999; backdrop-filter: blur(5px); }
        .qr-placeholder { width: 180px; height: 180px; background: #fff; margin: 15px auto; border-radius: 10px; display: flex; align-items: center; justify-content: center; color: #000; overflow: hidden; }
        .qr-placeholder img { width: 100%; height: 100%; object-fit: cover; }

        /* 底部法律与保证 */
        .guarantee { background: rgba(212, 175, 55, 0.05); border-radius: 15px; padding: 15px; margin-top: 25px; border: 1px dashed var(--gold); }
        .law-text { font-size: 11px; color: #666; margin-top: 15px; line-height: 1.5; padding: 0 10px; }
        
        button { background: var(--gold); color: #000; border: none; padding: 10px 20px; border-radius: 10px; font-weight: bold; cursor: pointer; width: 100%; margin-top: 10px; }
        .btn-buy { width: auto; padding: 8px 15px; }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <div class="brand">HOK GLOBAL RECHARGE</div>
        <div id="status-tag" class="status-box">系统加载中...</div>
        <div id="status-msg" style="font-size: 12px; color: #888;"></div>
    </div>

    <div class="price-grid" id="price-container">
        </div>

    <div class="guarantee">
        <div style="color: var(--gold); font-weight: bold; margin-bottom: 5px;">🛡️ 安全基金已注入 (Insurance Fund)</div>
        <div style="font-size: 12px; color: #aaa;">由 4364 TNG 实名背书，若因渠道问题导致封号，本店承诺按保额全赔。</div>
    </div>

    <div class="law-text">
        1. <b>汇率告知权：</b> 价格随 Smile.One 接口实时变动，付款时以当前显示为准。<br>
        2. <b>所有权明晰：</b> 充值完成后，点券及账号资产归属下单者所有。<br>
        3. <b>死单最终裁决：</b> 针对超时未到账等极端情况，本店拥有补单或退款的最终决定权。
    </div>
</div>

<div id="overlay" class="overlay" onclick="closeModal()"></div>
<div id="modal" class="modal">
    <div style="font-size: 18px; font-weight: bold; color: var(--gold);">订单确认</div>
    <div id="order-details" style="margin: 15px 0; font-size: 14px;"></div>
    <div class="qr-placeholder">
        <img src="qr.png" alt="请放入你的qr.png图片">
    </div>
    <div style="font-size: 12px; color: #ef4444; margin-bottom: 10px;">⚠️ 支付备注：Service (严禁填HOK/代充)</div>
    <button onclick="sendWhatsApp()">我已支付，通知老板</button>
</div>

<script>
    // 1. 全面价格表数据 (老板你的 10 档定价)
    const products = [
        { tokens: "16", price: "0.88", save: "0.11" },
        { tokens: "80", price: "4.15", save: "0.35" },
        { tokens: "240", price: "12.39", save: "1.60" },
        { tokens: "400", price: "21.19", save: "1.80" },
        { tokens: "560", price: "29.88", save: "3.02" },
        { tokens: "800+30", price: "42.88", save: "5.11" },
        { tokens: "1200+45", price: "64.99", save: "4.91" },
        { tokens: "2400+108", price: "127.88", save: "12.02" },
        { tokens: "4000+180", price: "218.00", save: "20.00" },
        { tokens: "8000+360", price: "439.00", save: "37.00" }
    ];

    // 2. 动态渲染价格表
    const container = document.getElementById('price-container');
    products.forEach(p => {
        container.innerHTML += `
            <div class="price-card">
                <div>
                    <div class="tokens">${p.tokens} Tokens</div>
                    <div class="save-label">立省 RM ${p.save}</div>
                </div>
                <div style="text-align: right;">
                    <div class="price-amount">RM ${p.price}</div>
                    <button class="btn-buy" onclick="openPay('${p.tokens}', '${p.price}')">立即订购</button>
                </div>
            </div>
        `;
    });

    // 3. 智能时间逻辑 (老板上课/睡觉系统)
    function updateStatus() {
        const hour = new Date().getHours();
        const tag = document.getElementById('status-tag');
        const msg = document.getElementById('status-msg');
        
        if (hour >= 17 && hour < 23) {
            tag.innerHTML = "🟢 极速秒充中";
            tag.style.background = "rgba(46, 204, 113, 0.2)";
            tag.style.color = "#2ecc71";
            msg.innerHTML = "老板在线，下单后预计 1-5 分钟内到账";
        } else if (hour >= 7 && hour < 17) {
            tag.innerHTML = "🟡 预约模式";
            tag.style.background = "rgba(241, 196, 15, 0.2)";
            tag.style.color = "#f1c40f";
            msg.innerHTML = "老板上课中，现在下单可锁定汇率，17:00 统一开单";
        } else {
            tag.innerHTML = "🌙 凌晨预定";
            tag.style.background = "rgba(155, 89, 182, 0.2)";
            tag.style.color = "#9b59b6";
            msg.innerHTML = "老板补眠中，明早 07:30 准时开始处理订单";
        }
    }

    // 4. 订单处理
    let activeOrder = {};
    function openPay(t, p) {
        activeOrder = { tokens: t, price: p };
        document.getElementById('order-details').innerHTML = `充值数量：${t} Tokens<br>需支付：<span style="font-size:20px; color:#d4af37;">RM ${p}</span>`;
        document.getElementById('overlay').style.display = 'block';
        document.getElementById('modal').style.display = 'block';
    }

    function closeModal() {
        document.getElementById('overlay').style.display = 'none';
        document.getElementById('modal').style.display = 'none';
    }

    function sendWhatsApp() {
        const phone = "你的马来西亚手机号"; // ⚠️ 请填入你的手机号，例如 60123456789
        const text = `老板你好，我已支付！%0A订单内容：${activeOrder.tokens} Tokens%0A支付金额：RM ${activeOrder.price}%0A请在[安全/预约期]内帮我充值。`;
        window.open(`https://wa.me/${phone}?text=${text}`);
    }

    updateStatus();
</script>

</body>
</html>
