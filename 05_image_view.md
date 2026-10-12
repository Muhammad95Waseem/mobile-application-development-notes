# 05. ImageView

## Definition
`ImageView` is a UI widget (a `View` subclass) used to **display images** such as drawables, bitmaps, icons and photos in an app.

Package: `android.widget.ImageView`

## Basic XML Example
```xml
<ImageView
    android:id="@+id/imageView"
    android:layout_width="200dp"
    android:layout_height="200dp"
    android:src="@drawable/my_image"
    android:scaleType="centerCrop"
    android:contentDescription="Profile picture" />
```
Images are placed in the `res/drawable` folders.

## Important Attributes
| Attribute | Description |
|---|---|
| `android:src` | The image (drawable) to display |
| `android:background` | Background colour/drawable behind the image |
| `android:scaleType` | How the image is scaled/positioned inside the view |
| `android:contentDescription` | Text read by screen readers (accessibility) |
| `android:adjustViewBounds` | Adjusts the view bounds to keep the image's aspect ratio |
| `android:tint` | Applies a colour tint to the image |
| `android:alpha` | Transparency (0.0 to 1.0) |
| `android:maxWidth` / `maxHeight` | Upper size limits |
| `android:padding` | Space around the image |

## scaleType Values
| Value | Behaviour |
|---|---|
| `fitXY` | Stretches to fill the view; ignores aspect ratio |
| `fitCenter` | Scales to fit inside, keeps aspect ratio, centered (default) |
| `fitStart` / `fitEnd` | Like fitCenter but aligned to start/end |
| `center` | Centers without scaling |
| `centerCrop` | Scales to fill the view, keeps aspect ratio, crops the extra |
| `centerInside` | Scales down only if larger than the view; keeps aspect ratio |
| `matrix` | Uses a custom Matrix for scaling |

## Using ImageView in Java
```java
ImageView imageView = findViewById(R.id.imageView);

// from resources
imageView.setImageResource(R.drawable.my_image);

// from a Bitmap
imageView.setImageBitmap(bitmap);

// from a Drawable
imageView.setImageDrawable(drawable);

// from a Uri
imageView.setImageURI(uri);
```

## Loading Images from the Internet
`ImageView` itself cannot load a URL. Libraries such as **Glide**, **Picasso** or **Coil** do it (and also handle caching and resizing):
```java
Glide.with(context).load("https://example.com/photo.jpg").into(imageView);
```
This needs the `INTERNET` permission in the manifest.

## Drawable Types That Can Be Shown
- Bitmap files (`.png`, `.jpg`, `.webp`, `.gif`)
- Vector drawables (`.xml`) - scalable, small size
- Shape drawables and layer lists

## Click Handling
```java
imageView.setOnClickListener(v ->
    Toast.makeText(this, "Image clicked", Toast.LENGTH_SHORT).show());
```

## Tips
- Always set `contentDescription` for accessibility (or `null` for decorative images).
- Use proper image densities (`drawable-hdpi`, `-xhdpi`, ...) or vector drawables to avoid blurry or huge images.
- Large images consume lots of memory; use an image-loading library.
- `ImageButton` is a subclass of ImageView that behaves like a button.
