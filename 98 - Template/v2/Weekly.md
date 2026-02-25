---
week: <% tp.date.now("YYYY-WW") %>
month: <% tp.date.now("YYYY-MM") %>
tags:
  - weekly-log
---

# Week <% tp.date.now("W") %> of <% tp.date.now("YYYY") %>

## Goals
- 

## Review
- Wins: 
- Lessons: 

## Days
<% tp.file.title %> | [[<% tp.file.title %>]]  
<% tp.date.now("YYYY-MM-DD", 1, tp.file.title, "YYYY-MM-DD") %> | [[<% tp.date.now("YYYY-MM-DD", 1, tp.file.title, "YYYY-MM-DD") %>]]  
<% tp.date.now("YYYY-MM-DD", 2, tp.file.title, "YYYY-MM-DD") %> | [[<% tp.date.now("YYYY-MM-DD", 2, tp.file.title, "YYYY-MM-DD") %>]]  
<% tp.date.now("YYYY-MM-DD", 3, tp.file.title, "YYYY-MM-DD") %> | [[<% tp.date.now("YYYY-MM-DD", 3, tp.file.title, "YYYY-MM-DD") %>]]  
<% tp.date.now("YYYY-MM-DD", 4, tp.file.title, "YYYY-MM-DD") %> | [[<% tp.date.now("YYYY-MM-DD", 4, tp.file.title, "YYYY-MM-DD") %>]]  
<% tp.date.now("YYYY-MM-DD", 5, tp.file.title, "YYYY-MM-DD") %> | [[<% tp.date.now("YYYY-MM-DD", 5, tp.file.title, "YYYY-MM-DD") %>]]  
<% tp.date.now("YYYY-MM-DD", 6, tp.file.title, "YYYY-MM-DD") %> | [[<% tp.date.now("YYYY-MM-DD", 6, tp.file.title, "YYYY-MM-DD") %>]]  


Next week: [[<% tp.date.now("YYYY-[W]WW", 7) %>]]
