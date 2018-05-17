# Project 7 - WordPress Pentesting

Time spent: **15** hours spent in total

> Objective: Find, analyze, recreate, and document **five vulnerabilities** affecting an old version of WordPress

## Pentesting Report

1. Weak Use Enumeration Protection
  - [ ] Summary: All registered users to the WP site are checked for and listed by the WPScan tool. Even better, the tool will note that default user names are in use, which may lead to default passwords also still being utilized. Having a full list of active users could lead to an easy login for a hacker. Weak passwords are common enough that if a site had dozens of users, one is bound to give away a login. 
    - Vulnerability types: User Enumeration
    - Tested in version: 4.2
    - Fixed in version: From my research, there is no obvious fix provided by WP. Instead, there are third party addons that can help prevnent User Enumeration. Even still, these have been exploited by hackers and require their own patching. Thus far, the only fix is to change the source code for how WP handles users. Although I don't quite understand how or why it works, this is some code that will change how WP handles users, and will prevent user enumeration.
     
      change in: .htaccess:
      ```
      RewriteCond %{REQUEST_URI} !^/wp-admin [NC]
      RewriteCond %{QUERY_STRING} author=\d
      RewriteRule ^ - [L,R=403]
      ```
      AND/OR
      ```
      RewriteCond %{REQUEST_URI} !^/wp-admin [NC]
      RewriteCond %{QUERY_STRING} ^author=\d+ [NC,OR]
      RewriteCond %{QUERY_STRING} ^author=\{num 
      RewriteRule ^ - [L,R=403]
      ```
      
  - [ ] GIF Walkthrough: ![](https://raw.githubusercontent.com/trezzan/RepoBoi/master/UserEnumeration.gif)
  - [ ] Steps to recreate: in Kali terminal enter
  ```
  "wpscan --url http://wpdistillery.vm --enumerate utp"
  ```
  - [ ] Affected source code: unknown
    
2. Easy Website Fingerprinting; Exposed Version #
  - [ ] Summary: The readme file for the WP site is exposed by default. Within the file is the version number of WP. Although the WPScan tool finds this easily, a user that wants to check without booting Kali or using tools can very simply check for a readme file. Once the version number is found, a simple internet search will net all possible vulns in that version number. Doing all of this without using Kali or WPScan makes for an even easier barrier of entry for any hacker to attack the WP site. 
    - Vulnerability types: Version Number Exposure
    - Tested in version: 4.2
    - Fixed in version: Not Fixed. Admins must force the readme files content to be changed or remove access to the page. 
  - [ ] GIF Walkthrough: ![](https://raw.githubusercontent.com/trezzan/RepoBoi/master/ReadMe.gif)
  - [ ] Steps to recreate: Any WP site that is known to be Wordpress: add the text
  '''
  /readme.html
  '''
  at the end. This will show the readme file and the correspondeing version number. 
  - [ ] Affected source code: unknown

3. Unauthenticated Stored Cross-Site Scripting (XSS)
  - [ ] Summary: The script executed would normally not affect the site. However, when overloading it with characters beyond the size of 64 KB, the comment gets stored in the WP database instead of cache (I think that is where it goes). The leads to it executing every single time the overflow comments are read (often). When the script is stored more than once, it will freeze the site. No scrolling. No functioning links. No useability. 
    - Vulnerability types: XXS
    - Tested in version: 4.2
    - Fixed in version: 4.2.1
  - [ ] GIF Walkthrough: ![](https://raw.githubusercontent.com/trezzan/RepoBoi/master/XSS.gif)
  - [ ] Steps to recreate: Find  a section to comment on a page or post. Fields other than body don't really matter. Insert the script to alert, and overload it with characters. Submit and wait for the site to die slowly. It is important to already be approved to comment or the admin will easily deny. Post something simple first, then go in for the kill!
  - [ ] Affected source code:
    - MySQL database up to v.5.5.41


## Assets
```
<a title='x onmouseover=alert(unescape(/gawt%20hahkd/.source)) style=position:absolute;left:0;top:0;width:5000px;height:5000px  AAAAAAAAAAAA...[a whole lot of these]..AAA'></a>
```
 
+ script

+ script
  

## Resources

- [WordPress Source Browser](https://core.trac.wordpress.org/browser/)
- [WordPress Developer Reference](https://developer.wordpress.org/reference/)

GIFs created with [LiceCap](http://www.cockos.com/licecap/).

## Notes

Understanding how to correctly initiate and manage the vagrant framework was a big challenge. Now that i know how it works, it seems like very powerful tool that i can imagine security researchers clamor for. 



## License

    Copyright [2018] [name of copyright owner]

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
