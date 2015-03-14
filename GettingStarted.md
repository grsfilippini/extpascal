# Introduction #

ExtPascal is an Ext JS wrapper/binding. ExtPascal lets you use the Ext JS from Object Pascal commands issued by the server. That brings the structure and strict syntax of the Object Pascal for programming the web browser.

The main advantages are:

  * Use your preferred language (in this case Object Pascal) and where you are more productive instead of using JavaScript, PHP, HTML and/or CSS.
  * Use of native and compiled code with a strongly typed language. That is, many errors are directly appointed by the compiler, without waste time testing in the browser.
  * Use of Code Completion (Intellisense), available in moderns IDEs (Delphi/Lazarus) more productivity in the coding.
  * Centering the programming on the server, where JavaScript and HTML is generated dynamically, makes development easier and less dispersed.
  * The business rules can be encapsulated in the server, in Object Pascal, instead of being visible in JavaScript.
  * Direct and easy debugging using Delphi/Lazarus/MSEide/FPIDE debugger.

# Prerequisites #

  * Windows, Linux, Mac OS X or FreeBSD machine. It was tested using Intel/AMD 32/64 bits and PowerPC, but should work with other processors supported by Free Pascal.
  * A web browser: IE 6+, Firefox 3.6+, Chrome 10+, Safari 4+, Opera 11+ or compatibles. JavaScript and Cookies should be enabled.
  * Download and install [Apache 2.4.4 64 bit for Windows](http://www.apachelounge.com/download/win64/) or later, but Apache 1.3 and IIS works using CGIGateway included in ExtPascal package, see below.
  * For native FastCGI operation download and install [FastCGI for Apache 2.2](http://www.fastcgi.com). See details below. For setting FastCGI thru CGIGateway on Apache or IIS see details below too.
  * Download [ExtJS 3.2.1](http://extjs.com/products/extjs/download.php).
  * Install Ext JS below Apache's Document Root (usually htdocs) at folder **ext**
  * For Windows install Delphi (7..XE3) or FreePascal 2.6+ with Lazarus 1.0+ or [MSEide 2.0+](http://sourceforge.net/projects/mseide-msegui). I use DXE3 and Lazarus 1.1 64 bit (CodeTyphon edition).
  * For Posix systems (Linux, Mac OS X, BSDs, Solaris, etc) install FreePascal 2.6+ with Lazarus 1.0+.
  * Download [ExtPascal 0.9.8](http://extpascal.googlecode.com/files/ExtPascal-0.9.8.zip) to a working directory, for example: C:\ExtPascal.
  * Copy **codepress** folder, which is in ExtPascal package, underneath the **htdocs** folder.

# First Steps #
01. Open ExtPascalSamples.dpr source in ExtPascalSamples folder using your favorite ObjectPascal IDE.

02. For Lazarus set these Parsing options (menu \Project\Compiler Options\Parsing):
```
Syntax mode: 'Delphi'
ON C++ Styled INLINE
ON C Style macros
OFF Constructor name must be init
OFF Static Keyword in Objects
ON Use Ansi Strings
```

03. For Delphi set these Syntax options (menu \Project\Options\Compiler):
```
OFF Strict var-strings
OFF Complete boolean eval
ON Extended syntax 
OFF Typed @ operator
ON Open parameters
ON Huge strings/Long strings by default 
ON Assignable typed constants
```

04. For FreePascal use _**`-Smdghie50 -venwhi -l`**_  as compiler options. Append these options _**`-O2pPENTIUM4 -CXpPENTIUM4 -XXsi -CfSSE`**_  to release/final version. In Lazarus these options are not necessary, but you can use this path:
```
\Project\Project Options\Compiler Options\Other\Custom options
```

05. Set the compiler "Search path" according. By example to C:\ExtPascal; C:\ExtPascal\ExtJSWrapper
```
In Lazarus browse: \Project\Project Options\Compiler Options\Paths\Other Unit Files.
In Delphi browse: \Project\Options\Directories/Conditionals\Search path.
```

06. Compile ExtPascalSamples as example.

# Setting up for FastCGI thru CGI gateway (any Apache version and similar procedure on IIS) #
01. Open CGIGateway.dpr.

02. Modify Port constant if necessary, default 2014.

03. Compile.

04. Copy CGIGateway.exe to cgi-bin directory.

05. Open your FastCGI application (by example ExtPascalSamples.dpr).

06. Modify Port optional parameter if necessary, default 2014. Near the bottom line of
the program, which states:

```
Application := CreateWebApplication(ServerName + ' ' + ExtPascalVersion, TSession, 2014);
```

07. Compile.

08. Copy your FastCGI application to cgi-bin directory. Use `.fcgi` extension for Posix (non-Windows) platforms.

09. Rename CGIGateway.exe to the same name of your FastCGI application changing the extension to .cgi or anything extension supported by your Web Server as CGI application.

10. To run the Samples, by example, you will have this files at cgi-bin:

```
ExtPascalSamples.exe // The real FastCGI application. The extension is `.fcgi` for Posix platforms.
ExtPascalSamples.cgi // The CGI Gateway.
```

11. None configuration is necessary on Apache or IIS, _**but I recommend to activate the HTTP compression to boost the performance (on IIS too)**_.

12. In your browser call Samples using:

```
http://localhost/cgi-bin/ExtPascalSamples.cgi
```

13. ExtPascalSamples.cgi (the gateway) will start automatically ExtPascalSamples.exe (the real FastCGI application).

# Setting up for native FastCGI, only for Apache 2.2+ #
01. Download FastCGI module at: http://www.fastcgi.com/dist/mod_fastcgi-2.4.6-AP22.dll

02. Rename to `mod_fastcgi.so`

03. Copy to Apache's **modules** folder (by example c:\apache\modules)

04. In httpd.conf file (Apache's **conf** folder) in LoadModule session insert the line below:

```
LoadModule fastcgi_module modules/mod_fastcgi.so
```

05. Declare the FastCGI application as external server, appending this line in httpd.conf:

```
fastcgiexternalserver cgi-bin/ExtPascalSamples –host localhost:2014 –idle-timeout 3
```

**CAUTION: Don't cut and paste lines from this wiki! This will cause Apache fails to start!!! The hifen character is not the same character (Google wiki uses the char 150 and the true hifen char is 45).**

06. `cgi-bin/ExtPascalSamples` is the URL that you will use to invoke the ExtPascal application by browser. Is not necessary copy the executable to cgi-bin folder. This URL is case-sensitive.

07. `–host` parameter declare the server where is hosted the FastCGI application (by example localhost).

08. The default TCP/IP port is 2014, but another could be declared, if you to recompile the application changing the port in source code.

09. Start manually your FastCGI application, via console for example. Your executable can be anywhere in the HD. It will works as a service or a daemon for the Web Server, receiving requests by the TCP port. To debugging set a breakpoint in your application and start it by the IDE.

10. In your browser call Samples using:

```
http://localhost/cgi-bin/ExtPascalSamples
```

11. See [FastCGI docs](http://www.fastcgi.com/mod_fastcgi/docs/mod_fastcgi.html#FastCgiExternalServer) for more info.

12. I also recommend to activate the HTTP compression to boost the performance. See [Advanced configuration](http://extpascal.googlecode.com/files/ExtPascal-Advanced-Configuration-complete-eng-v4.pdf).

# Setting up for native FastCGI, Apache 2.2+ (Ubuntu linux server 9.10, 10.04) #
01. Install [Ubuntu server](http://www.ubuntu.com) (clean installation without Apache server)

02. Install Apache server:
```
sudo apt-get install apache2 
```
03. Install Apache FastCGI module:
```
sudo apt-get install libapache2-mod-fastcgi
```
04. Declare the FastCGI application as external server, appending this line in /etc/apache2/apache2.conf:
```
FastCgiExternalServer /var/www/ExtPascalSamples –host somehost:2014 -idle-timeout 3
```

**CAUTION: Don't cut and paste lines from this wiki! This will cause Apache fails to start!!! The hifen character is not the same character (Google wiki uses the char 150 and the true hifen char is 45).**

05. Restart Apache so the changes take effect:
```
sudo /etc/init.d/ apache2 restart
```

06. Start ExtPascalSamples application and in your browser call Samples using:
```
http://ubuntuserver/ExtPascalSamples
```