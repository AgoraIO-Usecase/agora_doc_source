This page provides the TypeScript API reference of the Agora Classroom SDK for Web/Electron.

## AgoraEduSDK

`AgoraEduSDK` is the basic interface of the Agora Classroom SDK and provides the main methods that can be invoked by your app.

### config

```typescript
static config(params: ConfigParams):void
```

Configure the SDK.

**Sample code**

```typescript
AgoraEduSDK.config({
  // Agora App ID
  appId: "<YOUR AGORA APPID>",
  // Region
  region: "NA"
})
```

**Parameter**

| Parameter | Description |
| :------- | :----------------------------------------------------------- |
| `params` | The SDK global configuration. See [`ConfigParams`](#configparams). |

### launch

```typescript
static launch(dom: Element, option: LaunchOption):Promise<void>
```

Launch a classroom.

**Sample code**

```typescript
// Configure courseware
let resourceUuid = "xxxxx"
let resourceName = "my ppt slide"
let sceneInfos = []
let sceneInfo = {
    name: "1",
    ppt: {
        src: "pptx://....",
        width: 480,
        height: 360
    }
}
sceneInfos.push(sceneInfo)

let courseWareList = [{
    resourceUuid,
    resourceName,
    size: 10000,
    updateTime: new Date().getTime(),
    ext: "pptx",
    url:null,
    scenes: sceneInfos,
    taskUuid: "xxxx",
    taskToken: "xxx",
    taskProgress: NetlessTaskProgress
}]

// Launch a classroom
AgoraEduSDK.launch(document.querySelector(`#${this.elem.id}`), {
    rtmToken: "<your rtm token>",
    userUuid: "test",
    userName: "teacher",
    roomUuid: "4321",
    roleType: 1,
    roomType: 4,
    roomName: "demo-class",
    pretest: false,
    language: "en",
    startTime: new Date().getTime(),
    duration: 60 * 30,
    courseWareList: [],
    listener: (evt) => {
        console.log("evt", evt)
    }
})
```

**Parameter**

| Parameter | Description |
| :------- | :----------------------------------------------------------- |
| `dom` | See [Document](https://developer.mozilla.org/en-US/docs/Web/API/Document) for details. |
| `option` | The classroom launching configuration. See [`LaunchOption`](#launchoption). |

## Type definition

### ConfigParams

The SDK global configuration. Used when calling [`AgoraEduSDK.config`](#config).

```typescript
export type ConfigParams = {
    appId: string;
    region?: string;
};
```

| Attributes | Description |
| :------- | :----------------------------------------------------------- |
| `appId` | (Required) The Agora App ID. See [Get the Agora App ID](https://docs.agora.io/cn/agora-class/agora_class_prep#step1). |
| `region` | (Optional) The region where the classrooms is located. Agora recommends you set a region close to the region of the object storage service for your courseware or recording files, because cross-region transmission of large static resources can lead to delay. For example, if your S3 service is in North America, you should set this parameter to `NA`. All Smart Classroom clients must set the same area, otherwise they cannot communicate with each other. All clients must use the same region, otherwise, they may fail to communicate with each other. Flexible Classroom supports the following regions:<li>`CN`: Mainland China</li><li>`AP`: Asia Pacific</li><li>`EU`: Europe</li><li>`NA`: North America</li> |

### LaunchOption

The classroom launching configuration. Used when calling [`AgoraEduSDK.launch`](#launch).

```typescript
export type LaunchOption = {
    userUuid: string;
    userName: string;
    roomUuid: string;
    roleType: EduRoleTypeEnum;
    roomType: EduRoomTypeEnum;
    roomName: string;
    listener: ListenerCallback;
    pretest: boolean;
    rtmToken: string;
    language: LanguageEnum;
    startTime?: number;
    duration: number;
    courseWareList: CourseWareList;
    personalCourseWareList?: CourseWareList;
    recordUrl?: string;
    extApps?: IAgoraExtApp[];
    region?: AgoraRegion;
    widgets?: {[key: string]: IAgoraWidget};
    userFlexProperties?: {[key: string]: any};
    mediaOptions?: MediaOptions;
    latencyLevel?: 1 | 2;
    platform?: Platform;
};
```

| Parameter | Description |
| :----------------------- | :----------------------------------------------------------- |
| `rtmToken` | (Required) The RTM token used for authentication. For details, see [Generate an RTM Token](https://docs.agora.io/en/agora-class/agora_class_prep#step5). |
| `userUuid` | (Required) The user ID. This is the globally unique identifier of a user. **Must be the same as the User ID that you use for generating an RTM token**. The string length must be less than 64 bytes. Supported character scopes are:<li>All lowercase English letters: a to z.<li>All uppercase English letters: A to Z.<li>All numeric characters.<li>0-9<li>The space character.<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "\|", "~", "," |
| `userName` | (Required) The user name for display in the classroom. The string length must be less than 64 bytes. |
| `roomUuid` | (Required) The room ID. This is the globally unique identifier of a classroom. The string length must be less than 64 bytes. Supported character scopes are:<li>All lowercase English letters: a to z.<li>All uppercase English letters: A to Z.<li>All numeric characters.<li>0-9<li>The space character.<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "\|", "~", "," |
| `roomName` | (Required) The room name for display in the classroom. The string length must be less than 64 bytes. |
| `roleType` | (Required) The role of the user in the classroom. See [`EduRoleTypeEnum`](#eduroletypeenum). |
| `roomType` | (Required) The classroom type. See [`EduRoomTypeEnum`](#eduroomtypeenum). |
| `listener` | (Required) The state of classroom launching.<li>`ready`: The classroom is ready.</li><li>`destroyed`: The classroom has been destroyed.</li> |
| `pretest` | (Required) Whether to enable the pre-class device test:<li>`true`: Enable the pre-class device test. After this function is enabled, end users can see a page for the device test before entering the classroom. They can check whether their camera, microphone, and speaker can work properly.</li><li>`false`: Disable the pre-class device test.</li> |
| `language` | (Required) The UI language. See [`LanguageEnum`](#languageenum). |
| `startTime` | (Required) The start time (ms) of the class, determined by the first user joining the classroom. |
| `duration` | (Required) The duration (ms) of the class, determined by the first user joining the classroom. |
| `recordUrl` | (Optional) The URL address to be recorded. Developers need to pass in the URL of the web page deployed by themselves for page recording, such as `https://cn.bing.com/recordUrl`. |
| `courseWareList` | (Optional) The configuration of courseware assigned by the educational institution, which cannot be edited by the client. See [`CourseWareList`](#coursewarelist) for details. After passing this object, the SDK downloads the courseware from the Agora cloud storage component to the local when launching the classroom. |
| `extApps` | (Optional) Register an extension application by using the ExtApp tool. ExtApp is a tool for embedding extension applications in Flexible Classroom. For details, see [Customize Flexible Classroom with ExtApp](./agora_class_ext_app_android?platform=Web). |
| `userFlexProperties` | (Optional) User properties customized by the developer. For details, see [How can I set user properties? ](/en/agora-class/faq/agora_class_custom_properties) |
| `mediaOptions` | (Optional) Media stream configurations, including the encryption configuration and the encoding configurations of the screen-sharing stream and the video stream captured by the camera. See `MediaOptions` for details. |
| `latencyLevel` | (Optional) The latency level of an audience member in interactive live streaming:<li>`1`: Low latency. The latency from the sender to the receiver is 1500 ms to 2000 ms.</li><li>(Default) Ultra-low latency. The latency from the sender to the receiver is 400 ms to 800 ms.</li> |

### MediaOptions

```typescript
export type MediaOptions = {
  cameraEncoderConfiguration?: EduVideoEncoderConfiguration;
  screenShareEncoderConfiguration?: EduVideoEncoderConfiguration;
  encryptionConfig?: MediaEncryptionConfig;
};
```

Media options.

| Parameter | Description |
| :-------------------------------- | :----------------------------------------------------------- |
| `cameraEncoderConfiguration` | The encoding configuration of the video stream captured by the camera. See [EduVideoEncoderConfiguration](#eduvideoencoderconfiguration). |
| `screenShareEncoderConfiguration` | The encoding configuration of the screen-sharing stream. See [EduVideoEncoderConfiguration](#eduvideoencoderconfiguration). |
| `encryptionConfig` | The media stream encryption configuration. See [MediaEncryptionConfig](#mediaencryptionconfig). |

### EduVideoEncoderConfiguration

```typescript
export interface EduVideoEncoderConfiguration {
  width: number;
  height: number;
  frameRate: number;
  bitrate: number;
}
```

Video encoder configurations.

| Parameter | Description |
| :---------- | :------------------- |
| `width` | Width (pixel) of the video frame. |
| `height` | Height (pixel) of the video frame. |
| `frameRate` | The frame rate (fps) of the video. |
| `bitrate` | The bitrate (Kbps) of the video. |

### MediaEncryptionConfig

```typescript
export declare interface MediaEncryptionConfig {
  mode: MediaEncryptionMode,
  key: string
}
```

The media stream encryption configuration. Used in [MediaOptions](#mediaoptions).

| Parameter | Description |
| :----- | :----------------------------------------------------------- |
| `mode` | Encryption mode. See [MediaEncryptionMode](#mediaencryptionmode). All users in the same classroom must use the same encryption mode and encryption key. |
| `key` | The encryption key. |

### MediaEncryptionMode

```swift
export enum MediaEncryptionMode {
  AES_128_XTS = 1,
  AES_128_ECB = 2,
  AES_256_XTS = 3,
  AES_128_GCM = 5,
  AES_256_GCM = 6
}
```

Encryption modes. Used in [MediaEncryptionConfig](#mediaencryptionconfig).

| Parameter | Description |
| :------------ | :-------------------------- |
| `AES_128_XTS` | 128-bit AES encryption, XTS mode. |
| `AES_128_ECB` | 128-bit AES encryption, ECB mode. |
| `AES_256_XTS` | 256-bit AES encryption, XTS mode. |
| `AES_128_GCM` | 128-bit AES encryption, GCM mode. |
| `AES_256_GCM` | 256-bit AES encryption, GCM mode. |

### CourseWareList

The courseware pre-download configuration. Used when calling [`AgoraEduSDK.launch`](#launch).

```typescript
export type CloudDriveResourceConvertProgress = {
    totalPageSize: number;
    convertedPageSize: number;
    convertedPercentage: number;
    convertedFileList: {
        name: string;
        ppt: {
            width: number;
            height: number;
            preview?: string;
            src: string;
        };
    }[];
    currentStep: string;
};

export type CourseWareItem = {
    resourceName: string;
    resourceUuid: string;
    ext: string;
    url?: string;
    size: number;
    updateTime: number;
    taskUuid: string;
    conversion: {
        type: string;
        preview: boolean;
        scale: number;
        outputFormat: string;
    };
    taskProgress?: CloudDriveResourceConvertProgress;
};

export type CourseWareList = CourseWareItem[];
```

<details>
<summary><font color="#3ab7f8">Sample code A: without the whiteboard conversion file</font></summary>
<pre class="json show"><code class="language-json">courseWareList:
[
    {
            resourceName: "mechanicalEnergy",
            resourceUuid: "c06fed32d06268431601b0e0a804e70a",
            ext: "mp4",
            url: "https://gymoo-project-cdn.oss-cn-shenzhen.aliyuncs.com/hld_education/upload/9f4d3c149e6b3acfef378aca012780b3.mp4",
            size: 4560284
    }
],
</code></pre>
</details>

<details>
<summary><font color="#3ab7f8">Sample code B: with the whiteboard conversion file</font></summary>
<pre class="json show"><code class="language-json">courseWareList:
[
    {
      resourceName: xxxxxxx,
      resourceUuid: xxxxxxxxx,
      ext: 'pptx',
      url: 'https://xxxxxxxxxxxxxx',
      size: 0,
      updateTime: xxxxxxxx
      taskUuid: 'xxxxxxxxx',
      conversion: {
            type: 'dynamic',
            preview: true,
            scale: 2,
            outputFormat: 'png',
            },
      taskProgress: {
        totalPageSize: 3,
        convertedPageSize: 3,
        convertedPercentage: 100,
        convertedFileList: [
            {
                name: '1',
                ppt: {
                    src: 'pptx://convertcdn.netless.link/dynamicConvert/3bxxxxxxx/1.slide',
                    width: 1280,
                    height: 720,
                    preview:'dddddddddddddddurl'
                },
            },
            ...
        ] as any,
        currentStep: '',
        },
    },
],
</code></pre>
</details>

| Parameter | Description |
| :------------- | :----------------------------------------------------------- |
| `resourceName` | The file name for display in the classroom. The string length must be less than 64 bytes. |
| `resourceUuid` | The file ID. This is the unique identifier of a file. The string length must be less than 64 bytes. Supported character scopes are:<li>All lowercase English letters: a to z.<li>All uppercase English letters: A to Z.<li>All numeric characters.<li>0-9<li>The space character.<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "\|", "~", "," |
| `ext` | The file suffix. |
| `size` | The file size (bytes). |
| `updateTime` | The latest modified time of the file. |
| `taskUuid` | The unique identifier of the file conversion task. |
| `conversion` | The file conversion configuration object, which contains the following fields:<ul><li>`static`: Convert the file in format of PPT, PPTX, DOC, DOCX, or PDF to a static picture in format of PNG, JPG, JPEG, or WEBP. The converted file doesn't keep the animation effects of the original file.</li><li>`dynamic`: Convert the file in format of PPTX edited with Microsoft Office to an HTML page. The converted file keeps the animation effects of the original file.</li><li>`preview`: A Boolean value that indicates whether you need preview on the left.</li><li>`scale`: A Number value indicates the conversion scale in the range of [0, 3].</li><li>`outputFormat`: A String value indicates the export format of the images after file conversion. You can set it as `png`.</li><ul></ul></ul> |
| `url` | The address of the file. Flexible Classroom clients automatically convert files with the suffixes of `"ppt"`, `"pptx"`, `"doc"`, `"docx"`, and `"pdf"` to formats that can be displayed on the whiteboard in classrooms. If the suffix name is not listed above, you must set `url `. |
| `taskProgress` | The JSON object `CloudDriveResourceConvertProgress` that indicates the progress of the file conversion task, which contains the following fields:<ul><li>`totalPageSize`: Total page size.</li><li>`convertedPageSize`: number of converted pages.</li><li>`convertedPercentage`: Conversion progress in percentage.</li><li>`convertedFileList`: List of converted file pages. Each file page represents a record that contains the following fields:<ul><li>`name`: Name of the file page.</li><li>`ppt`: Details of the slide included in the file page, which contains the following fields:<ul><li>`width`: width of the slide.</li><li>`height`: height of the slide.</li><li>`src`: Download URL of the converted page.</li><li>`preview`: URL of the preview image.</li></ul></li></ul></li><li>`currentStep`: Current step of the conversion task. The possible values are: `extracting` (extracting the resources), `generatingPreview` (generating the preview image, `mediaTranscode` (transcoding the media file), and `packaging`(packaging the file).</li></ul> |

### EduRoleTypeEnum

```typescript
export enum EduRoleTypeEnum {
  audience = 0,
  teacher = 1,
  student = 2,
  assistant = 3
}
```

The role of the user in the classroom. Set in [`LaunchOption`](#launchoption).

| Parameter | Description |
| :---------- | :------------------------ |
| `audience` | `0`: Audience, only used for web page recording. |
| `teacher` | `1`: Teacher. |
| `student` | `2`: A student. |
| `assistant` | `3`: Teaching assistant. |

### EduRoomTypeEnum

```typescript
export enum EduRoomTypeEnum {
  Room1v1Class = 0,
  RoomBigClass = 2,
  RoomSmallClass = 4
}
```

The classroom type. Set in [`LaunchOption`](#launchoption).

| Parameter | Description |
| :--------------- | :----------------------------------------------------------- |
| `Room1v1Class` | `0`: One-to-one Classroom. An online teacher gives an exclusive lesson to only one student. |
| `RoomBigClass` | `2`: Lecture Hall. A teacher gives an online lesson to multiple students. Students do not send their audio and video by default. There is no upper limit on the number of students. During the class, students can "raise their hands" to apply for speaking up. Once the teacher approves, the student can send their audio and video to interact with the teacher. |
| `RoomSmallClass` | `4`: Small Classroom. A teacher gives an online lesson to multiple students. Students do not send their audio and video by default. The maximum number of users in a classroom is 500. During the class, the teacher can invite students to speak up "on stage" and have real-time audio and video interactions with the teacher. |

### LanguageEnum

```typescript
export type LanguageEnum = "en" | "zh"
```

The language of the user interface. Set in [`LaunchOption`](#launchoption).

| Parameter | Description |
| :----- | :----- |
| `"en"` | English. |
| `"zh"` | Chinese. |