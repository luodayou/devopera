Title: BrowserWindow.private
----
Date: 2012-04-25 08:07:13
----
Lang: en
----
Author: 
----
License: Creative Commons Attribution 3.0 Unported
----
License_url: http://creativecommons.org/licenses/by/3.0/
----
Text:

<h2>Description:</h2>

<h3>Attribute</h3>

<p>The readonly <code>private</code> attribute exposes the privacy mode of the browser window. On getting, the private attribute returns <code>true</code> if the privacy mode of the browser window is private, otherwise it returns <code>false</code>.</p>

<h3>Property</h3>

<p>When specified as an item in a <code>BrowserWindowProperties</code> object, the <code>private</code> property indicates the desired privacy mode of a browser tab. The value <code>true</code> indicates that the window should be private. The value <code>false</code> indicates that the privacy mode should be set to normal.</p>

<p>When <b>creating</b> a browser window, if this property is not specified, the default behaviour is the same as specifying <code>false</code>.</p>

<p>When <b>updating</b> a browser window, this property has no effect, whether specified or not. In other words, the privacy mode remains unchanged.</p>

<h2>Syntax:</h2>

<h3>Attribute</h3>

<p><code>readonly boolean private</code></p>

<h3>Property</h3>

<p><code>boolean private</code></p>

<h2>Example:</h2>

<p>The following example creates a button in the browser toolbar. When the button is clicked, the last focused window is detected. A new tab is then created with different URL depending on whether the window is private or not.</p>

<pre><code>//
// The background process (e.g. index.html) 
//

// Specify the properties of the button before creating it.
var UIItemProperties = {
  disabled: false,
  title: &quot;Window creation test&quot;,
  icon: &quot;images/icon_18.png&quot;,
  onclick: function() {
    // Get the last focused window
    var thisWin = opera.extension.windows.getLastFocused();
    
    // Create an empty tab properties object
    var tabProps = {};
    
    // Add a tab URL depending on whether the window is private or not
    if (thisWin.private) {
        tabProps.url = &#39;http://www.facebook.com/&#39;;
    } else {
        tabProps.url = &#39;http://www.wikipedia.org/&#39;;
    }
    
    // Open a new tab with the relevant URL
    opera.extension.tabs.create(tabProps);
  }
};

// Create the button and add it to the toolbar.
var button = opera.contexts.toolbar.createItem( UIItemProperties );  
opera.contexts.toolbar.addItem(button);</code></pre>
