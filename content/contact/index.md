---
title: 💬 留言板
date: 2022-10-24
type: landing

sections:
  - block: comments  
    content:
      title: 💬 留言板
      text: |-
        欢迎来到天才主包的留言板！请给天才主包留言吧！（功能测试中）
      
    design:
      columns: '1'
      spacing:
        padding: [20px, 0, 20px, 0]
        
    extra:
      # 留言表单
      form: |-
        <form id="messageForm" class="space-y-4 max-w-md mx-auto">
          <div>
            <label for="name" class="block text-sm font-medium text-gray-700">昵称</label>
            <input type="text" id="name" required placeholder="您的昵称" 
                  class="w-full p-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
          </div>
          
          <div>
            <label for="message" class="block text-sm font-medium text-gray-700">留言内容</label>
            <textarea id="message" required rows="3" placeholder="请输入留言..."
                      class="w-full p-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500 focus:border-blue-500"></textarea>
          </div>
          
          <button type="submit" class="bg-blue-500 text-white p-2 rounded-md hover:bg-blue-600 transition-colors">
            发送留言
          </button>
        </form>
        
      # 留言展示区域
      messages: |-
        <div class="max-w-4xl mx-auto mt-8">
          <h3 class="text-lg font-medium text-gray-800 mb-4">
            最新留言 (共 <span id="messageCount">0</span> 条)
          </h3>
          
          <div id="messagesContainer" class="space-y-4">
            <div id="emptyState" class="text-center py-8 bg-gray-50 rounded-md border border-gray-200">
              暂无留言，快来抢沙发吧！
            </div>
          </div>
        </div>
        
      # 功能脚本
      script: |-
        <script>
        // 存储和加载留言
        let messages = JSON.parse(localStorage.getItem('hugoGuestbook') || '[]');
        
        // DOM元素
        const form = document.getElementById('messageForm');
        const nameInput = document.getElementById('name');
        const messageInput = document.getElementById('message');
        const container = document.getElementById('messagesContainer');
        const countElem = document.getElementById('messageCount');
        const emptyState = document.getElementById('emptyState');
        
        // 初始化页面
        function init() {
          updateCount();
          renderMessages();
        }
        
        // 更新留言计数
        function updateCount() {
          countElem.textContent = messages.length;
          emptyState.style.display = messages.length > 0 ? 'none' : 'block';
        }
        
        // 渲染所有留言
        function renderMessages() {
          // 清空现有留言（保留空状态）
          while (container.children.length > 1) {
            container.removeChild(container.lastChild);
          }
          
          // 显示最新留言在前
          [...messages].reverse().forEach(msg => {
            const div = document.createElement('div');
            div.className = 'p-4 border border-gray-200 rounded-md bg-white shadow-sm';
            
            const date = new Date(msg.time);
            div.innerHTML = `
              <div style="display: flex; justify-content: space-between; align-items: center;">
                <strong>${escapeHTML(msg.name)}</strong>
                <small style="color: #666;">${date.toLocaleString()}</small>
              </div>
              <p style="margin: 10px 0; color: #333;">${escapeHTML(msg.text)}</p>
              <button onclick="deleteMessage('${msg.id}')" 
                      style="color: #dc2626; border: none; background: none; cursor: pointer; text-sm;">
                删除
              </button>
            `;
            
            container.insertBefore(div, emptyState);
          });
        }
        
        // 添加新留言
        function addMessage(name, text) {
          const newMsg = {
            id: Date.now().toString(),
            name: name,
            text: text,
            time: new Date().toISOString()
          };
          
          messages.push(newMsg);
          localStorage.setItem('hugoGuestbook', JSON.stringify(messages));
          updateCount();
          renderMessages();
          
          // 添加提交成功的视觉反馈
          const submitBtn = form.querySelector('button[type="submit"]');
          const originalText = submitBtn.innerHTML;
          submitBtn.disabled = true;
          submitBtn.innerHTML = '✓ 发送成功';
          submitBtn.classList.remove('bg-blue-500', 'hover:bg-blue-600');
          submitBtn.classList.add('bg-green-500');
          
          setTimeout(() => {
            submitBtn.disabled = false;
            submitBtn.innerHTML = originalText;
            submitBtn.classList.remove('bg-green-500');
            submitBtn.classList.add('bg-blue-500', 'hover:bg-blue-600');
          }, 1500);
        }
        
        // 删除留言
        function deleteMessage(id) {
          if (confirm('确定要删除这条留言吗？')) {
            messages = messages.filter(msg => msg.id !== id);
            localStorage.setItem('hugoGuestbook', JSON.stringify(messages));
            updateCount();
            renderMessages();
          }
        }
        
        // 防止XSS攻击
        function escapeHTML(str) {
          return str.replace(/[&<>"']/g, char => ({
            '&': '&amp;',
            '<': '&lt;',
            '>': '&gt;',
            '"': '&quot;',
            "'": '&#39;'
          }[char]));
        }
        
        // 表单提交事件
        form.addEventListener('submit', e => {
          e.preventDefault();
          const name = nameInput.value.trim();
          const text = messageInput.value.trim();
          
          if (name && text) {
            addMessage(name, text);
            messageInput.value = ''; // 清空输入框
          }
        });
        
        // 页面加载时初始化
        document.addEventListener('DOMContentLoaded', init);
        </script>
---

