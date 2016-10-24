Ubuntu 16.04 LTS install TeamViewer
======
***Author***: Neal Zhang

***Email***: Loong@pplns.com

======
Links
------
*   [Preparation] (#Preparation)
*   [Install] (#Install)
*   [Launch] (#Launch)

Preparation
------
    Download the deb install package of Ubuntu from the following link:

    http://www.teamviewer.com/en/download/linux/

Install
------
    CD the deb package dir,then excute the following command:

    sudo dpkg --add-architecture i386

    apt-get update

    sudo apt-get install libdbus-1-3:i386 libasound2:i386 libexpat1:i386 libfontconfig1:i386 libfreetype6:i386 libjpeg62:i386 libpng12-0:i386 libsm6:i386 libxdamage1:i386 libxext6:i386 libxfixes3:i386 libxinerama1:i386 libxrandr2:i386 libxrender1:i386 libxtst6:i386 zlib1g:i386 libc6:i386

    sudo dpkg -i teamviewer*.deb

Launch
------
    sudo teamviewer
