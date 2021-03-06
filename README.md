# UIAccess UAC bypass
![alt text](https://img.shields.io/badge/Python-2_only-blue.svg "Python 2 only")

In these examples, we start a host process (msra.exe) that we steal the UIAccess token from. We downgrade the token IL from Medium+ to Medium. We use the token to spawn a new process (uihack.exe) with the UIAccess flag, we can now send keyboard events to the elevated processes.

Not designed to be stealthy, but it's for sure possible! This is a demo in Python 2, just to display how it works.

You need to build the uihack python file to an executable, make sure it stays in *dist* folder. Once you created the uihack executable, you can launch uub.py from a non-elevated command prompt.

### Demo:
[![Msconfig demo video](https://i.imgur.com/wv40H4Y.png)](https://vimeo.com/344744930 "Msconfig demo video")

Here's a few methods, showing how hijacking UIAccess tokens can be used to bypass UAC.

### Rstrui method:
This executable is running elevated by default. Since rstrui executable is vulnerable to class hijacking, we use that to spawn our executable which is defined inside uihack file. In order to trigger the hijack, we need to send keyboard events to the window. Upon success, a elevated console window or custom executable should appear. Read more here https://rootm0s.github.io/researching-rstrui-process

### Taskmgr method:
This executable is running elevated by default, we send keyboard events to the window in order to launch an elevated console using the "Run new task" feature in Task Manager. Upon success, a elevated console window should appear.

### Msconfig method:
This executable is running elevated by default, we send keyboard events to the window in order to navigate through the list of available tools until we reach Command Prompt. Upon success, a elevated console window should appear.

## Known issues:
 * If the system language is not English, we cannot detect the window since we use FindWindowA API call.
 * If the window doesn't appear within 5 seconds, we won't be able to detect the window since it's not visible/created yet. Increasing the sleep-time could probably solve this problem.
 
## Build with py2exe:
In order for a successful build, install the py2exe (http://www.py2exe.org) module and use the provided build.py script to compile all the scripts in to a portable executable. This only seems to work on Python 2, not on Python 3.

```python build.py uihack.py```

#### Creds to:
 * https://tyranidslair.blogspot.com/2019/02/accessing-access-tokens-for-uiaccess.html
 
 #### More UAC bypasses:
  * https://github.com/rootm0s/WinPwnage
  * https://github.com/hfiref0x/UACME
