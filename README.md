# ngx- dropzone

A lightweight and highly customizable Angular dropzone component to catch file uploads.

[![NPM](https://img.shields.io/npm/v/ngx-dropzone.svg)](https://www.npmjs.com/package/ngx-dropzone) [![Build Status](https://travis-ci.com/peterfreeman/ngx-dropzone.svg?branch=master)](https://travis-ci.com/peterfreeman/ngx-dropzone)

<img src="_images/default.png">

<img src="_images/default_hovered.png">

**IMPORTANT UPDATE:**\
With the latest version the output event was renamed to `(filesAdded)`.\
I changed the way to apply your custom styling to the component, see [Custom style component](#custom-style-component).\
You can now use the option `[showImagePreviews]` to enable a live preview of the users images.

## Install

```
$ npm install --save ngx-dropzone
```

## Usage

Import the module

```js
// in app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';
import { NgxDropzoneModule } from 'ngx-dropzone';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    NgxDropzoneModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

```html
<!-- in app.component.html -->
<ngx-dropzone></ngx-dropzone>
```

## Options

| Property |   Type  | Description | Default  |
|--------------|-------|------------------------------------------------|---------|
| `[multiple]` | `boolean` | Allow drop or selection of more than one file. | `true` |
| `[label]`    | `string`  | Change the label text.   | `'Drop your files here (or click)'` |
| `[accept]`    | `string`  | Specify the accepted file types.   | `'*'` |
| `[maxFileSize]`    | `number`  | Set the maximum file size in bytes.   | `undefined` |
| `[showImagePreviews]`    | `boolean`  | Show image previews in the dropzone.   | `false` |
| `[disabled]`    | `boolean`  | Disable any drop or click interaction.   | `false` |

### Examples

```html
<ngx-dropzone [multiple]="false"></ngx-dropzone>
```

```html
<ngx-dropzone [label]="'This is a custom label text'"></ngx-dropzone>
```

```html
<ngx-dropzone [accept]="'image/png,image/jpeg'"></ngx-dropzone>
```

```html
<ngx-dropzone [maxFileSize]="2000"></ngx-dropzone>
```

```html
<ngx-dropzone [showImagePreviews]="true"></ngx-dropzone>
```

```html
<ngx-dropzone [disabled]="true"></ngx-dropzone>
```

<img src="_images/default_disabled.png">

### File selection event

Use the `(filesAdded)` output event to catch a file selection or drop.\
It returns a `File[]` of the dropped files that match the filters like file type and maximum size.\
Use the following example code to read the file's content.

```html
<!-- in app.component.html -->
<ngx-dropzone (filesAdded)="onFilesAdded($event)"></ngx-dropzone>
```

```js
// in app.component.ts
onFilesAdded(files: File[]) {
  console.log(files);

  files.forEach(file => {
    const reader = new FileReader();

    reader.onload = (e: ProgressEvent) => {
      const content = (e.target as FileReader).result;

      // this content string could be used as an image source
      // or be uploaded to a webserver via HTTP.
      console.log(content);
    };

    // use this for basic text files like .txt or .csv
    reader.readAsText(file);

    // use this for images
    // reader.readAsDataURL(file);
  });
}
```

## Custom style component

You can provide a custom style for the dropzone container which still keeps the behaviour.\
When the user hovers over the component, the css class `hovered` is added. The `disabled` class will be added automatically.\
See the following example on how to do it and provide custom styles.

```html
<!-- in app.component.html -->
<ngx-dropzone label="This is my custom dropzone"
              class="custom-dropzone"
              (filesAdded)="onFilesAdded($event)">
</ngx-dropzone>
```

```scss
/* in app.component.scss */
ngx-dropzone {
  margin: 20px;

  &.custom-dropzone {
    height: 250px;
    background: #333;
    color: #fff;
    border: 5px dashed rgb(235, 79, 79);
    border-radius: 5px;
    font-size: 20px;

    &.hovered {
      border-style: solid;
    }
  }
}
```

<img src="_images/custom.png">

<img src="_images/custom_hovered.png">


You can still use the same properties like for the default styling.

```html
<ngx-dropzone class="custom-dropzone" [disabled]="true"></ngx-dropzone>
```

<img src="_images/custom_disabled.png">


## Licence

MIT © Peter Freeman
