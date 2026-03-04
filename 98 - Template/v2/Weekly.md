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
<% tp.file.title %>  | [[<% tp.file.title %>]]  
<% tp.date.weekday("dddd", 0) %>    | [[<% tp.date.weekday("YYYY-MM-DD", 0) %>]]  
<% tp.date.weekday("dddd", 1) %>    | [[<% tp.date.weekday("YYYY-MM-DD", 1) %>]]  
<% tp.date.weekday("dddd", 2) %>   | [[<% tp.date.weekday("YYYY-MM-DD", 2) %>]]  
<% tp.date.weekday("dddd", 3) %> | [[<% tp.date.weekday("YYYY-MM-DD", 3) %>]]  
<% tp.date.weekday("dddd", 4) %>  | [[<% tp.date.weekday("YYYY-MM-DD", 4) %>]]  
<% tp.date.weekday("dddd", 5) %>    | [[<% tp.date.weekday("YYYY-MM-DD", 5) %>]]  
<% tp.date.weekday("dddd", 6) %>  | [[<% tp.date.weekday("YYYY-MM-DD", 6) %>]]  

Next week: [[<% tp.date.now("YYYY-[W]WW", 7) %>]]
