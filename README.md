name: iOS Build

on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  build:
    runs-on: macos-latest

    steps:
    - uses: actions/checkout@v3
    - name: Set up Xcode
      uses: maxim-lobanov/setup-xcode@v1
      with:
        xcode-version: '15.2'
    - name: Build iOS app
      run: |
        xcodebuild -workspace PicaComic.xcworkspace \
                   -scheme PicaComic \
                   -sdk iphoneos \
                   -configuration Release \
                   BUILD_DIR=$PWD/build \
                   CODE_SIGN_IDENTITY="" \
                   CODE_SIGNING_REQUIRED=NO
    - name: Upload IPA
      uses: actions/upload-artifact@v4
      with:
        name: PicaComic.ipa
        path: build/Release-iphoneos/*.ipa
