
Responsive Design RuleSet Wiki
================================


### Introduction
  In order to help developer build applications with good responsive design, we introduce some rules that indicate what are the "Anti Pattern" and what might be the "Best Practice". In the following document, we will explain in detail about these rules. We may give sample code for some rules.
### Category
#### [Multimedia](#multi)
1.	[Low image quality in high resolution screen](#multi1)
2.	[High image quality in low resolution screen](#multi2)
3.	[Detect non-compatible format of images, audio and video for target](#multi3)
4.	[Use non-compatible ways to play audio or videos (Flash etc.)](#multi4)

#### [CSS](#css)
1.	[Use vendor prefixed CSS styles](#css1)
2.	[Use absolute units in CSS](#css2)
3.	[Use 2D transform for 3D hardware accelerated platform](#css3)
4.	[Find font-size/font](#css4)
5.	[Use missing styles for target platform](#css5)
6.	[Media Queries misuse, invalid or inproper use](#css6)
7.	[Transform value may not fit for a different screen size](#css7)
8.	[Unrecognized CSS background value](#css8)
9.	[CSS syntax error](#css9)

#### [JS](#js)
1.	[Use consecutive multiple styles instead of class in scripts](#js1)
2.	[Use platform dependent Web Runtime APIs](#js2)

#### [HTML](#html)
1.	[Use consecutive multiple styles instead of class](#html1)
2.	[Viewport misuse](#html2)
3.	[Use deprecated old html tags](#html3)



<a name="multi"/>
<a name="multi1"/>
#### Multimedia
##### Low image quality in high resolution screen
The following picture shows a typical example：

![alt text](http://img13.poco.cn/mypoco/myphoto/20120813/16/6445810220120813160733026.jpg "low image quality")

When meeting this kind of situation, the developer should provide high resolution images.

<a name="multi2"/>

##### High image quality in low resolution screen
This situation is the opposite of the former. Although in this situation the image shown won't be hard to recognize, it indeed costs unnecessary bandwidth.

When meeting this kind of situation, the developer should compress images for low resolution

<a name="multi3"/>

#### Detect non-compatible format of images, audio and video for target
The images format of jpeg,gif and png can be almost displayed in all browsers, but there are some other image formats that might not be supported in some browser:

```html
<!-- Opera doesn't support '.tiff' -->
<!-- The developer should provide other image formats -->
<img src="boat.tiff" alt="Big Boat" />
```


* For more information about [image format support](http://en.wikipedia.org/wiki/Comparison_of_web_browsers#Image_format_support)。

The tag `<audio>` is a newly defined element in HTML5 ,it specifies a standard way to embed an audio file on a web page. Currently, there are 3 supported file formats for the `<audio>` element: mp3, wav, and ogg. But the supportability of the 3 file formats is not the same in different browsers. Developer should check the compatibility of audio format for target, and provides different formats for compatibility:


```html
<!-- Anti Pattern -->
<!-- suppose the target is IE9 which doesn't support '.ogg' -->
<audio controls="controls">
  <source src="song.ogg" type="audio/ogg" />
</audio>
<!-- The developmer should provided different formats for compatibility -->  
```

A good pattern is like:

```html
<!-- Good Pattern -->
<!-- The browser will use the first recognized format -->
<audio controls="controls">
  <source src="song.ogg" type="audio/ogg" />
  <source src="song.wav" type="audio/wav" />
  <source src="song.mp3" type="audio/mp3" />
</audio> 
```

The tag `<video>` is pretty much the same as the tag `<audio>` :

```html
<!-- Anti Pattern -->
<!-- suppose the target is IE which doesn't support '.ogv' -->
<video width="320" height="240" controls="controls">
  <source src="movie.ogv" type="video/ogv" />
</video>
<!-- The developer should  provided different formats for compatibility -->  
```

A good pattern is like:

```html
<!-- Good Pattern -->
<!-- The browser will use the first recognized format -->
<video width="320" height="240" controls="controls">
  <source src="movie.mp4" type="video/mp4" />
  <source src="movie.ogv" type="video/ogv" />
</video>
```

* For more information about [video format support](http://en.wikipedia.org/wiki/HTML5_video#Browser_support).


<a name="multi4"/>

#### Use non-compatible ways to play audio or videos (Flash etc.)
There are some platforms that doesn't support some ways to play audio or videos,or it is impossible for the platform to install Flash plugin. For example, iPhone and iPad don't support Flash:

```html
<!-- iPhone and iPad don't support Flash -->
<embed 
  src="hello.swf" quality="high" bgcolor="#000000" width="320" height="200"> 
</embed>
```


A good pattern is to provide HTML5 ways of playing audio and video :

	
```html
<!-- HTML5 ways of playing audio and video -->
<audio controls="controls">
  <source src="hello.ogg" type="audio/ogg" />
  <source src="hello.wav" type="audio/wav" />
  <source src="hello.mp3" type="audio/mp3" />
</audio> 

<video width="320" height="240" controls="controls">
  <source src="hello.mp4" type="video/mp4" />
  <source src="hello.ogv" type="video/ogv" />
  <embed src="hello.swf" type="application/x-shockwave-flash"          
   width="320" height="200" allowscriptaccess="always" allowfullscreen="true"></embed>
</video>
```

<a name="css"/>
<a name="css1"/>

#### CSS
##### Use vendor prefixed CSS styles
The user may hate writing vendor prefixes for different browsers which is necessary for different browsers to run the code correctly. An anti pattern is to ignore some standard or vendor prefixed styles:
```css
/* here are some vendor prefixed CSS styles without */
/* providing standard and other vendor prefixed styles */
div {
    -moz-border-radius: 10px;
    -webkit-border-bottom-left-radius:2em;
    -ms-layout-flow: horizontal;
    -o-border-image:url(border.png) 30 30 round; 
}
```

A good pattern is like :

```CSS
div {
    -moz-border-radius: 10px;
    -webkit-border-radius: 10px;
    border-radius: 10px;
    -webkit-border-bottom-left-radius: 2em;
    -moz-border-radius-bottomleft: 2em;
    border-bottom-left-radius: 2em;
    -ms-layout-flow: horizontal;
    -moz-border-image: url(border.png) 30 30 round;
    -ms-border-image: url(border.png) 30 30 round;
    -o-border-image: url(border.png) 30 30 round;
    -webkit-border-image: url(border.png) 30 30 round;
    border-image: url(border.png) 30 30 round;
    }
```

* For more information about [vendor prefixed css](http://peter.sh/experiments/vendor-prefixed-css-property-overview/).

<a name="css2"/>

##### Use absolute units in CSS
It is better to use relative rather than absolute units. This way the content of a page will adjust better to the browser window and fonts will be displayed relative to the users specifications or relative to the default settings of the browser. **But this may not always be the case**, in some situation, such as when developer needs to make accurate positioning of a widget in a page, then it might be better to use absuloute units. Anyway, the developer should deal with absuloute units when designing responsive applications.


```CSS
/* these are some absolute units that will be */
/* hard to adjust for multiple screens */
h1 { margin: 0.5in;}      /* inches  */
h2 { line-height: 3cm;}   /* centimeters */
h3 { word-spacing: 4mm;}  /* millimeters */
h4 { font-size: 12pt;}    /* points */
h4 { font-size: 1pc;}     /* picas */
p  { font-size: 12px;}    /* pixel */
```
	
Here are some relative units :

```CSS
/* these are some relative units */
h1 {line-height: 1.2em; }  /* relative to font size of the element */
h2 {margin: 2ex; }         /* relative to x-height of the element's font */
h3 {wird-spacing: 3ch; }   /* relative to width of the "0" glyph in the element's font */
h4 {font-size: 2rem; }	   /* relative to font size of the root element */
p {font-size: 8vw; }       /* relative to viewport's width */
p { margin: 2vh; }         /* relative to viewport's height */
h4 { padding: 3vmin; }       /* relative to minimum of the viewport's height and width */
```
	
<a name="css3"/>

##### Use 2D transform for 3D hardware accelerated platform
Some platforms such as Firefox and Chrome support 3D transform, developer should provide 3D transform instead of 2D if possible：

```CSS
/* Suppose the platform is Chrome which supports 3D transform */
/* these are 2D transform, developer should provide 3D transform  */
div
{
  transform: translate(50px,100px);
  -ms-transform: translate(50px,100px);        /* IE 9 */
  -webkit-transform: translate(50px,100px);    /* Safari and Chrome */
  -o-transform: translate(50px,100px);         /* Opera */
  -moz-transform: translate(50px,100px);       /* Firefox */
 }
```

developer should provide 3D transform instead of 2D if possible：

```CSS
/* 3D tranform instead of 2D if possible */
div
{
  transform: rotateX(50px);
  -webkit-transform: translateX(50px);      /* Safari and Chrome */
  -moz-transform: translateX(50px);         /* Firefox */
  transform: translateY(50px);
  -webkit-transform: translateY(50px);      /* Safari and Chrome */
  -moz-transform: translateY(50px);         /* Firefox */
}
```

<a name="css4"/>

##### Find font-size/font
The font-size/font in responsive design is not only concerned with screen resolution but also DPI(dots per inch).

fond-size/font may change the layout of a site considerably. Different browsers interpret font sizes differently, so a font that appears readable in IE may be smaller when viewed in Chrome. In addition, font sizes on different platforms are not always the same. It's better to specify a font size in pixels (px) not points (pt) or em. Using a pt or em font-size property instead of px allows for the site text to be resized according to the viewer's system settings. If their system is set to view very large text, your web site's layout will become distorted and your web site may be illegible to them. 

Also, user may set the font-size pixels too small. Some people may not be able to read tiny text and adjusting their system text size will have no effect on the site because the font-size is specified as px. The developer should provide a recommended font-size for target device.

<a name="css5"/>

##### Use missing styles for target platform
Some platform may not support some styles. When this problem occurs, the developer should be informed with a fallback or warned to change it manually:

```CSS	
/* IE */
/* IE doesn't support 'outline' */
/* the developer should be informed with a fallback or warned to change it manually */
p {
  border:1px solid red;
  outline:green dotted thick;
｝
```

<a name="css6">

##### Media Queries misuse, invalid or inproper use
TBD

<a name="css7">

##### Media Queries misuse, invalid or inproper use
TBD

<a name="css8">

##### Media Queries misuse, invalid or inproper use
TBD

<a name="css9">

##### Media Queries misuse, invalid or inproper use
TBD


	
<a name="js"/>
<a name="js1"/>

#### JS
##### Use consecutive multiple styles instead of class in scripts
Manipulating CSS styles in JavaScript code is considered to be an anti pattern :


```JavaScript
// JavaScript Code 
// the code tries to manipulate CSS styles directly which is an anti pattern
function changeCSS(){
  document.getElementById("t").style.color= "red";
  document.getElementById("t").style.fontSize= "30px";
  document.getElementById("t").style.backgroundColor = "blue";
}
```


The developer should provide extracted CSS class of the change so that the user can manipulate the CSS styles by changing the tag's class, which is considered to be  a best practice: 

```CSS
/* Developer should provide extracted class in CSS for user to manipulate */
.converted {
  color: red;
  font-size: 30px;
  background-color: blue;
}
```

User then can manipulate the style using JavaScript like:

```JavaScript
//Use JavaScript to change the class of the tag instead of manupulating CSS style directly
function changeCSS() {
  document.getElementById("t").className = "converted";
 }
```


<a name="js2"/>

##### Use platform dependent Web Runtime APIs
The developer should be aware that the supportability of platform dependent web runtime APIs vary in different platforms. 
Here is a picture showing the supportability of web sockets in different platforms.

![alt text](http://img13.poco.cn/mypoco/myphoto/20120814/11/6445810220120814112612097.png "websocket support")


<a name="html"/>
<a name="html1"/>

#### HTML
##### Use consecutive multiple styles instead of class
In-line CSS in style attribute and CSS code in HTML file are considered to be anti pattern. The developer should provide extracted classes and move CSS code to external style sheet:

```html
<!-- Internal Style Sheet in HTML file is considered to be anti pattern -->
<head>
<style type="text/css">
  hr {color:sienna;}
  p {margin-left:20px;}
  body {background-image:url("images/back40.gif");}
</style>
</head>
```

```html
<!-- In-line CSS is considered to be anti pattern -->
<p style="font-size: 10px; color: #FFFFFF;background: blue; ">
  Hello! 
</p>
```
These are solutions to the problems discussed above: 

```html
 <!-- Move CSS code from html file to external style sheet file is a good pattern -->
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css" />
</head>
```

```CSS
/* extracted class in CSS */
p {
  font-size: 10px;
  color: #FFFFFF;
  background: blue;
}
```

<a name="html2"/>

##### Viewport misuse
The viewport meta tag is to let web developers control the viewport's size and scale. There is no need to set the scale attributes because different platform has different DPI. A typical setting of viewport is like:
```html
<!-- a typical setting of viewport -->
<meta name="viewport" content="width=device-width">
```

<a name="html3"/>

##### Use deprecated old html tags
Older HTML tags that have been superseded by other more functional or flexible alternatives (whether as HTML or as CSS ) are declared as deprecated by the W3C. Browsers may continue to support deprecated tags and attributes, but eventually these tags are likely to become obsolete and so future support cannot be guaranteed. And the developer should avoid using these deprecated old html tags.

Here is a table showing some of the deprecated HTML tags and replacements:

<table border="1" width="100%" summary="Deprecated HTML tags and replacements">
<colgroup span="3"><col width="25%"></col><col width="25%"></col>
<col width="50%"></col></colgroup>
<tr><th>Deprecated</th><th>Description</th><th>Replacement</th></tr><tr>
<td>&lt;applet&gt;</td>
<td class="h">Inserts applet</td>
<td>&lt;object&gt;</td>
</tr><tr>
<td>&lt;basefont&gt;</td>
<td class="h">sets font styles</td>
<td>font style sheets</td>
</tr><tr>
<td>&lt;center&gt;</td>
<td class="h">centers elements</td>
<td>&lt;div style="text-align:center"&gt;
<a href="http://www.w3.org/TR/REC-html40/present/graphics.html#h-15.1.2">(W3C help)</a>
</td></tr><tr>
<td>&lt;dir&gt;</td>
<td class="h">directory list</td>
<td>&lt;ul&gt;</td>
</tr><tr>
<td>&lt;font&gt;</td>
<td class="h">applies font styles</td>
<td>font style sheets</td>
</tr><tr>
<td>&lt;isindex&gt;</td>
<td class="h">adds search field</td>
<td>&lt;form&gt;</td>
</tr><tr>
<td>&lt;menu&gt;</td>
<td class="h">menu list</td>
<td>&lt;ul&gt;</td>
</tr><tr>
<td>&lt;s&gt;</td>
<td class="h">strike through</td>
<td>text style sheets</td>
</tr><tr>
<td>&lt;strike&gt;</td>
<td class="h">strike through</td>
<td>text style sheets</td>
</tr><tr>
<td>&lt;u&gt;</td>
<td class="h">underline</td>
<td>text style sheets</td>
</tr>
</table>