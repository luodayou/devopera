Title: JavaScript best practices
----
Date: 2009-03-06 11:47:05
----
Lang: en
----
Author: 
----
License: Creative Commons Attribution, Non Commercial - Share Alike 2.5
----
License_url: http://creativecommons.org/licenses/by-nc-sa/2.5/
----
Text:

<div class="note">
<h2 style="color:red;font-weight:bold;padding-top:0;margin-top:0;">11th October 2012: Material moved to <a href="http://www.webplatform.org">webplatform.org</a></h2>

<p style="padding-bottom: 20px;">The Opera web standards curriculum has now been moved to the <a href="http://docs.webplatform.org/wiki/Main_Page">docs section of the W3C webplatform.org site</a>. Go there to find updated versions of these docs, and much more besides!</p>

<h2 style="color:red;font-weight:bold;padding-top:0;margin-top:0;">12th April 2012: This article is obsolete</h2>

<p>The web standards curriculum has been donated to the <a href="http://www.w3.org/community/webed/">W3C web education community group</a>, to become part of a much bigger educational resource. It is constantly being updated so that it remains current with modern web design practices and technologies. To find the most up-to-date web standards curriculum, visit the <a href="http://www.w3.org/community/webed/wiki/Main_Page">web education community group Wiki</a>. Please make changes to this Wiki yourself, or suggest changes to <a href="mailto:cmills@opera.com">Chris Mills</a>, who is also the chair of the web education community group.</p>
</div>

<ul class="seriesNav">
<li class="prev"><a href="http://dev.opera.com/articles/view/first-look-at-javascript/" rel="prev" title="link to the previous article in the series">Previous article—Your first look at JavaScript</a></li>
<li class="next"><a href="http://dev.opera.com/articles/view/unobtrusive-javascript/" rel="next" title="link to the next article in the series">Next article—The principles of unobtrusive JavaScript</a></li>
<li><a href="http://dev.opera.com/articles/view/1-introduction-to-the-web-standards-cur/#toc" rel="index">Table of contents</a></li>
</ul>

<p>Writing a best practice article is quite a tricky business. To a number of you, what you are about to read will appear to be very obvious and just the sensible thing to do.</p>

<p>However, looking around the web and getting code handed over to me from other developers for years has taught me that common sense is actually quite a rarity in live code on the web, and the “sensible and logical thing to do” gets pushed far down the priority list once you are in the middle of a project, and the deadline is looming.</p>

<p>So I’ve decided to make it easier for you by creating this article, which is a compilation of best practices and good advice I’ve amassed over the years, much of it learnt the hard way (experimentation and suchlike). Take the advice below to heart and keep it in a part of your brain that has a quick access route so you can apply it without thinking about it. I am sure you will find things to disagree with, and that is a good thing - you should question what you read, and strive to find better solutions. However, I have found that following these principles has made me a more effective developer and allowed other developers to build upon my work more easily.</p>

<p>The article is structured as follows:</p>

<ul><li><a href="#easynames">Call things by their name — easy, short and readable variable and function names</a></li>
<li><a href="#avoidglobals">Avoid globals</a></li>
<li><a href="#strictstyle">Stick to a strict coding style</a></li>
<li><a href="#comments">Comment as much as needed but not more</a></li>
<li><a href="#mixing">Avoid mixing with other technologies</a></li>
<li><a href="#shortcut">Use shortcut notation when it makes sense</a></li>
<li><a href="#modules">Modularize — one function per task</a></li>
<li><a href="#progressiveenhancement">Enhance progressively </a></li>
<li><a href="#configurations">Allow for configuration and translation</a></li>
<li><a href="#nesting">Avoid heavy nesting</a></li>
<li><a href="#loops">Optimize loops</a></li>
<li><a href="#dom">Keep DOM access to a minimum</a></li>
<li><a href="#browsercode">Don’t yield to browser whims</a></li>
<li><a href="#filter">Don’t trust any data </a></li>
<li><a href="#add">Add functionality with JavaScript, don’t create too much content</a></li>
<li><a href="#libraries">Build on the shoulders of giants</a></li>
<li><a href="#liveanddev">Development code is not live code</a></li></ul>

<h2 id="easynames">Call things by their name — easy, short and readable variable and function names</h2>

<p>This is a no-brainer but it is scary how often you will come across variables like <code>x1</code>, <code>fe2</code> or <code>xbqne</code> in JavaScript, or — on the other end of the spectrum — variable names like <code>incrementorForMainLoopWhichSpansFromTenToTwenty</code> or <code>createNewMemberIfAgeOverTwentyOneAndMoonIsFull</code>.</p>

<p>None of these make much sense — good variable and function names should be easy to understand and tell you what is going on — not more and not less. One trap to avoid is marrying values and functionality in names. A function called <code>isLegalDrinkingAge()</code> makes more sense than <code>isOverEighteen()</code> as the legal drinking age varies from country to country, and there are other things than drinking to consider that are limited by age.</p>

<p><strong>Hungarian notation</strong> is a good variable naming scheme to adopt (there are <a href="http://en.wikipedia.org/wiki/Identifier_naming_convention#Metadata_and_hybrid_conventions">other naming schemes</a> to consider), the advantage being that you know what something is supposed to be and not just what it is.</p>

<p>For example, if you have a variable called <code>familyName</code> and it is supposed to be a string, you would write it as <code>sFamilyName</code> in “Hungarian”. An object called <code>member</code> would be <code>oMember</code> and a Boolean called <code>isLegal</code> would be <code>bIsLegal</code>.It is very informative for some, but seems like extra overhead to others — it is really up to you whether you use it or not.</p>

<p>Keeping to English is a good idea, too. Programming languages are in English, so why not keep this as a logical step for the rest of your code. Having spent some time debugging Korean and Slovenian code, I can assure you it is not much fun for a non-native speaker. </p>

<p>See your code as a narrative. If you can read line by line and understand what is going on, well done. If you need to use a sketchpad to keep up with the flow of logic, then your code needs some work. Try reading Dostojewski if you want a comparison to the real world — I got lost on a page with 14 Russian names, 4 of which were pseudonyms. Don&#39;t write code like that — it might make it more art than product, but this is rarely a good thing.</p>

<h2 id="avoidglobals">Avoid globals</h2>

<p>Global variables and function names are an incredibly bad idea. The reason is that every JavaScript file included in the page runs in the same scope. If you have global variables or functions in your code, scripts included after yours that contain the same variable and function names will overwrite your variables/functions.</p>

<p>There are several workarounds to avoid using globals — we’ll go through them one by one now. Say you have three functions and a variable like this:</p>

<pre><code>var current = null;
function init(){...}
function change(){...}
function verify(){...}</code></pre>

<p>You can protect those from being overwritten by using an object literal:</p>

<pre><code>var myNameSpace = {
  current:null,
  init:function(){...},
  change:function(){...},
  verify:function(){...}
}</code></pre>

<p>This does the job, but there is a drawback — to call the functions or change the variable value you’ll always need to go via the name of the main object: <code>init()</code> is <code>myNameSpace.init()</code>, current is <code>myNameSpace.current</code> and so on. This can get annoying and repetitive.</p> 

<p>It is easier to wrap the whole thing in an anonymous function and protect the scope that way. That also means you don’t have to switch syntax from <code>function name()</code> to <code>name:function()</code>. This trick is called the Module Pattern:</p>

<pre><code>myNameSpace = function(){
  var current = null;
  function init(){...}
  function change(){...}
  function verify(){...}
}();</code></pre>

<p>Again, this appproach is not without issues. None of these are available from the outside at all any more. If you want to make them available you need to wrap the things you want to make public in a <code>return</code> statement:</p>

<pre><code>myNameSpace = function(){
  var current = null;
  function verify(){...}
  return{
    init:function(){...}
    change:function(){...}
  }
}();</code></pre>

<p>That pretty much brings us back to square one in terms of linking from one to the other and changing syntax. I therefore prefer to do something like the following (which I dubbed the “revealing module pattern”):</p>

<pre><code>myNameSpace = function(){
  var current = null;
  function init(){...}
  function change(){...}
  function verify(){...}
  return{
    init:init,
    change:change
  }
}();</code></pre>

<p>Instead of returning the properties and methods I just return pointers to them. This makes it easy to call functions and access variables from other places without having to go through the <code>myNameSpace</code> name.</p> 

<p>It also means that you can have a public alias for a function in case you want to give it a longer, descriptive name for internal linking but a shorter one for the outside:</p>

<pre><code>myNameSpace = function(){
  var current = null;
  function init(){...}
  function change(){...}
  function verify(){...}
  return{
    init:init,
    set:change
  }
}();</code></pre>

<p>Calling <code>myNameSpace.set()</code> will now invoke the <code>change()</code> method.</p> 

<p>If you don’t need any of your variables or functions to be available to the outside, simply wrap the whole construct in another set of parentheses to execute it without assigning any name to it:</p>

<pre><code>(function(){
  var current = null;
  function init(){...}
  function change(){...}
  function verify(){...}
})();</code></pre>

<p>This keeps everything in a tidy little package that is inaccessible to the outside world, but very easy to share variables and functions inside of.</p>

<h2 id="strictstyle">Stick to a strict coding style</h2>

<p>Browsers are very forgiving when it comes to JavaScript syntax. This should not however be a reason for you to write sloppy code that relies on browsers to make it work.</p> 

<p>The easiest way to check the syntactical quality of your code is to run it through <a href="http://www.jslint.com/">JSLint — a JavaScript validation tool</a> that gives you a detailed report about the syntax warnings and their meaning. People have been writing extensions for editors (for example the <a href="http://andrewdupont.net/2006/10/01/javascript-tools-textmate-bundle/">JS Tools for TextMate</a>) that automatically lint your code when you save it.</p> 

<p>JSLint can be a bit touchy about the results it returns and — as its developer Douglas Crockford says — it can hurt your feelings. I found myself write much better code however, since I installed the TextMate JS bundle and started subjecting my code to JSLint scrutiny.</p> 

<p>Clean and valid code means less confusing bugs to fix, easier handover to other developers and better code security. When you rely on hacks to get your code to work it is likely that there is also a security exploit that uses the same hacks. In addition, as hacks get fixed in browsers, your code will cease to work in the next version of the browser.</p> 

<p>Valid code also means that it can be converted by scripts to other formats — hacky code will need a human to do that. </p>

<h2 id="comments">Comment as much as needed but not more</h2>

<p>Comments are your messages to other developers (and yourself, if you come back to your code after several months working on something else). There’s been numerous battles raging over the years about whether to use comments at all, the main argument being that good code should explain itself. </p>

<p>What I see as a flaw in this argument is that explanations are a very subjective thing — you cannot expect every developer to understand what some code is doing from exactly the same explanation.</p>

<p class="note">Comments don’t hurt anybody if you do them right. We’ll come back to that in the last point of this article, but let’s say that if your comments end up in the code that end users see then something is not going right.  </p>

<p>Again the trick is moderation. Comment when there is an important thing to say, and if you do comment use the <code>/* */</code> notation. Single line comments using <code>//</code> can be problematic if people minify your code without stripping comments and in general are less versatile.</p>

<p>If you comment out parts of your code to be used at a later stage or to debug code there is a pretty sweet trick you can do:</p>

<pre><code>module = function(){
  var current = null;
  function init(){
  };
/*
  function show(){
    current = 1;
  };
  function hide(){
    show();
  };
*/
  return{init:init,show:show,current:current}
}();</code></pre>

<p>Adding a double slash before the closing star-slash sets your code up so that you can comment and uncomment the whole block by simply adding or removing a slash before the opening slash-star:</p>

<pre><code>module = function(){
  var current = null;
  function init(){
  };
/*
  function show(){
    current = 1;
  };
  function hide(){
    show();
  };
// */
  return{init:init,show:show,current:current}
}();</code></pre>

<p>With the code set up as shown in the above block, adding a slash before the opening slash-star will turn the multiline comment into two one-line comments, “unhiding” the code in between and causing it to be executed. Removing the slash will comment it out again.</p> 

<p class="note">For larger applications comment documentation in <a href="http://java.sun.com/j2se/javadoc/writingdoccomments/">JavaDoc style</a> makes a lot of sense — you are seeding the overall documentation of your product by writing code. The Yahoo User Interface library&#39;s success is partly attributable to this, and there is even <a href="http://yuiblog.com/blog/2008/12/08/yuidoc/">a tool you can use to build the same documentation for your products</a>. Don’t worry too much about this until you become more experienced with JavaScripting — JavaDoc is mentioned here for completeness.</p>

<h2 id="mixing">Avoid mixing with other technologies</h2>

<p>Whilst it is possible to create everything you need in a document using JavaScript and the DOM it is not necessarily the most effective way of doing so. The following code puts a red border around every input field when its class is “mandatory” and there’s nothing in it.</p>

<pre><code>var f = document.getElementById(&#39;mainform&#39;);
var inputs = f.getElementsByTagName(&#39;input&#39;);
for(var i=0,j=inputs.length;i&lt;j;i++){
  if(inputs[i].className === &#39;mandatory&#39; &amp;&amp;
     inputs[i].value === &#39;&#39;){
    inputs[i].style.borderColor = &#39;#f00&#39;;
    inputs[i].style.borderStyle = &#39;solid&#39;;
    inputs[i].style.borderWidth = &#39;1px&#39;;
  }
}</code></pre>

<p>This works, however it means that if you later need to make a change to these styles you need to go through the JavaScript and apply the changes there. The more complex the change is the harder it’ll be to edit this. Furthermore, not every JavaScript developer is proficient or interested in CSS, which means there’ll be a lot of back and forth until the outcome is reached. By adding a class called “error” to the element when there is an error, you can ensure that the styling information is kept inside the CSS, which is more appriate:</p>

<pre><code>var f = document.getElementById(&#39;mainform&#39;);
var inputs = f.getElementsByTagName(&#39;input&#39;);
for(var i=0,j=inputs.length;i&lt;j;i++){
  if(inputs[i].className === &#39;mandatory&#39; &amp;&amp;
     inputs[i].value === &#39;&#39;){
    inputs[i].className += &#39; error&#39;;
  }
}</code></pre>

<p>This is much more efficient as CSS was meant to cascade through the document. Say for example you want to hide all DIVs with a certain class in a document. You could loop through all the DIVs, check their classes and then change their style collection. In newer browsers you could use a CSS selector engine and then alter the style collection. The easiest way however is to use JavaScript to set a class on a parent element and use syntax along the lines of <code>element.triggerclass div.selectorclass{}</code> in the CSS. Keep the job of actually hiding the DIVs to the CSS designer, as he’ll know the best way of doing that.</p>

<h2 id="shortcut">Use shortcut notation when it makes sense</h2>

<p>Shortcut notation is a tricky subject: on the one hand it keeps your code small but on the other you might make it hard for developers that take over from you as they might not be aware of the shortcuts. Well, here’s a small list of what can (and should) be done.</p>

<p>Objects are probably the most versatile thing you have in JavaScript. The old-school way of writing them is doing something like this:</p>

<pre><code>var cow = new Object();
cow.colour = &#39;brown&#39;;
cow.commonQuestion = &#39;What now?&#39;;
cow.moo = function(){
  console.log(&#39;moo&#39;);
}
cow.feet = 4;
cow.accordingToLarson = &#39;will take over the world&#39;;</code></pre>

<p>However, this means you need to repeat the object name for each property or method, which can be annoying. Instead it makes much more sense to have the following construct, also called an object literal:</p>

<pre><code>var cow = {
  colour:&#39;brown&#39;,
  commonQuestion:&#39;What now?&#39;,
  moo:function(){
    console.log(&#39;moo);
  },
  feet:4,
  accordingToLarson:&#39;will take over the world&#39;
};</code></pre>

<p>Arrays are a confusing point in JavaScript. You’ll find a lot of scripts defining an Array in the following way:</p>

<pre><code>var aweSomeBands = new Array();
aweSomeBands[0] = &#39;Bad Religion&#39;;
aweSomeBands[1] = &#39;Dropkick Murphys&#39;;
aweSomeBands[2] = &#39;Flogging Molly&#39;;
aweSomeBands[3] = &#39;Red Hot Chili Peppers&#39;;
aweSomeBands[4] = &#39;Pornophonique&#39;;</code></pre>

<p>This is a lot of useless repetition; this can be written a lot more quickly using the <code>[ ]</code> array shortcut:</p>

<pre><code>var aweSomeBands = [
  &#39;Bad Religion&#39;,
  &#39;Dropkick Murphys&#39;,
  &#39;Flogging Molly&#39;,
  &#39;Red Hot Chili Peppers&#39;,
  &#39;Pornophonique&#39;
];</code></pre>

<p>You will come across the term “associative array” in some tutorials. This is a misnomer as arrays with named properties rather than an index are actually objects and should be defined as such.</p>

<p>Conditions can be shortened using “ternary notation”. For example, the following construct defines a variable as 1 or -1, depending on the value of another variable:</p>

<pre><code>var direction;
if(x &gt; 100){
  direction = 1;
} else {
  direction = -1;
}</code></pre>

<p>This can be shortened to a single line:</p>

<pre><code>var direction = (x &gt; 100) ? 1 : -1;</code></pre>

<p>Anything before the question mark is the condition, the value immediately after it is the true case and the one after the colon the false case. Ternary notation can be nested, but I’d avoid that to keep things readable.</p>

<p>Another common situation in JavaScript is providing a preset value for a variable if it is not defined, like so:</p>

<pre><code>if(v){
  var x = v;
} else {
  var x = 10;
}</code></pre>

<p>The shortcut notation for this is the double pipe character:</p>

<pre><code>var x = v || 10;</code></pre>

<p>This will automatically give <code>x</code> a value of <code>10</code> if <code>v</code> is not defined — simple as that.</p>

<h2 id="#modules">Modularize — one function per task</h2>

<p>This is a general programming best practice — making sure that you create functions that fulfill one job at a time makes it easy for other developers to debug and change your code without having to scan through all the code to work out what code block performs what function.</p>

<p>This also applies to creating helper functions for common tasks. If you find yourself doing the same thing in several different functions then it is a good idea to create a more generic helper function instead, and reuse that functionality where it is needed.</p> 

<p>Also, one way in and one way out makes more sense than forking the code in the function itself. Say you wanted to write a helper function to create new links. You could do it like this:</p>


<pre><code>function addLink(text,url,parentElement){
  var newLink = document.createElement(&#39;a&#39;);
  newLink.setAttribute(&#39;href&#39;,url);
  newLink.appendChild(document.createTextNode(text));
  parentElement.appendChild(newLink);
}</code></pre>

<p>This works ok, but you might find yourself having to add different attributes depending on which elements you apply the link to. For example:</p>

<pre><code>function addLink(text,url,parentElement){
  var newLink = document.createElement(&#39;a&#39;);
  newLink.setAttribute(&#39;href&#39;,url);
  newLink.appendChild(document.createTextNode(text));
  if(parentElement.id === &#39;menu&#39;){
    newLink.className = &#39;menu-item&#39;;
  }
  if(url.indexOf(&#39;mailto:&#39;)!==-1){
    newLink.className = &#39;mail&#39;;
  }
  parentElement.appendChild(newLink);
}</code></pre>

<p>This makes the function more specific and harder to apply to different situations. A cleaner way is to return the link and cover the extra cases in the main functions that need them. This turns <code>addLink()</code> into the more generic <code>createLink()</code>:</p>

<pre><code>function createLink(text,url){
  var newLink = document.createElement(&#39;a&#39;);
  newLink.setAttribute(&#39;href&#39;,url);
  newLink.appendChild(document.createTextNode(text));
  return newLink;
}

function createMenu(){
  var menu = document.getElementById(&#39;menu&#39;);
  var items = [
    {t:&#39;Home&#39;,u:&#39;index.html&#39;},
    {t:&#39;Sales&#39;,u:&#39;sales.html&#39;},
    {t:&#39;Contact&#39;,u:&#39;contact.html&#39;}
  ];
  for(var i=0;i&lt;items.length;i++){
    var item = createLink(items.t,items.u);
    item.className = &#39;menu-item&#39;;
    menu.appendChild(item);
  }
}</code></pre>

<p>By having all your functions only perform one task you can have a main <code>init()</code> function for your application that contains all the application structure. That way you can easily change the application and remove functionality without having to scan the rest of the document for dependencies.</p>

<h2 id="progressiveenhancement">Enhance progressively</h2>

<p>Progressive Enhancement as a development practice is discussed in detail in the <a href="http://dev.opera.com/articles/view/graceful-degradation-progressive-enhancement/">Graceful degredation versus progressive enhancement</a>. In essence what you should do is write code that works regardless of available technology. In the case of JavaScript, this means that when scripting is not available (say on a BlackBerry, or because of an over-zealous security policy) your web products should still allow users to reach a certain goal, not block them because of the lack of JavaScript which they can’t turn on, or don’t want to.</p>

<p>It is amazing how many times you will build a massively convoluted JavaScript solution for a problem that can be solved easily without it. One example I encountered was a search box on a page that allowed you to search different data: the web, images, news and so on.</p> 

<p>In the original version the different data options were links that would re-write the <code>action</code> attribute of the form to point to different scripts on the backend to perform the searches.</p> 

<p>The problem was that if JavaScript was turned off the links would still show up but every search would return standard web results as the action of the form never got changed. The solution was very simple: instead of links we provided the options as a radio button group and did the forking to the different specialist search scripts using a backend script. </p>

<p>This not only made the search work correctly for everybody, it also made it easy to track how many users chose which option. By using the correct HTML construct we managed to get rid of both the JavaScript to switch the form action and the click tracking scripts and made it work for every user out there — regardless of environment.</p>

<h2 id="configurations">Allow for configuration and translation</h2>

<p>One of the most successful tips to keep your code maintainable and clean is to create a configuration object that contains all the things that are likely to change over time. These include any text used in elements you create (including button values and alternative text for images), CSS class and ID names and general parameters of the interface you build.</p> 

<p>For example the <a href="http://icant.co.uk/easy-youtube">Easy YouTube player</a> has the following configuration object:</p>

<pre><code>/*
  This is the configuration of the player. Most likely you will
  never have to change anything here, but it is good to be able 
  to, isn&#39;t it? 
*/
config = {
  CSS:{
    /* 
      IDs used in the document. The script will get access to 
      the different elements of the player with these IDs, so 
      if you change them in the HTML below, make sure to also 
      change the name here!
    */
    IDs:{
      container:&#39;eytp-maincontainer&#39;,
      canvas:&#39;eytp-playercanvas&#39;,
      player:&#39;eytp-player&#39;,
      controls:&#39;eytp-controls&#39;,

      volumeField:&#39;eytp-volume&#39;,
      volumeBar:&#39;eytp-volumebar&#39;,

      playerForm:&#39;eytp-playerform&#39;,
      urlField:&#39;eytp-url&#39;,

      sizeControl:&#39;eytp-sizecontrol&#39;,

      searchField:&#39;eytp-searchfield&#39;,
      searchForm:&#39;eytp-search&#39;,
      searchOutput:&#39;eytp-searchoutput&#39;
      /* 
        Notice there should never be a comma after the last 
        entry in the list as otherwise MSIE will throw  a fit!
      */
    },
    /*
      These are the names of the CSS classes, the player adds
      dynamically to the volume bar in certain 
      situations.
    */
    classes:{
      maxvolume:&#39;maxed&#39;,
      disabled:&#39;disabled&#39;
      /* 
        Notice there should never be a comma after the last 
        entry in the list as otherwise MSIE will throw  a fit!
      */
    }
  },
  /* 
    That is the end of the CSS definitions, from here on 
    you can change settings of the player itself. 
  */
  application:{
    /*
      The YouTube API base URL. This changed during development of this,
      so I thought it useful to make it a parameter.
    */
    youtubeAPI:&#39;http://gdata.youtube.com/apiplayer/cl.swf&#39;,
    /* 
      The YouTube Developer key,
      please replace this with your own when you host the player!!!!!
    */
    devkey:&#39;AI39si7d...Y9fu_cQ&#39;,
    /*
      The volume increase/decrease in percent and the volume message 
      shown in a hidden form field (for screen readers). The $x in the 
      message will be replaced with the real value.
    */
    volumeChange:10,
    volumeMessage:&#39;volume $x percent&#39;,
    /*
      Amount of search results and the error message should there 
      be no reults.
    */
    searchResults:6,
    loadingMessage:&#39;Searching, please wait&#39;,
    noVideosFoundMessage:&#39;No videos found : (&#39;,
    /*
      Amount of seconds to repeat when the user hits the rewind 
      button.
    */
    secondsToRepeat:10,
    /*
      Movie dimensions.
    */
    movieWidth:400,
    movieHeight:300
    /* 
      Notice there should never be a comma after the last 
      entry in the list as otherwise MSIE will throw  a fit!
    */
  }  
}</code></pre>

<p>If you have this as a part of a module pattern and make it public you even allow implementers to only override what they need before initializing your module.</p>

<p>It is of utmost importance to keep code maintenance simple, avoiding the need for future maintainers having to read all your code and find where they need to change things. If it isn’t obvious, your solution will be either completely ditched or hacked. Hacked solutions can’t be patched once you need to upgrade them and that kills re-use of code.</p>

<h2 id="nesting">Avoid heavy nesting</h2>

<p>Nesting code explains its logic and makes it much easier to read, however nesting it too far can also make it hard to follow what you are trying to do. Readers of your code shouldn’t have to scroll horizontally, or suffer confusion when their code editors wrap long lines (this makes your indentation efforts moot anyway).</p>

<p>The other problem of nesting is variable names and loops. As you normally start your first loop with <code>i</code> as the iterator variable, you’ll go on with j,k,l and so on. This can become messy quite quickly:</p>

<pre><code>function renderProfiles(o){
  var out = document.getElementById(&#x2018;profiles&#x2019;);
  for(var i=0;i&lt;o.members.length;i++){
    var ul = document.createElement(&#x2018;ul&#x2019;);
    var li = document.createElement(&#x2018;li&#x2019;);
    li.appendChild(document.createTextNode(o.members[i].name));
    var nestedul = document.createElement(&#x2018;ul&#x2019;);
    for(var j=0;j&lt;o.members[i].data.length;j++){
      var datali = document.createElement(&#x2018;li&#x2019;);
      datali.appendChild(
        document.createTextNode(
          o.members[i].data[j].label + &#x2018; &#x2018; +
          o.members[i].data[j].value
        )
      );
      nestedul.appendChild(datali);
    }
    li.appendChild(nestedul);
  } 
  out.appendChild(ul);
}</code></pre>

<p>As I am using the generic — really throw-away — variable names <code>ul</code> and <code>li</code> here, I need <code>nestedul</code> and <code>datali</code> for the nested list items. If the list nesting were to go even deeper I would need more variable names, and so on and so on. It makes more sense to put the task of creating nested lists for each member in its own function and call this with the right data. This also prevents us from having a loop inside a loop. The <code>addMemberData()</code> function is pretty generic and is very likely to come in handy at another time. Taking these thoughts on board, I would rewrite the code as follows:</p>

<pre><code>function renderProfiles(o){
  var out = document.getElementById(&#x2018;profiles&#x2019;);
  for(var i=0;i&lt;o.members.length;i++){
    var ul = document.createElement(&#x2018;ul&#x2019;);
    var li = document.createElement(&#x2018;li&#x2019;);
    li.appendChild(document.createTextNode(data.members[i].name));
    li.appendChild(addMemberData(o.members[i]));
  } 
  out.appendChild(ul);
}
function addMemberData(member){
  var ul = document.createElement(&#x2018;ul&#x2019;);
  for(var i=0;i&lt;member.data.length;i++){
    var li = document.createElement(&#x2018;li&#x2019;);
    li.appendChild(
      document.createTextNode(
        member.data[i].label + &#x2018; &#x2018; +
        member.data[i].value
      )
    );
  }
  ul.appendChild(li);
  return ul;
}</code></pre>

<h2 id="loops">Optimize loops</h2>

<p>Loops can become very slow if you don’t do them right. One of the most common mistake is to read the length attribute of an array at every iteration:</p>

<pre><code>var names = [&#39;George&#39;,&#39;Ringo&#39;,&#39;Paul&#39;,&#39;John&#39;];
for(var i=0;i&lt;names.length;i++){
  doSomeThingWith(names[i]);
}</code></pre>

<p>This means that every time the loop runs, JavaScript needs to read the length of the array. You can avoid that by storing the length value in a different variable:</p>

<pre><code>var names = [&#39;George&#39;,&#39;Ringo&#39;,&#39;Paul&#39;,&#39;John&#39;];
var all = names.length;
for(var i=0;i&lt;all;i++){
  doSomeThingWith(names[i]);
}</code></pre>

<p>An even shorter way of achieving this is to create a second variable in the pre-loop statement:</p>

<pre><code>var names = [&#39;George&#39;,&#39;Ringo&#39;,&#39;Paul&#39;,&#39;John&#39;];
for(var i=0,j=names.length;i&lt;j;i++){
  doSomeThingWith(names[i]);
}</code></pre>

<p>Another thing to ensure is that you keep computation-heavy code outside loops. This includes regular expressions and — more importantly — DOM manipulation. You can create the DOM nodes in the loop but avoid inserting them into the document. You’ll find more on DOM best practices in the next section.</p>

<h2 id="dom">Keep DOM access to a minimum</h2>

<p>Accessing the DOM in browsers is an expensive thing to do. The DOM is a very complex API and rendering in browsers can take up a lot of time. You can see this when running complex web applications when your computer is already maxed out with other work — changes take longer or get shown half way through and so on.</p> 

<p>To make sure that your code is fast and doesn’t slow down the browser to a halt try to keep DOM access to a bare minimum. Instead of constantly creating and applying elements, have a tool function that turns a string into DOM elements and call this function at the end of your generation process to disturb the browser rendering once rather than continually.</p>

<h2 id="browsercode">Don’t yield to browser whims</h2>

<p>Writing code specific to a certain browser is a sure-fire way to keep your code hard to maintain and make it get dated really quickly. If you look around the web you’ll find a lot of scripts that expect a certain browser and stop working as soon as a new version or another browser comes around. </p>

<p>This is wasted time and effort — we should build code based on agreed standards as outlined in this course of articles, not for one browser. The web is for everybody, not an elite group of users with a state-of-the-art configuration. As the browser market moves quickly you will have to go back to your code and keep fixing it. This is neither effective nor fun.</p>

<p>If something amazing works in one browser only and you really have to use it, put that code in its own script document and name it with browser and version. This means that you can find and remove this functionality more easily, should this browser become obsolete.</p>

<h2 id="filter">Don’t trust any data</h2>

<p>One of the main points to bear in mind when talking about code and data security is not to trust any data. This is not only about evil people wanting to hack your systems; it starts with plain usability. Users will enter incorrect data, all the time. Not because they are stupid, but because they are busy, distracted or the wording on your instructions is confusing them. For example, I just booked a hotel room for a month rather than six days as I entered a wrong number … I consider myself fairly smart. </p>

<p>In short, make sure that all the data that goes into your systems is clean and exactly what you need. This is most important on the backend when writing out parameters retrieved from the URL. In JavaScript, it is very important to test the type of parameters sent to your functions (using the <code>typeof</code> keyword). The following would be an error if <code>members</code> is not an Array (for example for a string it’ll create a list item for each character of the string):</p>

<pre><code>function buildMemberList(members){
  var all = members.length;
  var ul = document.createElement(&#39;ul&#39;);
  for(var i=0;i&lt;all;i++){
    var li = document.createElement(&#39;li&#39;);
    li.appendChild(document.createTextNode(members[i].name));
    ul.appendChild(li);
  }
  return ul;
}</code></pre>

<p>In order to make this work, you need to check the type of <code>members</code> and make sure it is an array:</p>

<pre><code>function buildMemberList(members){
  if(typeof members === &#39;object&#39; &amp;&amp; 
     typeof members.slice === &#39;function&#39;){
    var all = members.length;
    var ul = document.createElement(&#39;ul&#39;);
    for(var i=0;i&lt;all;i++){
      var li = document.createElement(&#39;li&#39;);
      li.appendChild(document.createTextNode(members[i].name));
      ul.appendChild(li);
    }
    return ul;
  }
}</code></pre>

<p>Arrays are tricky as they tell you they are objects. To ensure that they are arrays, check one of the methods only arrays have.</p>

<p>Another very insecure practice is to read information from the DOM and use it without comparison. For example, I once had to debug some code that caused the JavaScript functionality to break. The code that caused it was — for some reason beyond me — reading a user name out of the innerHTML from a page element and calling a function with the data as a parameter. As the user name could be any UTF-8 character this included quotation marks and single quotes. These would end any string and the remaining part would be erroneous data. In addition, any user changing the HTML using a tool like Firebug or Opera DragonFly could change the user name to anything and inject this data into your functions.</p>

<p>The same applies to forms that validate only on the client side. I once signed up for an unavailable email address by rewriting a select to provide another option. As the form wasn’t checked on the backend the process went through without a hitch.</p>

<p>For DOM access, check that the element you try to reach and alter is really available and what you expect it to be — otherwise your code may fail or cause strange rendering bugs.</p>

<h2 id="add">Add functionality with JavaScript, don’t create too much content</h2>

<p>As you can see in some of the other examples here, building a lot of HTML in JavaScript can be pretty daunting and flaky. Especially on Internet Explorer you can run into all kinds of trouble by altering the document while it is still loading and manipulating the content (look up “operation aborted error” on <a href="http://www.google.com">Google</a> for a tale of woe and misery) with <code>innerHTML</code>.</p> 

<p>In terms of page maintenance it is also a terribly bad idea to create a lot of markup with HTML as not every maintainer will have the same level of skill as you have and could potentially really mess with your code.</p> 

<p>I found that when I had to build an application that is very much dependent on JavaScript using an HTML template and loading this template via Ajax made much more sense. That way maintainers can alter the HTML structure and most importantly text without having to interfere with your JavaScript code. The only snag is to tell them which IDs are needed and if there are certain HTML constructs that need to be in the order you defined. You can do that with inline HTML comments (and then strip the comments out when you load the template. Check the source of <a href="http://icant.co.uk/easy-youtube/template.html">the Easy YouTube template</a> as an example.</p>

<p>In the script I load the template when the correct HTML container is available and apply the event handlers in the <code>setupPlayer()</code> method afterwards:</p>

<pre><code>var playercontainer = document.getElementById(&#39;easyyoutubeplayer&#39;);
if(playercontainer){
  ajax(&#39;template.html&#39;);
}; 

function ajax(url){
  var request;
  try{
    request = new XMLHttpRequest();
  }catch(error){
    try{
      request = new ActiveXObject(&quot;Microsoft.XMLHTTP&quot;);
    }catch(error){
      return true;
    }
  }
  request.open(&#39;get&#39;,url,true);
  request.onreadystatechange = function(){
    if(request.readyState == 4){
      if(request.status){ 
        if(request.status === 200 || request.status === 304){
          if(url === &#39;template.html&#39;){
            setupPlayer(request.responseText);
          }
        }
      }else{
        alert(&#39;Error: Could not find template...&#39;);
      }
    }
  };
  request.setRequestHeader(&#39;If-Modified-Since&#39;,&#39;Wed, 05 Apr 2006 00:00:00  GMT&#39;);
  request.send(null);
};</code></pre>

<p>This way I enable people to translate and change the player any way they want to without having to alter the JavaScript code.</p>

<h2 id="libraries">Build on the shoulders of giants</h2>

<p>There is no denying that over the last few years JavaScript libraries and frameworks have taken over the web development market. And that is not a bad thing — if they are used correctly. All good JavaScript libraries want to do one thing and one thing only: make your life as a developer easier by working around cross-browser inconsistencies and patching browser support holes. JavaScript libraries provide you with a predictable, functioning base line to build upon.</p> 

<p>It’s a good idea to learn JavaScript without libraries first, so you really know what’s going on, but you should make use of a JS library when you start developing web sites. You’ll have less issues to deal with and at least the bugs that appear will be ones that can be reproduced — not random browser issues.</p>

<p>My personal favourite is <a href="http://developer.yahoo.com/yui">the Yahoo User Interface library (YUI)</a>, followed by <a href="http://jquery.com/">jQuery</a>, <a href="http://www.dojotoolkit.org/">Dojo</a> and <a href="http://www.prototypejs.org/">Prototype</a> but there are dozens of other good libraries out there and you should find the one that suits you and your product best.</p>

<p>Whilst all libraries do work well together, it is not a good idea to use several libraries in the same project. This brings in another superfluous level of complexity and maintenance.</p>

<h2 id="liveanddev">Development code is not live code</h2>

<p>The last point I want to make is not about JavaScript itself but about how it fits into the rest of your development strategy. As any change in JavaScript has an immediate effect on the performance and functionality of your web applications it is very tempting to optimize your code as much as possible regardless of the consequences for maintenance.</p> 

<p>There are a lot of clever tricks you can apply to JavaScript to make it perform great. Most of them come with the drawback of making your code hard to understand and maintain.</p>

<p>In order to write secure, working JavaScript we need to break this cycle and stop optimizing code for machines rather than other developers. Most — something that is very common in other languages but not as well known amongts JavaScripters. A build script can remove whitespace, comments, replace strings with Array lookups (to avoid MSIE creating a string object for every single instance of a string — even in conditions) and do all the other small tweaks needed to make our JavaScript fly in browsers.</p> 

<p>If we concentrate more on making the initial code easy to understand and extend by other developers we can create the perfect build script. If we keep optimizing prematurely we’ll never get there. Do not build for yourself or the browser — build for the next developer who takes over from you.</p>


<h2>Summary</h2>

<p>The main trick with JavaScript is to avoid taking the easy path. JavaScript is a wonderfully versatile language and as the environment it is executed in is very forgiving it is easy to write sloppy code that seemingly does the job. This same code however will come back to bite you a few months down the line.</p>

<p>JavaScript development has mutated from a fringe knowledge area to an absolute necessity if you want to have a job as a web developer. If you are starting right now you are lucky, as myself and many others have already made most of the mistakes and done all the trial and error self-teaching; we can now pass that knowledge along.</p>

<ul class="seriesNav">
<li class="prev"><a href="http://dev.opera.com/articles/view/first-look-at-javascript/" rel="prev" title="link to the previous article in the series">Previous article—Your first look at JavaScript</a></li>
<li class="next"><a href="http://dev.opera.com/articles/view/unobtrusive-javascript/" rel="next" title="link to the next article in the series">Next article—The principles of unobtrusive JavaScript</a></li>
<li><a href="http://dev.opera.com/articles/view/1-introduction-to-the-web-standards-cur/#toc" rel="index">Table of contents</a></li>
</ul>

<h2>About the author</h2>

<div class="right">

<img src="http://forum-test.oslo.osa/kirby/content/articles/243-42-javascript-best-practices/chrisheilmann.jpg" alt="Picture of the article author Chris Heilmann" />
<p class="comment">Photo credit: <a href="http://www.flickr.com/photos/bluesmoon/1545636474/">Bluesmoon</a></p>

</div>

<p>Chris Heilmann has been a web developer for ten years, after dabbling in radio journalism. He works for Yahoo! in the UK as trainer and lead developer, and oversees the code quality on the front end for Europe and Asia.</p>

<p>Chris blogs at <a href="http://wait-till-i.com">Wait till I come</a>  and is available on many a social network as &#x201C;codepo8&#x201D;.</p>
        