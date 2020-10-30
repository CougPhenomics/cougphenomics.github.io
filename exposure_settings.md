# Notes on exposure settings

In principle it is best to find a single focus, aperture, and x:y:z for each camera that is repeatable for all experiments. In practice there are some complications:

1. 2 pot types so you end up with different heights of plants
2. different plants will have different heights (although since we mostly image arabidopsis this isn't a huge difference)
3. the plants (even arabidopsis) aren't perfectly flat and as they grow the focal plane shifts upwards
4. make sure the field of view contains the entire plant for the entire experiment!
5. depending on the experiment, the surface phenotype might change. e.g. mutants such as *kea* or other genotypes during a drought or salt stress experiment the leaves will get pale and reflect more light. In general I find erring on the under exposed side of things is better. It is also possible to increase image brightness afterwards to recover dark areas, but details can't usually get recovered from overly saturated (white) area.
6. spindly plants will waver in the advection of the chamber so you need a minimum exposure time to freeze them


I think the best strategy is to default the camera at the maximum height to maximize the field of view. Set the focal plane ~1 cm above the top of the 3" pots. I usually use an aperture of f/8 in improve the depth of field.  Larger values (smaller hole) would improve it further but I found that you are light limited, primarily because shining the lights directly at the tray will cause glare.  Use the set screws on the lens to fix the f/stop and focal depth.  Then you can use the z position to move the camera closer (increase z value since datum is at the top) if you have a plane that is farther from the camera.

Another item to note is that you can only adjust the settings for deck position A1. In theory this should be ok because the light conditions are the same. I found that neighboring trays reflect enough light to change the illumination and optimal lighting. I have thought about spray painting all the trays black but I've never been time limited with the imaging system so I instead skip every other deck position.

As noted above, exposure time needs to be high enough to freeze the plants in an image. I've seen leaf ghosting even in Arabidopsis. The windspeed in the chamber averages about 0.8 m/s with a range of 0.2-1.

## VIS camera settings

There are several settings in addition to exposure that affect your image. These primarily affect color/white balance and contrast. It is helpful to read the [Prosilica Technical Manual and Features Reference](https://www.alliedvision.com/en/support/technical-documentation/prosilica-gt-documentation.html) for a description of all the settings.
Keep in mind that LemnTec has template settings that are automatically set for new experiments.   Defaults mentioned below are the defaults from Allied Vision. Often the LemnaTec value is in units [1/100], presumably to stick to integers.

Specically, LemnaTec lets you control the Gain, Gamma, R white balance, and B white balance.

[Gain](https://cdn.alliedvision.com/fileadmin/content/documents/products/cameras/various/features/GigE_Features_Reference.pdf#G6.1339048) is useful to boost the signal if lighting is too low. The effect is exponential. This works directly on the voltages at the sensor. The downside is that it increases noise however I've not noticed anything detectable even at higher values. The default of 0 turns this off.

[Gamma](https://cdn.alliedvision.com/fileadmin/content/documents/products/cameras/various/features/GigE_Features_Reference.pdf#G6.1339358) controls the gamma correction of pixel intensity. I'm not entirely sure what this means - historically in photography a gamma of 1.4 is used to adjust for non-linear color perception. It is a power relationship where output = input^gamma

[R white balance and B white balance](https://cdn.alliedvision.com/fileadmin/content/documents/products/cameras/various/features/GigE_Features_Reference.pdf#G6.1282612) changes the relationship between R, B, G. It is a power relationship and G is considered a baseline so you can only change R and B. output = input^gamma. 1 is the default. range is 0.8-3

The 50 mm Kowa lens on this camera is in my opinion too long a focal length.

## SWIR camera settings

The only parameter you can change with LemnaTec is exposure time. VERY IMPORTANT - the internal automatic pixel defect correction (DPC) only works between 5ms and 1000ms.

This camera has a [C-mount adapter](https://cdn.alliedvision.com/fileadmin/content/documents/products/cameras/Goldeye_2/techman/Goldeye_G-CL_TechMan.pdf#G10.1032230) on it and includes a narrowband filter at 1450 nm.

The 35 mm Kowa lens on this camera is specially designed for infrared applications although also works with the VIS camera.

[Documentation](https://www.alliedvision.com/en/support/technical-documentation/goldeye-gcl-documentation.html)

As of early 2020, the LED bars mounted to this camera do not produce light anymore. They appear to be a custom LED circuit board powered by 24V. This is a bit overwhelming to solve without better understanding how these work so I'm currently using the camera with incandescent bulbs in external lamps. These arent' optimal because they are less consistent over their lives and with temperature variations.


## AlliedVision Deep Dive

If you really want to see how all the settings affect the image you can use Vimba Viewer from Allied. However you need to use LemnaTec software to mount the camera and then "end task" for Phenocenter in task manager to release control of the camera without LT putting it away. Then you can access the cameras with full control using Vimba Viewer. I *think* the settings you change in Vimba Viewer will stick until you unmount the camera so be careful of that! I don't know how to change the settings permanently, but probably you could find the path to a camera config file - maybe in hardwareIni.cfg?  You can also mount the camera using the teaching tools and then you don't have to End Task because I don't think the Teaching Tools open a connection. It just moves the camera.

## Walz PSII camera

Manufacturer documentation is [here](https://www.walz.com/products/chl_p700/imaging-pam_ms/downloads.html)

IMPORTANT- do not install a newer ImagingWin on the control pc (CPPC0) because it has a version of ImagingWin that is customized for LemnaTec.

Most of the settings for this camera in LemnaTec are not that important because they are all defined in the script you will run. The script # is determines the script in Data_MAXI it will run. e.g. 2 means LemnaTec2.prg

Note that the camera is designed to image with a specific light intensity from the LED array. In the standard benchtop setup this would mean the plants and light are 17-20 cm apart. SImilarly to the VIS camera, I've been maximizing the field of view and keeping the aperture ~f/8. It is likely that I am not getting the full light intensity as designed but I haven't had any trouble getting good images and data. If you have a user with very specific light intensity needs though, you should have them measure the light intensity with a light meter to make sure they are happy about it.

I typically use LemnaTec2.prg (so script #2). This replicates the default Induction Curve in ImagingWin.

## Frauenhof EZRT laser scanner

I'm not super familiar with this camera or the settings. We ultimately sent it back to Fraunenhof in Germany though because it seemed that the 2 sensors were not well aligned.
