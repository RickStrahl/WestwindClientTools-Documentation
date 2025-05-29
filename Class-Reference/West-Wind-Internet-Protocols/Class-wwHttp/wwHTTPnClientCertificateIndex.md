If a client certificate is requested this flag determines which certificate is used. This is a zero based index. By default the first is used (0).

<div class="notebox">**Note:**  
There are no APIs to determine the available certificates through WinInet so the numeric index is unfortunately the only way that you can specify a certificate at this point</div>

For more info on the WinInet issues please see:
http://www.codeguru.com/cpp/i-n/internet/generalinternet/article.php/c3367