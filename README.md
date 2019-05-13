###  API Usage in PHP

An API key is required for requests to be processed by the system. Once a user registers, an API key is automatically generated for this user. The API key must be sent with each request via the key parameter (see full example below). If the API key is not sent or is expired, there will be an error. Please make sure to keep your API key secret to prevent abuse.

For more information, please visit:<https://url.filecloud.fun> OR <https://filecloud.fun>

**Your API key**

<pre class="code"><span>Look in the developer API under tools after logging in
</span></pre>

* * *

##### Sending a request for shortening a URL

To send a request, the user must use the following format where the variables api and url are required. In the example below, the URL of the demo is used but you should use your own domain name. To request a custom alias, simply add &amp;custom= at the end.

<pre class="code"><span>GET https://url.filecloud.fun/api/?key=YOUR-API-KEY&amp;url=THELONGURLTOBESHORTENED&amp;custom=CUSTOMALIAS</span></pre>

##### For premium members only

<pre class="code"><span>GET https://url.filecloud.fun/api/?key=YOUR-API-KEY&amp;url=THELONGURLTOBESHORTENED&amp;custom=CUSTOMALIAS&amp;type=REDIRECTYPE</span></pre>

* * *

##### Server response

As before, the response will encoded in JSON format (default). This is done to facilitate cross-language usage. The first element of the response will always tell if an error has occurred (error: 1) or not (error: 0). The second element will change with respect to the first element. If there is an error, the second element will be named 'msg'. which contains the source of error, otherwise it will be named “short” which contains the short URL. (See below for an example)

**No errors**

<pre class="code"><span>{</span><span class="m-x-4">"error":0,</span><span class="m-x-4">"short":"https:\/\/url.filecloud.fun\/DkZOb"</span><span>}</span></pre>

**An error has occurred**

<pre class="code"><span>{</span><span class="m-x-4">"error":1,</span><span class="m-x-4">"msg":"Please enter a valid url"</span><span> }</span></pre>

**Using plain text format**

You can now request the response to be in plain text by just adding &amp;format=text at the end of your request. Note that if an error occurs, it will not output anything so you can assume if it is empty then there is an error.

* * *

**Using the API in PHP**

To use the API in your PHP application, you have to send a GET request through file_get_contents or cURL: Both are reliable methods. You can copy the function below. Everything is already set up for you.

<pre class="code"><span>&lt;?php</span><span class="m-x-3">
/**** Sample PHP Function ***/
</span><span class="m-x-3">
function shorten($url, $custom = "", $format = "json") 
{ </span><span class="m-x-4">
$api_url = "https://url.filecloud.fun/api/?key=YOUR-API-KEY";
</span><span class="m-x-4">
$api_url .= "&amp;url=".urlencode(filter_var($url, FILTER_SANITIZE_URL));
</span><span class="m-x-4">
if(!empty($custom)){</span><span class="m-x-5">
$api_url .= "&amp;custom=".strip_tags($custom);
</span><span class="m-x-4">}</span><span class="m-x-4">
$curl = curl_init();</span>
<span class="m-x-4">
curl_setopt_array($curl, array(</span><span class="m-x-5">
CURLOPT_RETURNTRANSFER =&gt; 1,</span><span class="m-x-5">CURLOPT_URL =&gt;
$api_url</span><span class="m-x-4">));</span><span class="m-x-4">
$Response = curl_exec($curl);</span><span class="m-x-4">curl_close($curl);
<span class="m-x-3"></span><span class="m-x-4">if($format == "text"){
</span><span class="m-x-5">$Ar = json_decode($Response,TRUE);
</span><span class="m-x-5">if($Ar["error"]){</span><span class="m-x-6">
return $Ar["msg"];</span><span class="m-x-5">}else{</span><span class="m-x-6">
return $Ar["short"];</span><span class="m-x-5">}</span><span class="m-x-4">
    
}else{
</span><span class="m-x-5">return $Response;
</span><span class="m-x-4">}</span><span class="m-x-3">}</span><span>?&gt;</span></span></pre>

**Simple Usage**

<pre class="code"><span>&lt;?php</span><span class="m-x-4"> echo shorten("https://google.com");</span><span>?&gt;</span></pre>

**Usage with custom alias**

<pre class="code"><span>&lt;?php</span><span class="m-x-4"> echo shorten("https://google.com", "google");</span><span>?&gt;</span></pre>

**Usage with custom alias and text format**

<pre class="code"><span>&lt;?php</span><span class="m-x-4"> echo shorten("https://google.com", "google", "text");</span><span>?&gt;</span></pre>
</div>
