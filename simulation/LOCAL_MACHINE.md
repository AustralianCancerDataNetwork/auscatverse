# Local Machine

## How to Install and Setup Ubuntu on a Local Machine

1. **Download Ubuntu:** Go to the official [Ubuntu website](https://ubuntu.com/download) and download the latest version of Ubuntu. You can choose to download either the Desktop version or the Server version, depending on your needs.

2. **Create a bootable USB drive:** Once you've downloaded the Ubuntu ISO file, you need to create a bootable USB drive. You can use a tool like Rufus or Etcher to create a bootable USB drive.

3. **Boot from USB:** Insert the bootable USB drive into your laptop and restart the laptop. When your laptop restarts, press the appropriate key (usually F12, F10 or Esc) to access the boot menu. Select the USB drive from the boot menu to start the Ubuntu installation.

4. **Install Ubuntu:** Follow the on-screen instructions to install Ubuntu. You can choose to either erase your entire hard drive and install Ubuntu, or you can install Ubuntu alongside your existing operating system (dual-boot). Make sure you select the correct time zone and language settings during the installation process.

5. **Update Ubuntu:** Once Ubuntu is installed, you should update it to the latest version. Open the Terminal and run the following command: 
    ```
    sudo apt-get update && sudo apt-get upgrade
    ```

6. **Install additional software:** Depending on your needs, you may need to install additional software. Ubuntu has a Software Center where you can browse and install various applications. You can also install software using the Terminal by running the following command:
    ```
    sudo apt-get install [package name]
    ```

That's it! You should now have a fully functioning Ubuntu installation on your local laptop.

