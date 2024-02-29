# Module 8: Web Security - Single Origin Policy

* This module requires that you work in pairs. However, please put your answers in your individual GitHub repository.
* Put your answers in the `README.md` file in the GitHub repository.
* Github Classroom Link: [https://classroom.github.com/a/MdiCeWmP](https://classroom.github.com/a/MdiCeWmP)


## Important Note:

In the following instructions, you must replace the __# sign__ with the server number assigned to you for this module. For example, if your number is 4 then `spiderwebdev#.xyz` should be replaced with `spiderwebdev4.xyz`


## Introduction

This module is designed to introduce the basics of web security using an Apache web server on Ubuntu Linux and the fundamentals of html, javascript, and cookies. 

## Getting familiar with the web servers

The servers are a newly provisioned Virtual Machines (VMs) with a LAMP stack software bundle installed. [LAMP](https://en.wikipedia.org/wiki/LAMP_(software_bundle)) (`L`inux, `A`pache, `M`ySQL, `P`HP/`P`ython) is an acronym denoting one of the most common software stacks for the web's most popular applications. 

__Step 1.__ In a browser window navigate to the URLs of your assigned servers:

```
https://spiderwebdev#.xyz
https://attackwebdev#.xyz
```

Once loaded read the initial out of the box index.html page shown in the browser.

__Step 2.__ 

Login via ssh to your web servers.

```SHELL
$ ssh spiderwebdev#
$ ssh attackwebdev#
```

__Step 2.__

Once you've logged into the server. Verify that the apache2 web server is running. Press `q` or type `Ctrl-C` to return to the prompt.

```SHELL
$ sudo systemctl status apache2
```

__Step 3.__

Navigate to the `/var/www/html` directory. And list the files. This is the home folder for the web content that is served by the Apache web server. Notice that the index.html is the same as what you read above. When using `more` command, use the space bar to go to the next page.

```SHELL
$ cd /var/www/html
$ ls
$ more index.html
```

__Step 4.__

Create your own home page. You can work with `nano` or `VSCode` connecting remotely. 

```SHELL
$ sudo rm index.html
$ nano index.html
```

For `spiderwebdev#.xyz` add the following to `index.html`:

```HTML
<html>
  <body>
    <h1>Welcome to SpiderWebDev</h1>
  </body>
</html>
```

For `attackwebdev#.xyz` add the following to `index.html`

```HTML
<html>
  <body>
    <h1>Welcome to AttackWebDev</h1>
  </body>
</html>
```

__Step 5.__

Refresh your browser to verify that your changes are shown on the web.

```
https://spiderwebdev#.xyz
https://attackwebdev#.xyz
```

__Step 6.__

Copy the files in the repository folder SpiderWebDev to the `/var/www/html` folder on `spiderwebdev#.xyz`.

Copy the files in the repository folder AttackWebDev to the `/var/www/html` folder on `attackwebdev#.xyz`.

Edit each file and replace `spiderwebdev#.xyz` with your number. For example, `spiderwebdev4.xyz`

__Step 8.__

Open the web developer tools in your browser. 


__Step 9.__

Refresh the pages:

```
https://spiderwebdev#.xyz
https://attackwebdev#.xyz
```

Note the network traffic in the __Network__ tab.


__Step 10.__

Click on the link "Cross origin request to attackwebdev" and then click the button "Send Ajax". 

What error is shown in the __Network__ tab of the browser tools?

__Step 11.__

On attackwebdev#.xyz modify the file `getdata.php` uncomment the line.

```
#header("Access-Control-Allow-Origin: https://spiderwebdev#.xyz");
```
change to (Don't forget to modify the #)
```
header("Access-Control-Allow-Origin: https://spiderwebdev#.xyz");
```

__Step 12.__

Click on the link "Cross origin request to attackwebdev" and then click the button "Send Ajax". The cross site request should now work correctly.
























