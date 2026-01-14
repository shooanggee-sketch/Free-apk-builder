---
layout: default
title: AI App & Web Builder
---

<div id="ai-builder-interface" style="max-width: 600px; margin: auto; padding: 20px; border: 2px solid #007bff; border-radius: 15px; background: #ffffff; box-shadow: 0 4px 15px rgba(0,0,0,0.1);">
    <h2 style="text-align: center; color: #007bff;">🚀 خودکار AI بلڈر</h2>
    <div id="status-window" style="height: 250px; overflow-y: auto; border: 1px solid #eee; padding: 15px; margin-bottom: 15px; background: #fdfdfd; font-size: 14px;">
        <p><strong>بوٹ:</strong> میں آپ کی ہدایت کا انتظار کر رہا ہوں۔ بتائیں کیا بنانا ہے؟</p>
    </div>
    <textarea id="user-prompt" style="width: 100%; height: 100px; padding: 10px; border-radius: 8px; border: 1px solid #ccc; margin-bottom: 10px;" placeholder="مثلاً: ایک ایسی ویب سائٹ بنائیں جس میں لاگ ان فارم ہو اور پس منظر کالا ہو..."></textarea>
    <button onclick="startBuilding()" style="width: 100%; padding: 15px; background: #28a745; color: white; border: none; border-radius: 8px; cursor: pointer; font-size: 16px; font-weight: bold;">کوڈنگ شروع کریں اور ڈاؤن لوڈ لنک حاصل کریں</button>
</div>

<script>
async function startBuilding() {
    const prompt = document.getElementById('user-prompt').value;
    const status = document.getElementById('status-window');
    const apiKey = "یہاں_اپنی_OPENAI_API_KEY_ڈالیں"; // اپنی کی یہاں پیسٹ کریں

    if(!prompt) { alert("پہلے ہدایت لکھیں!"); return; }

    status.innerHTML += `<p><strong>آپ:</strong> ${prompt}</p>`;
    status.innerHTML += `<p id='loader' style='color: orange;'><strong>بوٹ:</strong> کوڈ تیار کیا جا رہا ہے... براہ کرم چند سیکنڈ انتظار کریں...</p>`;

    try {
        const response = await fetch('https://api.openai.com/v1/chat/completions', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json', 'Authorization': `Bearer ${apiKey}` },
            body: JSON.stringify({
                model: "gpt-4o",
                messages: [{role: "user", content: `You are an expert developer. Create a single-file HTML/CSS/JS solution for: ${prompt}. Only provide the raw code.`}]
            })
        });

        const data = await response.json();
        const finalCode = data.choices[0].message.content.replace(/```html|```/g, '');

        const blob = new Blob([finalCode], { type: 'text/html' });
        const fileLink = URL.createObjectURL(blob);
        
        document.getElementById('loader').remove();
        status.innerHTML += `<p style='color: green;'><strong>بوٹ:</strong> مبارک ہو! آپ کا پروجیکٹ تیار ہے۔</p>`;
        status.innerHTML += `<a href="${fileLink}" download="index.html" style="display: block; text-align: center; padding: 12px; background: #007bff; color: white; text-decoration: none; border-radius: 8px; margin-top: 10px;">اپنا پروجیکٹ ڈاؤن لوڈ کریں</a>`;
    } catch (e) {
        status.innerHTML += `<p style='color: red;'><strong>بوٹ:</strong> معذرت، رابطے میں مسئلہ ہوا۔ اپنی API Key چیک کریں۔</p>`;
    }
}
</script>
