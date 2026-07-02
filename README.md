# Camera Kit wrapper for React Native

> [!IMPORTANT] 
> This repository contains example projects to help you get started with Camera Kit integrations. The software is provided "as is" without any warranties or guarantees, and it is not officially supported for production use.
>
> Advanced functionalities like Remote API support, Inclusive Camera features, etc. are not supported in this wrapper implementation. If your project needs missing features, feel free to implement them yourself and submit a PR to this repo or use native development environment.

The project provides a wrapper to Snap's [Camera Kit](https://ar.snap.com/camera-kit) solution that simplifies and speeds up the integration process for developers building React Native apps. While development on native platforms is still a recommended way, this wrapper provides a convenient way to implement basic functionalities of Camera Kit in React Native application.

## Installation

You can install the Camera Kit React Native package using npm:

```sh
npm install @snap/camera-kit-react-native
```

## Usage

Start with importing the following modules:

```js
import { CameraKitContext, CameraPreviewView, useCameraKit } from '@snap/camera-kit-react-native';
```

`CameraKitContext` component will contain global configuration for CameraKit session, `CameraPreviewView` renders the camera preview, and the `useCameraKit` hook provides the API for managing the native CameraKit session: load lenses, apply a lens, take snapshots and videos, etc.

For Android, make sure you have following permissions defined in `AndroidManifest.xml` file:

```xml
<uses-permission android:name="android.permission.CAMERA" />

<!-- optionally, if you want to record audio: -->
<uses-permission android:name="android.permission.RECORD_AUDIO" />
```

### Please refer to the [example](./example) directory for detailed usage examples on how to integrate and use this wrapper in your React Native project.

***Usage example:***
```tsx
import { CameraKitContext, CameraPreviewView } from '@snap/camera-kit-react-native';
import { Lenses } from './Lenses';

export function App() {
    return (
        <CameraKitContext apiToken="<API Token from Camera Kit Portal>">
            <CameraPreviewView
                style={{ flex: 1 }}
                cameraPosition="front"
                mirrorFramesHorizontally={false}
                safeRenderArea={{ top: 100, left: 0, bottom: 200, right: 0 }}
            />
            <Lenses groupId="<Lens Group ID from Camera Kit Portal>" />
        </CameraKitContext>
    );
}
```

***Lens carousel example:***
```tsx
import { useCameraKit, type Lens } from '@snap/camera-kit-react-native';
import { useEffect, useState } from 'react';
import { View, FlatList, Pressable, Image } from 'react-native';

export function Lenses({ groupId }: { groupId: string }) {
    const { loadLensGroup, applyLens, isSessionReady } = useCameraKit();
    const [lenses, setLenses] = useState<Lens[]>([]);

    useEffect(() => {
        if (isSessionReady) {
            loadLensGroup(groupId).then(setLenses).catch(console.error);
        }
    }, [loadLensGroup, groupId, isSessionReady]);

    return (
        <View style={{ position: 'absolute' }}>
            <FlatList
                horizontal={true}
                data={lenses}
                renderItem={({ item }) => (
                    <Pressable
                        onPress={() => {
                            applyLens(item.id).catch(console.error);
                        }}>
                        <Image style={{ width: 75, height: 75 }} source={{ uri: item.icons[0]?.imageUrl }} />
                    </Pressable>
                )}
                keyExtractor={(item) => item.id}
            />
        </View>
    );
}
```

## Contributing
Thank you for your interest in improving our project!  :pray:

Here's how you can contribute:

1. Fork and clone this repository.
2. Install dependencies by running `yarn install --immutable && yarn prepare`.
3. Make your changes.
4. Test your changes with the example app using `yarn example start`. Ensure everything works as expected.
5. Update the documentation if necessary by running `yarn docs`.
6. Submit a pull request with a clear description of your changes.

## License
Please refer to the [LICENSE](/LICENSE) file for license information.
