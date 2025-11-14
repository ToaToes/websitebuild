# websitebuild
steps for building website hosted on GitHub

1. create io on github, html page for the project 
2. link domain: <br>
   On domain host website, configure DNS point to github server and CNAME<br>
   A	     @	 185.199.108.153 <br>
   A	     @	 185.199.109.153 <br>
   A	     @	 185.199.110.153 <br>
   A	     @	 185.199.111.153 <br>
   CNAME  www   [projectname].github.io <br>
   
4. enable www.domain.com and domain.com
5. enable http and https -> 
     check SSL/TLS Mode to see if https supported <br>
     <i>Enforce HTTPS — Unavailable for your site because your domain is not properly configured to support HTTPS<i/>



#Chinese:

1. 打开域名提供服务商，添加DNS解析
   找到DNS界面 -> 找到Record选项 -> 选择 Add Record（添加DNS Record）:
   ""
   Type	Name	Value
   A	   @	   185.199.108.153
   A	   @	   185.199.109.153
   A	   @	   185.199.110.153
   A	   @	   185.199.111.153
   ""
