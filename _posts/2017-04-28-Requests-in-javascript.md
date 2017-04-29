


---
layout: post
title:  "Sending Http requests in Javascript"
date:   2017-04-28
desc: "Sending Http requests in Javascript"
keywords: "Web,HttpRequest"
categories: [HTML]
tags: [Web,Javascript]
icon: icon-html
---





**The Introduction :** As you probably know very well by now, the internet is made up of a bunch of interconnected computers called servers. When you are surfing the web and navigating between web pages, our browser  requests information from any of these servers. The chart below explains explicitly on the request.<br> ![46](https://rahulkrishnanlive.files.wordpress.com/2017/04/46.png)<br> That is, our browser sends a request, waits for the server to respond to the request, and (once the server responds) processes the request. All of this is governed by protocols or rules which is a topic for another day. **Application Program Interface (API) ** Now the Wikipedia definition of the API will tell you, an **Application Programming Interface** (**API**) is a set of subroutine definitions, protocols, and tools for building ![47.png](https://rahulkrishnanlive.files.wordpress.com/2017/04/47.png)application software. But in layman terms , in the context of the web, the API's generally allow you to send commands to programs running on the servers that you connect to from your browser or with the browser itself. This allows you to access resources only available on the server. A common usage is of the authorisation feature to login to websites. [youtube https://www.youtube.com/watch?v=6STSHbdXQWI&w=640&h=360] **XMLHttpRequest :** Now there are countless tutorials out there executing the requests via JavaScript or an extended framework of the same. For starters, the remainder of the tutorial is focused on plain vanilla Javascript and could be cumbersome( Angular JS does the same in half of the number of lines *sighs!*). All that being said, lets actually see the code and later break it down one by one.
