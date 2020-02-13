# *Android studio 3.5.0 Offline installation - Completely Offline*

*First I would like to inform you, that the best and secure way is to find good internet connection and connect your computer and simply wait android studio doing all of these above steps for you.but if you are like me with limited internet access or for some reason if you can't connect your computer at all and you are using windows 7, 8 and 10 64bit machine, here is a detailed steps to download all required components at once in another computer with internet and install on your computer **COMPLETELY OFFLINE**.*

**Download Files**

  - android studio 3.5.0 - [download link][1]
   
  - android-sdk -[download link][2]
   
  - gradle 5.4.1 - [download link][3] 

  - android gradle plugin 3.5.0

  At the time of writing this, the official website developer.android.com only provide android gradle plugin 3.5.0-beta 01 and the required version by android studio 3.5.0 is not the beta version but android gradle plugin 3.5.0 - so you have to recursively download every required files and folders using wget from this site ([link][4]).

In order to do that, first download wget ([download link][5]) copy it to c:\ location and add it to the [windows path environment variable][6].

download the .bat file ([android gradle plugin downloader][7])

After that select and right click the download.bat file and run it as an administrator.   The batch command will use wget to download and construct all files and folder for android-gradle-plugin 3.5.0

wait for the download to complete....

And follow these below steps :

**1 - unzip the android sdk to this below location**

%USERPROFILE%/AppData/Local\Android/Sdk

<img src="https://i.imgur.com/Ih43FRy.jpg">

**2 - install android studio 3.5.0 and let android studio try to build your created project and close it when it gives you an error.**

<img src="https://i.imgur.com/T12OX4l.jpg">

**3 - copy gradle 5.4.1.zip to**

%USERPROFILE%/.gradle/wrapper/dists/gradle-5.4.1-all/3221gyojl5jsh0helicew7rwx/
if the folder didn't exist create it.

<img src="https://i.imgur.com/LeDEMSb.jpg">

**4 - copy all folders from C:\android-gradle-plugin-3.5.0\nexus.xebialabs.com\nexus\content\repositories\public**

<img src="https://i.imgur.com/TTmjC37.jpg">

and paste it to %USERPROFILE%/.android/manual-offline-m2/android-gradle-plugin-3.5.0
create the folder android-gradle-plugin-3.5.0 if it didn't exist.

<img src="https://i.imgur.com/PiLr9v3.jpg">

**5 - finally, to tell the Android build system to use the offline android gradle plugin, you need to create a script, as described below.**

Create an empty text file with the following path and file name:
windows won't allow to create init.d folder you have to do it from command line, so within the .gradle folder press shift + right click - > click open command_window_here and type mkdir init.d

"%USERPROFILE%/.gradle/init.d/offline.gradle/"

<img src="https://i.imgur.com/LDtTnhD.jpg">

then from within the init.d folder - right click > hover over new > click text document. copy and paste below script and save as the file as offline.gradle

<pre>
<code>

    def reposDir = new File(System.properties['user.home'], ".android/manual-offline-m2")
    
    def repos = new ArrayList()  
    
    reposDir.eachDir {repos.add(it) }  
    
    repos.sort()  
    
    allprojects {  
    
      buildscript {  
    
        repositories {  
    
          for (repo in repos) {  
    
            maven {  
    
              name = "injected_offline_${repo.name}"  
    
              url = repo.toURI().toURL()  
    
            } 
          } 
        } 
      }  
      repositories {  
    
        for (repo in repos) {  
    
          maven {  
    
            name = "injected_offline_${repo.name}"  
    
            url = repo.toURI().toURL()
          }
        } 
      }
    }

</code> 
</pre>

Save the text file.

<img src="https://i.imgur.com/6lnVvxa.jpg">

**6 - run Android studio**

To make sure your are running offline, comment these below lines with "//" in build.gradle file, as shown below.

<pre><code>   

        buildscript {
        repositories {
            // Hide these repositories to test your build against
            // the offline components. You can include them again after
            // you've confirmed that your project builds ‘offline’.
            // google()
            // jcenter()
        }
        ...
    }
    allprojects {
        repositories {
            // google()
            // jcenter()
        }
        ...

}</code></pre>

By now, your offline android studio should work.


  [1]: https://getintopc.com/softwares/development/android-studio-2019-free-download/
  [2]: https://drive.google.com/uc?id=1IR1AG2I5E6i2vNrtCkIpINSVIXArR_yx&export=download
  [3]: https://services.gradle.org/distributions/gradle-5.4.1-all.zip
  [4]: https://nexus.xebialabs.com/
  [5]: https://eternallybored.org/misc/wget/
  [6]: https://helpdeskgeek.com/windows-10/add-windows-path-environment-variable/
  [7]: http://s000.tinyupload.com/index.php?file_id=80180673566481714255
