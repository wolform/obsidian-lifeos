---
week: <% tp.date.now("YYYY-[W]WW") %>
month: <% tp.date.now("YYYY-MM") %>
tags: [weekly]
---

# Week <% tp.date.now("W") %> of <% tp.date.now("YYYY") %>

## Goals
- 

## Review
- Wins: 
- Lessons: 

## Days
<% tp.date.now("YYYY-MM-DD", 0, "Monday") %> | [[<% tp.date.now("YYYY-MM-DD", 0, "Monday") %>]]  
<% tp.date.now("YYYY-MM-DD", 0, "Tuesday") %> | [[<% tp.date.now("YYYY-MM-DD", 0, "Tuesday") %>]]  
... (or use Dataview query later)

Next week: [[<% tp.date.now("YYYY-[W]WW", 7) %>]]
