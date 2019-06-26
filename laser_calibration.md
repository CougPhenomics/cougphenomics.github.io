# Laser Calibration for Frauenhofer EZRT

octopus 3d scan v1.1, sn:403709

## Summary
The laser calibration target needs to be scanned at 340 to 140 mm in 10 mm increments. These are the z-axis values for the PLC (Zplc).  Then a file structure needs to be setup for the Frauenhofer calibration software. This is located in  `C:/Lemnatec/3DScanner/CalibratorInput`. Then open cmd and run `C:/Files/LemnaTec/PhenoCenter/<version#>/PhenoCenter/Environment.exe DriverTestDialog` and select `_Do_Calib_`.

## Steps
1. backup everything in `C:/Lemnatec/3DScanner/CalibratorInput` plus save `CalibrationConfig.calib`

2. remove imaging deck frame and install calibration target

3. Open phenocenter
and setup experiment (no db import, don't delete files) and job, setting z = 340 and plant height = 0 in the various settings.

4. Run the job for plant heights 0-220 in 10 mm increments
	- plantheight 200 mm = 20 cm above the target
	- plantheight 300 mm = 30 cm above the target
	- plantheight 400 mm = 40 cm above the target

5. Move the snapshot folders to `C:/LemnaTec/3DScanner/CalibratorInput` and rename each snapshot folder 340 (plantheight = 0) to 140 (highest plantheight). Also unpack each .rawx and the enclosed image.raw file. Save the .png and .ply in the upper most directory for the scan, e.g. `140`.

6. In each numbered directory, you need an a `captureResultInfo.xml` that looks like the example below.

7. Each numbered directory also needs a corresponding `captureResultInfo_<plantheight>_DistanceInMM_<dist>.xml` at the same level as the directory. Dist = 534 to 734 in increments of 10. plant height = 0 corresponds to dist = 734. Each xml file should follow the pattern below. PeakPath refers to height map .png, GrayPath refers to Intensity map .png.

```xml
<CaptureResult>
	<Sensor SensorName="sensor_01">
		<CaptureData PeakPath="C:\LemnaTec\3DScanner\CalibratorInput\340\sensor0_Height.png" GreyPath="C:\LemnaTec\3DScanner\CalibratorInput\340\sensor0_Intensity.png"/>
	</Sensor>
	<Sensor SensorName="sensor_02">
		<CaptureData PeakPath="C:\LemnaTec\3DScanner\CalibratorInput\340\sensor1_Height.png" GreyPath="C:\LemnaTec\3DScanner\CalibratorInput\340\sensor1_Intensity.png"/>
	</Sensor>
</CaptureResult>
```
8. open the phenocenter debug tools, connect to 3d scanner

9. in cmd run `C:/Files/LemnaTec/PhenoCenter/<version#>/PhenoCenter/Environment.exe DriverTestDialog`

10. Select Fraunhofer camera and create a driver

11. Run `_Do_Calib_` calibration tool. This should result in `CalibrationResult.calib` in `C:/LemnaTec/3DScanner/CalibratorInput`.  Move this up one level to `C:/LemnaTec/3DScanner` for lemna software to find when merging the point clouds.

## Other Notes

If there is an `output` folder in `3DScanner` this is used as a temporary directory during the processing of the .png to .ply
