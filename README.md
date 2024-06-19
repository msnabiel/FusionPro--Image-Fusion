Here’s a detailed README for your **FusionPro** project, complete with the provided MATLAB code and a brief project description.

---

# FusionPro

FusionPro is a digital signal processing (DSP) project that performs image fusion using various fusion strategies. It uses wavelet transforms to combine two images into one, enhancing the features and details of both images.

## Project Overview

FusionPro allows users to fuse two images using different wavelet-based fusion techniques. The available fusion methods include 'MeanMean', 'MeanMax', 'MeanMin', 'MaxMean', 'MaxMax', 'MaxMin', 'MinMean', 'MinMax', and 'MinMin'. The final fused image combines the significant features from both source images.

## Key Features

- **Flexible Fusion Types:** Choose from various fusion strategies for different fusion outcomes.
- **Wavelet Transformations:** Uses discrete wavelet transforms (DWT) for image fusion.
- **User-Friendly Interface:** Select images interactively through file dialog boxes.
- **Color Image Support:** Handles RGB images, performing fusion on each color channel separately.

## How to Use

1. **Select Images:** The script prompts you to select two images for fusion.
2. **Image Fusion:** It then fuses the images using the specified fusion strategy and wavelet type.
3. **View and Save Result:** The fused image is displayed and saved as `fusedimage.jpg`.

## Usage Instructions

1. **Run the Script:**
   - Execute the `FusionPro.m` file in MATLAB.

2. **Select Images:**
   - Use the file dialog to select two images you want to fuse.

3. **Fusion Output:**
   - The script will display the two input images and the resulting fused image in a 3-panel subplot.
   - The fused image is saved to the current directory as `fusedimage.jpg`.

## Requirements

- **MATLAB:** Ensure you have MATLAB installed.
- **Image Processing Toolbox:** Required for image reading, resizing, and displaying.
- **Wavelet Toolbox:** Needed for performing wavelet transformations.

## Example Code

```matlab
clear;
fusiontype = 'MaxMax';
wavetype = 'coif5';

% Load Image 1
[filename1, pathname1] = uigetfile('.', 'Select the 1st Image');
filewithpath1 = strcat(pathname1, filename1);
img1 = imread(filewithpath1);

% Load Image 2
[filename2, pathname2] = uigetfile('.', 'Select the 2nd Image');
filewithpath2 = strcat(pathname2, filename2);
img2 = imread(filewithpath2);

[row, col] = size(img1(:,:,1));

% Resize Image 2 if necessary
if ~isequal(size(img1), size(img2))
    img2 = imresize(img2, [row, col]);
end

% Perform Image Fusion
fusedimageR = imgfusion(img1(:,:,1), img2(:,:,1), fusiontype, wavetype);
fusedimageG = imgfusion(img1(:,:,2), img2(:,:,2), fusiontype, wavetype);
fusedimageB = imgfusion(img1(:,:,3), img2(:,:,3), fusiontype, wavetype);
fusedimage = uint8(cat(3, fusedimageR, fusedimageG, fusedimageB));

% Save Fused Image
imwrite(imresize(fusedimage, [row, col]), 'fusedimage.jpg', 'Quality', 100);

% Display Images
subplot(131);
imshow(img1);
title('Image1');

subplot(132);
imshow(img2);
title('Image2');

subplot(133);
imshow(fusedimage);
title('Fused Image');

% Function to create fused Image
function outimage = imgfusion(Im1, Im2, ftype, wtype)
    [cA1, cH1, cV1, cD1] = dwt2(double(Im1), wtype, 'per');
    [cA2, cH2, cV2, cD2] = dwt2(double(Im2), wtype, 'per');
    [r, c] = size(cA1);
    cA = zeros(r, c);
    cH = zeros(r, c);
    cV = zeros(r, c);
    cD = zeros(r, c);

    switch ftype
        case 'MeanMean'
            for i = 1:r
                for k = 1:c
                    cA(i, k) = mean([cA1(i, k), cA2(i, k)]);
                    cH(i, k) = mean([cH1(i, k), cH2(i, k)]);
                    cV(i, k) = mean([cV1(i, k), cV2(i, k)]);
                    cD(i, k) = mean([cD1(i, k), cD2(i, k)]);
                end
            end
        case 'MeanMax'
            for i = 1:r
                for k = 1:c
                    cA(i, k) = mean([cA1(i, k), cA2(i, k)]);
                    cH(i, k) = max([cH1(i, k), cH2(i, k)]);
                    cV(i, k) = max([cV1(i, k), cV2(i, k)]);
                    cD(i, k) = max([cD1(i, k), cD2(i, k)]);
                end
            end
        case 'MeanMin'
            for i = 1:r
                for k = 1:c
                    cA(i, k) = mean([cA1(i, k), cA2(i, k)]);
                    cH(i, k) = min([cH1(i, k), cH2(i, k)]);
                    cV(i, k) = min([cV1(i, k), cV2(i, k)]);
                    cD(i, k) = min([cD1(i, k), cD2(i, k)]);
                end
            end
        case 'MaxMean'
            for i = 1:r
                for k = 1:c
                    cA(i, k) = max([cA1(i, k), cA2(i, k)]);
                    cH(i, k) = mean([cH1(i, k), cH2(i, k)]);
                    cV(i, k) = mean([cV1(i, k), cV2(i, k)]);
                    cD(i, k) = mean([cD1(i, k), cD2(i, k)]);
                end
            end
        case 'MaxMax'
            for i = 1:r
                for k = 1:c
                    cA(i, k) = max([cA1(i, k), cA2(i, k)]);
                    cH(i, k) = max([cH1(i, k), cH2(i, k)]);
                    cV(i, k) = max([cV1(i, k), cV2(i, k)]);
                    cD(i, k) = max([cD1(i, k), cD2(i, k)]);
                end
            end
        case 'MaxMin'
            for i = 1:r
                for k = 1:c
                    cA(i, k) = max([cA1(i, k), cA2(i, k)]);
                    cH(i, k) = min([cH1(i, k), cH2(i, k)]);
                    cV(i, k) = min([cV1(i, k), cV2(i, k)]);
                    cD(i, k) = min([cD1(i, k), cD2(i, k)]);
                end
            end
        case 'MinMean'
            for i = 1:r
                for k = 1:c
                    cA(i, k) = min([cA1(i, k), cA2(i, k)]);
                    cH(i, k) = mean([cH1(i, k), cH2(i, k)]);
                    cV(i, k) = mean([cV1(i, k), cV2(i, k)]);
                    cD(i, k) = mean([cD1(i, k), cD2(i, k)]);
                end
            end
        case 'MinMax'
            for i = 1:r
                for k = 1:c
                    cA(i, k) = min([cA1(i, k), cA2(i, k)]);
                    cH(i, k) = max([cH1(i, k), cH2(i, k)]);
                    cV(i, k) = max([cV1(i, k), cV2(i, k)]);
                    cD(i, k) = max([cD1(i, k), cD2(i, k)]);
                end
            end
        case 'MinMin'
            for i = 1:r
                for k = 1:c
                    cA(i, k) = min([cA1(i, k), cA2(i, k)]);
                    cH(i, k) = min([cH1(i, k), cH2(i, k)]);
                    cV(i, k) = min([cV1(i, k), cV2(i, k)]);
                    cD(i, k) = min([cD1(i, k), cD2(i, k)]);
                end
            end
    end
    outimage = idwt2(cA, cH, cV, cD, wtype,

 'per');
end
```

## Acknowledgements

- The wavelet transformation is performed using MATLAB's Wavelet Toolbox.
- The project was developed and tested in a MATLAB environment.

## Contact

Author: Syed Nabiel Hasaan M
For any queries or support, please contact [msyednabiel@gmail.com](mailto:msyednabiel@gmail.com]).
