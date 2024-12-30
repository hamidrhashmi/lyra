I have used Ubuntu 22.04 machine and followed the following instructions

### Install bazel
Add Repo for Bazel. If you are using any other OS please follow the [documentation](https://bazel.build/install)
```bash
sudo apt install apt-transport-https curl gnupg -y
curl -fsSL https://bazel.build/bazel-release.pub.gpg | gpg --dearmor >bazel-archive-keyring.gpg
sudo mv bazel-archive-keyring.gpg /usr/share/keyrings
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/bazel-archive-keyring.gpg] https://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
```
Install bazel v5.3.2
```bash
apt update
apt install bazel=5.3.2
```

### Install Python
Lyra codec need pytho3, numpy, and six
```bash
apt install python3 python3-pip python3-numpy python3-six unzip zip
```

### Install JDK
Download [OracleJDK](https://www.oracle.com/ca-en/java/technologies/downloads/) version 21
```bash
wget https://download.oracle.com/java/21/latest/jdk-21_linux-x64_bin.deb
dpkg -i jdk-21_linux-x64_bin.deb
```

### Install Android Command Line Tools
Download [sdkmanager](https://developer.android.com/studio)
```bash
wget https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip
unzip commandlinetools-linux-11076708_latest.zip
mv cmdline-tools /usr/local/android-cmd
```
### Set Envoirnmnet Variables
```bash
vim /root/.bashrc
```
> export ANDROID_NDK_HOME=$HOME/android/sdk/ndk/21.4.7075529
> 
> export ANDROID_HOME=$HOME/android/sdk
> 
> export JAVA_HOME=/usr/lib/jvm/jdk-21.0.5-oracle-x64
> 
> export PYTHON_BIN_PATH=/usr/bin/python3

### Download Android SDK/NDK
```bash
/usr/local/android-cmd/bin/sdkmanager  --sdk_root=$HOME/android/sdk --list
/usr/local/android-cmd/bin/sdkmanager  --sdk_root=$HOME/android/sdk --install  "platforms;android-30" "build-tools;30.0.3" "ndk;21.4.7075529"
```

### Download Lyra
```bash
git clone https://github.com/hamidrhashmi/lyra.git
cd lyra
```
make soft links to android SDK and then execute the following commands
```bash
bazel sync
bazel build -c opt lyra/android_example:lyra_android_lib --config=android_arm64
```
If you want to create the binaries for Linux
```bash
bazel build -c opt lyra/cli_example:encoder_main --config=android_arm64
bazel build -c opt lyra/cli_example:decoder_main --config=android_arm64
```

### Extra
Some Extra useful commands
```bash
apt install ninja
apt-get install libc++-dev
export ANDROID_BIN_PATH=/root/android/sdk/ndk/21.4.7075529/toolchains/llvm/prebuilt/linux-x86_64/
bazel build -c opt lyra/android_example:lyra_android_example --config=android_arm64 --config=clang_toolchain
echo $ORIGIN
bazel-out/android-arm64-v8a-opt/bin/
```

### More Extra
```
apt install git aptitude
cd /usr/local/
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
export PATH=/usr/local/depot_tools:$PATH
mkdir /root/WebRTC/
cd /root/WebRTC/
fetch --nohooks webrtc_android
gclient sync
/src/build/install-build-deps.sh
cd src/
gn gen out/Debug --args='target_os="android" target_cpu="arm"'
ninja -C out/Debug
ninja -C out/Release

gn gen out/Release-Android-With-Opus --args='target_os="android" target_cpu="arm64" is_debug=false is_component_build=false rtc_include_tests=false use_openh264=false use_vpx=false use_system_opus=true use_system_zlib=true'
gn gen out/Release --args='target_os="android" target_cpu="arm64" is_debug=false'
```
### Some USeful links

https://commondatastorage.googleapis.com/chrome-infra-docs/flat/depot_tools/docs/html/depot_tools_tutorial.html#_setting_up
https://webrtc.github.io/webrtc-org/native-code/android/
https://webrtc.github.io/webrtc-org/native-code/development/prerequisite-sw/  


Enjoy :wink:
