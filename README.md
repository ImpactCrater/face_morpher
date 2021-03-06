## AutoFaceMorpher

Most of this program was cloned from the following repositories;
https://github.com/alyssaq/face_morpher
- **Warp, average and morph human faces.**
- **Scripts will automatically detect frontal faces and skip images if none is detected.**

Built with Python, [dlib][], Numpy, Scipy, [OpenCV][].

- Supported on Python 3.6+
- Tested on 64bit Linux (Ubuntu 18.04 LTS).

### Requirements
--------------
-  ``pip install -r requirements.txt``
- Download http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2 and extract file.
- Export environment variable ``DLIB_DATA_DIR`` to the folder where ``shape_predictor_68_face_landmarks.dat`` is located. Default ``data``. E.g ``export DLIB_DATA_DIR=/Downloads/data``

Either:

- [Use as local command-line utility](#Use-as-local-command-line-utility)
- [Use as pip library](#Use-as-pip-library)

### Use as local command-line utility
```bash
sudo git clone https://github.com/ImpactCrater/AutoFaceMorpher
```

### Morphing Faces

- Morph from a source to destination image:
```bash
python3 face_morpher-dlib/facemorpher/morpher.py --src=<src_imgpath> --dest=<dest_imgpath> --plot
```

- Morph through a series of images in a folder:
```bash
python3 face_morpher-dlib/facemorpher/morpher.py --images=<folder> --out_video=out.avi
```

- All options listed in ``morpher.py`` (pasted below):
```
    Morph from source to destination face or
    Morph through all images in a folder

    Usage:
        morpher.py (--src=<src_path> --dest=<dest_path> | --images=<folder>)
                [--width=<width>] [--height=<height>]
                [--num=<num_frames>] [--fps=<frames_per_second>]
                [--out_frames=<folder>] [--out_video=<filename>]
                [--plot] [--background=(black|transparent|average)]

    Options:
        -h, --help              Show this screen.
        --src=<src_imgpath>     Filepath to source image (.jpg, .jpeg, .png)
        --dest=<dest_imgpath>   Filepath to destination image (.jpg, .jpeg, .png)
        --images=<folder>       Folderpath to images
        --width=<width>         Custom width of the images/video [default: 500]
        --height=<height>       Custom height of the images/video [default: 600]
        --num=<num_frames>      Number of morph frames [default: 20]
        --fps=<fps>             Number frames per second for the video [default: 10]
        --out_frames=<folder>   Folder path to save all image frames
        --out_video=<filename>  Filename to save a video
        --plot                  Flag to plot images to result.png [default: False]
        --background=<bg>       Background of images to be one of (black|transparent|average) [default: black]
        --version               Show version.
```

### Averaging Faces

- Average faces from all images in a folder:
```bash
python3 face_morpher-dlib/facemorpher/averager.py --images=<images_folder> --out=average.png
```

- All options listed in ``averager.py`` (pasted below):
```
Face averager

    Usage:
        averager.py --images=<images_folder> [--blur]
                [--background=(black|transparent|average)]
                [--width=<width>] [--height=<height>]
                [--out=<filename>] [--destimg=<filename>]

    Options:
        -h, --help             Show this screen.
        --images=<folder>      Folder to images (.jpg, .jpeg, .png)
        --blur                 Flag to blur edges of image [default: False]
        --width=<width>        Custom width of the images/video [default: 500]
        --height=<height>      Custom height of the images/video [default: 600]
        --out=<filename>       Filename to save the average face [default: result.png]
        --destimg=<filename>   Destination face image to overlay average face
        --background=<bg>      Background of image to be one of (black|transparent|average) [default: average]
        --version              Show version.
```

### Steps (facemorpher folder)
--------------------------

1. Locator

-  Locates face points
-  For a different locator, return an array of (x, y) control face points

2. Aligner

-  Align faces by resizing, centering and cropping to given size

3. Warper

-  Given 2 images and its face points, warp one image to the other
-  Triangulates face points
-  Affine transforms each triangle with bilinear interpolation

4a. Morpher

-  Morph between 2 or more images

4b. Averager

-  Average faces from 2 or more images

5. Blender
    Optional blending of warped image:

- Weighted average
- Alpha feathering
- Poisson blend

### Examples - `Being John Malkovich`_

#### Create a morphing video between the 2 images:
```bash
python3 face_morpher-dlib/facemorpher/morpher.py --src=alyssa.jpg --dest=john_malkovich.jpg --out_video=out.avi
```

(out.avi played and recorded as gif)

![morphing gif](https://raw.githubusercontent.com/ImpactCrater/AutoFaceMorpher/dlib/examples/being_john_malvokich.gif)

#### Save the frames to a folder:
```bash
python3 face_morpher-dlib/facemorpher/morpher.py --src=alyssa.jpg --dest=john_malkovich.jpg --out_frames=out_folder --num=30
```

#### Plot the frames:
```bash
python3 face_morpher-dlib/facemorpher/morpher.py --src=alyssa.jpg --dest=john_malkovich.jpg --num=12 --plot
```
![frames plot](https://raw.githubusercontent.com/ImpactCrater/AutoFaceMorpher/dlib/examples/plot.png)

#### Average all face images in a folder:

- 208 images used
```bash
python3 face_morpher-dlib/facemorpher/averager.py --images=images --blur --background=average --width=1200 --height=1200
```

![averaged female face](https://raw.githubusercontent.com/ImpactCrater/AutoFaceMorpher/dlib/examples/result-adjusted.png)

This is a result image with unsharp mask filtering applied and cropped.
</br>
</br>
- 85 images used
```bash
python3 face_morpher-dlib/facemorpher/averager.py --images=images --blur --background=transparent --width=220 --height=250
```

![averaged male face](https://raw.githubusercontent.com/ImpactCrater/AutoFaceMorpher/dlib/examples/average_faces.png)


#### Build & publish Docs
```bash
./scripts/publish_ghpages.sh
```

License
-------
[MIT][]

[MIT]:https://github.com/ImpactCrater/AutoFaceMorpher/blob/dlib/LICENSE
[OpenCV]:http://opencv.org
[Homebrew]:https://brew.sh
[source]:https://github.com/opencv/opencv
[dlib]:http://dlib.net
