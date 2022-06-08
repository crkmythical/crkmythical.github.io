---
title: macOS使用记录
encrypt: true
enc_pwd: askding
date: 2022-03-02 19:07:59
top:
categories:
tags:
layout:
---
编写进度
- [x] 

# 基础配置
## 第三方软件管理工具Homebrew
- [软件包管理工具]: https://zhuanlan.zhihu.com/p/111014448
- [Git Topics-macOS]: https://github.com/topics/macos
- [Awesome List]: https://wangchujiang.com/awesome-mac/index.zh.html
[awesome-mac](https://github.com/jaywcjlove/awesome-mac/blob/master/README-zh.md)

## 基础优化脚本
```shell
echo '
#==============================================================#
#                System Preferences -> Sharing                 #
#==============================================================#
# forked on https://github.com/mathiasbynens/dotfiles/blob/main/.macos
'
xcode-select --install
# 设置hostname
#echo '----------> 配置《设置》-《共享》...'
#read -p "Enter new hostname of the machine: " hostname
#scutil --set HostName "$hostname" && echo -e "Setting new hostname: $hostname ... \033[32mDone!\033[0m"

# 设置computer name
#compname=$(sudo scutil --get HostName | tr '-' '.')
#scutil --set ComputerName "$compname" && echo -e "Setting new ComputerName: $compname ...\033[32m Done!\033[0m"

# 设置NetBIOSName
#sudo defaults write /Library/Preferences/SystemConfiguration/com.apple.smb.server NetBIOSName -string "$compname"  && echo -e "Setting NetBIOSName: $compname ... \033[32mDone!\033[0m"

echo '# 关闭开机音效'
sudo nvram StartupMute=%01
echo '# 复制时的高亮颜色-绿色'
defaults write NSGlobalDomain AppleHighlightColor -string "0.764700 0.976500 0.568600"  

# Set sidebar icon size to medium
defaults write NSGlobalDomain NSTableViewDefaultSizeMode -int 1

# Adjust toolbar title rollover delay
defaults write NSGlobalDomain NSToolbarTitleViewRolloverDelay -float 0

echo '# 预开展存储视窗'
defaults write NSGlobalDomain NSNavPanelExpandedStateForSaveMode -bool true
defaults write NSGlobalDomain NSNavPanelExpandedStateForSaveMode2 -bool true

echo '# 加快窗口反应速度 '
defaults write NSGlobalDomain NSWindowResizeTime -float 0.001
echo '# 禁用icloud存储'
defaults write NSGlobalDomain NSDocumentSaveNewDocumentsToCloud -bool false
echo '# 打印完毕自动退出打印机软件'
defaults write com.apple.print.PrintingPrefs "Quit When Finished" -bool true
echo '# 禁止提醒是否打开此软件'
defaults write com.apple.LaunchServices LSQuarantine -bool false 

# Set Help Viewer windows to non-floating mode
defaults write com.apple.helpviewer DevMode -bool true

# Reveal IP address, hostname, OS version, etc. when clicking the clock
# in the login window
sudo defaults write /Library/Preferences/com.apple.loginwindow AdminHostInfo HostName

# Disable Notification Center and remove the menu bar icon
#launchctl unload -w /System/Library/LaunchAgents/com.apple.notificationcenterui.plist 2> /dev/null

# Disable automatic capitalization as it’s annoying when typing code
defaults write NSGlobalDomain NSAutomaticCapitalizationEnabled -bool false

# Disable smart dashes as they’re annoying when typing code
defaults write NSGlobalDomain NSAutomaticDashSubstitutionEnabled -bool false

# Disable automatic period substitution as it’s annoying when typing code
defaults write NSGlobalDomain NSAutomaticPeriodSubstitutionEnabled -bool false

# Disable smart quotes as they’re annoying when typing code
defaults write NSGlobalDomain NSAutomaticQuoteSubstitutionEnabled -bool false

# Disable auto-correct
defaults write NSGlobalDomain NSAutomaticSpellingCorrectionEnabled -bool false

###############################################################################
# Trackpad, mouse, keyboard, Bluetooth accessories, and input                 #
###############################################################################
# 轻触点击Trackpad: enable tap to click for this user and for the login screen
defaults write com.apple.driver.AppleBluetoothMultitouch.trackpad Clicking -bool true
defaults -currentHost write NSGlobalDomain com.apple.mouse.tapBehavior -int 1
defaults write NSGlobalDomain com.apple.mouse.tapBehavior -int 1

# 触摸板轻触点击功能 Trackpad: map bottom right corner to right-click 
 defaults write com.apple.driver.AppleBluetoothMultitouch.trackpad TrackpadCornerSecondaryClick -int 2
defaults write com.apple.driver.AppleBluetoothMultitouch.trackpad TrackpadRightClick -bool true
defaults write com.apple.driver.AppleBluetoothMultitouch.mouse MouseButtonMode "TwoButton"
 defaults -currentHost write NSGlobalDomain com.apple.trackpad.trackpadCornerClickBehavior -int 1
 defaults -currentHost write NSGlobalDomain com.apple.trackpad.enableSecondaryClick -bool true

echo '# 三指拖拽'
defaults -currentHost write NSGlobalDomain com.apple.trackpad.threeFingerDragGesture -bool true
defaults write com.apple.driver.AppleBluetoothMultitouch.trackpad TrackpadThreeFingerDrag -bool true

echo '# 四指下滑出现expose'
defaults write com.apple.AppleMultitouchTrackpad TrackpadThreeFingerVertSwipeGesture -int 0
defaults write com.apple.driver.AppleBluetoothMultitouch.trackpad TrackpadThreeFingerVertSwipeGesture -int 0
defaults write com.apple.dock showAppExposeGestureEnabled -int 1

echo '# 加速触控板/鼠标的速度'
# defaults read/write -g com.apple.mouse.scaling 7.5
defaults write NSGlobalDomain com.apple.trackpad.scaling -int 2
defaults write NSGlobalDomain com.apple.mouse.scaling -int 2


echo '# 开启全部视窗组建支持键盘控制'
defaults write NSGlobalDomain AppleKeyboardUIMode -int 3


# Increase sound quality for Bluetooth headphones/headsets
defaults write com.apple.BluetoothAudioAgent "Apple Bitpool Min (editable)" -int 40

# key rates, normal minimum is 15 (225 ms)
echo '# 关闭按住键盘的输入限制'
defaults write NSGlobalDomain ApplePressAndHoldEnabled -bool false

echo '# >>>需要退出重新登录 ---加快键盘输入速度 Set a blazingly fast keyboard repeat rate'
defaults write -g NSGlobalDomain  KeyRepeat -int 1 # normal minimum is 2 (30 ms)
defaults write NSGlobalDomain InitialKeyRepeat -float 10.0

# Set language and text formats in US
defaults write NSGlobalDomain AppleLanguages -array "en" "nl"
defaults write NSGlobalDomain AppleLocale -string "en_US@currency=USD"
defaults write NSGlobalDomain AppleMeasurementUnits -string "Inches"
defaults write NSGlobalDomain AppleMetricUnits -bool false

# Show language menu in the top right corner of the boot screen
#sudo defaults write /Library/Preferences/com.apple.loginwindow showInputMenu -bool true

# Set the timezone; see `sudo systemsetup -listtimezones` for other values
sudo systemsetup -settimezone "Asia/Shanghai" > /dev/null

echo '
###############################################################################
#                            Energy saviing                                   #
###############################################################################
'

# Enable lid wakeup
sudo pmset -a lidwake 1

# Restart automatically on power loss
sudo pmset -a autorestart 1

# Restart automatically if the computer freezes
sudo systemsetup -setrestartfreeze on

# Sleep the display after 15 minutes
sudo pmset -a displaysleep 15

# Disable machine sleep while charging
sudo pmset -c sleep 0

# Set machine sleep to 5 minutes on battery
sudo pmset -b sleep 30

echo '# 设置电池进入睡眠间隔为24小时'
sudo pmset -a standbydelay 86400   
echo '# 关闭电源进入睡眠模式 '
sudo systemsetup -setcomputersleep Off > /dev/null

# Hibernation mode
# 0: Disable hibernation (speeds up entering sleep mode)
# 3: Copy RAM to disk so the system state can still be restored in case of a
#    power failure.
sudo pmset -a hibernatemode 0

# Remove the sleep image file to save disk space
sudo rm /private/var/vm/sleepimage
# Create a zero-byte file instead…
sudo touch /private/var/vm/sleepimage
# …and make sure it can’t be rewritten
sudo chflags uchg /private/var/vm/sleepimage

echo '
###############################################################################
#                                Screen                                       #
###############################################################################
'

# Require password immediately after sleep or screen saver begins
defaults write com.apple.screensaver askForPassword -int 1
defaults write com.apple.screensaver askForPasswordDelay -int 0

# Save screenshots to the desktop
defaults write com.apple.screencapture location -string "${HOME}/Desktop"

#  截屏格式为png 。Save screenshots in PNG format (other options: BMP, GIF, JPG, PDF, TIFF)
defaults write com.apple.screencapture type -string "png"

echo '# 关闭截屏阴影 Disable shadow in screenshots'
defaults write com.apple.screencapture disable-shadow -bool true

# Enable subpixel font rendering on non-Apple LCDs
# Reference: https://github.com/kevinSuttle/macOS-Defaults/issues/17#issuecomment-266633501
defaults write NSGlobalDomain AppleFontSmoothing -int 1

# Enable HiDPI display modes (requires restart)
sudo defaults write /Library/Preferences/com.apple.windowserver DisplayResolutionEnabled -bool true

echo '
###############################################################################
#                                    Finder                                   #
###############################################################################
'

# Finder: disable window animations and Get Info animations
defaults write com.apple.finder DisableAllAnimations -bool true

defaults write com.apple.dock workspaces-swoosh-animation-off -bool YES


echo '# 预设Finder起始位置为Download'
defaults write com.apple.finder NewWindowTarget -string "PfLo"
defaults write com.apple.finder NewWindowTargetPath -string "file://${HOME}/Downloads/"

# Show icons for hard drives, servers, and removable media on the desktop
echo '# 桌面显示外部硬盘'
defaults write com.apple.finder ShowExternalHardDrivesOnDesktop -bool true  
defaults write com.apple.finder ShowHardDrivesOnDesktop -bool false   # 桌面不显示硬盘
defaults write com.apple.finder ShowMountedServersOnDesktop -bool true
defaults write com.apple.finder ShowRemovableMediaOnDesktop -bool true

echo '# 显示系统隐藏文件'
# Finder: show hidden files by default
defaults write com.apple.finder AppleShowAllFiles -bool true


echo '# 显示文件扩展名、状态栏、文件路径'
# 显示文件扩展名
defaults write NSGlobalDomain AppleShowAllExtensions -bool true
# 显示状态栏
defaults write com.apple.finder ShowStatusBar -bool true
# 显示文件路径条
defaults write com.apple.finder ShowPathbar -bool true 

# Finder窗口标题显示完整路径 Display full POSIX path as Finder window title
defaults write com.apple.finder _FXShowPosixPathInTitle -bool true

echo '# 允许匡选Finde Quick Look文字'
defaults write com.apple.finder QLEnableTextSelection -bool true

echo '# 排序时目录始终在顶部'
defaults write com.apple.finder _FXSortFoldersFirst -bool true
echo '# 搜索时在当前文件夹内搜索'
defaults write com.apple.finder FXDefaultSearchScope -string "SCcf"
echo '# 更改文件后缀时不提醒'
defaults write com.apple.finder FXEnableExtensionChangeWarning -bool false 

# Enable spring loading for directories
defaults write NSGlobalDomain com.apple.springing.enabled -bool true

# Remove the spring loading delay for directories
defaults write NSGlobalDomain com.apple.springing.delay -float 0

echo '# 在网络和硬盘上不创建.DS_Store文件 ，重启生效'
sudo find / -name ".DS_Store" -depth -exec rm {} \;
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool true
defaults write com.apple.desktopservices DSDontWriteUSBStores -bool true
# 恢复.DS_store生成
#defaults delete com.apple.desktopservices DSDontWriteNetworkStores

# 删除已经存在的.DS_Store文件
sudo find / -name ".DS_Store" -depth -exec rm -f {} \;

echo '# 关闭磁盘镜像验证'
defaults write com.apple.frameworks.diskimages skip-verify -bool true
defaults write com.apple.frameworks.diskimages skip-verify-locked -bool true
defaults write com.apple.frameworks.diskimages skip-verify-remote -bool true

echo '# 有新磁盘被挂在时自动打开'
# Automatically open a new Finder window when a volume is mounted
defaults write com.apple.frameworks.diskimages auto-open-ro-root -bool true
defaults write com.apple.frameworks.diskimages auto-open-rw-root -bool true
defaults write com.apple.finder OpenWindowForNewRemovableDisk -bool true

# Show item info near icons on the desktop and in other icon views
/usr/libexec/PlistBuddy -c "Set :DesktopViewSettings:IconViewSettings:showItemInfo true" ~/Library/Preferences/com.apple.finder.plist
/usr/libexec/PlistBuddy -c "Set :FK_StandardViewSettings:IconViewSettings:showItemInfo true" ~/Library/Preferences/com.apple.finder.plist
/usr/libexec/PlistBuddy -c "Set :StandardViewSettings:IconViewSettings:showItemInfo true" ~/Library/Preferences/com.apple.finder.plist

# Show item info to the right of the icons on the desktop
/usr/libexec/PlistBuddy -c "Set DesktopViewSettings:IconViewSettings:labelOnBottom false" ~/Library/Preferences/com.apple.finder.plist

# Enable snap-to-grid for icons on the desktop and in other icon views
/usr/libexec/PlistBuddy -c "Set :DesktopViewSettings:IconViewSettings:arrangeBy grid" ~/Library/Preferences/com.apple.finder.plist
/usr/libexec/PlistBuddy -c "Set :FK_StandardViewSettings:IconViewSettings:arrangeBy grid" ~/Library/Preferences/com.apple.finder.plist
/usr/libexec/PlistBuddy -c "Set :StandardViewSettings:IconViewSettings:arrangeBy grid" ~/Library/Preferences/com.apple.finder.plist

# Increase grid spacing for icons on the desktop and in other icon views
/usr/libexec/PlistBuddy -c "Set :DesktopViewSettings:IconViewSettings:gridSpacing 100" ~/Library/Preferences/com.apple.finder.plist
/usr/libexec/PlistBuddy -c "Set :FK_StandardViewSettings:IconViewSettings:gridSpacing 100" ~/Library/Preferences/com.apple.finder.plist
/usr/libexec/PlistBuddy -c "Set :StandardViewSettings:IconViewSettings:gridSpacing 100" ~/Library/Preferences/com.apple.finder.plist

# Increase the size of icons on the desktop and in other icon views
/usr/libexec/PlistBuddy -c "Set :DesktopViewSettings:IconViewSettings:iconSize 80" ~/Library/Preferences/com.apple.finder.plist
/usr/libexec/PlistBuddy -c "Set :FK_StandardViewSettings:IconViewSettings:iconSize 80" ~/Library/Preferences/com.apple.finder.plist
/usr/libexec/PlistBuddy -c "Set :StandardViewSettings:IconViewSettings:iconSize 80" ~/Library/Preferences/com.apple.finder.plist

# Use list view in all Finder windows by default
# 使用column view作文Finder默认显示选项 Four-letter codes for the other view modes: `icnv`, `clmv`, `glyv`
defaults write com.apple.finder FXPreferredViewStyle -string "clmv"

echo '# 清空垃圾桶不提醒'
defaults write com.apple.finder WarnOnEmptyTrash -bool false 

echo ' Show the ~/Library folder'
chflags nohidden ~/Library && xattr -d com.apple.FinderInfo ~/Library

# Show the /Volumes folder
# sudo chflags nohidden /Volumes

echo '
###############################################################################
#                    Dock, Dashboard, and hot corners                         #
###############################################################################
'

# Enable highlight hover effect for the grid view of a stack (Dock)
defaults write com.apple.dock mouse-over-hilite-stack -bool true

echo '# 使用单选列和Dock使用黑色'
defaults write NSGlobalDomain AppleInterfaceStyle Dark



echo '# Set the icon size of Dock items to 36 pixels'
defaults write com.apple.dock tilesize -int 36

echo '# 使用缩放改变窗口大小 Change minimize/maximize window effect'
defaults write com.apple.dock mineffect -string "scale"

echo '# 程序缩小至图标 Minimize windows into their application’s icon'
defaults write com.apple.dock minimize-to-application -bool true

# Enable spring loading for all Dock items
defaults write com.apple.dock enable-spring-load-actions-on-all-items -bool true

echo '# 显示Docker中打开程序的小灯提示 Show indicator lights for open applications in the Dock'
defaults write com.apple.dock show-process-indicators -bool true

echo '# 关闭打开程序的动画效果 Don’t animate opening applications from the Dock'
defaults write com.apple.dock launchanim -bool false

echo '# 加快Mission Control的动画速度 Speed up Mission Control animations'
defaults write com.apple.dock expose-animation-duration -float 0.1
echo '# 关闭Mission Control群组化显示'
defaults write com.apple.dock expose-group-by-app -bool false


echo '# Disable Dashboard'
defaults write com.apple.dashboard mcx-disabled -bool true

echo '# 在多重桌面中移除Don’t show Dashboard as a Space'
defaults write com.apple.dock dashboard-in-overlay -bool true

# Don’t automatically rearrange Spaces based on most recent use
defaults write com.apple.dock mru-spaces -bool false

echo '# 关闭隐藏dock的延迟 Remove the auto-hiding Dock delay'
defaults write com.apple.dock autohide-delay -float 0
echo '# 关闭隐藏dock的动画效果 Remove the animation when hiding/showing the Dock'
defaults write com.apple.dock autohide-time-modifier -float 0

echo '# 自动隐藏dock栏 Automatically hide and show the Dock'
defaults write com.apple.dock autohide -bool true

echo '# 将隐藏的程序Dock图表半透明化显示Make Dock icons of hidden applications translucent'



defaults write com.apple.dock showhidden -bool true

echo '# Don’t show recent applications in Dock'
defaults write com.apple.dock show-recents -bool true

# Add iOS & Watch Simulator to Launchpad
#sudo ln -sf "/Applications/Xcode.app/Contents/Developer/Applications/Simulator.app" "/Applications/Simulator.app"
#sudo ln -sf "/Applications/Xcode.app/Contents/Developer/Applications/Simulator (Watch).app" "/Applications/Simulator (Watch).app"

# Hot corners
# Possible values:
#  0: no-op
#  2: Mission Control
#  3: Show application windows
#  4: Desktop
#  5: Start screen saver
#  6: Disable screen saver
#  7: Dashboard
# 10: Put display to sleep
# 11: Launchpad
# 12: Notification Center
# 13: Lock Screen
# Top left screen corner → MMission Control
defaults write com.apple.dock wvous-tl-corner -int 2
defaults write com.apple.dock wvous-tl-modifier -int 0
# Top right screen corner → Desktop
defaults write com.apple.dock wvous-tr-corner -int 4
defaults write com.apple.dock wvous-tr-modifier -int 0
# Bottom left screen corner → Lock Screen
defaults write com.apple.dock wvous-bl-corner -int 13
defaults write com.apple.dock wvous-bl-modifier -int 0

###############################################################################
#                            Safari & WebKit                                  #
###############################################################################

# Privacy: don’t send search queries to Apple
defaults write com.apple.Safari UniversalSearchEnabled -bool false
defaults write com.apple.Safari SuppressSearchSuggestions -bool true

# Press Tab to highlight each item on a web page
defaults write com.apple.Safari WebKitTabToLinksPreferenceKey -bool true
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2TabsToLinks -bool true

# Show the full URL in the address bar (note: this still hides the scheme)
defaults write com.apple.Safari ShowFullURLInSmartSearchField -bool true

# Set Safari’s home page to `about:blank` for faster loading
defaults write com.apple.Safari HomePage -string "about:blank"

# Prevent Safari from opening ‘safe’ files automatically after downloading
defaults write com.apple.Safari AutoOpenSafeDownloads -bool false

# Allow hitting the Backspace key to go to the previous page in history
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2BackspaceKeyNavigationEnabled -bool true

# Hide Safari’s bookmarks bar by default
defaults write com.apple.Safari ShowFavoritesBar -bool false

# Hide Safari’s sidebar in Top Sites
defaults write com.apple.Safari ShowSidebarInTopSites -bool false

# Disable Safari’s thumbnail cache for History and Top Sites
defaults write com.apple.Safari DebugSnapshotsUpdatePolicy -int 2

# Enable Safari’s debug menu
defaults write com.apple.Safari IncludeInternalDebugMenu -bool true

# Make Safari’s search banners default to Contains instead of Starts With
defaults write com.apple.Safari FindOnPageMatchesWordStartsOnly -bool false

# Remove useless icons from Safari’s bookmarks bar
defaults write com.apple.Safari ProxiesInBookmarksBar "()"

# Enable the Develop menu and the Web Inspector in Safari
defaults write com.apple.Safari IncludeDevelopMenu -bool true
defaults write com.apple.Safari WebKitDeveloperExtrasEnabledPreferenceKey -bool true
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2DeveloperExtrasEnabled -bool true

# Add a context menu item for showing the Web Inspector in web views
defaults write NSGlobalDomain WebKitDeveloperExtras -bool true

# Enable continuous spellchecking
defaults write com.apple.Safari WebContinuousSpellCheckingEnabled -bool true
# Disable auto-correct
defaults write com.apple.Safari WebAutomaticSpellingCorrectionEnabled -bool false

# Disable AutoFill
defaults write com.apple.Safari AutoFillFromAddressBook -bool false
defaults write com.apple.Safari AutoFillPasswords -bool false
defaults write com.apple.Safari AutoFillCreditCardData -bool false
defaults write com.apple.Safari AutoFillMiscellaneousForms -bool false

# Warn about fraudulent websites
defaults write com.apple.Safari WarnAboutFraudulentWebsites -bool true

# Disable plug-ins
defaults write com.apple.Safari WebKitPluginsEnabled -bool false
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2PluginsEnabled -bool false

# Disable Java
defaults write com.apple.Safari WebKitJavaEnabled -bool false
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2JavaEnabled -bool false
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2JavaEnabledForLocalFiles -bool false

# Block pop-up windows
defaults write com.apple.Safari WebKitJavaScriptCanOpenWindowsAutomatically -bool false
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2JavaScriptCanOpenWindowsAutomatically -bool false

# Enable “Do Not Track”
defaults write com.apple.Safari SendDoNotTrackHTTPHeader -bool true

# Update extensions automatically
defaults write com.apple.Safari InstallExtensionUpdatesAutomatically -bool true

###############################################################################
#                                Mail                                         #
###############################################################################

# Disable send and reply animations in Mail.app
defaults write com.apple.mail DisableReplyAnimations -bool true
defaults write com.apple.mail DisableSendAnimations -bool true

# Copy email addresses as `foo@example.com` instead of `Foo Bar <foo@example.com>` in Mail.app
defaults write com.apple.mail AddressesIncludeNameOnPasteboard -bool false

# Add the keyboard shortcut ⌘ + Enter to send an email in Mail.app
defaults write com.apple.mail NSUserKeyEquivalents -dict-add "Send" "@\U21a9"

# Display emails in threaded mode, sorted by date (oldest at the top)
defaults write com.apple.mail DraftsViewerAttributes -dict-add "DisplayInThreadedMode" -string "yes"
defaults write com.apple.mail DraftsViewerAttributes -dict-add "SortedDescending" -string "yes"
defaults write com.apple.mail DraftsViewerAttributes -dict-add "SortOrder" -string "received-date"

# Disable inline attachments (just show the icons)
defaults write com.apple.mail DisableInlineAttachmentViewing -bool true

# Disable automatic spell checking
defaults write com.apple.mail SpellCheckingBehavior -string "NoSpellCheckingEnabled"

###############################################################################
#                                 Spotlight                                   #
###############################################################################

# Hide Spotlight tray-icon (and subsequent helper)
#sudo chmod 600 /System/Library/CoreServices/Search.bundle/Contents/MacOS/Search
# Disable Spotlight indexing for any volume that gets mounted and has not yet
# been indexed before.
# Use `sudo mdutil -i off "/Volumes/foo"` to stop indexing any volume.
sudo defaults write /.Spotlight-V100/VolumeConfiguration Exclusions -array "/Volumes"

# 关闭spotlight索引的进程mdworker
# sudo mdutil -a -i off

# 关闭系统崩溃报错
defaults write com.apple.CrashReporter DialogType none
# 开启报错
# defaults write com.apple.CrashReporter DialogType crashreport 

###############################################################################
#                          Terminal & iTerm 2                                 #
###############################################################################

# Only use UTF-8 in Terminal.app
defaults write com.apple.terminal StringEncodings -array 4

# Use a modified version of the Solarized Dark theme by default in Terminal.app
osascript <<EOD
tell application "Terminal"
	local allOpenedWindows
	local initialOpenedWindows
	local windowID
	set themeName to "Solarized Dark xterm-256color"
	(* Store the IDs of all the open terminal windows. *)
	set initialOpenedWindows to id of every window
	(* Open the custom theme so that it gets added to the list
	   of available terminal themes (note: this will open two
	   additional terminal windows). *)
	do shell script "open '$HOME/init/" & themeName & ".terminal'"
	(* Wait a little bit to ensure that the custom theme is added. *)
	delay 1
	(* Set the custom theme as the default terminal theme. *)
	set default settings to settings set themeName
	(* Get the IDs of all the currently opened terminal windows. *)
	set allOpenedWindows to id of every window
	repeat with windowID in allOpenedWindows
		(* Close the additional windows that were opened in order
		   to add the custom theme to the list of terminal themes. *)
		if initialOpenedWindows does not contain windowID then
			close (every window whose id is windowID)
		(* Change the theme for the initial opened terminal windows
		   to remove the need to close them in order for the custom
		   theme to be applied. *)
		else
			set current settings of tabs of (every window whose id is windowID) to settings set themeName
		end if
	end repeat
end tell
EOD

# Enable Secure Keyboard Entry in Terminal.app
# See: https://security.stackexchange.com/a/47786/8918
defaults write com.apple.terminal SecureKeyboardEntry -bool true

# Disable the annoying line marks
defaults write com.apple.Terminal ShowLineMarks -int 0


# Don’t display the annoying prompt when quitting iTerm
defaults write com.googlecode.iterm2 PromptOnQuit -bool false

###############################################################################
#                             Time Machine                                    #
###############################################################################

# Prevent Time Machine from prompting to use new hard drives as backup volume
defaults write com.apple.TimeMachine DoNotOfferNewDisksForBackup -bool true

# 关闭Time Machine .Disable local Time Machine backups
#hash tmutil &> /dev/null && sudo tmutil disablelocal

###############################################################################
#                         Activity Monitor                                    #
###############################################################################

# Show the main window when launching Activity Monitor
defaults write com.apple.ActivityMonitor OpenMainWindow -bool true

# Visualize CPU usage in the Activity Monitor Dock icon
defaults write com.apple.ActivityMonitor IconType -int 5

# Show all processes in Activity Monitor
defaults write com.apple.ActivityMonitor ShowCategory -int 0

# Sort Activity Monitor results by CPU usage
defaults write com.apple.ActivityMonitor SortColumn -string "CPUUsage"
defaults write com.apple.ActivityMonitor SortDirection -int 0


###############################################################################
#                           TextEdit, and Disk Utility                        #
###############################################################################
# Enable Dashboard dev mode (allows keeping widgets on the desktop)
defaults write com.apple.dashboard devmode -bool true

# Use plain text mode for new TextEdit documents
defaults write com.apple.TextEdit RichText -int 0
# Open and save files as UTF-8 in TextEdit
defaults write com.apple.TextEdit PlainTextEncoding -int 4
defaults write com.apple.TextEdit PlainTextEncodingForWrite -int 4

# Enable the debug menu in Disk Utility
defaults write com.apple.DiskUtility DUDebugMenuEnabled -bool true
defaults write com.apple.DiskUtility advanced-image-options -bool true

# Auto-play videos when opened with QuickTime Player
defaults write com.apple.QuickTimePlayerX MGPlayMovieOnOpen -bool true


###############################################################################
# Mac App Store                                                               #
###############################################################################

# Enable the WebKit Developer Tools in the Mac App Store
defaults write com.apple.appstore WebKitDeveloperExtras -bool true

# Enable Debug Menu in the Mac App Store
defaults write com.apple.appstore ShowDebugMenu -bool true

# Enable the automatic update check
defaults write com.apple.SoftwareUpdate AutomaticCheckEnabled -bool true

# Check for software updates daily, not just once per week
defaults write com.apple.SoftwareUpdate ScheduleFrequency -int 1

# Download newly available updates in background
defaults write com.apple.SoftwareUpdate AutomaticDownload -int 1

# Install System data files & security updates
defaults write com.apple.SoftwareUpdate CriticalUpdateInstall -int 1

# Automatically download apps purchased on other Macs
defaults write com.apple.SoftwareUpdate ConfigDataInstall -int 1

# Turn on app auto-update
defaults write com.apple.commerce AutoUpdate -bool true

# Allow the App Store to reboot machine on macOS updates
defaults write com.apple.commerce AutoUpdateRestartRequired -bool true

###############################################################################
# Photos                                                                      #
###############################################################################

# Prevent Photos from opening automatically when devices are plugged in
defaults -currentHost write com.apple.ImageCapture disableHotPlug -bool true

###############################################################################
# Messages                                                                    #
###############################################################################

# Disable automatic emoji substitution (i.e. use plain text smileys)
defaults write com.apple.messageshelper.MessageController SOInputLineSettings -dict-add "automaticEmojiSubstitutionEnablediMessage" -bool false

# Disable smart quotes as it’s annoying for messages that contain code
defaults write com.apple.messageshelper.MessageController SOInputLineSettings -dict-add "automaticQuoteSubstitutionEnabled" -bool false

# Disable continuous spell checking
defaults write com.apple.messageshelper.MessageController SOInputLineSettings -dict-add "continuousSpellCheckingEnabled" -bool false


###############################################################################
# Google Chrome & Google Chrome Canary                                        #
###############################################################################

# Disable the all too sensitive backswipe on trackpads
defaults write com.google.Chrome AppleEnableSwipeNavigateWithScrolls -bool false
defaults write com.google.Chrome.canary AppleEnableSwipeNavigateWithScrolls -bool false

# Disable the all too sensitive backswipe on Magic Mouse
defaults write com.google.Chrome AppleEnableMouseSwipeNavigateWithScrolls -bool false
defaults write com.google.Chrome.canary AppleEnableMouseSwipeNavigateWithScrolls -bool false

# Use the system-native print preview dialog
defaults write com.google.Chrome DisablePrintPreview -bool true
defaults write com.google.Chrome.canary DisablePrintPreview -bool true

# Expand the print dialog by default
defaults write com.google.Chrome PMPrintingExpandedStateForPrint2 -bool true
defaults write com.google.Chrome.canary PMPrintingExpandedStateForPrint2 -bool true

###############################################################################
# Mac App Store                                                               #
###############################################################################

# Enable the WebKit Developer Tools in the Mac App Store
defaults write com.apple.appstore WebKitDeveloperExtras -bool true

# Enable Debug Menu in the Mac App Store
defaults write com.apple.appstore ShowDebugMenu -bool true

# Enable the automatic update check
defaults write com.apple.SoftwareUpdate AutomaticCheckEnabled -bool true

# Check for software updates daily, not just once per week
defaults write com.apple.SoftwareUpdate ScheduleFrequency -int 1

# Download newly available updates in background
defaults write com.apple.SoftwareUpdate AutomaticDownload -int 1

# Install System data files & security updates
defaults write com.apple.SoftwareUpdate CriticalUpdateInstall -int 1

# Automatically download apps purchased on other Macs
defaults write com.apple.SoftwareUpdate ConfigDataInstall -int 1

# Turn on app auto-update
defaults write com.apple.commerce AutoUpdate -bool true

# Allow the App Store to reboot machine on macOS updates
defaults write com.apple.commerce AutoUpdateRestartRequired -bool true


###############################################################################
# Address Book, Dashboard, iCal, TextEdit, and Disk Utility                   #
###############################################################################

# Enable the debug menu in Address Book
defaults write com.apple.addressbook ABShowDebugMenu -bool true

# Enable Dashboard dev mode (allows keeping widgets on the desktop)
defaults write com.apple.dashboard devmode -bool true

# Enable the debug menu in iCal (pre-10.8)
defaults write com.apple.iCal IncludeDebugMenu -bool true

# Use plain text mode for new TextEdit documents
defaults write com.apple.TextEdit RichText -int 0
# Open and save files as UTF-8 in TextEdit
defaults write com.apple.TextEdit PlainTextEncoding -int 4
defaults write com.apple.TextEdit PlainTextEncodingForWrite -int 4

# Enable the debug menu in Disk Utility
defaults write com.apple.DiskUtility DUDebugMenuEnabled -bool true
defaults write com.apple.DiskUtility advanced-image-options -bool true

# Auto-play videos when opened with QuickTime Player
defaults write com.apple.QuickTimePlayerX MGPlayMovieOnOpen -bool true


###############################################################################
# Time Machine                                                                #
###############################################################################

# Prevent Time Machine from prompting to use new hard drives as backup volume
defaults write com.apple.TimeMachine DoNotOfferNewDisksForBackup -bool true

# Disable local Time Machine backups
hash tmutil &> /dev/null && sudo tmutil disablelocal

###############################################################################
# Activity Monitor                                                            #
###############################################################################

# Show the main window when launching Activity Monitor
defaults write com.apple.ActivityMonitor OpenMainWindow -bool true

# Visualize CPU usage in the Activity Monitor Dock icon
defaults write com.apple.ActivityMonitor IconType -int 5

# Show all processes in Activity Monitor
defaults write com.apple.ActivityMonitor ShowCategory -int 0

# Sort Activity Monitor results by CPU usage
defaults write com.apple.ActivityMonitor SortColumn -string "CPUUsage"
defaults write com.apple.ActivityMonitor SortDirection -int 0

# Don’t display the annoying prompt when quitting iTerm
defaults write com.googlecode.iterm2 PromptOnQuit -bool false


# 参考 https://github.com/mathiasbynens/dotfiles/blob/main/.macos
```


## Finder操作
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211205224736.png)
### [窗口管理技巧](https://zhuanlan.zhihu.com/p/20132125)
- _**Command + W**_   关闭当前窗口
- _**Command + Q**_   退出程序
- _**Command + M**_  最小化当前窗口
- **_Command + H _**  隐藏当前程序所有窗口
- _**_Control Command + F**_  **窗口全屏 / 退出全屏**

高级技巧
- **_Command + Option + W_**   关闭当前程序的所有窗口
- **_Command + Option + M_**  最小化当前程序的所有窗口
进阶技巧
- _**Command + Tab 若干次，然后只松开 Tab 再按 Q**_        在切换程序界面快速关闭程序
        这是一个快捷键组合，操作过程中需要保持 Command 一直处于按住状态 
- _**Command + Option + Esc**_ 选择要强制退出的程序（可以用 Shift 键选中多个）遇到程序假死的情况下特别有用，可以强制关闭，无需等待。
- **_Command-Shift-Option-Esc  _**   强制退出当前程序
- 选中程序图标，**_按住 Option + 右键单击_**

### 置顶窗口
拖拽其他窗口的时候按住command键即可，原来的窗口会永远在最上面。

### 文件操作-重命名
选中文件回车是对文件重命名

### 复制目录下文件名列表
command+a，command+c。然后打开一个文本编辑器（比如TextMate），command+v即可。

### Finder当前路径
- 在Finder顶部显示文件路径`defaults write com.apple.finder _FXShowPosixPathInTitle -bool YES` 
- 快Finder底部显示路径信息 -`Option+Cmd+P` 
- 复制文件路径 -选中文件，按`Command+C`
### 在Finder的当前目录打开终端
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211123203414.png)

### 自动清空废纸篓
在Finder中选中文件，使用`command+delete`删除文件，
如果想彻底清除，使用`shift+command+delete`就会自动清空废纸篓。

### 显示隐藏文件
Finder中输入`shift+command+.`可以显示隐藏文件，
想恢复原来的设置，再输入一遍`shift+command+.`即可。
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/displayOrHideen.gif)
## 程序坞Dock设置
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20210925173610osx_dock.png)
```shell
defaults write com.apple.dock autohide-time-modifier -float 0.2 && killall Dock   # 设置启动坞动画时间设置为 0.2 秒

defaults delete com.apple.dock autohide-time-modifier && killall Dock       # 恢复启动坞默认动画时间

defaults write com.apple.dock autohide-delay -int 0 && killall Dock         # 设置启动坞响应时间最短

defaults delete com.apple.Dock autohide-delay && killall Dock               # 恢复默认启动坞响应时间
defaults write com.apple.dock use-new-list-stack -bool TRUE; killall Dock   # Dock中的文件夹(可能不习惯)
```
[Dock栏优化](https://sspai.com/post/33493)
![](https://camo.githubusercontent.com/4571fb7ddee3d9e2b1ca3e5b70d539a51f989a999176161f3b4274dff285ca45/687474703a2f2f696d672e736d79687661652e636f6d2f32303230303331335f313933352e706e67)

## 显示器
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20210925223619display.png)

### 如何移动菜单栏图标的位置
我们可以通过按住 `CMD` + 拖拽来调整标志点的位置

![](https://cdn.sspai.com/2019/02/07/3b914168ad82d7d9da040c8dbc66d128.gif)

### 菜单栏图标管理-Bartender 4

### 屏幕滚动到应用程序的顶部或底部
command+上下方向键



### 调整视觉大小
`Command+ -/+` 

## 触摸板设置
触摸板轻触和右键: 系统偏好设置 - 触控板 - 光标与点按 - 勾选 `轻点来点按` 和 `辅助点按`（双指点按或轻点）

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20210925184530osx_trackpad.png)

三指拖移: 系统偏好设置 - 辅助功能 - 指针控制 - 触控板选项 - 启用拖移 - 三指拖移
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20210925184911osx_three_drag.png)

## 调度中心
取消自动重新排列空间: 系统偏好设置 - 调度中心 - 取消勾选 根据最近使用情况自动重新排列空间

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20210925192854osx_missioncontrol.png)

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109282229322osx_misscontrol.png)

## 网络配置
### 如何配置多种网络环境
打开系统偏好设置-网络，点击位置下拉菜单，找到编辑位置，打开后即可增删编辑多套网络设置，设置完成后保存。 这时点击屏幕左上角的苹果图标，在下拉菜单里增加了一个位置选项，里面就是你配置好的多种网络设置，点击切换即可。


## 声音设置
控制音频切换: 系统偏好设置 - 声音 - 在菜单中显示音量,关闭开机提示音"咚“

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211206212108.png)
或者 `sudo nvram StartupMute=%01`

## 键盘设置
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20210925185520keyboard.png)
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220203111217.png)

Tab 键移动焦点: 系统偏好设置 - 键盘 - 快捷键 - 全键盘控制 `所有控制`
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20210925193433osx_kbd_tab.png)
添加程序快捷键
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202110081541473quickAction.png)
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202110081543444kbd_cs.png)

### 安全与隐私
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220203111915.png)



### Mac的原生输入法
- 中英文混合输入，输入中文的时候，打开caps_lock键，可以直接输入英文，关掉又切换回中文 
- 选词，通过－＋号可以切换字或词
- 使用Fn可以输入表情
- 用'可以进行手动分词，比如fang'an（方案） 
mac pinyin提供了“拆字”功能，比如“鱻”，输入“yu yu”然后按 「shift+空格」即可。
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/%E6%8B%86%E5%AD%97.gif)

### 特殊字符输入
省略号，option+; 
约等于，option+x 
度，shift+option+8 
除号，option+/ 
无穷大，option+5 
小于等于，option+, 
大于等于，option+. 
不等于，option+= 
圆周率Pi，option+p 
正负，shift+option+= 
平方根，option+v 
总和，option+w 
商标Trademark，option+2 
注册，option+r 
版权，option+g


### command+I直接打开邮件
使用Safari浏览网页的时候，如果你想把当前页面通过邮件发送给自己或别人，使用command+I，可以直接打开邮件并把当前网页附加到待发送的邮件中。

### 键盘映射神器karabiner-elements & 自动化神奇keyboard-maestro 
键盘映射 karabiner-elements
参考[capslox](https://www.waerfa.com/capslox-review)
```shell
brew install --cask karabiner-elements keyboard-maestro
```
## 电池设置
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109291625095osx_battery.png)

## 常用软件
### 防火墙
```bash
brew install --cask lulu
```
### uTools-效率神器
```shell
brew install --cask utools  m-cli
```
### 代理软件
#### 全局代理 proxychains & proxifier
```shell
brew install proxychains-ng proxifier
vim /usr/local/etc/proxychains.conf    # 配置文件
proxychains4 -q -f config_file program_name [arguments]  # 全局代理特定程序
proxychains4 -q /bin/zsh        # 全局代理zsh shell
```
#### VPN
```shell
brew install tinc shimo  openvpn-connect expressvpn zerotier-one 
brew install --cask viscosity
```
#### socks/vmess/ssr代理
```shell
brew install --cask clash-for-windows clashx v2rayu tunnelblick shadowsocksx-ng  frps frpc
```
clash源

```http
https://api.dler.io/sub?target=clash&new_name=true&url=https%3A%2F%2Fjiang.netlify.app&insert=false&config=https%3A%2F%2Fraw.githubusercontent.com%2FMazeorz%2Fairports%2Fmaster%2FClash%2FSkslaPro-Balance.ini
https://jiedian1.coding.net/p/1/d/1/git/raw/master/2.txt?download=true
```
[Google插件](https://github.com/zhaoolee/ChromeAppHeroes)
```
brew install --cask google-chrome firefox wpsoffice baidunetdisk  folx  bitwarden tencent-lemon
```
### 通讯软件
```shell
brew install --cask slack wechat qq dingtalk twitter telegram-desktop  tencent-meeting
brew install sunnyyoung/repo/wechattweak-cli && wechattweak-cli --install 
```
[weechat上手指南](https://gist.github.com/ShadowRZ/8db83c8d8dfc4a8f90c2d4e252c2c30b)
### 远程桌面
- **ToDesk**
```bash
ln -s "/System/Library/CoreServices/Applications/Screen Sharing.app" ~/Applications/
```
### 影音处理
```shell
brew install neteasemusic bilibili   yesplaymusic   androidtool handbrake omniplayer
brew reinstall --cask obs  blackhole-2ch
```
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109272027758obs_output.png)
### 磁盘挂载-mounty
```shell
brew install --cask mounty
```

### 网盘
```shell
brew install --cask cyberduck  syncthing   adrive  baidunetdisk
```

### 学习软件
- XMind
```shell
brew install --cask anki  marginnote bob
brew install --cask devonthink  # 文献管理工具
brew install basictex && sudo tlmgr update --self && sudo tlmgr install dvipng
```
[Bob插件集合](https://github.com/topics/bobplugin)       
安装  [bobplugin-google-translate](https://github.com/roojay520/bobplugin-google-translate)
![](https://github.com/roojay520/bobplugin-google-translate/raw/master/images/preview-setting.png)
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211206112838.png)

### RSS神器-winds
```shell
brew install --cask winds
```
### 解压缩The Unarchiver 
```bash
brew install --cask keka
```

### Nosleep
```shell
brew install --cask nosleep 
```
### 虚拟机
```shell
brew install  --cask  parallels vmware-fusion
```
vmware-fusion key:
```text
# for VMWare Fusion 12.x

ZF3R0-FHED2-M80TY-8QYGC-NPKYF
YF390-0HF8P-M81RQ-2DXQE-M2UT6
ZF71R-DMX85-08DQY-8YMNC-PPHV8

YU5XH-8KZ0Q-M80XY-HQZ7T-PC0EA

# generate used keygen fo VMWare Fusion 12.x
YF3T8-A6Y16-M819Z-LGNZZ-WFUAA
ZZ7JR-4YX57-088EZ-JGNQE-P78AF
UZ71U-FTG5M-484JQ-EXZGG-M7RRA
UG3NH-8DD4L-480NQ-MDPGZ-YGKU0
CU7TH-42G1K-08D0Q-FGXGV-W3AT2
GV59K-84F84-484JQ-WXPQG-PP8A6
YZ5HK-2RW87-48EUQ-Y6QQC-ZLHC4
UA1NK-8MY0L-4888Z-DPWZC-WGH9A
UY3J8-FPD8Q-M85AQ-T5WNV-MZHV6
YU5XH-8KZ0Q-M80XY-HQZ7T-PC0EA
FU5X0-4NEE4-0855Z-KMPXG-ZVKC0
YF5T8-8TZ4K-H84LY-X6N59-XQ29D
AU5T2-8QX5J-08E8Q-2YZQZ-PAAZD
AG7EA-66F55-M8DTZ-2FPZX-YLRV4
AF5RH-8PX8L-M8EMP-0EXZE-ZZ08F
AV5N8-2KYEH-48EMY-UYXZG-X20V8
AY3TR-DVFEM-M8DUY-V4M5Z-ZVUZF
VF3DA-43Z83-481TZ-34W5Z-NVUZF
GZ31A-48E1N-08EGY-MZNGT-QK2X8
YY108-6TE44-M81KY-46XEE-MCHW2
```

# [macOS终端优化](https://huadeyu.tech/tools/high-efficient-software.html)
- [command-not-find](https://command-not-found.com/)
## iTerm2
```shell
brew install --cask iterm2 tabby electerm
```
- Apperance

  ![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20210925200801iterm2_apperance.png)

- Profile

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20210925201359iterm2_profile.png)

Color-Sechemes

```shell
git clone  https://github.com/mbadolato/iTerm2-Color-Schemes.git   ~/.config/iterm2/color-schemes    
```
导入文件时，按住`Command+Shift+.` 可显示隐藏文件
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20210925204153iterm_im/export.png)

### SSH-vps配置
[SSH登录过程](https://mounui.com/549.html)
```shell
 Host VPS
        HostName 10x.132.34.104
        User root
	      Port 22222
        IdentityFile ~/.ssh/ed25519_vps
```
- 文件传输命令
```shell
scp -i ~/.ssh/ed25519_vps -P 22222  /Path/to/file  root@10x.132.34.104:/var/opt/  # 上传文件
scp -i ~/.ssh/ed25519_vps -P 22222   root@10x.132.34.104:/Path/to/file  .  # 下载文件到当前目录
```
- 端口转发
```bash
ssh -CNfg   -L 127.0.0.1:3389:52.192.75.217:63389  -i ~/.ssh/ed25519_crkmyth1cal   -p22222 root@52.192.75.217
```

### SSH超时登录解决方案
- 修改服务器参数 `/etc/ssh/sshd_config`
```shell
ClientAliveInterval 60
ClientAliveCountMax 60
```
- 修改本地参数 `/etc/ssh/ssh_config`
```shell
ServerAliveInterval 60
ServerAliveCountMax 60
```
或者使用ssh登录时 `ssh -o ServerAliveInterval=30 root@server-ip`



## Oh-My-Zsh
```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"  # 安装oh-my-zsh
echo "DISABLE_MAGIC_FUNCTIONS=true" >> ~/.zshrc    # 关掉URL转义
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.zshrc  && source ~/.zshrc    # brew换清华源
brew install gnu-sed && alias sed=gsed  # 使用gsed命令
sed -i 's/robbyrussell/gnzh/g'  .zshrc  # 替换主题
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions  # 下载zsh-autosuggestions插件
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting  # 下载syntax-highling插件
git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-completions   # 下载zsh-completions
brew install autojump
sed -i "s/plugins.*/plugins=(git web-search autojump zsh-autosuggestions zsh-syntax-highlighting zsh-completions )/g"  ~/.zshrc  # 配置插件
source ~/.zshrc
```

## Tmux
- [Tmux Plugin Manager使用及具体插件](https://www.cnblogs.com/hongdada/p/13528984.html) 
- [tmux插件管理器和插件](http://blog.codepiano.com/2018/06/11/tmux-tpm)
- [tmux入坑指南](http://koyo922.github.io/2016/02/21/tmux/)
- [tpm—套件管理工具](https://medium.com/starbugs/tpm-%E5%A5%97%E4%BB%B6%E7%AE%A1%E7%90%86%E5%B7%A5%E5%85%B7-%E8%AE%93%E4%BD%A0%E7%9A%84-tmux-%E6%9B%B4%E5%A5%BD%E7%94%A8-95ecd924c9d)
- [Tmux使用手册](http://louiszhai.github.io/2017/09/30/tmux/)

```shell
brew install tmux
git clone --recursive https://github.com/tony/tmux-config.git ~/.tmux
ln -s ~/.tmux/.tmux.conf ~/.tmux.conf
```

##  Swiss Army Knife for macOS 
```bash
brew install m-cli
```
## 实用命令
### 关闭SIP
后面不关闭 SIP 的话类似于 proxychains-ng 这种代理神器就无法使用。
重启 Mac，按住 Option 键进入启动盘选择模式，按 ⌘ + R 进入 Recovery 模式。
「菜单栏」 ->「 实用工具（Utilities）」-> 「终端（Terminal）」：

```shell
csrutil disable    # 关闭SIP
csrutil status    # 查看SIP状态

System Integrity Protection status: disabled.
```
### 取消4位密码限制
```shell
pwpolicy -clearaccountpolicies  # 取消4位数密码限制 
passwd                          # 更改密码
```
### 允许App的来源
```shell
sudo spctl --master-disable   
```
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20210925190032osx_appsource.png)


### Tips-pfetch-查看主机信息
```shell
brew install pfetch  # 查看主机配置信息
brew install ipinfo-cli
brew install nnn
brew install peco
```

### 开机静音(咚)
`sudo nvram SystemAudioVolume=%80`

### 恢复截屏损坏图片
你的可能是被修改过了，我们可以通过以下命令恢复默认路径：
```bash
defaults delete com.apple.screencapture location
```
注销重新登录，再次截屏看看文件是否保存在桌面上了。

### 让终端说英文
`say Hello Hacker`
### 搜索命令mdfind
mdfind是一个非常灵活的全局搜索命令，类似Spotlight的命令行模式，可以在任何目录执行文件名、文件内容进行检索

### 元信息命令mdls
```bash
mdls ~/Desktop/a.jpg
mdls ~/Desktop/a.jpg|grep ISO
```

### 查看文件信息的命令file
查看相关文件的类型和属性

### 生成man page的pdf文档
```bash
man -t grep |open -f -a Preview
```

### 开启root用户
打开Finder，输入shift+command+g，在前往文件夹中输入：
```Bash
/System/Library/CoreServices
```
然后在目录中找到目录实用工具并打开，解开左下角的小锁，然后点击顶部菜单的，你就会看到启用或停用root用户的选项了。然后我们在命令行下执行su -，就可以切换到root目录下，root的默认目录是/var/root。

### 多个用户登陆一个程序
```bash
open -n /Applications/XXX.app
```

### 使用sips命令批量处理图片
```bash
sips -Z 800~/Pictures/        # 把当前用户图片文件夹下的所有JPG图片宽度缩小为800px，高度按比例缩放
sips -r 90~/Pictures/         # 顺时针旋转90°
sips -f vertical ~/Pictures/*.JPG   # 垂直反转
```
### 获悉目录空间
`du -sh *`

### 对磁盘权限进行检查和修复
```bash
sudo periodic daily weekly monthly
```

### 截图默认类型
```bash
defaults write com.apple.screencapture type -string JPEG
```

### 重建Spotlight索引
```bash
sudo mdutil -i off /   # 该命令用来关闭索引
sudo mdutil -E /`      # 该命令用来删除索引
sudo mdutil -i on /`   # 该命令用来重建索引
```
### 批量复制文件
```bash
cp *.png *.jpeg *.gif /destpath
```

### Mac不休眠
在终端中输入：pmset noidle，即可。只要该命令一直运行，Mac就不会进入睡眠状态。关掉终端或ctrl+c可以取消该命令。 pmset是OS X提供的命令行管理电源的工具，其功能远不止于此。
pmset -g，查看当前电源的使用方案 sudo pmset -b displaysleep 5，设置电池供电时，显示器5分钟内进入睡眠 sudo pmset schedule wake "02/01/13 20:00:00"，设置电脑在2013年2月1日晚8点唤醒电脑 ……


# 写作软件
## 宇宙最强编辑器-Emacs
```shell
brew tap d12frosted/emacs-plus
sudo chown -R $(whoami) /usr/local/share/man/man8
brew install emacs-plus  --HEAD --with-modern-black-dragon-icon
echo "ln -s /usr/local/opt/emacs-plus@28/Emacs.app /Applications "
brew install git ripgrep coreutils fd
git clone --depth 1 https://github.com/hlissner/doom-emacs ~/.emacs.d
~/.emacs.d/bin/doom install
# 配置emacs配置文件
echo '--------> 添加emacs配置到zshrc ...'
echo "
### Emacs
alias e='emacsclient -a emacs'
alias vi='emacsclient -a emacs'
#export PATH='\$HOME/.emacs.d/bin:\$PATH'
### Emacs END
" >> .zshrc

sudo ln -s ~/.emacs.d/bin/doom /usr/bin/doom
```


## Markdown编辑器-Zettlr
```bash
brew install --cask zettlr notable
```
## 截屏/GIF录屏工具
- Cleanshot X
- iShot
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109272019777osx_ishot.png)
- kap
- LICEcap
```bash
brew install --cask keycastr # keycastr录制视频屏幕上显示键盘快捷键
brew install --cask kap
brew install --cask licecap
```

## 截屏提字OCR识别
- TextSniper
- Text Scanner

## 终端录制工具
```shell
brew install ttygif asciinema
```
## 图床软件-PicGo
```bash
brew install --cask picgo
```
- picgo命令行 `/Applications/PicGo.app/Contents/MacOS/PicGo upload`
- picgo配置文件 `~/Library/Application Support/picgo/data.json`
```json
{
  "picBed": {
    "current": "github",
    "list": [
      {
        "name": "SM.MS图床",
        "type": "smms",
        "visible": false
      },
      {
        "name": "腾讯云COS",
        "type": "tcyun",
        "visible": false
      },
      {
        "name": "微博图床",
        "type": "weibo",
        "visible": false
      },
      {
        "name": "GitHub图床",
        "type": "github",
        "visible": true
      },
      {
        "name": "七牛图床",
        "type": "qiniu",
        "visible": false
      },
      {
        "name": "Imgur图床",
        "type": "imgur",
        "visible": false
      },
      {
        "name": "阿里云OSS",
        "type": "aliyun",
        "visible": false
      },
      {
        "name": "又拍云图床",
        "type": "upyun",
        "visible": false
      }
    ],
    "github": {
      "branch": "main",
      "customUrl": "",
      "path": "images/",
      "repo": "crkmythical/PicGo",
      "token": "ghp_PAs54ZDPUKBDQvWuuFanv5UTiTYVKm1Af2tT"
    }
  },
  "settings": {
    "shortKey": {
      "picgo:upload": {
        "enable": true,
        "key": "CommandOrControl+Shift+P",
        "name": "upload",
        "label": "快捷上传"
      }
    },
    "server": {
      "port": 36677,
      "host": "127.0.0.1",
      "enable": true
    },
    "showUpdateTip": false,
    "pasteStyle": "markdown",
    "autoStart": true,
    "uploadNotification": true,
    "rename": false,
    "autoRename": false,
    "customLink": "[[Resource/image$fileName][$url]]",
    "privacyEnsure": true
  },
  "picgoPlugins": {},
  "debug": true,
  "PICGO_ENV": "GUI",
  "needReload": false,
  "uploaded": []
}
```

## 静态博客生成器-[Hexo](https://askding.github.io/Evolution/Built-Mr-Frame-with-Hexo-and-Git.html)
```shell
brew install git node  # 安装git 和node
npm config set registry https://registry.npm.taobao.org # 配置npm源
npm install -g hexo-cli  # 安装hexo
proxychains4 hexo  init Mr-Framework   # 初始化博客
git clone https://github.com/yelog/hexo-theme-3-hexo.git themes/3-hexo   # 下载3-hexo主题
echo '--------> 添加hexo配置到zshrc ...'
echo "
###### Hexo
alias hs='hexo clean && hexo g && hexo s'
alias hdb='hexo clean && hexo g && hexo d && git add . && git commit -m "update" && git push -f '
##### Hexo END
" >> ~/.zshrc
```

# 编程开发软件
## 基础配置
### [Git配置](https://mounui.com/183.html)
- 配置socks5代理
```shell
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'
# 取消git代理
git config --global --unset http.proxy
git config --global --unset https.proxy
cat ~/.gitconfig       # 查看 git 配置 或 git config -l
```
- 配置用户信息
```shell
git config --global user.name "crkmyth1cal"
git config --global user.email "ckrmyth1cal@protonmail.com"

git config user.name crkmyth1cal
git config user.email crkmyth1cal@protonmail.com
```
- 配置编辑器和对比工具
```shell
git config --global core.editor emacs
git config --global merge.tool vimdiff
```

- 配置密码
```shell
git config --global credential.helper <passwd>
git config --global credential.helper  'cache --timeout 3600'
```

- 查询配置
```shell
git config --system --list    # 查看系统配置
git config --global --list    # 查看当前用户配置
git config --local --list     # 查看当前仓库配置
git config --list             # 查看全部配置
git config --list             # 我们执行最后一条指令
core.symlinks=false
core.autocrlf=true
core.fscache=true
color.diff=auto
color.status=auto
color.branch=auto
color.interactive=true
```

- 获取git帮助
```shell
git help <verb>
git <verb> --help
man git-<berb>
```

- 配置ssh-key并添加到git账户
```shell
ssh-keygen 
cat ~/.ssh/id_rsa.pub | clipcopy
ssh-copy-id -i ~/.ssh/id_rsa.pub -p 22 root@vps
```
- 通过 `ssh-add` 添加到密钥管理器`ssh-agent`中
```shell
ssh-add ~/.ssh/ed25519_crkmy1cal
ssh-add ~/.ssh/ed25519_askDing
```
- 给当前仓库添加[子仓库](https://www.jianshu.com/p/9000cd49822c)
```shell
  cd Advanced-Ethical-Hacking-Training
  git submodule add git@github.com:askDing/PicGo.git Resources

  # 删除子模块
  git submodule deinit {MODULE}
  git rm --cached {MODULE}
```
### 克隆私有仓库
#### 配置[SSH Keys](https://github.com/settings/keys)
执行如下命令，新建SSH Key，直接 `Command+V`
```bash
pbcopy < ~/.ssh/*.pub| open https://github.com/settings/keys
```
#### 克隆
```bash
ssh-add  ~/.ssh/ed25519_crkmythical  # 添加私钥
git clone git@github.com:crkmythical/dotfiles.git
```


### JDK
[Oracle JDK](https://www.oracle.com/java/technologies/downloads/archive/)
```shell
brew install --cask zulu  # https://github.com/mdogan/homebrew-zulu
echo '<------------JDK版本管理脚本------->配置JDK切换版本函数jdk'
echo '
#### 切换jdk版本的函数
jdk(){
	 echo "
	  >>	Usage: jdk  [-v version]
	  			"
   	export JAVA_HOME=$(/usr/libexec/java_home $@);

	java -version
  	echo "JAVA_HOME:" $JAVA_HOME
}
' >> ~/.zshrc
```

### Maven
```shell
brew install maven
echo "
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                              http://maven.apache.org/xsd/settings-1.0.0.xsd">
   <localRepository>/Users/crkmyth1cal/Code/.repository/</localRepository>

   <mirrors>
      <mirror>
         <id>alimaven</id>
         <name>aliyun maven</name>
         <mirrorOf>central</mirrorOf>
         <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      </mirror>
   </mirrors>

</settings>
" > ~/.m2/settings.xml
```
### Dash
### [code snippets manager-SnippetsLab](http://www.renfei.org/snippets-lab/)
### 编码解码Devutils & Boop
### Beyond Compare

### HTTP接口调试工具http-toolkit &paw
```bash
brew install --cask  http-toolkit  paw
```

### ASCII艺术编辑器Monodraw
### Docker
```shell
brew install --cask docker 
```

### termius
```shell
brew install --cask termius
```

### macOS自带的Apache服务器
- Apache服务器的文件目录 `/Library/WebServer/Documents`
相关命令：
```bash
sudo apachectl start #开启apache
sudo apachectl restart #重启apache
sudo apachectl stop #关闭apache
```

## IntelliJ IDE
[激活教程](https://blog.lupf.cn/articles/2021/11/27/1637944722305.html)
[JetBrains激活](http://www.idejihuo.com/) 
- /Users/用户名/Library/Application Support/IntelliJIdeaXXXXXX，用于保存安装的插件
- /Users/用户名/Library/Caches/IntelliJIdeaXXXXXX，用于保存缓存、日志、以及本地的版本控制信息（local history 这个功能）
- /Users/用户名/Library/Preferences/IntelliJIdeaXXXXXX，用于保存自己IDEA的个人配置，相当于 Windows版本的config目录
- /Users/用户名/Library/ApplicationSupport/JetBrains/IntelliJIdeaXXXXXX，这个目录下也有其配置文件
```shell
brew install --cask  intellij-idea  scenebuilder pycharm  android-studio 
```

## Database
```shell
brew install --cask dbeaver-community   # 数据库客户端
```
### MySQL
```shell
brew install mysql@5.7                 # 安装mysql5.7
brew install mycli
brew services start/stop mysql@5.7     # 启动/停止mysql服务
echo 'export PATH="/usr/local/opt/mysql@5.7/bin:$PATH"' >> ~/.zshrc
mysql_secure_installation              # 配置root密码
mysql -uroot -p                        # 连接mysql
```
- MySQL服务操作

```shell
mysql.server start/stop/restart/status    # 启动/停止/重启/状态 mysql
 ```
- 设置MySQL密码
```shell
use mysql;
show variables like 'validate_password%'; # 查看MySQL完整的初始密码规则
set global validate_password.length=4;
set global validate_password.policy=low;
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'r00t'; # set password for 'root'@'localhost' = password('你设置的密码'); 
flush privileges;   # 刷新权限 并退出
quit; 
```
- 数据库开启外连
```shell
grant all on *.* to root@'%' identified by '你设置的密码' with grant option;
flush privileges;
```
### SqliteBrowser
```shell
brew install --cask db-browser-for-sqlite
```
### MongoDB
```shell
brew tap mongodb/brew
brew install mongodb-community
brew services start mongodb-community
brew install --cask robo-3t
```
### Redis
```shell
brew install --cask another-redis-desktop-manager
```
## 安全相关

### hexfiend
```shell
brew install --cask hex-fiend
```
### aircrack-ng 
```shell
brew install aircrack-ng binwalk hashcat masscan metasploit nmap sqlmap wpscan wireshark
brew tap caffix/amass
brew install amass
brew install --cask jd-gui maltego
```
### radare2 -like IDA
```shell
 brew install radare2
```

### linkliar
可以帮助你哄骗 Wi-Fi 和以太网接口的 MAC 地址
```bash
brew install --cask linkliar
```

# FAQ
## [Mac到底要不要关机](https://www.ifanr.com/app/657745)
* 如果每天用，建议 Mac 不关机，使用睡眠模式；
* 如果不常用，建议 Mac 关机。

## 关于是否需要一直充电
 Apple 官方的建议：
> 不要一直使用便携式电脑电源。
> 比较理想的使用方法是：外出时用电池，回到家充电，以保持电池电流的流动状态。


## 如何重置SMC
    重置 SMC 是个 Mac 常用操作，电源、背光、亮屏、不能充电等等，都可以以此解决，不过很多人不知道。如何 重置 SMC 呢？
   1、将 Mac 关机。
   2、在内建键盘上，按下键盘左侧的 Shift-Control-Option 键，然后同时按下电源按钮。按住这些按键和电源按钮 10 秒钟。 如果您的 MacBook Pro 带有 Touch ID，则 Touch ID 按钮也是电源按钮。 
   3、松开所有按键。
   4、再次按下电源按钮以开启 Mac。
   
## 卸载软件后，清楚残留
- 删除 `~/Library/Application Support`下对应的目录
- 删除`~/Library/Preferences`下对应的偏好设置文件

## 多QQ登陆
在已经打开的QQ中，按住「command + N」即可。
   
## 如何解决MAC软件（dmg，akp，app）出现程序已损坏的提示
```bash
sudo spctl --master-disable
```

##  **is damaged and can’t be opened. You should move it to the Trash.**
```bash
xattr -cr /Applications/AppName.app
```


## macbook pro 待机一段时间后就自动关机重启
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211018131026.png)


## 程序切换
command+tab进行顺序切换，
command+shift+tab进行逆序切换
使用command+\`（esc下面的键）进行同组程序切换。
## Spotlight注释功能定位文件
OSX的文件系统提供了Spotlight注释功能，可以帮助用户更有针对性的定位文件。选中一个文件或文件夹，command+I打开简介，在Spotlight注释功能中加入自己特定的关键词。关掉简介窗口，呼出Spotlight并输入刚才的关键词，可以准确定位到相关的文件或文件夹。

## 查看WI-FI地址
按住 Option 键，同时点击右上角 WiFi 图标即可。
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211211115140.png)
## 删除appStore下载的程序
打开launchpad，按住option键，就会看到所有的程序图标都会像iOS图标那样晃动起来，点击图标左上角的叉，即可删除程序

## 强制关闭程序
方法一：option+command+esc，调出强制退出应用程序的窗口，选择要退出的进程即可。
方法二：打开活动监视器，类似windows的任务管理器一样操作就好了。 
方法三：命令行下的kill命令，比如想杀掉TextMate，首先用ps -ax|grep TextMate找到进程号，然后用kill -9进程号，
即可。 至此，天下无杀不掉的进程。

## [Mac如何创建并隐藏用户](https://blog.csdn.net/lilyssh/article/details/120224824)
启用Root用户
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220502212837.png)

### 查看各用户ID
```bash
dscl . -list /Users UniqueID    
```
### 查看用户所属组ID
```bash
dscl . -read /Users/root PrimaryGroupID
```
### 查看各个组ID
```bash
dscl . -list /Groups PrimaryGroupID
```

### 创建用户 "luser" 并隐藏
```bash
dscl . -create /Users/luser IsHidden 1
dscl . -create /Users/luser UserShell /bin/zsh
dscl . -create /Users/luser RealName "Ethan Hunter"
dscl . -create /Users/luser UniqueID "1010"
dscl . -create /Users/luser PrimaryGroupID 80  # 80 admin组
dscl . -create /Users/luser NFSHomeDirecotry /Users/luser
```

### 创建组
```bash
dscl . -list /Groups PrimaryGroupID | awk '{print $2}' | sort -n
dscl . -create /Groups/<newGroup>
dscl . -create /Groups/<newGroup> PrimaryGroupID <9527>
```

### 隐藏个人专属目录和分享点
```bash
sudo chflags hidden /Users/luser
sudo dscl . -delete "/SharePoints/Hidden User’s Public Folder" #the user with the long name “Hidden User”
```

### 修改密码
```bash
dscl . -passwd /Users/luser <password>
```

### 加入admin用户组
```bash
dscl . -append /Groups/wheel GroupMembership luser
```

### 从组中删除用户
```bash
delete <groupName> GroupMembership <luser>
```

## 开机自启
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20210925235438login.png)


# 参考
[国光的macOS配置优化]: https://www.sqlsec.com/2019/12/macos.html#toc-heading-44
[MacOS工作环境配置]: https://github.com/zouzonghua/macOS
破解网站：
- [麦氪搜 iMacSO.com-验证码](https://www.imacso.com/)
- [点点Mac下载网-付费](https://macdwn.site/)
- [One麦普-麻烦](https://vanmaple.com/)
- [Digit77.com-麻烦](https://www.digit77.com/)
- [精品Mac应用分享-方便](https://xclient.info/)
- [MacWK-方便](https://macwk.com/) 
- [appstorrent-简单](https://appstorrent.ru)

- [MacOS之程序员](https://www.kancloud.cn/chandler/mac_os/480595)
- [awesome-macos-command-line](https://git.herrbischoff.com/awesome-macos-command-line)
- [MacOS X配置指南](https://wild-flame.github.io/guides/)
- [MacUpdate价格跟踪](https://www.macupdate.com/)
- [awosome-mac](https://github.com/jaywcjlove/awesome-mac)
