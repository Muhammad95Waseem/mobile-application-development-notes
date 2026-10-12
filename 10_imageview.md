# 10. ImageView

## Definition

`ImageView` is a view that displays an image (drawable, bitmap, vector).

## Key attributes

| Attribute | Purpose |
|---|---|
| `android:src` | The image to show (`@drawable/photo`) |
| `android:scaleType` | How the image is scaled/positioned inside the view |
| `android:contentDescription` | Text for screen readers (accessibility) |
| `android:layout_width` / `layout_height` | Size of the **view** (the image is then scaled by `scaleType`) |
| `android:adjustViewBounds` | `true` lets the view's bounds follow the image's aspect ratio (use with `wrap_content` and a `maxWidth`/`maxHeight`) |

## scaleType values

| Value | Behaviour |
|---|---|
| `fitCenter` (**default**) | Scales the whole image to **fit inside** the view, keeping the aspect ratio, centred; may leave empty bars |
| `centerCrop` | Scales to **fill** the view, keeping the aspect ratio; **crops** the overflow |
| `centerInside` | Like fit, but **never enlarges** a small image |
| `center` | No scaling; image centred (cropped if bigger) |
| `fitXY` | Stretches to exactly fill the view; **distorts** the aspect ratio |
| `fitStart` / `fitEnd` | Like `fitCenter`, aligned to the start / end |
| `matrix` | Uses a custom matrix (rare) |

Reference: [ImageView.ScaleType](https://developer.android.com/reference/android/widget/ImageView.ScaleType).

## XML example

```xml
<ImageView
    android:id="@+id/imgProfile"
    android:layout_width="200dp"
    android:layout_height="200dp"
    android:src="@drawable/profile"
    android:scaleType="centerCrop"
    android:contentDescription="Profile photo"/>
```

In real projects, use a string resource: `@string/profile_photo`.

## Changing the image in Java

```java
ImageView img = findViewById(R.id.imgProfile);
img.setImageResource(R.drawable.new_photo);      // from a drawable resource
// img.setImageBitmap(bitmap);  img.setImageURI(uri);  img.setImageDrawable(drawable);
```

**XML vs Java:** put the initial/static image and layout settings in XML; use Java when the image changes with a user action or loaded data (a button click, a picked or captured photo). Loading from the internet needs an image-loading library (Glide/Picasso) or a background download; it is not done with `android:src`.

## Common mistakes (why it looks wrong even when the dimensions are right)

1. **Cropped image:** `scaleType="centerCrop"` (or `center` with a big image) deliberately cuts the edges. Use `fitCenter` if the whole image must be visible.
2. **Stretched/squashed image:** `fitXY` in a view whose aspect ratio differs from the image.
3. **Empty bars:** `fitCenter` inside a view whose shape doesn't match the image.
4. **Image not shrinking:** the view is `wrap_content` and the image is huge; set a fixed size, or use `adjustViewBounds` with `maxHeight`.
5. **Missing `contentDescription`** gives an accessibility warning. Decorative images can be marked as not important for accessibility.
6. `android:src` vs `android:background`: `background` stretches to fill the whole view; `src` obeys `scaleType`.

## Quick summary

- `src` = image, `scaleType` = how it fits (default `fitCenter`), `contentDescription` = accessibility.
- Crop = `centerCrop`; whole image = `fitCenter`; distort = `fitXY`.
- Change in code: `setImageResource(R.drawable.x)`.
