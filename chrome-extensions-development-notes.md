# **Chrome Extensions Development Notes** 

05/08/2019, Caglar Orhan, Santa Clara /CA

There is only one base file to set a chrome extension. This is manifest file and it is json formatted. (_manifest.json_). In this json, **_manifest_version_** must 2 and must not change!
> {
    "name": "My Chrome Extension",
    "description" : "This Extension Does This",
    "version": "2019.0.2",
    "manifest_version": 2
  }

If this manifest file stays in any folder, Chrome can recognize folder as a Chrome Extension. Chrome can compile a file to .crx (Chrome Extension File) and CRX file can be uploaded to Chrome Web Store via https://chrome.google.com/webstore/devconsole/ .
Any user can download this crx into Chrome Browser and can use from this point.
By the way you don't need to compile it (even Chrome browser can compile). Zip the manifest.json containing folder and upload this zip. webstore accepts zip files too.

After first manifest.json creation step with the basic informations above. 
You can develop and test in your local with chrome browser.

Follow these steps to create an chrome extension and add i to your browser:
- Create a folder and put a simple manifest.json file like written above
- Open a chrome browser and open extensions page (chrome://extensions/) Or open it from  _**Customize and Control Google Chrome**_ menu (tri dots on a row, at the most right top of Chrome window) ⇒ More Tools ⇒ Extensions 
- On this Extensions page activate Developer mode with switch on the upper right corner.
- When you switch-on the developer mode you can see every extensions has unique ID. And a menu appears on top which inludes 3 buttons. 

From right to left: 
- **Update** button updates the extensions page (so the list of extensions, and extensions themselves).
- **Pack Extension** button asks you the folder of spesific (you choose) extension and compile it to **_.crx_** file. While doing this it asks for _**.pem**_ (provate key) file


