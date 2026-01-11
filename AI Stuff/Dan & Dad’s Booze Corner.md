1. Channel: Weekly Stock Post
      ↓
2. Discussion Group: Users chat under the post
      ↓
3. User sends /new_order
      ↓
4. Bot posts Anchor Message:
      "Daniel's Order Session. Please reply here."
      ↓
5. User replies to anchor
      ↓
6. Bot identifies:
      reply_to = anchor_id[user]
      ↓
7. Bot forwards text to LangGraph
      ↓
8. LangGraph responds with:
      - next question
      - recommendations
      - reservation confirmation
      ↓
9. Bot posts reply under the same anchor


```text
**👋 Welcome to Dan and Dad’s! Here's a quick guide to getting started:**

**1️⃣ Weekly Stock**
Every Friday we post our latest stock. Tap the post to join the discussion. Feel free to chat, ask questions, or recommend your favourite drinks to me!

The stock list is more like an indication of what I’m comfortable letting go. If you need more or something not listed, just ask later.

**2️⃣ Start Your Own Session**
Inside this week's discussion thread, type:
`/recommend_me` – if you're unsure
`/new_order` – to place an order

When you use a command, **our AI agent BarrelBuddy** will create a **new message in the channel’s discussion** titled something like:
**“Daniel’s Cheers Corner 🍻”**

👉 **Reply to that personal message.**  
That becomes your own little corner where BarrelBuddy will chat with you.

**3️⃣ Examples of What You Can Say:**
(Just say anything under the sun. BarrelBuddy will figure it out.)

• “I’m new. What’s your signature?”
• “I need something strong for pre-drinks but below $40.”
• “I want a beer that's fruity but not too bitter.”
• “We’re hosting a block party in 2 weeks. Help me pick a variety.”
• “Your mead stock shows 5 bottles, but I want 10. Can?”

**Order timing:**  
• Try to **order before monday** because my house ain't the closest to school
• For bulk quantities, give **at least 3 weeks** so I can prep properly

🍻 **Why am I doing this?**  
Honestly, I just wanna share **good stuff with my friends**, not the commercialised stuff everyone’s used to.

There’s a whole world to brewing and crafting, I want common folks like us to get a taste of these flavours.

I’m keeping everything close to cost price. And if I raise by a little, it'll be to recover equipment and spillage costs — because I believe **Alcohol should be free — for friends.**
```

![[LangGraph.svg]]