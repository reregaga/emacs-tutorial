# emacs-tutorial

## About Emacs [^1]
Emacs is a malleable system, so everybody will have their personal preferences to undertake a task. Any article on how to be productive with Emacs is thus opinionated.
## Set up
### Windows 10 [^1]
1. Download from http://ftp.gnu.org/gnu/emacs/windows/ `emacs-<version>/emacs-<version>-x86_64-installer.exe`.
2. Run file to install Emacs on your system.
#### Home dir [^1]
Set the default folder for Emacs by setting the `HOME` env variable.
Open Powershell in administrator mode and enter this line:
```
setx HOME %USERPROFILE%
```
Output: `SUCCESS: Specified value was saved.`
#### Emacs configuration folder [^1] [^2]
When you start Emacs, it loads the [initialisation file](https://www.gnu.org/software/emacs/manual/html_node/emacs/Init-File.html), or init file in short. This file contains Lisp code that loads any additional packages and configures your Emacs system. You can run Emacs without an init file, but you will undoubtedly want to change the defaults.

The first time you start Emacs, it will create the configuration folder. The init file lives in this folder, which also contains the packages you need to personalise your system. Emacs looks for a file called `.emacs`, `.emacs.el` or `init.el`. The dot in front of the file means that it is hidden from view to prevent clutter. Most Emacs documentation talks about your `.emacs` file.

The location of the configuration folder depends on your operating system and Emacs version. If you are unsure where to store the `init.el` file, then you can use the Emacs help functionality.

Type `C-h v` for help on variables and type `user-emacs-directory` and enter. The help buffer that now appears provides the correct folder name. *For me output it is:* 
 ```
user-emacs-directory is a variable defined in ‘subr.el’.
Its value is "~/.emacs.d/"
 ```
 *or more concrete:*
 ```
 %userprofile%\.emacs.d
 ```
You can close this buffer by pressing `q`. Save the `init.el` file to the folder listed here. Your init file will become active after your restart Emacs. You can download the complete [configuration file for the articles on this website from GitHub](https://github.com/pprevos/EmacsLife/blob/master/init.el) and save it in the user directory.

**Emacs sometimes automatically writes settings to the init file**. To **prevent Emacs from changing your hand-crafted config**, we can use **two separate init files**. The configuration below changes the automated configuration file's name to `custom.el` and loads it when it exists. The **line that starts with two semi colons is a comment which is not evaluated**.
```
;; Define the init file
(setq custom-file (expand-file-name "custom.el" user-emacs-directory))
(when (file-exists-p custom-file)
  (load custom-file))
```
The first line of code instructs Emacs to use a separate file for the internal configuration system (`custom.el`). The `expand-file-name` expression adds the filename to the location of the Emacs user directory. This method ensures that the code works on all systems, irrespective of the Emacs user directory location. The second and third line load this file when it exists. **As this is the first action in the configuration, your personal init file will override any settings in the custom file**.

## Configure Emacs [^2]
A **Lisp program consists of expressions, which are instructions nested between parentheses. Each expression starts with the name of a function, in most cases followed by some parameters**. The `setq` function is commonly used to set the content of a parameter. For example, `(setq inhibit-startup-message t)` has the same effect as ticking a box called `'inhibit startup message'`, while `(setq inhibit-startup-message nil)` is the same as removing the tick from that same box. The expression in this example determines whether Emacs will show a startup message when you first open it.

## Loading packages [^2]
The Emacs base system has extensive functionality, but you can also **extend its capability with packages**. Many people write packages in Emacs Lisp to improve or enhance what the system can do. Developers of these packages can distribute them through a **package repository**, which are websites that let you easily download and install packages.
- **ELPA**: [GNU Emacs Lisp Package Archive](https://elpa.gnu.org/) — the official package archive, enabled by default.
- **MELPA**: [Milkypostman’s Emacs Lisp Package Archive](https://melpa.org/) — Unofficial archive.

You can also download Lisp files directly from GitHub or other sources. Emacs has a built-in package manager, which does the job quite well. John Wiggle has developed the `use-package` software, which simplifies configuring packages. 

The next blob of Lisp code adds the MELPA archive to the packages archive list. The second part checks whether `use-package` is already installed. If that is not the case, Emacs will download the software from the relevant repository and install it. The last two lines activate `use-package` and set a variable that instructs it to ensure that any missing packages are automatically installed.
```
;; Define and initialise package repositories
(require 'package)
(add-to-list 'package-archives '("melpa" . "https://melpa.org/packages/") t)
(package-initialize)

;; use-package to simplify the config file
(unless (package-installed-p 'use-package)
  (package-refresh-contents)
  (package-install 'use-package))
(require 'use-package)
(setq use-package-always-ensure 't)
```
>Output error:
>```
>Warning (initialization): An error occurred while loading ‘c:/Users/[>username<]/.emacs.d/init.el’:
>
>error: Package ‘use-package-’ is unavailable
>
>To ensure normal operation, you should investigate and remove the cause of the error in your initialization file.
>Start Emacs with the ‘--debug-init’ option to view a complete error backtrace.
>```

This is the basic setup you need to run any of the other configurations in this series of articles.

## Spellcheck [^1]
The Flyspell package performs spell checking on Emacs. The ezwinports project maintains a large collection of useful open source software compiled for Windows computers, including Hunspell. To install Hunspell on Windows, download the software from [ezwinports](https://sourceforge.net/projects/ezwinports/files/hunspell-1.3.2-3-w32-bin.zip/). Extract the files in a folder on your drive, for example: `C:\ProgramData\ezwinports\`.

You need to add the `bin` folder to your path environment variable. A simple workaround is to add the full path of the Hunspell variable to your configuration, as such:
```
(setq ispell-program-name "C:/ProgramData/ezwinports/bin/hunspell")
```

Note that Emacs uses a forward slash, not the Windows backslash.

The binary distribution includes dictionaries for US English and UK English. You can find the dictionaries for other languages on the Internet; install them into `share/hunspell` directory in your Hunspell installation directory.

## Links [^1] [^2]
- [gnu.org/software/emacs/manual/html_node/emacs/Init-File.html](https://www.gnu.org/software/emacs/manual/html_node/emacs/Init-File.html)
- [github.com/pprevos/EmacsLife/blob/master/init.el](https://github.com/pprevos/EmacsLife/blob/master/init.el)
- [elpa.gnu.org](https://elpa.gnu.org/)
- [melpa.org](https://melpa.org/)
## Sources
[^1]: [lucidmanager.org/productivity/emacs-windows/](https://lucidmanager.org/productivity/emacs-windows/)
[^2]: [lucidmanager.org/productivity/configure-emacs/](https://lucidmanager.org/productivity/configure-emacs/)
