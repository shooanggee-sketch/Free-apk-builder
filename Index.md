---
layout: default
title: AI App & Web Builder
---

<div id="ai-builder-ui" style="font-family: sans-serif; max-width: 600px; margin: auto; padding: 20px; border: 1px solid #ddd; border-radius: 10px; background: #fff;">
    <h2 style="text-align: center; color: #333;">AI ایپ اور ویب بلڈر</h2>
    <div id="chat-display" style="height: 300px; overflow-y: auto; border: 1px solid #eee; padding: 10px; margin-bottom: 20px; background: #f9f9f9;">
        <p><strong>بوٹ:</strong> میں تیار ہوں! بتائیں میں آپ کے لیے کیا کوڈ کروں؟</p>
    </div>
    <textarea id="user-task" style="width: 100%; height: 80px; padding: 10px; border-radius: 5px; border: 1px solid #ccc;" placeholder="مثلاً: ایک خوبصورت کیلکولیٹر ویب سائٹ بنائیں جس میں ڈارک موڈ ہو..."></textarea>
    <button onclick="generateProject()" style="width: 100%; padding: 12px; background: #007bff; color: white; border: none; border-radius: 5px; cursor: pointer; margin-top: 10px; font-weight: bold;">پروجیکٹ تیار کریں اور لنک حاصل کریں</button>
</div>

<script>
async function generateProject() {
    const task = document.getElementById('user-task').value;
    const chatDisplay = document.getElementById('chat-display');
    const apiKey = "یہاں_اپنی_OPENAI_API_KEY_پیسٹ_کریں"; // اپنی کی یہاں ڈالیں

    if(!task) { alert("پہلے کچھ لکھیں!"); return; }

    chatDisplay.innerHTML += `<p><strong>آپ:</strong> ${task}</p>`;
    chatDisplay.innerHTML += `<p id='working'><strong>بوٹ:</strong> آپ کا کوڈ لکھا جا رہا ہے اور فائلیں ترتیب دی جا رہی ہیں...</p>`;

    try {
        const response = await fetch('https://api.openai.com/v1/chat/completions', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json', 'Authorization': `Bearer ${apiKey}` },
            body: JSON.stringify({
                model: "gpt-4o",
                messages: [{role: "user", content: `Create a complete single-file HTML website with CSS and JS for: ${task}. Output only the code.`}]
            })
        });

        const data = await response.json();
        const code = data.choices[0].message.content;

        // فائل بنانا اور ڈاؤن لوڈ لنک دینا
        const blob = new Blob([code], { type: 'text/html' });
        const downloadUrl = URL.createObjectURL(blob);
        
        document.getElementById('working').remove();
        chatDisplay.innerHTML += `<p style='color: green;'><strong>بوٹ:</strong> کام مکمل! نیچے بٹن سے اپنا پروجیکٹ حاصل کریں:</p>`;
        chatDisplay.innerHTML += `<a href="${downloadUrl}" download="my_project.html" style="display: block; text-align: center; padding: 10px; background: #28a745; color: white; text-decoration: none; border-radius: 5px; margin-top: 10px;">پروجیکٹ ڈاؤن لوڈ کریں</a>`;
    } catch (error) {
        chatDisplay.innerHTML += `<p style='color: red;'><strong>بوٹ:</strong> معذرت، کوئی تکنیکی خرابی پیش آگئی۔</p>`;
    }
}
</script>
