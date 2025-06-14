---
layout: post
title:  "SQL Injection"
author: hungbeo197
categories: [ tech ]
image: assets/images/2017/05-17-linux-banner.jpg
---
Chỉnh thời gian Windows và Linux cùng chạy một lúc
```bash
foo@bar:~$ timedatectl set-local-rtc 1 --adjust-system-clock
```

Cài Google Chrome
```bash
foo@bar:~$ wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
foo@bar:~$ sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
foo@bar:~$ sudo apt-get update
foo@bar:~$ sudo apt-get install google-chrome-stable
```

Sử dụng packages:
```bash
foo@bar:~$ sudo apt-get install
```

<style>
.blder-tb {
    border: 1px solid black;
    padding: 5px 10px 5px 10px;
    font-family : "Consolas", "Courier New", Courier, monospace;
    font-size: 14px;
}
</style>
<table class="blder-tb">
    <tr class="blder-tb">
        <td class="blder-tb"> ttf-mscorefonts-installer (Font) </td>  <td class="blder-tb"> build-essential                  </td>
    </tr>
    <tr class="blder-tb">
        <td class="blder-tb"> default-jre (Java)               </td>  <td class="blder-tb"> default-jdk (Java)               </td>
    </tr>
    <tr class="blder-tb">
        <td class="blder-tb"> vlc                              </td>  <td class="blder-tb"> gnuplot                          </td>
    </tr>
    <tr class="blder-tb">
        <td class="blder-tb"> inkscape                         </td>  <td class="blder-tb"> gimp                             </td>
    </tr>
    <tr class="blder-tb">
        <td class="blder-tb"> qbittorrent                      </td>  <td class="blder-tb"> ubuntu-restricted-extras (Media) </td>
    </tr>
    <tr class="blder-tb">
        <td class="blder-tb"> curl                             </td>  <td class="blder-tb"> git                              </td>
    </tr>
    <tr class="blder-tb">
        <td class="blder-tb"> unity-tweak-tool                 </td>  <td class="blder-tb">                                  </td>
    </tr>
</table>
<br>

Cài skype:
```bash
foo@bar:~$ dpkg -s apt-transport-https > /dev/null || bash -c "sudo apt-get update; sudo apt-get install apt-transport-https -y"
foo@bar:~$ curl https://repo.skype.com/data/SKYPE-GPG-KEY | sudo apt-key add -
foo@bar:~$ echo "deb [arch=amd64] https://repo.skype.com/deb stable main" | sudo tee /etc/apt/sources.list.d/skype-stable.list
foo@bar:~$ sudo apt-get update
foo@bar:~$ sudo apt-get install skypeforlinux
```

Cài ibus:
```bash
foo@bar:~$ sudo apt-get install ibus-unikey    #Vietnamese
foo@bar:~$ sudo apt-get install ibus-mozc      #Japanese
foo@bar:~$ ibus restart
```