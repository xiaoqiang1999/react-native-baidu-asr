# react-native-baidu-asr

<p align="center">
  <a href="https://www.npmjs.com/package/react-native-baidu-asr">
    <img src="https://img.shields.io/npm/v/react-native-baidu-asr" alt="react-native-baidu-asr">
  </a>
  <a href="https://www.npmjs.com/package/react-native-baidu-asr">
    <img src="https://img.shields.io/npm/dm/react-native-baidu-asr" alt="react-native-baidu-asr">
  </a>
  <a href="https://github.com/gdoudeng/react-native-baidu-asr/issues">
    <img src="https://img.shields.io/github/issues/gdoudeng/react-native-baidu-asr" alt="issues">
  </a>
  <a href="https://opensource.org/licenses/MIT">
    <img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="License: MIT">
  </a>
  <a href="#badge">
    <img alt="semantic-release" src="https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg">
  </a>
</p>

`react-native-baidu-asr` is a Baidu speech library under React Native, which can perform speech recognition.

English | [简体中文](./README-zh.md)


## Preview

<p align="left">
  <img width=360 title="Preview" src="./sreenshot/ezgif.gif" alt="Preview">
</p>

## Support
- React Native >= 0.47.0
- Android

Currently, the iOS platform is not implemented. I will fill it up when I have time, as well as voice synthesis and voice wake-up.

## Install

- RN >= 0.60

1. `yarn add react-native-baidu-asr`

- RN < 0.60

1. `yarn add react-native-baidu-asr`

2. `react-native link react-native-baidu-asr`

## Usage

- See for details [example](https://github.com/gdoudeng/react-native-baidu-asr/tree/master/example)

The first is that you have to go to the [Baidu Voice Console]((https://console.bce.baidu.com/ai/?_=1620713753811&fromai=1#/ai/speech/overview/index)) to create an application, get authentication information: AppID, API Key, Secret Key.

```typescript
import BaiduAsr, { RecognizerStatusCode, RecognizerData, RecognizerResultError, RecognizerResultData, VolumeData } from 'react-native-baidu-asr';

// Initialize Baidu speech engine
BaiduAsr.init({
  APP_ID: 'Your authentication information AppID',
  APP_KEY: 'Your authentication information API Key',
  SECRET: 'Your authentication information Secret Key',
});

// Processing recognition results
this.resultListener = BaiduAsr.addResultListener(this.onRecognizerResult);
// Handling wrong results
this.errorListener = BaiduAsr.addErrorListener(this.onRecognizerError);
// Processing volume
this.volumeListener = BaiduAsr.addAsrVolumeListener(this.onAsrVolume);

// Start speech recognition
// For more input parameters, please refer to Baidu Voice Document
// https://ai.baidu.com/ai-doc/SPEECH/bkh07sd0m#asr_start-%E8%BE%93%E5%85%A5%E4%BA%8B%E4%BB%B6%E5%8F%82%E6%95%B0
BaiduAsr.start({
  // Long speech
  VAD_ENDPOINT_TIMEOUT: 0,
  BDS_ASR_ENABLE_LONG_SPEECH: true,
  // Disable punctuation
  DISABLE_PUNCTUATION: true,
});
```

## API

### Speech Recognition

#### Methods

- `BaiduAsr.init(options: InitOptions)`

Initialize Baidu speech engine

- `BaiduAsr.start(options: AsrOptions)`

Start speech recognition

- `BaiduAsr.stop()`

Pause the recording, the SDK will no longer recognize the stopped recording.

- `BaiduAsr.cancel()`

Cancel the recording, the SDK will cancel this recognition and return to the original state.

- `BaiduAsr.release()`

Release the resource. If you need to use it again next time, you must call the `init` method to initialize the engine.

#### Events

The recognition result callback data has a unified format, similar to the return of the api interface，has code，msg，data。

`RecognizerData` The data types are as follows：
```typescript
interface RecognizerData<T = any> {
  /**
   * status code
   */
  code: RecognizerStatusCode,
  /**
   * message
   */
  msg: string,
  /**
   * data
   */
  data: T
}
```

- `addResultListener(callback: (data: RecognizerData<RecognizerResultData | undefined>) => void): EmitterSubscription`  
  Voice recognition result callback, the event will be triggered continuously during voice recognition，`data` is of type `RecognizerData<RecognizerResultData | undefined>`，Its value：

    - `code`：status code
    - `msg`：message
    - `data`：Identification data

The data types of `data` are as follows:

```typescript
interface RecognizerResultData {
  best_result: string,
  // If there is no accident, the first value is the recognition result
  results_recognition: Array<string>,
  result_type: ResultType,
  origin_result: {
    corpus_no: number,
    err_no: number,
    raf: number,
    result: {
      word: Array<string>
    },
    sn: string
  },
  error: number,
  desc: string
}
```

- `addErrorListener(callback: (data: RecognizerData<RecognizerResultError>) => void): EmitterSubscription`  
  There is an error in speech recognition. The error message is consistent with the Baidu speech document. Its value:

    - `code`：status code
    - `msg`：message
    - `data`：Wrong data

The data types of `data` are as follows:

```typescript
interface RecognizerResultError {
  errorCode: number // Error code comparison Baidu voice document https://ai.baidu.com/ai-doc/SPEECH/qk38lxh1q
  subErrorCode: number
  descMessage: string
}
```

- `addAsrVolumeListener(listener: (volume: VolumeData) => void): EmitterSubscription`  
  The volume of speech recognition. This event will be triggered when the recognized speech changes the volume. `volume` is of type `VolumeData`, and its value is:

    - `volumePercent`: Current volume percentage
    - `volume`: Current volume

## Contribute

Looking forward to making relevant suggestions, contributions are welcome, thank you star.

[Github](https://github.com/gdoudeng/react-native-baidu-asr)

## License

[MIT License](https://github.com/gdoudeng/react-native-baidu-asr/blob/master/LICENSE)
