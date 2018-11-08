# Start ionic4 app on ios and android

## Create app
```
ionic start --type=angular
or
ionic start myApp tabs --type=angular
```

## Running on web
```
ionic serve
```

## Running on ios

### Build code
```
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
sudo npm install -g ios-deploy --unsafe-perm=true
sudo gem install cocoapods
ionic build                 //build成ios上可執行的程式 (build上ios device 或 simulator 的話每次都要執行)
ionic capacitor copy ios    //把build好的檔案放到ios目錄 (build上ios device 或 simulator 的話每次都要執行)
```

### Open the project in Xcode.
```
ionic capacitor open ios
```
or run /Project/ios/App/App.xcworkspace

### Setting Xcode

1. 在 Xcode -> Perferences -> Accounts 設定 signing team 新增 Apple ID
2. 在 General -> Identity -> Bundle Identifier 中打入 domain ex:com.iii
3. 在 General -> Identity -> Signing -> Team 中選擇剛剛新增的  Apple ID
4. 在 Xcode上 直接選擇要deploy的iphone就可以deploy上去

P.S.: XCode 中可以使用(shift+cmd+K)來Clean Project

參考:
https://juejin.im/entry/5960d749f265da6c3a54daba
https://stackoverflow.com/questions/49413956/error-exportarchive-no-profiles-for-io-ionic-starter-were-found
https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E5%BE%9E-xcode-8-%E5%B0%87app%E5%AE%89%E8%A3%9D%E5%88%B0-iphone-%E7%9A%84%E5%B0%8F%E5%B0%8F2%E5%80%8B%E6%AD%A5%E9%A9%9F-39f7b81b69a6


