---
layout: post
title: "Magic The Gathering: Arena; On Linux With Wine"
date: 2020-03-16 12:00
comments: true
categories: linux
---

1. Download the latest .msi file, <a target="_blank" href="https://mtgarena-support.wizards.com/hc/en-us/articles/4402583467156-Installing-through-the-Windows-Installer-Package-MSI-">here</a>
2. Execute the following from the terminal: `wine start MTGAInstaller_0.1.3213.823095.msi`.
3. Follow the Setup Wizard prompts. It will look something like this:

<img src="https://web.archive.org/web/20210424093119im_/https://s3.amazonaws.com/duxsite-mtgarena/424edacfa6c259e7ea380dbacf5a5b90b4eb6a9874eb1985c500ecb309f1b6f9u46.png" />

4. Once installation is complete, go to your Wine path then enter `Wizards of the Coast` and run `chmod +x MTGA.exe`.
5. And finally launch it via `wine MTGA.exe`.
