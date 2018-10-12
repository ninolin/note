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
```
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
sudo npm install -g ios-deploy --unsafe-perm=true
sudo gem install cocoapods
ionic build
ionic capacitor copy ios
```

## Open the project in Xcode.
```
ionic capacitor open ios
```
or run /Project/ios/App/App.xcworkspace
