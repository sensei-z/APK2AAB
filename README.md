# APK2AAB

## Windows-exe

Exe file found in APK2AAB\Windows-exe\APK2AAb.exe <br />

Download .Net 6.0 runtime https://dotnet.microsoft.com/en-us/download/dotnet/6.0 & Java 8 & add to system variable <br />

Selected apk & out put aab path should not contain any space in directory <br />
Min-sdk , target sdk , version name & code  should be same as your apk have in AndroidManifest.xml<br />
You need to sign the generated aab file with your keystore in order to publish the application in play store <br />

## Android-Tool <br />

Apk file found in APK2AAB\Android-Tool\APK2AAb.apk<br />

Download the application and install it <br />
Pick the apk and select output path for aab file <br />
Your file generate in output path<br />
You need to sign the generated aab file with your keystore in order to publish the application in play store <br />

## Manual-way <br />

Tools are required<br />

Bundletool.jar 
https://github.com/google/bundletool/releases<br />

Apktool.jar
https://github.com/iBotPeaches/Apktool<br />

Aapt2.exe
https://dl.google.com/dl/android/maven2/com/android/tools/build/aapt2/4.2.1-7147631/aapt2-4.2.1-7147631-windows.jar<br />

Android.jar
Get the ANDROID SDK: $ANDROID_SDK/platforms/android-$latestVersion/android.jar<br />

Jave 8 https://www.oracle.com/in/java/technologies/javase/javase8-archive-downloads.html <br />
Add to system variable https://www.java.com/en/download/help/path.html <br />

**Unzip the apk**<br />

Put all tool files in one folder <br />
Open your command prompt in current directory <br />
Decompile the apk through apktool.jar

execute below command in command line
```
java -jar apktool_2.5.0.jar d test.apk -s -o decompile_apk -f
```

**Compile the resource**

Compile the resources using aapt2 <br />

```
aapt2.exe compile --dir decompile_apk\res -o res.zip
```
After that you will see a res.zip will generate in your current directory <br />

**Link the resources**

execute below command in command line <br />
```
aapt2.exe link --proto-format -o base.zip -I android.jar --manifest decompile_apk\AndroidManifest.xml --min-sdk-version $version --target-sdk-version $version --version-code $version --version-name $version -R res.zip --auto-add-overlay
```
<br />

**$version** should be replace with your apk version for example my apk have **min-sdk is 7** , **target-sdk is 30** , **version-code is 1** , **verison-name is 1.0** . So my command will be -> <br />

```
aapt2.exe link --proto-format -o base.zip -I android.jar --manifest decompile_apk\AndroidManifest.xml --min-sdk-version 7 --target-sdk-version 30 --version-code 1 --version-name 1.0 -R res.zip --auto-add-overlay
```

After that you will see a base.zip will generate in your current directory <br />

**Unzip the base.zip** <br />

Directory structure: <br />

```
base/
/AndroidManifest.xml
/res
/resources.pb
```


**Copy the files**
 <br />
Take base folder as your main folder for now ! <br />
Create a folder **manifest** name folder inside base folder and move your base/AndroidManifest.xml to manifest/AndroidManifest.xml <br />
copy whole assets folder from decompile_apk/assets and paste to base/assets <br />
copy lib folder from decompile_apk/lib  and paste to base/lib <br />
copy all files inside unknown folder from decompile_apk/unknown and paste to base/root <br />
copy whole kotlin folder from decompile_apk/kotlin and paste to base/root/kotlin <br />

Final directory structure <br />

```
base/
/assets
/dex
/lib
/manifest
/res
/root
/resources.pb
```
**Create a zip** <br />

Open your command prompt in **/base** directory <br />

we are going to create a zip using cmd or you can use any software like Winrar & 7zip <br />

execute below command in command line <br />
```
jar cMf base.zip manifest dex res root lib assets resources.pb
```

It will create **base.zip** in current directory now copy the base.zip and paste where you put all tool file (.jars , .exe) <br />

**Compile aab** <br />
Open your command prompt in **/tools** directory <br />

execute below command in command line  <br />
```
java -jar bundletool.jar build-bundle --modules=base.zip --output=base.aab
```

**base.aab** file will generate in current folder <br />
You need to sign the generated aab file with your keystore in order to publish the application in play store <br />

