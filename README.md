# Unity Lightmap Build With PsExec
Unity LightmapBuild on other local remote with PsExec

Purpose of this project is to make simple unity lightmap build server.

## How to build unity lightmap via cmd

1. Make Unity project. and make script file like below.

> ![Make BuildLightMap.cs Script](./images/makeScriptFile.jpg)

2. type the script.
> ```csharp
> using UnityEditor;
> using UnityEngine;
> using UnityEditor.SceneManagement;
> 
> public class BuildLightMap : MonoBehaviour
> {
>     public static void Bake()
>     {
>         string[] scenes = { "Assets/Scenes/SampleScene.unity", "Assets/Scenes/SampleScene2.unity" };
>         var SceneIterator = scenes.GetEnumerator();
>         while (SceneIterator.MoveNext())
>         {
>             EditorSceneManager.OpenScene(SceneIterator.Current as string);
>             Lightmapping.Bake();
>             EditorSceneManager.SaveScene(EditorSceneManager.GetActiveScene());
>         }
>     }
> }
> ```
> before run this script you must make scenes on upward script "scenes" string array have. I think you already know about it.

3. close Unity project and open cmd as an Administrator. type below.
> ```
> >> "Your Unity.exe Path" -projectPath "Your UnityProject Path"  -executeMethod static_function_you_want_run -quit -batchmode -username "Your Unity ID" -password "Your Unity Passward"
> ```
> on upward command you can run static function BuildLightMap.Bake().
> This is example.
> ```
> >> "E:\Unity\2018.2.4f1\Editor\Unity.exe" -projectPath "D:\WorkSpace\UnityProject" -executeMethod BuildLightMap.Bake -quit -batchmode -username "user@gmail.com" -password "password"
> ```

## Make it as batch file
1. It is very annoying to type upward command whenever you build lightmap. so we make it as executable command file.
> [What is batch file?](https://en.wikipedia.org/wiki/Batch_file)<br>
> [learn batch](https://www.tutorialspoint.com/batch_script/)

2. Make txt file on your Unity Project Path. and type command we use before on **"How to build unity lightmap via cmd"** number3.
> ![Make txt and type command](./images/makeTextFile.jpg)

3. Change txt file extension to .bat
> ![Change Extension](./images/changeToBat.jpg)

4. Now you can bake lightmap by execute this batch file!

## Run in Remote
From this part it will be little different with real network environment. Because i tested it on VMWare.
> #### Before Start
> [PsExec](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec)<br>
> [PsExec Port Information](http://jamesrayanderson.blogspot.com/2010/04/psexec-and-ports.html)<br>
> [PsExec Access Denied](https://stackoverflow.com/questions/828432/psexec-access-denied-errors)<br>
> [specific port firewall off via cmd](https://helpdeskgeek.com/networking/windows-firewall-command-prompt-netsh/)


I will call lightmap build computer as remote, and lightmap build request computer as work or client computer.


1. Download PsExec and Extract on C:\

2. Copy to unity project your remote compute(lightmap build computer).

3. Open batch file and change the Unity.exe path and Unity Project path to fit your remote computer.

4. Make Batch file with below command.
> ```
> >> "C:\PSTools\PsExec.exe" \\remote_name -u user_id_on_remote -p password_on_remote -i buildlightmap_batchfile_path
> ```
> this is example.
> ```
> >> "C:\PSTools\PsExec.exe" \\WINDEV1809EVAL -u user -p password -i C:\Users\user\Desktop\Share\UnityProject\buildlightmap.bat
> ```

5. When you meet the message "Access Denied.", read upward **Before Start**'s **PsExec Access Denied**.
> if you think it is very annoying to read. just follow it.<br>
> type below on cmd of your remote computer. you must execute cmd as an Administrator.<br>
> ```>> reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\system /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1 /f```<br>
> and then type below too.<br>
> ```>> netsh advfirewall firewall add rule name="PsExec Port Open" protocol=TCP dir=in localport=445 action=allow```<br>
> and now you can execute lightmap build command on your remote computer without touch.<br>
>
> **Attention! You must execute cmd as an Administrator on your client computer too.**
