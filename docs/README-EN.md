<p align="center">
  <img src="assets/images/icon_256.png" height="256">
  <h1 align="center">MessAuto</h1>
  <h4 align="center">Automatic extraction of SMS and email verification codes on Mac platform</h4>
<p align="center">
<a href="https://github.com/LeeeSe/MessAuto/blob/master/LICENSE.txt">
<img src="https://img.shields.io/github/license/LeeeSe/messauto"
            alt="License"></a>
<a href="https://github.com/LeeeSe/MessAuto/releases">
<img src="https://img.shields.io/github/downloads/LeeeSe/messauto/total.svg"
            alt="Downloads"></a>
<a href="https://img.shields.io/badge/-macOS-black?&logo=apple&logoColor=white">
<img src="https://img.shields.io/badge/-macOS-black?&logo=apple&logoColor=white"
            alt="macOS"></a>
</p>

<p align="center">
  [<a href="./README.md">中文</a>] [<a href="docs/README-EN.md">English</a>]<br>
</p>

# MessAuto

MessAuto is a software for automatically extracting SMS and email verification codes on macOS, developed in Rust and suitable for any app.

Below is a demonstration of logging in with an SMS verification code using MessAuto.

https://github.com/LeeeSe/MessAuto/assets/44465325/6e0aca37-377f-463b-b27e-a12ff8c1e70b

🎉🎉🎉 MessAuto now supports the Mail app.

https://github.com/LeeeSe/MessAuto/assets/44465325/33dcec87-61c4-4510-a87c-ef43e69c4e9d

## Features

- Supports both Mail.app and iMessage.app
- Multilingual support: currently supports Chinese, English, and Korean; switches automatically based on system language
- Lightweight: occupies 8 MB storage, 14 MB memory
- Simple: no GUI, just a quiet taskbar tray icon, but functional
- Wide applicability: Safari's solution can only be used in Safari browser, this software is suitable for any app
- Automation: automatic paste and enter, or pop-up floating window
- Open source and free: paid solutions like [2FHey](https://2fhey.com/) require at least $5

## Usage

MessAuto is a menu bar software without a GUI. When first launched, MessAuto will prompt the user to grant full disk access. After granting permission, the system will require a restart of the software. Click the icon to list the menu:

- Auto Paste: MessAuto will store the detected verification code in your clipboard. If you don't want to manually paste when entering the code, you can enable this option. When enabled, MessAuto will actively prompt you to grant accessibility permissions.
- Auto Enter: Automatically presses the enter key after auto pasting the verification code.
- No Clipboard Occupation: MessAuto will not affect your current clipboard content. It will automatically restore your previous clipboard content, whether it's an image or text, after pasting the verification code. This feature will enable auto paste automatically.
- Temporarily Hide: Temporarily hide the icon, the icon will reappear upon restarting the application (requires exiting the background first), suitable for users who do not restart their Mac frequently.
- Permanently Hide: Permanently hide the icon, the icon will not reappear even after restarting the application, suitable for users who restart their Mac frequently. To show the icon again, edit the `~/.config/messauto/messauto.json` file, set `hide_forever` to `false`, and restart the application.
- Configuration: Click to open the configuration file in JSON format, where you can customize keywords.
- Log: Quickly open logs.
- Monitor Email: When enabled, it will monitor emails as well, requiring the Mail app to be running in the background.
- Floating Window: A convenient floating window will pop up after retrieving the verification code.

> Keywords: Also known as trigger words, when the message contains keywords such as "verification code", the program will execute a series of subsequent operations, otherwise, it will ignore the message.

<!-- <p align="center">
<img src="assets/images/status_item.png" alt="statesitem.jpg" width=548 style="padding:20px" >
</p> -->

## Note

The ARM64 version will prompt that the file is damaged when opened, as it requires an Apple developer signature to start normally. The author does not have an Apple developer certificate, but you can solve the problem with one command:

- Move MessAuto.app to the `/Applications` folder
- Execute `xattr -cr /Applications/MessAuto.app` in Terminal

If MessAuto fails to automatically paste the verification code, it is usually due to lack of accessibility or automation permissions. Try the following solutions:

1. Open System Preferences -> Security & Privacy -> Accessibility, remove MessAuto and add the new MessAuto.app.
2. Run `tccutil reset AppleEvents com.doe.messauto` to reset MessAuto's automation permissions, then restart the application and keep selecting the auto paste option to prompt MessAuto to request automation permissions.

## TODO

- [x] Optimize verification code extraction rules
- [x] Customizable keywords
- [x] Add configuration options in the menu
- [ ] ~~Automatically delete extracted verification code messages (no effective solution)~~
- [x] In-app updates
- [x] Automatic release with Github Action
- [x] Add logging functionality
- [ ] Create app homepage
- [x] Add email verification code detection
- [x] Add floating window for verification codes
- [x] Enhance floating window experience: the floating window starts at the current mouse position and can be moved
- [x] Enhance floating window experience: the floating window no longer takes user focus, simplifying the floating window usage process
- [x] Fix occasional issue where the cmd key fails to simulate being pressed
- [x] No longer occupies clipboard
- [ ] Optimize the logic for hiding the icon
- [ ] App signing
- [ ] Publish to Homebrew

## Motivation

The macOS platform can easily receive SMS from an iPhone, without needing to check the phone for the verification code each time. Safari can directly retrieve the verification code and display it in the input box, but this useful feature is limited to the Safari browser. Not everyone likes to use Safari. To bring this functionality to all apps, I developed this software.

Later, I found that many verification codes are sent not only via SMS but also via email, so I added support for emails.

## Requirements

- **macOS system** that can receive **iPhone** SMS (with "Text Message Forwarding" enabled)
- Mail app needs to run in the background to receive the latest emails in real-time
- Full disk access (to access the `chat.db` file of `Message.app` located under `～/Library` to get the latest SMS)
- Accessibility permissions (to simulate keyboard operations, auto paste, and enter)
- Automation permissions (to simulate keyboard operations, located in: Settings -> Security & Privacy -> Privacy -> Automation, this permission can only be granted through the app's request)

## Known Issues

- Some apps or websites do not support enter to log in, requiring manual click
- Occasionally, an unknown bug causes the CPU to max out one core (reproduction conditions not found)

## Build from Source

```bash
# Download the source code
git clone https://github.com/LeeeSe/MessAuto.git
cd MessAuto

# Build and run (optional, for development testing only)
cargo run

# Install cargo-bundle
cargo install cargo-bundle --git https://github.com/zed-industries/cargo-bundle.git --branch add-plist-extension
# Package the app
cargo bundle --release
```

The generated MessAuto application is located at `target/release/bundle/osx/MessAuto.app`.

## Log Directory

Log file directory: `~/.config/messauto/messauto.log`
Only the log from the most recent startup is kept.

## FAQ

- Given permissions but still prompts for permissions: The temporary solution is to remove the original MessAuto from the Accessibility and Disk Access permissions list in the settings panel, then agree to the permissions request when it pops up again.
- Permissions granted, but verification code can be extracted to clipboard but not auto-pasted: This may be due to the initial automation permission request being denied or ignored. This permission is located at: Settings -> Security & Privacy -> Privacy -> Automation. Users cannot directly add it, it can only be requested by the program again. To resolve, reset permissions by running `tccutil reset AppleEvents com.doe.messauto` and restart the program, repeatedly select the auto paste option to trigger the automation permission request.

## Acknowledgements

- Thanks to [@尚善若拙](https://sspai.com/post/73072) for providing the SMS retrieval idea.
