# **Chrome Extensions Development Notes** 

05/08/2019, Caglar Orhan, Santa Clara /CA

There is only one base file to set a chrome extension. This is manifest file and it is json formatted. (_manifest.json_). In this json, **_manifest_version_** must 2 and must not change! (v3 is on its way)...

    {
        "name": "My Chrome Extension",
        "description" : "This Extension Does This",
        "version": "2019.0.2",
        "manifest_version": 2
      }

If this manifest file stays in any folder, Chrome can recognize folder as a Chrome Extension. Chrome can compile this folder to .crx (Chrome Extension File) and CRX file can be uploaded to Chrome Web Store via https://chrome.google.com/webstore/devconsole/ .
Any user can download this crx into Chrome Browser and can use from this point.
By the way you don't need to compile it (even Chrome browser can compile). Zip the manifest.json containing folder and upload this zip. webstore accepts zip files too.

After first manifest.json creation step with the basic informations above. 
You can develop and test in your local with chrome browser.

Follow these steps to create a chrome extension and add it to your browser:
- Create a folder and put a simple manifest.json file like written above
- Open a chrome browser and open extensions page (chrome://extensions/) Or open it from  _**Customize and Control Google Chrome**_ menu (three dots on a row, at the most right top of Chrome window) ⇒ More Tools ⇒ Extensions 
- On this Extensions page, activate Developer mode from the switch (upper right corner).
- When you switch-on the developer mode you can see every extensions has unique ID. And a menu appears on top, which includes 3 buttons. 

Buttons from right to left: 
- **Update** button updates the extensions page (so the list of extensions, and extensions themselves).
- **Pack Extension** button asks you the folder of specific (you choose) extension and compile it to **_.crx_** file. While doing this it asks for _**.pem**_ (private key) file, don't worry, it creates if you can't pick one. The important of pem file is to recompiling processes, you need it again if you would compile. You need same file again. I can suggest create a second folder to encapsulate these files.
- **Load Unpacked** button let you add a folder (which includes manifest.json file) as chrome extension to your chrome browser. To test, develop, debug etc.

These are the first basic steps to create empty, aimless, meaningless, useless chrome extension :)

---
Now, let us choose a purpose, for us, for the chrome extension we will develop.
We can use all js, html, css and browser environment APIs etc in extensions. 
You can find useful links down below (must have a look). 
Before we start to develop lets have a look over main manifest file parts...


Most important part of Chrome Extension is the _**manifest.json**_ file.
All configurations, permissions, specifications, file locations and options created and defined in this file.

Some of the main parts of **manifest.json** file and other extension notes:

**Browser Actions** 

    "browser_action": 
    {"default_popup": "index.html",
    "default_title": "My GitHub",
    "default_icon": "./img/mygithub_16.png"
    }
 
_**default_popup**_ is the target page that pops up when you click the extensions icon in browser.

_**default_title**_ is the title when you move your mouse and wait over the icon of your extension

_**default_icon**_ is the 16x16 pixels dimension of a png icon of your extension which is sits right upper corner of your chrome browser 
(https://developers.chrome.com/extensions/browserAction)



**Permissions** 

    "permissions": ["declarativeContent","storage","notifications", "contextMenu"],

Extensions needs permissions to reach browser API system. Storage for local storages, notifications for standard notifications (rise up from right bottom, you know), contextMenu as is, right mouse click opened menu on any web page.
(https://developers.chrome.com/extensions/declare_permissions)

**Icons**

      "icons": {
        "16": "./img/mygithub_16.png",
        "32": "./img/mygithub_32.png",
        "48": "./img/mygithub_48.png",
        "64": "./img/mygithub_64.png",
        "128": "./img/mygithub_128.png",
        "256": "./img/mygithub_256.png",
        "512": "./img/mygithub_512.png"
      }
      
These icons are used in browser, extensions page and web store pages etc..    
      
**Background** 

    "background" : { "scripts" : ["./js/background.js"] },
When extension installed, on the background you can do something with javascript. For example: send request to APIs, calculate something, listening opened web pages etc. These files must listed in this section. To use any script at the background you need to add the script to the array as shown above. The second property of background is ``persistent`` .If its value is ``false`` page is an ``event page`` else if its value is ``true`` it is ``background page``.
 


**Options Page**

Option page is a kind of extensions configuration page. This is only one page that you can design whatever you like. But as its name tells generally used for options. User can reach the options page with right clicking the extensions icon and select Options option from the context menu. And your options page opens in a new tab. Address will be like this
 ``chrome-extension://oefphpcpamajoaakdopblifhfneealbi/yourOptionsPageName.html``
 
To create options page first add a key-value in manifest file.

    "options": "yourOptionsPageName.html"

After preparing your `yourOptionsPageName.html` it is ready to go. You can use css, html and javascript in this html page.

_**JS Note:**_ ``close()`` command in javascript of options page, closes the opened options page tab.





--- 


**Notifications**

A notification is classical chrome notification which arises from right bottom corner of the window. This action must be declared in permissions section inside manifest.json file (check the permissions header).
After adding notifications to permissions array, you can use Notifications API.
There are types, methods and events on this API. Basically create a notification object and give it to the notifications create method with a given name.

    var notifOptions = {
                        type: 'basic',
                        iconUrl: 'icon48.png',
                        title: 'Baslik',
                        message: 'Mesaj metni'
                        }
    chrome.notifications.create('notificationID',notifOptions)
            
(https://developers.chrome.com/extensions/notifications)

---
**Context Menu**

First add to the background section into the manifest file 

    "background": {
                    "scripts": ["eventPage.js"],
                    "persistent": false 
    }
after that, add ``contextMenus`` to the ``permissions`` array.

Now write into the ``eventPage.js`` and create an object first.

    var contextMenuItem = {
    "id": "chooseYourOwnId",
    "title": "choose your own title",
    "context": ["selection"]
    }
 And give this object to the ``contextMenus`` APIs ``create`` method.
 ``chrome.contextMenus.create(contextMenuItem)`` in the value of  ``context`` key is an Array and specify which content click would triggering the contextmenu. You can write ``"all"`` for all types of contents, or add from the list ``"all", "page", "frame", "selection", "link", "editable", "image", "video", "audio", "launcher", "browser_action", or "page_action"``.
 
 After setting contextmenu, you should add a listener for click action to set another actions with the page content etc.
 
    chrome.contextMenus.onClicked.addListener(function(clickedData){
        if(clickedData.menuItemId ==="chooseYourOwnId" && clickedData.selectionText) 
    })
    
`selectionText` is changing the context you choose.
 
 You can glance over https://developers.chrome.com/extensions/contextMenus    
    
    
---


**Note:**  All API processes are pushed into callback queue, after callback stack emptied event loop push them into call stack. Because of this asynchronous work flow you should plan your code logic carefully.


**...to be continued...**
 









---
**_Links:_**
- https://developers.chrome.com/extensions 
- https://developers.chrome.com/extensions/devguide
- https://www.youtube.com/playlist?list=PLRqwX-V7Uu6bL9VOMT65ahNEri9uqLWfS 
- https://www.udemy.com/chrome-extension-dev/

