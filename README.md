![Demo gif](https://raw.githubusercontent.com/Michaelvilleneuve/react-native-document-scanner/master/images/demo.gif)

# React Native Document Scanner (iOS only)

Live document detection library. Returns a base64 encoded string of the captured image, allowing you to easily store it or use it as you wish !

Features :
 - Live detection
 - Perspective correction and crop of the image
 - Live camera filters (brightness, saturation, contrast)
 - Flash
 - Easy to use base64 image

 #### Can be easily plugged with [react-native-custom-crop](https://github.com/Michaelvilleneuve/react-native-custom-crop)


 ![Demo crop gif](https://camo.githubusercontent.com/0ac887deaa7263172a5fd2759dba3d692e98585a/68747470733a2f2f73332d65752d776573742d312e616d617a6f6e6177732e636f6d2f6d69636861656c76696c6c656e657576652f64656d6f2d63726f702e676966)

## Getting started

`$ npm install react-native-document-scanner --save`

`$ react-native link react-native-document-scanner`

Edit the `info.plist` file in XCode and add the following permission : `NSCameraUsageDescription`

Remember, this library uses your device camera, you can't run it on a simulator.


## Usage
```javascript
import React,{ Component } from 'react';
import { View, Image } from 'react-native';

import DocumentScanner from 'react-native-document-scanner';

class YourComponent extends Component {
  render() {
    return (
      <View>
        <DocumentScanner
          onPictureTaken={data => this.setState({
            image: data.image,
            initialImage: data.initialImage,
            rectangleCoordinates: data.rectangleCoordinates,
          })}
          overlayColor="rgba(255,130,0, 0.7)"
          enableTorch={false}
          brightness={0.3}
          saturation={1}
          contrast={1.1}
          quality={0.5}
          onRectangleDetect={({ stableCounter, lastDetectionType }) => this.setState({ stableCounter, lastDetectionType })}
          detectionCountBeforeCapture={5}
          detectionRefreshRateInMS={50}
        />
        <Image source={{ uri: `data:image/jpeg;base64,${this.state.image}`}} resizeMode="contain" />
      </View>
    );
  }
}

```

## Properties

| Prop  | Default  | Type | Description |
| :-------- |:----:| :--------:| :----------|
| overlayColor | `none` | `string` | Color of the detected rectangle : rgba recommended |
| detectionCountBeforeCapture | `5` | `integer` | Number of correct rectangle to detect before capture |
| detectionRefreshRateInMS | `50` | `integer` | Time between two rectangle detection attempt |
| enableTorch | `false` | `bool` | Allows to active or deactivate flash during document detection |
| brightness | `0` | `float` | Increase or decrease camera brightness. Normal as default. |
| saturation | `1` | `float` | Increase or decrease camera saturation. Set `0` for black & white |
| contrast | `1` | `float` | Increase or decrease camera contrast. Normal as default |
| quality | `0.8` | `float` | Image compression. Reduces both image size and quality |

## Each rectangle detection
| Props             | Params                                 | Type     | Description |
|-------------------|----------------------------------------|----------|-------------|
| onRectangleDetect | `{ stableCounter, lastDetectionType }` | `object` | See below   |

The returned object includes the following keys :

- `stableCounter`

Number of correctly formated rectangle found (this number triggers capture once it goes above `detectionCountBeforeCapture`)

- `lastDetectionType`

Enum (0, 1 or 2) corresponding to the type of rectangle found
0. Correctly formated rectangle
1. Wrong perspective, bad angle
2. Too far


## Returned image

| Prop | Params | Type | Description |
| :----------- |:-------:| :--------:| :----------|
| onPictureTaken | `data` | `object` | Returns the captured image in an object `{ image: 'BASE64 string', initialImage: 'BASE64 string', rectangleCoordinates: 'object of coordinates' }` |



### If you prefer manual installation

1. In XCode, in the project navigator, right click `Libraries` ➜ `Add Files to [your project's name]`
2. Go to `node_modules` ➜ `react-native-pdf-scanner` and add `RNPdfScanner.xcodeproj`
3. In XCode, in the project navigator, select your project. Add `libRNPdfScanner.a` to your project's `Build Phases` ➜ `Link Binary With Libraries`
4. Run your project (`Cmd+R`)<

## Credits

This repo is a React Native implementation of the following native library : https://github.com/mmackh/IPDFCameraViewController
Special thank to Mark Peysale :)
