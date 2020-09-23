<div align="center">

## ASP\.NET does NOT honor IIS security accounts by default\!


</div>

### Description

When working on a website that was supposed to use the new ASP.NET file upload object (great feature by the way) to upload files and then copy them to a network file server, I discovered that ASP.NET does not honor the security account that you setup in IIS by default! You have to override it to get it to do so. This article explains how and will hopefully save you some time and aggravation of your own.
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Ian Ippolito \(vWorker\)](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/ian-ippolito-vworker.md)
**Level**          |Beginner
**User Rating**    |4.8 (48 globes from 10 users)
**Compatibility**  |VB\.NET, ASP\.NET
**Category**       |[Internet/ Browsers/ HTML](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/internet-browsers-html__10-9.md)
**World**          |[\.Net \(C\#, VB\.net\)](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/net-c-vb-net.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/ian-ippolito-vworker-asp-net-does-not-honor-iis-security-accounts-by-default__10-193/archive/master.zip)





### Source Code

I was using the following simple code to copy a file.&nbsp; It would work
when copied to a local drive:</p>
<p><font SIZE="2" COLOR="#0000ff">Dim</font> strSourceFile, strDestFile
<font SIZE="2" COLOR="#0000ff">As</font> <font SIZE="2" COLOR="#0000ff">String<br>
</font>strSourceFile = &quot;<a href="file:///c:/temp/temp.txt">c:\temp\temp.txt</a>&quot;<br>
<font SIZE="2" COLOR="#0000ff">Dim</font> fileinfo2
<font SIZE="2" COLOR="#0000ff">As</font> <font SIZE="2" COLOR="#0000ff">New</font>
FileInfo(strSourceFile)</p>
<p>strDestFile = &quot;<a href="file://atcsrflfnp01/software/temp/temp.txt">c:\temp2.txt</a>&quot;<br>
fileinfo2.CopyTo(strDestFile, <font SIZE="2" COLOR="#0000ff">True</font>)</p>
<p>But when I changed the file path to a UNC file path, it bombed with a
permissions error.</p>
<p>strDestFile = &quot;<a href="file://atcsrflfnp01/software/temp/temp.txt">\\atcsrflfnp01\software\temp\temp.txt</a>&quot;<br>
&nbsp;</p>
<p>Setting IIS using the anonomous login account, like in ASP classic did not
improve the situation at all...I still got the same error.</p>
<p>After doing quite a bit of reading and digging through the 'help' files, I
found out that ASP.NET by default runs under its own system account, and doesn't
honor IIS's settings!<br>
<br>
Fortunately there is a simple way to fix this, once you realize the problem.&nbsp;
If you add the following to the &lt;system&gt; section of your web.config file, it
will allow you to use the IIS account. </p>
</font><font SIZE="2" COLOR="#0000ff">
<p>&lt;</font><font SIZE="2" COLOR="#800000">identity</font><font SIZE="2" COLOR="#ff00ff">
</font><font SIZE="2" COLOR="#ff0000">impersonate</font><font SIZE="2" COLOR="#0000ff">=&quot;true&quot;/&gt;</p>
</font><font SIZE="2">
<p>You can also use this method to override IIS, if you specify a name and
password paramater.</p>
</font>

