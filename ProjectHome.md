# Purpose #
This rakefile makes it simple to package your extension and even will create signed update.rdf for you. However it needs some tuning before it could be used.

# Installation #
Make sure your sources are placed into svn. None of others versions controls are supported yet. Although it's easy to drop this dependency.

Make sure you have the following directory structure

```
root (doesn't have to be named this way)
  src (doesn't have to be named this way)
    content
       extension_name
         extension_code
    skin
    defaults
    Rakefile
    options.json
    chrome.manifest.install
    files.rb
  mykey.pem (you going to create it later) 
```

install gems it uses
```
$ gem install json
$ gem install rubyzip
```

Make new cert
```
$ openssl req -x509 -nodes -days 3650 -newkey rsa:1024 -keyout ../mykey.pem -out mycert.pem
```

create options.json and put configurations inside. Example
```
{ "version": "0.5",  //version of extension
   // this section describes what applications are supported by this extension
   "apps": [{"name": "Thunderbird",
 	    "aid": "{3550f703-e582-4d05-9a08-453d09bdfdc6}",
 	    "minVersion": "2.0",
 	    "maxVersion": "3.0b1"
 	   },
 	   {"name": "SeaMonkey",
 	    "aid": "{92650c4d-4b8e-4d2a-b7eb-24ecf4f6b63a}",
 	    "minVersion": "2.0a2",
 	    "maxVersion": "2.0a3"
 	   }
   ],
   //information about extension itself
   "info": {
       "eid":"{282C3C7A-15A8-4037-A30D-BBEB17FFC76B}",
       "name":"Colored Diffs",
       "creator":"Vadim Atlygin",
       "description":"Color diffs sent by SVN/CVS notifications.",
       "homepageURL":"http://code.google.com/p/colorediffs/",
       "iconURL":"chrome://colorediffs/content/icon.png",
       "optionsURL":"chrome://colorediffs/content/options/options.xul",
       "nickname":"colorediffs"
   },
   //describes different builds you could make
   "sites": [
   	 {"name": "amo"}, //build for AMO site, no update information necessary.
 	 {"name": "google-code",
 	     "upgrade_info": {
 	  	"install_rdf": {
                     //this is public key, make it with "openssl x509 -in mycert.pem -pubkey"
 		      "updateKey":"MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQC3qsDK94RUSs6nF3YWMdLAN0m1ohu21z61KkqfBBmGC60cm1YiCVF6qykfrKrJsJ3JTt/fX/cMFPBs8UZG24lmwWiNr3FsjE6KFL0upYt827z1fij/vcyQuHNWxhjgsZ2a2J/leJojOoOZPq4ua+araMnaTtNLlw88qCk67I+3JQIDAQAB",
                     //where update.rdf would be living
                    "updateURL":"http://colorediffs.googlecode.com/svn/trunk/update-google.rdf"
 		},
 		"update_rdf": {
                     //where xpi file would be placed
 		      "update_link":"https://colorediffs.googlecode.com/files/"
 		}
 	     }
 	 }
   ]
 } 
```

create chrome.manifest.install, example included. This is going to be included into xpi file as chrome.manifest

# `CruiseControl.rb` #
if you're using `CruiseControl.rb` put mykey.pem into ~/cc-private/mykey.pem

# I want to help / it's not working #
Make a patch and send it to me. Put a note if you'd like to be co-owner.