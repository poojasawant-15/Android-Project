# PhotoEditor
 
 
 A Photo Editor library with simple, easy support for image editing using Paints, Text, Filters, Emoji and Sticker like stories.
 
 [Download link](https://drive.google.com/drive/folders/1pw_iZ_PIyOSJzCWR_uLnoe7PKCDTgosp?usp=sharing)
 
 ![](https://i.imgur.com/ZYtLHTZ.png)
 
 
 
 ## Features
 
 - [**Drawing**](#drawing) on image with option to change its Brush's Color, Size, Opacity, Erasing and basic shapes.
 - Apply [**Filter Effect**](#filter-effect) on image using MediaEffect
 - Adding/Editing [**Text**](#text) with option to change its Color with Custom Fonts.
 - Adding [**Emoji**](#emoji) with Custom Emoji Fonts.
 - Adding [**Images/Stickers**](#adding-imagesstickers)
 - Pinch to Scale and Rotate views.
 - [**Undo and Redo**](#undo-and-redo) for Brush and Views.
 - [**Deleting**](#deleting) Views
 - [**Saving**](#saving) Photo after editing.
 - More [**FAQ**](#faq).
 - [Lesson Learned from building successful android library PhotoEditor: Droidcon Berlin 2021](#lesson-learned-from-building-successful-android-library-photoeditor-droidcon-berlin-2021)
 
 
 
 ## Benefits
 - Hassle free coding
 - Increase efficiency
 - Easy image editing
 
 ## Getting Started
 To start with this, we need to simply add the dependencies from `mavenCentral()` in the gradle file of our app module like this
 ```groovy
 implementation 'com.burhanrashid52:photoeditor:3.1.0'
 implementation 'com.burhanrashid52:photoeditor:3.0.2'
 ```
 or we can also import the :photoeditor module from sample for further customization
 
 ## Migrations
 ### AndroidX
 PhotoEditor  is a migration to androidX and dropping the support of older support library. There are no API changes. If you find any issue migrating to v.1.0.0 , please follow this [Guide]. If you still facing the issue than you can always rollback to . Any fix in PR are Welcome :)
 
 ### Kotlin
 PhotoEditor is fully migrated to Kotlin. You can use for the Java version. There are no breaking API changes in these two versions.
 
 ## Setting up the View
 First we need to add `PhotoEditorView` in our xml layout
 
 ```xml
  <ja.burhanrashid52.photoeditor.PhotoEditorView
         android:id="@+id/photoEditorView"
         android:layout_width="match_parent"
         android:layout_height="match_parent"
         app:photo_src="@drawable/got_s" />
 
 ```
 We can define our drawable or color resource directly using `app:photo_src`
 
 We can set the image programmatically by getting source from `PhotoEditorView` which will return a `ImageView` so that we can load image from resources,file or (Picasso/Glide)
 ```kotlin
 mPhotoEditorView = findViewById(R.id.photoEditorView)
 
 mPhotoEditorView.source.setImageResource(R.drawable.paris_tower)
 ```
 
 ## Building a PhotoEditor
 To use the image editing feature we need to build a PhotoEditor which requires a Context and PhotoEditorView which we have to setup in our xml layout
 
 
 ```Kotlin
 //Use custom font using latest support library
 val mTextRobotoTf = ResourcesCompat.getFont(this, R.font.roboto_medium)
 //loading font from asset
 val mEmojiTypeFace = Typeface.createFromAsset(getAssets(), "emojione-android.ttf")
 
 mPhotoEditor = PhotoEditor.Builder(this, mPhotoEditorView)
             .setPinchTextScalable(pinchTextScalable) // set flag to make text scalable when pinch
             .setDefaultTextTypeface(mTextRobotoTf)
             .setDefaultEmojiTypeface(mEmojiTypeFace)
             .build() // build photo editor sdk
  ```
 We can customize the properties in the PhotoEditor as per our requirement
 
 | Property  | Usage |
 | ------------- | ------------- |
 | `setPinchTextScalable()`  | set false to disable pinch to zoom on text insertion. Default: true. |
 | `setClipSourceImage()` | set true to clip the drawing brush to the source image. Default: false. |
 | `setDefaultTextTypeface()`  | set default text font to be added on image  |
 | `setDefaultEmojiTypeface()`  | set default font specifc to add emojis |
 
 That's it we are done with setting up our library
 
 
 
 ## Drawing
 We can customize our brush and paint with different set of property. To start drawing on image we need to enable the drawing mode
 
 ![](https://i.imgur.com/INi5LIy.gif)
 
 | Type                                         | Method                                                                 |
 |----------------------------------------------|------------------------------------------------------------------------|
 | Enable/Disable                               | `mPhotoEditor.setBrushDrawingMode(true);`                              |
 | Shape (brush, line, oval, rectangle, arrow)  | `mPhotoEditor.addShape(shape)`                                         |
 | Shape size (px)                              | `mPhotoEditor.setBrushSize(brushSize)` or through the a ShapeBuilder   |
 | Shape opacity (In %)                         | `mPhotoEditor.setOpacity(opacity)` or through the a ShapeBuilder       |
 | Shape color                                  | `mPhotoEditor.setBrushColor(colorCode)` or through the a ShapeBuilder  |
 | Brush Eraser                                 | `mPhotoEditor.brushEraser()`                                           |
 
 **Note**: Whenever we set any property of a brush for drawing it will automatically enable the drawing mode
 
 ## Shapes
 We can draw shapes . We use `ShapeBuilder` to define shape and other properties.
 
 ![](https://im2.ezgif.com/tmp/ezgif-2-5d5f7ddbe72e.gif)
 
 ```kotlin
 val shapeBuilder = ShapeBuilder()
     .withShapeOpacity(100)
     .withShapeType(ShapeType.Oval)
     .withShapeSize(50f)
 
 photoEditor.setShape(mShapeBuilder)
 ```
 
 
 ## Filter Effect
 We can apply inbuild filter to the source images using 
 
  `mPhotoEditor.setFilterEffect(PhotoFilter.BRIGHTNESS)`
 
 ![](https://i.imgur.com/xXTGcVC.gif)
 
 We can also apply custom effect using `Custom.Builder`
 
 
 
 
 
 ## Text
 
 ![](https://i.imgur.com/491BmE8.gif)
 
 We can add the text with inputText and colorCode like this
 
 `mPhotoEditor.addText(inputText, colorCode);` 
 
 It will take default fonts provided in the builder. If we want different fonts for different text we can set typeface with each text like this
 
 `mPhotoEditor.addText(mTypeface,inputText, colorCode);`
 
 In order to edit the text we need the view, which we will receive in our PhotoEditor callback. This callback will trigger when we **Long Press** the added text
 
  ```java
  mPhotoEditor.setOnPhotoEditorListener(new OnPhotoEditorListener() {
             @Override
             public void onEditTextChangeListener(View rootView, String text, int colorCode) {
 
             }
         });
   ```
 Now we can edit the text with a view like this
 
 `mPhotoEditor.editText(rootView, inputText, colorCode);`
 
 If you want more customization on text. Please refer the wiki page for more details.
 
 
 ## Emoji
 
 ![](https://i.imgur.com/RP8kqz6.gif)
 
 We can add the Emoji by `PhotoEditor.getEmojis(getActivity());` which will return a list of emojis unicode.
 
 `mPhotoEditor.addEmoji(emojiUnicode);`
 
 It will take default fonts provided in the builder. If we want different Emoji fonts for different emoji we can set typeface with each Emoji like this
 
 `mPhotoEditor.addEmoji(mEmojiTypeface,emojiUnicode);`
 
 
 
 
 ## Adding Images/Stickers
  We need to provide a Bitmap to add our Images  `mPhotoEditor.addImage(bitmap);`
 
 
 
 
 ## Undo and Redo
 
 ![](https://i.imgur.com/1Y9WcCB.gif)
 
  ```java
    mPhotoEditor.undo();
    mPhotoEditor.redo();
  ```
 
 
 
 ## Deleting
   For deleting a Text/Emoji/Image we can click on the view to toggle the view highlighter box which will have a close icon. So, by clicking on the icon we can delete the view.
 
 ## Saving
 
 In onward, we can save an image to a file using coroutines:
 
 ```kotlin
 // Please note that if you call this from a fragment, you should call
 // 'viewLifecycleOwner.lifecycleScope.launch' instead.
 lifecycleScope.launch {
     val result = photoEditor.saveAsFile(filePath)
     if (result is SaveFileResult.Success) {
         showSnackbar("Image saved!")
     } else {
         showSnackbar("Couldn't save image")
     }
 }
 ```
 
 You can also save an image to a file from Java. We need to provide a file with callback method when edited image is saved.
 
    ```java
     mPhotoEditor.saveAsFile(filePath, new PhotoEditor.OnSaveListener() {
                     @Override
                     public void onSuccess(@NonNull String imagePath) {
                        Log.e("PhotoEditor","Image Saved Successfully");
                     }
 
                     @Override
                     public void onFailure(@NonNull Exception exception) {
                         Log.e("PhotoEditor","Failed to save Image");
                     }
                 });
 ```
 
 
 
