---
id: 20210906181547
uuid: 06e71c60-9ac7-4eae-94eb-e40fd45305a8
title: URI Handler
date: 2021-09-06 18:15:47
modified: 
types: tech-note
categories: miscellaneous
pinned: false
tags: [gvim, vim, uri, handler, gnome, macos, fedora, macvim, xdg-open]
private: false
draft: false
---

## Gnome

You can create a URI handler for anything really easily. Below is an example for creating a URI handler for Gnome Text Editor.

Create `/usr/share/applications/gte.desktop` and populate it with the below:

```sh
[Desktop Entry]
Version=1.0
Type=Application
Name=gte
Exec=gte %U
Terminal=false
NoDisplay=true
MimeType=x-scheme-handler/gte
```

Then create `/usr/local/bin/gte`:

```sh
#!/bin/bash

PKG=$(echo $1|sed 's|^gte://||')
gnome-text-editor "$PKG"
```

You can then test it with the below commands:

```sh
sudo update-desktop-database
xdg-open 'gte:///home/jprice/my/files/documents/log/content/inbox.md'
```

## Fedora 34

Install Ruby, `dnf install ruby`.

Create `/usr/share/applications/guivim.desktop`:

```ini
[Desktop Entry]
Encoding=UTF-8
Name=Vim (GUI)
Comment=Edit text files in a console using Vim
Exec=/usr/local/bin/guivim %U
Terminal=false
Type=Application
Icon=/usr/src/vim/runtime/vim48x48.xpm
Categories=Application;Utility;TextEditor;
MimeType=text/plain;x-scheme-handler/guivim;
StartupNotify=true
StartupWMClass=GUIVIM
```

Then create `/usr/local/bin/guivim`. To make sure it doesn't clash with the existing `gvim` in `/usr/bin/gvim`, we name it `guivim`, but note that we point to `gvim` in the script.

```ruby
#! /usr/bin/env ruby

require 'uri'
require 'cgi'

full_path = ARGV[0]

if full_path
  uri = URI::parse(full_path)

  vim_params = %Q["#{uri.path}"]

  if uri.query
    params = CGI::parse(uri.query)
    line = params["line"][0]
  end

  vim_params << " +#{line}" if line
end

# --remote-silent opens a session, and then opens futher files
# in that session:
`gvim --remote-silent #{vim_params}`
```

Finally run the below commands:

```sh
sudo chmod a+x /usr/local/bin/guivim
sudo update-desktop-database
xdg-open 'guivim:///etc/hosts'
```

The original source can be found [here](/attachments/20210906181547.html)


## MacOS

There's a number of seemingly complicated, and possibly quite unclear ways to do this on MacOS, with Vim in the Terminal, but by far the easiest, simplest way is to just use MacVim.

The MacVim help file details how it works, and it works superbly well.

```text
9. mvim:// URL handler				*mvim://* *macvim-url-handler*

MacVim supports a custom URL handler for "mvim://" URLs. The handler is
supposed to be compatible to TextMate's URL scheme as documented at >
	http://blog.macromates.com/2007/the-textmate-url-scheme/.
Currently, this means that the format is >
	mvim://open?<arguments>
where "arguments" can be:
	* url — the actual file to open (i.e. a file://... URL), if you leave
		out this argument, the frontmost document is implied
	* line — line number to go to (one based)
	* column — column number to go to (one based)

For example, the link >
	mvim://open?url=file:///etc/profile&line=20
will open the file /etc/profile on line 20 when clicked in a web browser.

Note that url has to be a file:// url pointing to an existing local file.
```

### Open In Same Window, with New Buffer

1. Go to "Preferences" (`⌘ ,`), and make sure you're on the "General" tab.
1. Under "Open files from applications:" select "in the current window".
1. In the pull down menu below this option select "and set the arglist" and files will open in a new buffer, in the same window.

