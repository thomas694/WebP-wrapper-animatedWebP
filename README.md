## Note

This repository contains the original code from the [source repo](https://github.com/JosePineiro/WebP-wrapper) plus...
 * my wrapper code to handle animated WebP files,
   e.g.
   ```C#
   var frames = webp.AnimLoad(strFilepath);
   // or
   var frames = webp.AnimDecode(byteArray);
   ```

improvements by [russaa](https://github.com/russaa) ([russaa/WebP-wrapper-animatedWebP](https://github.com/russaa/WebP-wrapper-animatedWebP/tree/dc5b979a8ba5b00be7b15b0c3055ae3c5a3cc355)):
 * Refactor: avoid duplicate library files
 * support optional frame ranges (by indices) when decoding animated webp files (see `WebP.AnimDecode(..)`),
   e.g.
   ```c#
   var frames = webp.AnimDecode(byteArray, 0, 1);
   ```
   would only decode the 1st frame
 * use `libwebp` version 1.3.0

and my newest extension:
 * functionality for getting animated WebP's (compressed) frame data,
   e.g.
   ```C#
   _ = webp.AnimInit(strFilepath, out uint frameCount);
   var frameData = webp.AnimGetFrame(intFrameNumber);
   ```
   for decoding at a later time,
   e.g.
   ```C#
   var bmp = webp.Decode(frameData.Data);
   ```

<br>
These changes are intended to be integrated back into the upstream repo, waiting for the decision whether and how they should be integrated.

&nbsp;
&nbsp;

[original]

---


# WebP-wrapper
Wrapper for libwebp in C#. The most complete wrapper in pure managed C#.

Exposes Simple Decoding and Encoding API, Advanced Decoding and Encoding API (with statistics of compression), Get version library and WebPGetFeatures (info of any WebP file). Exposed get PSNR, SSIM or LSIM distortion metrics.

The wrapper is in safe managed code in one class. No need for external dll except libwebp_x86.dll(included v0.4.4) and libwebp_x64.dll (included v1.2.1). The wrapper works in 32, 64 bit or ANY (auto swith to the appropriate library).

The code is commented and includes simple examples for using the wrapper.

## Decompress Functions:
Load WebP image for WebP file
```C#
using (WebP webp = new WebP())
  Bitmap bmp = webp.Load("test.webp");
```

Decode WebP filename to bitmap and load in PictureBox container
```C#
byte[] rawWebP = File.ReadAllBytes("test.webp");
using (WebP webp = new WebP())
  this.pictureBox.Image = webp.Decode(rawWebP);
```

Advanced decode WebP filename to bitmap and load in PictureBox container
```C#
byte[] rawWebP = File.ReadAllBytes("test.webp");
WebPDecoderOptions decoderOptions = new WebPDecoderOptions();
decoderOptions.use_threads = 1;     //Use multhreading
decoderOptions.flip = 1;   			//Flip the image
using (WebP webp = new WebP())
  this.pictureBox.Image = webp.Decode(rawWebP, decoderOptions);
```

Get thumbnail with 200x150 pixels in fast/low quality mode
```C#
using (WebP webp = new WebP())
	this.pictureBox.Image = webp.GetThumbnailFast(rawWebP, 200, 150);
```

Get thumbnail with 200x150 pixels in slow/high quality mode
```C#
using (WebP webp = new WebP())
	this.pictureBox.Image = webp.GetThumbnailQuality(rawWebP, 200, 150);
```


## Compress Functions:
Save bitmap to WebP file
```C#
Bitmap bmp = new Bitmap("test.jpg");
using (WebP webp = new WebP())
  webp.Save(bmp, 80, "test.webp");
```

Encode to memory buffer in lossy mode with quality 75 and save to file
```C#
byte[] rawWebP = File.ReadAllBytes("test.jpg");
using (WebP webp = new WebP())
  rawWebP = webp.EncodeLossy(bmp, 75);
File.WriteAllBytes("test.webp", rawWebP); 
```

Encode to memory buffer in lossy mode with quality 75 and speed 9. Save to file
```C#
byte[] rawWebP = File.ReadAllBytes("test.jpg");
using (WebP webp = new WebP())
  rawWebP = webp.EncodeLossy(bmp, 75, 9);
File.WriteAllBytes("test.webp", rawWebP); 
```

Encode to memory buffer in lossy mode with quality 75, speed 9 and get information. Save to file
```C#
byte[] rawWebP = File.ReadAllBytes("test.jpg");
using (WebP webp = new WebP())
  rawWebP = webp.EncodeLossy(bmp, 75, 9, true);
File.WriteAllBytes("test.webp", rawWebP); 
```

Encode to memory buffer in lossless mode and save to file
```C#
byte[] rawWebP = File.ReadAllBytes("test.jpg");
using (WebP webp = new WebP())
  rawWebP = webp.EncodeLossless(bmp);
File.WriteAllBytes("test.webp", rawWebP); 
```

Encode to memory buffer in lossless mode with speed 9 and save to file
```C#
byte[] rawWebP = File.ReadAllBytes("test.jpg");
using (WebP webp = new WebP())
  rawWebP = webp.EncodeLossless(bmp, 9);
File.WriteAllBytes("test.webp", rawWebP); 
```

Encode to memory buffer in near lossless mode with quality 40 and speed 9 and save to file
```C#
byte[] rawWebP = File.ReadAllBytes("test.jpg");
using (WebP webp = new WebP())
  rawWebP = webp.EncodeNearLossless(bmp, 40, 9);
File.WriteAllBytes("test.webp", rawWebP); 
```

## Another Functions:	
Get version of libwebp.dll
```C#
using (WebP webp = new WebP())
  string version = "libwebp.dll v" + webp.GetVersion();
```

Get info from WebP file
```C#
byte[] rawWebp = File.ReadAllBytes(pathFileName);
using (WebP webp = new WebP())
  webp.GetInfo(rawWebp, out width, out height, out has_alpha, out has_animation, out format);
MessageBox.Show("Width: " + width + "\n" +
                "Height: " + height + "\n" +
                "Has alpha: " + has_alpha + "\n" +
                "Is animation: " + has_animation + "\n" +
                "Format: " + format);
```

Get PSNR, SSIM or LSIM distortion metric between two pictures
```C#
int metric = 0;  //0 = PSNR, 1= SSIM, 2=LSIM
Bitmap bmp1 = Bitmap.FromFile("image1.png");
Bitmap bmp2 = Bitmap.FromFile("image2.png");
using (WebP webp = new WebP())
	result = webp.GetPictureDistortion(source, reference, metric);
	                    MessageBox.Show("Red: " + result[0] + "dB.\nGreen: " + result[1] + "dB.\nBlue: " + result[2] + "dB.\nAlpha: " + result[3] + "dB.\nAll: " + result[4] + "dB.", "PSNR");

MessageBox.Show("Red: " + result[0] + dB\n" +
                "Green: " + result[1] + "dB\n" +
                "Blue: " + result[2] + "dB\n" +
                "Alpha: " + result[3] + "dB\n" +
                "All: " + result[4] + "dB\n");
```


## Thanks to jzern@google.com
Without their help this wapper would not have been possible.


[1]: https://github.com/thomas694/WebP-wrapper-animatedWebP
