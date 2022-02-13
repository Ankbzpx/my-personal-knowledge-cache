---
id: jCj29hk2AV4O0At74rn9W
title: Exif Metadata Edit
desc: ''
updated: 1644551315161
created: 1644551065086
---

## Dependency
- [pillow](https://pillow.readthedocs.io/en/stable/)
- [piexif](https://piexif.readthedocs.io/en/latest/)

## Implementation
> [Tag reference](https://exiv2.org/tags.html) (Rational => Fraction)

```
img = Image.open(image_path)
if 'exif' in img.info:
    exif_dict = piexif.load(img.info['exif'])
    if '0th' not in exif_dict:
        exif_dict['0th'] = {}
    if 'Exif' not in exif_dict:
        exif_dict['Exif'] = {}
else:
    exif_dict = {'0th': {}, 'Exif': {}}

if args.make:
    exif_dict['0th'][piexif.ImageIFD.Make] = args.make

if args.model:
    exif_dict['0th'][piexif.ImageIFD.Model] = args.model

if args.focal_length:
    focal_length = Fraction(args.focal_length)
    exif_dict['Exif'][piexif.ExifIFD.FocalLength] = (
        focal_length.numerator, focal_length.denominator)

exif_bytes = piexif.dump(exif_dict)
img.save(image_path, exif=exif_bytes)
```