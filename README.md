# PowerShell-Profile
Custom PowerShell config using [oh-my-posh](https://ohmyposh.dev/) themes and [posh-git](https://github.com/dahlbyk/posh-git). 

## Allow PowerShell to run scripts

Prior to installing anything, you will need to allow PowerShell to run Scripts. Check the script execution policy setting by executing `Get-ExecutionPolicy`. If it is not set to `unrestricted`, use the following line to change the status : 

```
set-ExecutionPolicy unrestricted -Scope CurrentUser
```

This will be usefull for `posh-git` as well as for running `conda` commands in PowerShell. 

## Get access to your PROFILE file

To access your PowerShell profile, type :

```
notepad $PROFILE
```

This will open your profile file in a text editor allowing you to edit it. 

## Editing auto-completion features

By default in PowerShell, command names, parameter names, argument values and file paths can all be completed by pressing the `Tab` key. Using `right arrow` will also complete the whole command line based on previous inputs. However, there is not default configuration to auto-complete only 1 word at a time.

Adding `Set-PSReadLineKeyHandler -Chord "Ctrl+RightArrow" -Function ForwardWord` to the Profile file will solve this issue

## Installing oh-my-posh

[oh-my-posh](https://ohmyposh.dev/) is a custom theme engine that can be used on any Shell - including PowerShell. To install it, [instructions](https://ohmyposh.dev/docs/installation/windows) are available on their website. 

First, install oh-my-posh using :

```
winget install JanDeDobbeleer.OhMyPosh -s winget
```

Then restart the terminal. If oh-my-posh is not recognized as a command, you can run the installer again, or add it manually to your PATH (for example: `$env:Path += ";C:\Users\user\AppData\Local\Programs\oh-my-posh\bin`).

For your PowerShell to run oh-my-posh everytime you load it, you will need to add the following snippet in the Profile file : 

```
oh-my-posh init pwsh | Invoke-Expression
```

Then reload your profile using `. $PROFILE`. 

## Setting up posh-git

[posh-git](https://github.com/dahlbyk/posh-git) is a PowerShell module that integrates Git and PowerShell by providing Git status summary information that can be displayed in the PowerShell prompt. 

Installation guidelines are provided in the readme of [posh-git repository](https://github.com/dahlbyk/posh-git). First check that the prerequisites are met. Then run (if posh-git has never been installed before) :

```
Install-Module posh-git -Scope CurrentUser
```

Then to automatically load posh-git everytime PowerShell is opened add the following line to the Profile file : 

```
Import-Module posh-git
```

Then reload your profile using `. $PROFILE`. Now git tab completion should be enabled.

## Setting up conda for use in PowerShell

For conda to run in PowerShell, you will need to run the following line : 

```
conda init powershell
```

Then restart PowerShell. Note conda won't be functionnal is the `ExecutionPolicy` is not set to `Unrestricted`.

By default, conda will autoactivate itself, when the terminal is opened. If you prefer not, then disable auto-activation with:

```
conda config --set auto_activate_base false
```

## Customizing an oh-my-posh theme

With the above instructions, a basic PowerShell theme has been set. But oh-my-posh allows for endless customisation possibilities. Below are a few things that can be done to improve the PowerShell theme. 

### Using a custom theme

A large amount of themes are available with oh-my-posh. They can be used either by downloading .json files and loading them through the Profile file or by loading themes through [oh-my-posh github repository](https://github.com/JanDeDobbeleer/oh-my-posh/blob/main/themes/stelbent-compact.minimal.omp.json). All instructions are described in [oh-my-posh's documentation](https://ohmyposh.dev/docs/installation/customize).

Here, only the way using a json file is described. First browse through the available themes and download the json for the one you chose (e.g. [stelbent-compact.minimal](https://github.com/JanDeDobbeleer/oh-my-posh/blob/main/themes/stelbent-compact.minimal.omp.json))

Then place the downloaded .json in a dedicated folder, for instance `C:Users/user/Posh`. Then add the following snippet to the Profile file : 

```
oh-my-posh init pwsh --config 'C:/Users/user/Posh/stelbent-compact.minimal.withcondaenv.omp.json' | Invoke-Expression
```

⚠️ This line should replace the `oh-my-posh init pwsh | Invoke-Expression` that was previously added to the Profile file. 

Now reload your profile and the theme should be updated.

### Adding python .venv to PowerShell segments 

By default, python .venv won't necessarly be added to your PowerShell config. Luckily this can be changed by modifiying the .json used for the PowerShell theme. All instructions for creating custom configurations with oh-my-posh are listed [here](https://ohmyposh.dev/docs/configuration/general).

Adding the following block at the end of the segment will allow to display the python version and .venv that is currently used :

```
{
  "type": "python",
  "style": "powerline",
  "powerline_symbol": "\ue0b0",
  "foreground": "#100e23",
  "background": "#906cff",
  "template": "  {{ .Full }} {{ if .Venv }}{{ .Venv }}{{ end }}",
  "properties": {
      "fetch_virtual_env": true
    }
  }
```

See the theme file in the repo to know where to place this block in the segments.

### Installing a custom nerd font for icons

The default font in PowerShell won't readme most icons - such as the python icon for instance. To fix this, nerd fonts can be downloaded. For instance the `CaskaydiaCove Nerd Font` can be downloaded [here](https://github.com/ryanoasis/nerd-fonts/releases/download/v2.1.0/CascadiaCode.zip)

To install the font, unzip the downloaded folder, then open it, right click on the font you want to install and click 'install'.

### Set a font in PowerShell

Then, it is needed to select this font in the PowerShell setting file. To do that, open the terminal and click on the little down facing arrow on the upper part of the terminal. Then hit 'settings'. Then go to '> Windows PowerShell > Appearance' and chose the desired font. 

![image](https://github.com/user-attachments/assets/bc761bfd-d284-4c2d-a709-2d0cc9508e9a)
