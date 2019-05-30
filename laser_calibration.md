# Laser Calibration for Frauenhofer EZRT

octopus 3d scan v1.1, sn:403709

The laser calibration target needs to be scanned at 340 to 140 mm in 10 mm increments. These are the z-axis values for the PLC (Zplc).  Then a file structure needs to be setup for the Frauenhofer calibration software. This is located in  C:/Lemnatec/3DScanner/CalibratorInput. Then open cmd and run C:/Files/LemnaTec/PhenoCenter/<version#>/PhenoCenter/Environment.exe DriverTestDialog

1. backup everything in C:/Lemnatec/3DScanner/CalibratorInput

2. remove imaging deck frame and install calibration target

3. Open phenocenter
and setup experiment (no db import, don't delete files) and job, setting z = 337
and plant height = 0 in the various settings.

4. Run the job for plant heights
0-220 in 10 mm increments

5. Move the snapshot folders to
C:/LemnaTec/3DScanner/CalibratorInput and rename each snapshot folder 340
(plantheight = 0) to 140 (highest plantheight).

6. Each folder needs a
corresponding captureResultInfo_<plantheight>_DistanceInMM_<dist>.xml where dist
= 534 to 734 in increments of 10. Each xml file should follow the pattern below.
PeakPath refers to height map .png, GrayPath refers to Intensity map .png.

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

7. in cmd run
C:/Files/LemnaTec/PhenoCenter/<version#>/PhenoCenter/Environment.exe
DriverTestDialog

8. Select Fraunhofer camera for the correct position number and
create a driver

9. Run calibration tool. This should result in
CalibrationResult.calib in C:/LemnaTec/3DScanner/CalibratorInput.  Leave this
here for lemna software to find when merging the point clouds.
