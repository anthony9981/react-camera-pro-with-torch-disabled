![npm][npm-badge]
![downloads][downloads-badge]

# react-camera-pro-with-torch

Universal Camera component for React.

Designed with focus on Android and iOS cameras.
Works with standard webcams as well.

See [this](http://caniuse.com/#feat=stream) for browser compatibility.

Note: WebRTC is only supported on secure connections. So you need to serve it from https. You can test and debug in Chrome from localhost though (this doesn't work in Safari).

## Features

- mobile friendly camera solution (tested on iOS and Android)
- video element is fully responsive
  - you can setup parameter to cover your container
  - you can define aspectRatio of view: 16/9, 4/3, 1/1, ...
- taking photo to base64 jpeg file - with same aspect Ratio as view, with FullHD resolution (or maximum supported by camera).
- working with standard webcams or other video input devices
- supports autofocus
- supports torch
- switching facing/environment cameras (with your own button)
- detect number of cameras
- facing camera is mirrored, environment is not
- controlled by react [Ref](https://reactjs.org/docs/refs-and-the-dom.html)
- public functions to take photo, to switch camera and to get number of cameras
- function to switch flash on/off
- typescript library

## Installation

```
npm install --save react-camera-pro-with-torch
```

<!-- ## Demo -->

<!-- https://purple-technology.github.io/react-camera-pro/ -->

## Example

https://github.com/purple-technology/react-camera-pro/blob/master/example/src/App.tsx

## Usage

```javascript
import React, { useState, useRef } from "react";
import {Camera} from "react-camera-pro-with-torch";

const Component = () => {
  const camera = useRef(null);
  const [image, setImage] = useState(null);

  return (
    <div>
      <Camera ref={camera} />
      <button onClick={() => setImage(camera.current.takePhoto())}>Take photo</button>
      <img src={image} alt='Taken photo'/>
    </div>
  );
}

export Component;
```

### Props

| prop                    | type                             | default      | notes                                          |
| ----------------------- | -------------------------------- | ------------ | ---------------------------------------------- |
| facingMode              | `'user'\|'environment'`          | `'user'`     | default camera - 'user' or 'environment'       |
| aspectRatio             | `'cover'\|number`                | `'cover'`    | aspect ratio of video (16/9, 4/3);             |
| numberOfCamerasCallback | `(numberOfCameras: number):void` | `() => null` | callback is called if number of cameras change |
| errorMessages           | `object?` see below              | see below    | Error messages object (optional)               |

#### Error messages (prop errorMessages)

Type:

```
errorMessages: {
  noCameraAccessible?: string;
  permissionDenied?: string;
  switchCamera?: string;
  canvas?: string;
};
```

Default:

```
  {
    noCameraAccessible: 'No camera device accessible. Please connect your camera or try a different browser.',
    permissionDenied: 'Permission denied. Please refresh and give camera permission.',
    switchCamera:
    'It is not possible to switch camera to different one because there is only one video device accessible.',
    canvas: 'Canvas is not supported.'
  }
```

### Methods

- `takePhoto(): string` - Returns a base64 encoded string of the taken image.
- `switchCamera(): 'user'|'environment'` - Switches the camera - user to environment or environment to user. Returns the new value 'user' or 'environment'.
- `getNumberOfCameras(): number` - Returns number of available cameras.
- `toggleTorch(): void` - Toggles between on and off state of torch.
- `flashStatus(): Boolean` - Returns if torch is supported by the current stream or not.

<!-- [See demo](https://purple-technology.github.io/react-camera-pro/) -->

<!-- [See example code](https://github.com/purple-technology/react-camera-pro/blob/8290b1319d7436c77403784fe845060f6c4ed3bd/example/src/App.tsx#L120) -->

```javascript

const Component = () => {
  const camera = useRef(null);
  const [numberOfCameras, setNumberOfCameras] = useState(0);
  const [image, setImage] = useState(null);

  //...

  return (
    <Camera ref={camera} numberOfCamerasCallback={setNumberOfCameras} />
      <img src={image} alt='Image preview' />
      <button
        onClick={() => {
            const photo = camera.current.takePhoto();
            setImage(photo);
        }}
      />
      <button
        hidden={numberOfCameras <= 1}
        onClick={() => {
          camera.current.switchCamera();
        }}
      />

      {camera?.current?.flashStatus() ? (
        <button  onClick={()=>{
          if (camera?.current) {
            camera.current.toggleTorch();
          }}}>
          {/*Lightning Icon*/}
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width={20}
            height={20}
            viewBox="0 0 16 16"
            fill="#00000"
          >
            <path d="M11.251.068a.5.5 0 0 1 .227.58L9.677 6.5H13a.5.5 0 0 1 .364.843l-8 8.5a.5.5 0 0 1-.842-.49L6.323 9.5H3a.5.5 0 0 1-.364-.843l8-8.5a.5.5 0 0 1 .615-.09zM4.157 8.5H7a.5.5 0 0 1 .478.647L6.11 13.59l5.732-6.09H9a.5.5 0 0 1-.478-.647L9.89 2.41 4.157 8.5z" />
          </svg>
        </button>
      ) : (
        ''
      )}

  )
```

## Camera options

### User/Enviroment camera

```javascript
  const Cam = () => <Camera ref={camera} facingMode='environment'} />
```

### Aspect ratio

```javascript
const Cam = () => <Camera ref={camera} aspectRatio={16 / 9} />;
```

## Using within an iframe

```
<iframe src="https://example.com/camera-pro-iframe" allow="camera;"/>
```

## Credits

- Thanks for boilerplate to: https://medium.com/@xfor/developing-publishing-react-component-library-to-npm-styled-components-typescript-cc8274305f5a
- Thanks for Camera Template to: https://www.kasperkamperman.com/blog/camera-template/

## License

MIT

[downloads-badge]: https://img.shields.io/npm/dw/react-camera-pro-with-torch.svg?style=flat-square
[npm-badge]: https://img.shields.io/npm/v/react-camera-pro-with-torch
