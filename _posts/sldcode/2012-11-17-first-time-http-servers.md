---
categories: sldcode
layout: post
title: First time HTTP servers
---

There are two things you will need to watch out for if you are to write your own small HTTP server for the first time:

- The **request** ends with a line containing exactly

  ```
  \r\n
  ```

- If the server tries to read further after the finishing `\r\n` in the request, the read will block, resulting in a useless thread doing nothing. However, this can be conveniently checked using a `BufferedReader`.

- The minimal **response** you will need to send is

  ```java
  output.print("HTTP/1.1 200 OK\r\n");
  output.print("\r\n");
  output.print(response);
  output.flush();
  output.close();
  ```

  A `PrintWriter` is a good friend here.
  
- The *OK* after the after the response code is not required, but should be written out, since [HTTP 1.1 RFC](http://www.w3.org/Protocols/rfc2616/rfc2616-sec6.html) tells us so:

  ```
  Status-Line = HTTP-Version SP Status-Code SP Reason-Phrase CRLF
  ```

  It works without a *Reason-Phrase*, but let's be pedantic here.
