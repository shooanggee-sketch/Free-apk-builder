---
layout: default
title: AI App & Web Builder
---

<div id="chat-container" style="background: #f4f7f6; padding: 20px; border-radius: 10px; max-width: 500px; margin: auto; box-shadow: 0px 4px 10px rgba(0,0,0,0.1);">
    <div id="chat-box" style="height: 300px; overflow-y: auto; background: white; padding: 10px; border-radius: 5px; margin-bottom: 10px; border: 1px solid #ddd;">
        <p><strong>بوٹ:</strong> خوش آمدید! آپ کس قسم کی ایپ یا ویب سائٹ بنوانا چاہتے ہیں؟</p>
    </div>
    <input type="text" id="user-input" placeholder="اپنی ہدایت یہاں لکھیں..." style="width: 80%; padding: 10px; border-radius: 5px; border: 1px solid #ccc;">
    <button onclick="sendMessage()" style="padding: 10px; background: #28a745; color: white; border: none; border-radius: 5px; cursor: pointer;">بھیجیں</button>
</div>

<script>
function sendMessage() {
    var input = document.getElementById('user-input');
    var chatBox = document.getElementById('chat-box');
    if(input.value !== "") {
        chatBox.innerHTML += "<p><strong>آپ:</strong> " + input.value + "</p>";
        chatBox.innerHTML += "<p style='color: blue;'><strong>بوٹ:</strong> آپ کی ہدایت موصول ہو گئی۔ میں کوڈ تیار کر رہا ہوں...</p>";
        input.value = "";
        chatBox.scrollTop = chatBox.scrollHeight;
    }
}
</script>
