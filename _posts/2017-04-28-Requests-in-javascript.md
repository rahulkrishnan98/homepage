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

 **The Introduction :**

<br>As you probably know very well by now, the internet is made up of a bunch of interconnected computers called servers. When you are surfing the web and navigating between web pages, our browser  requests information from any of these servers. The chart below explains explicitly on the request.<br> ![46](https://rahulkrishnanlive.files.wordpress.com/2017/04/46.png)<br> That is, our browser sends a request, waits for the server to respond to the request, and (once the server responds) processes the request. All of this is governed by protocols or rules which is a topic for another day.
<br>
### **Application Program Interface (API) **

<br>Now the Wikipedia definition of the API will tell you, an **Application Programming Interface** (**API**) is a set of subroutine definitions, protocols, and tools for building <br> ![47.png](https://rahulkrishnanlive.files.wordpress.com/2017/04/47.png)<br>application software. But in layman terms , in the context of the web, the API's generally allow you to send commands to programs running on the servers that you connect to from your browser or with the browser itself. This allows you to access resources only available on the server. A common usage is of the authorisation feature to login to websites. <br>  [![API's](https://img.youtube.com/vi/6STSHbdXQWI&w=640&h=360/0.jpg)](https://www.youtube.com/watch?v=6STSHbdXQWI&w=640&h=360 "API")
<br>
### **XMLHttpRequest :**

<br>Now there are countless tutorials out there executing the requests via JavaScript or an extended framework of the same. For starters, the remainder of the tutorial is focused on plain vanilla Javascript and could be cumbersome( Angular JS does the same in half of the number of lines *sighs!*). All that being said, lets actually see the code and later break it down one by one.

> <pre>
>     API testing
>
>
>         var xhr=new XMLHttpRequest();
>         xhr.open("GET","http://ipinfo.io/json", true);
>         xhr.send();
>         xhr.addEventListener("readystatechange", processRequest , false);
>         function processRequest(e){
>         if(xhr.readyState == 4 && xhr.status == 200) {
>             var r = JSON.parse(xhr.responseText);
>             var dis = "current IP:" + r.ip
>             document.write(dis);
>         }}
>
>
> </pre>

The above code is in a simple HTML file. Now any JavaScript code that we write goes under the script tag of the code. Explicitly mention **type=text/javascript .**
<br>
### 1]  **Initialising object and sending request :**
<br>
> <table style="height: 48px;" border="0" width="626" cellspacing="0" cellpadding="0">
>
> <tbody>
>
> <tr>
>
> <td class="code">
>
> <div class="container">
>
> <div class="line number1 index0 alt2">`var` `xhr =` `new` `XMLHttpRequest();`</div>
>
> <div class="line number2 index1 alt1 highlighted">`xhr.open(``'GET'``,` `"//ipinfo.io/json"``,` `true``);`</div>
>
> </div>
>
> </td>
>
> </tr>
>
> </tbody>
>
> </table>

The above block initialised the _**xhr**_ variable with the request. Now open is a method which takes three arguments to carry out the requests. It takes in **GET**( in this case) to specify it's action,** //ipinfo.io/json**, for where it getting it from and** true/false** for executing the request asynchronously/synchronously. On a quick glance, up until now we have set the request prameters , but have not sent it yet,

> **xhr.send();**

Now even when the request is sent, other parts of your code are run not awaiting confirmation for the request from the server side. This makes the user experience much better than a synchronous request which will probably through most users off! Now I hope it's more clear on why we used the asynchronous ( TRUE ) request instead.
<br>
### **2] Readystate and readystatechange**
<br>
Now by the time we have actually sent requests to the browser, it's important to know the state of the request. For this we use the the **readystatechange. **While this still can indicate on the status of the request, **<span class="inlineCode">readystatechange</span>** event being tied to our **<span class="inlineCode">XMLHttpRequest</span>** object's **<span class="inlineCode">readyState</span>** property makes it difficult to just parse the data soon afterwards. The readyState changes a 5 set value ranging from 0 to 4\. The exact status is mentioned below in the tabular column.

![48](https://rahulkrishnanlive.files.wordpress.com/2017/04/48.png) <br>**credits : www.kirupa.com**

So practically the readystatechange event is being fired 5 times, but in actuality we are only  interested in the final state of the request ( 4- DONE) . Secondly the the Http status codes we all know about, for example **_ 202 - Accepted  ; 200 - OK ; and the most hideous 404 - Page not found (Bad request) _** So even if our event state is an ideal "4", we don't want to return "empty" successful requests. We need the return of the content we requested to the server. Hence we write the following  condition in our function

> <pre>function processRequest(e){
>         if(xhr.readyState == 4 && xhr.status == 200) {}
>            }</pre>

### **3\. Parsing data and final end output**
<br>
Now the request to the server has returned successfully with the data we have to parse into user readable text. So based on the data type we access we have to choose alter paths. For instance this current data is in the form of JSON.<br> ![49.png](https://rahulkrishnanlive.files.wordpress.com/2017/04/49.png)<br> Now we parse this particular JSON dataset to get the ip parameter alone for the purpose of this tutorial.

> <pre>var r = JSON.parse(xhr.responseText);
> var dis = "current IP:" + r.ip + "
> "
> document.write(dis);</pre>

What this does is it initialises a new variable with the parsed return data and uses the variable to access parts of it.<br><br> So hope this helps, suggestions are welcome at mrahul.krishnan@gmail.com as  always or drop a comment below. Until next time, bye!


