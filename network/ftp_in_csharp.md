# FTP in CSharp

<script type="text/javascript" src="../gitbook/app.js"></script>
<script type="text/javascript" src="../js/general.js"></script>

### Web.config 設定

Web.config 於微軟開發環境中常用來儲存重要的字串內容，如帳號、密碼、資料庫連接與實體路徑等字串。

```Xml
<?xml version="1.0" encoding="utf-8"?>

<!--
  如需如何設定 ASP.NET 應用程式的詳細資訊，請造訪
  http://go.microsoft.com/fwlink/?LinkId=169433
  -->

<configuration>

    <system.web>
      <compilation debug="true" targetFramework="4.5.2" />
      <httpRuntime targetFramework="4.5.2" />
    </system.web>

    <appSettings>

      <!-- production -->
      <add key="ftpUser" value="user"/>
      <add key="ftpPassword" value="password"/>
      <add key="upload_to" value="ftp://ip:port/folder/file.txt"/>
      <add key="upload_from" value="filepath\file.txt"/>

    </appSettings>
  
</configuration>
```

###FTP 上傳函式
---

使用的必要資源

```C#
using System;
using System.Collections.Generic;
using System.Data.SqlClient;
using System.IO;
using System.Linq;
using System.Net;
using System.Text.RegularExpressions;
using System.Threading;
using System.Web;
using System.Web.Configuration;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Xml;
```

從 Web.config 取得 ftp 必要上傳資料

```C#
private string ftpUser = System.Web.Configuration.WebConfigurationManager.AppSettings["ftpUser"];   // ftp user name
private string ftpPassword = System.Web.Configuration.WebConfigurationManager.AppSettings["ftpPassword"];   // ftp user password
private string upload_to = System.Web.Configuration.WebConfigurationManager.AppSettings["upload_to"];
private string upload_from = System.Web.Configuration.WebConfigurationManager.AppSettings["upload_from"];
```

ftp 上傳實作內容，執行後會回傳一個 ftp 上傳狀態字串，如下；

```C#
private String ftpUpload()
{
    // Get the object used to communicate with the server.
    FtpWebRequest request = (FtpWebRequest)WebRequest.Create(upload_to);
    request.Method = WebRequestMethods.Ftp.UploadFile;

    // Use user and its password to login the ftp server
    request.Credentials = new NetworkCredential(ftpUser, ftpPassword);

    // Copy the contents of the file to the request stream.
    StreamReader sourceStream = new StreamReader(upload_from);

    // Use UTF-8 as encoding type
    byte[] fileContents = System.Text.Encoding.UTF8.GetBytes(sourceStream.ReadToEnd());

    sourceStream.Close();

    request.ContentLength = fileContents.Length;

    // make sure access to the remote is connected
    try
    {
        Stream requestStream = request.GetRequestStream();

        requestStream.Write(fileContents, 0, fileContents.Length);

        requestStream.Close();
    }
    catch (Exception e)
    {
        return e.Message;
    }

    // get status of ftp
    FtpWebResponse response = (FtpWebResponse)request.GetResponse();

    // regular expression to check 226 status reporting
    string pattern = "226";

    // use check flag to make sure ftp uploading is complete
    int checkStatus = 0;

    foreach (Match match in Regex.Matches(response.StatusDescription, pattern, RegexOptions.IgnoreCase))
    {
        if (match.ToString().Equals(pattern)) {
            checkStatus = 1;
        }
    }

    response.Close();

    switch(checkStatus)
    {
        case 1:
            return "Status : FTP upload successfully.";
        default:
            return "Status : FTP uploading is failure.";
    }
}
```




