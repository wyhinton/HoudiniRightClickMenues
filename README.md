# Custom Houdini Right Click Menues
I use a good number of installed packages, and I've always wondered how the custom right click menu's for the packages worked. After some amount of trial and error, I was able to figure out how add my own menues. 

First I created my own package WebbLib. I duplicated a package I alredy installed, [BeeHou](https://github.com/simonreeves/BeeHou), and renamed it to WebbLib. I deleted everything but the ```scripts``` and ```PARMMenu.xml``` leaving me with the following file structure:

```
- WebbLib
 - scripts
 - PARMMenu.xml
```

I added the following WebbLib.json to my houdini packages folder:
```js
{
    "env": [
        {
            "WEBB_HOU": "C:\Users\Primary User\Desktop\LabsInstall\WebbLib"
        },
    ],
    "path": "$WEBB_HOU"
}
```
Here's the xml for my menu, which appears when right-clicking a parameter.


```xml
<?xml version="1.0" encoding="UTF-8"?>
<menuDocument>
	<menu>
    <subMenu id="Webb">
      <label>Webb</label>
      <insertBefore />
      <scriptItem id="Say Hello">
        <label>set Optype Python</label>
        <scriptCode><![CDATA[
print("hello")
      ]]></scriptCode>
      </scriptItem>
    </subMenu>
	</menu>
</menuDocument>
```
And here it is!
![image](https://user-images.githubusercontent.com/38958118/182684039-89bf141e-2afd-4a40-97e3-bd1199f7a248.png)


It's pretty painfult to write actual python in the menue's XML file, so looks like a better practice is to write your code in the packages script folder, then import it into the menu. I created a directory ```webb``` within my ```scripts``` folder, then created a ```say_hello.py``` script. Our function will receive a ```kwargs``` argument generated by our right click. In this case, kwargs has information about our right click action, as well as the parameter we clicked on.  


```
- WebbLib
 - scripts
   - webb
    - say_hello.py
 - PARMMenu.xml
```

```
#say_hello.py
def main(kwargs):
    print(kwargs)
    print("hello")
```

Then we import our ```say_hello``` script into our menu:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<menuDocument>
	<menu>
    <subMenu id="Webb">
      <label>Webb</label>
      <insertBefore />
      <scriptItem id="Say Hello">
        <label>Say Hello</label>
        <scriptCode><![CDATA[
import webb.say_hello
webb.say_hello.main(kwargs)
      ]]></scriptCode>
      </scriptItem>
    </subMenu>
	</menu>
</menuDocument>
```
Here's the output:
![image](https://user-images.githubusercontent.com/38958118/182685175-e4333ac9-1447-4c44-b1c8-5c1fd2cd8a80.png)

I've included the code to reproduce this in the repo. 



