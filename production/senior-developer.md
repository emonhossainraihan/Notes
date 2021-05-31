## GitBash 

- ssh-agent bash
- ssh-add <path>

## ssh-keygen

- ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

 Want to learn more about Asymmetric Encryption? Check out these extra videos :

- https://www.youtube.com/watch?v=NmM9HA2MQGI
- https://www.youtube.com/watch?v=Yjrfm_oRO0w
- https://www.youtube.com/watch?v=vsXMMT2CqqE&t=
- https://www.youtube.com/watch?v=NF1pwjL9-DE
 
 
## performence 

- <link rel="stylesheet" type="text/css" href="style.css">
- <script src="script.js"></script>

## image file format 

- **jpg**: For complex images with lots of colors
- **png**: Limit the number of colors 
- If you want transparency: use a PNG
- If you want colorful images: use a JPG 
- Reduce PNG with TinyPNG
- Reduce JPG with JPEG-optimizer
- Use CDNs like imigx
- Remove image metadata

## media query 

- Display different images for different devices

## critical render path

- DOM > CSSOM > (DOMContentLoaded) Render Tree > Layout > Paint (Load)
- never put <script>alert('pause')</script> inside head beacuse it pause the download css resource 

## append stylesheet 

<script type="text/javascript">
 const loadStyleSheet = src => {
  if(document.createStylesheet){
   document.createStylesheet(src)
   } else{
     const stylesheet = document.createElement('link')
     stylesheet.rel = 'stylesheet'
     stylesheet.href = src
     stylesheet.type = 'text/css'
     document.getElementsByTagName('head')[0].appendChild('stylesheet')
   }
  }
 window.onload = function(){loadStyleSheet('./stylesheet/style.css')} 
</script>

## inline CSS

- Allows us to not have to request CSS file 

## different phase

- document.addEventListener("DOMContentLoaded",function(e){//run this})
- window.addEventListener("load",function(e){//run this})

## summary https://developers.google.com/web/fundamentals

### HTML

- Load style tag in the <head>
- Load script right before </body>

### CSS
- Media Attributes

### JS
- Load scripts asynchronously
- Defer loading of scripts 
- Minimize DOM manipulation
- Avoid long running JavaScript 

## JIT vs AoT(Ahead-of-Time)

## PWA whatwecando.today

## Security

### Injection

https://www.hacksplaining.com
 
- Sanitize input
- Parametrize Queries: Object relational Mapper like knex.js

 ```js
function sqlSelect(name, email, id) {
  if (name == something) {
    // do this
  }
}
```

```
 
```

## PostgreSQL
 
The first question many ask is, “What is the default password for the user postgres?” The answer is easy… there isn’t a default password. The default authentication mode for PostgreSQL is set to ident.
 
The root account is disabled by default in Ubuntu, so there is no root password, that's why su fails with an authentication error `sudo su`. 

```
  emonhossain@xenon  ~  psql                
psql: error: FATAL:  role "emonhossain" does not exist
 ✘ emonhossain@xenon  ~  su - postgres        
Password: 
su: Authentication failure
 ✘ emonhossain@xenon  ~  sudo -i              
xenon# psql
psql: error: FATAL:  role "root" does not exist
xenon# exit
 ✘ emonhossain@xenon  ~  sudo su - postgres        
postgres@xenon:~$ psql
psql (12.6 (Ubuntu 12.6-0ubuntu0.20.04.1))
Type "help" for help.

postgres=# \q
postgres@xenon:~$ exit
logout
``` 
