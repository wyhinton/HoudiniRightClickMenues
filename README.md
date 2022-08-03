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
Great, now my menu was showing up. It's pretty painfult to write actual python in the menue's XML file, so looks like a better practice is to write your code in the packages script folder, then import it into the menu. We can pass kwargs into these functions. 


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
![image](https://user-images.githubusercontent.com/38958118/182684039-89bf141e-2afd-4a40-97e3-bd1199f7a248.png)

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
			import webb.say_hello
			webb.say_hello(kwargs)
      ]]></scriptCode>
      </scriptItem>
    </subMenu>
	</menu>
</menuDocument>
```
