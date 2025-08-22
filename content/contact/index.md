---
title: 💬 留言板
date: 2022-10-24
type: landing

sections:
  - block: contact  
    content:
      title: 💬 留言板
      text: |-
        欢迎来到天才主包的留言板！请给天才主包留言吧！（功能测试中）
      
    design:
      columns: '1'
      spacing:
        padding: [20px, 0, 20px, 0]
        
        # Automatically link email and phone or display as text?
      autolink: true
    
      # Email form provider
      form:
        provider: netlify
        formspree:
          id:
        netlify:
          # Enable CAPTCHA challenge to reduce spam?
          captcha: false
    design:
      columns: '1'

---

