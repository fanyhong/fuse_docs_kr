원본: https://www.fusetools.com/docs/fuse/camera/camera

FuseJS/Camera Module (JS)

 Show advanced things
Jump to:
Table of Contents
Allows the capture of still images from the system camera.

Images are returned as frozen JavaScript Image objects, consisting of a path, a filename, a width and a height. Once created or acquired, Images can be passed around to other APIs to use, fetch or alter their underlying data. All images are temporary "scratch images" until storage has been specified either through publishing to the CameraRoll or other.

You need to add a reference to "Fuse.Camera" in your project file to use this feature.

On Android using this API will request the CAMERA and WRITE_EXTERNAL_STORAGE permissions.

Example

var camera = require('FuseJS/Camera');
camera.takePicture(640,480).then(function(image)
{
    //Do things with image here
}).catch(function(error) {
    //Something went wrong, see error for details
});
Interface of Camera

