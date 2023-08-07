## Introduction
The focus of this document is to demonstrate one among many issues which is faced generally while writing the pre-processing or post-processing functions referred from one framework to putting it in another framework. Let'us directly jump to our topic.

## Pre-processing
Below is the pre-processing done in pytorch

```python
transform = torchvision.transforms.Compose([
    torchvision.transforms.ToTensor(),
    torchvision.transforms.Normalize(
        mean=[0.485, 0.456, 0.406],
        std=[0.229, 0.224, 0.225],
    ),
])
normalized_img = transform(img_pil)
permute = [2, 1, 0]
normalized_img_bgr = normalized_img[:, permute]
```
As you can see in above code the image is pil. So definitely channel are not in sequence as that of opecv which takes BGR. Here it is RGB. Thus finally we just switch the channels.


C++ with same operations

What many implementation does
```c++
void normalization(float *& normalize_tensor, cv::Mat &image) {
for(int i=0;i < image.rows; i++) {
for(int j=0;j < image.cols; j++) {
Vec3f bgrPixel = image.at<Vec3f>(i, j);
normalize_tensor[i*j + j] = (bgrPixel[2]/255 - 0.485)/0.229;
normalize_tensor[i*j + j + 1] = (bgrPixel[1]/255 - 0.456)/0.224;
normalize_tensor[i*j + j + 2] = (bgrPixel[0]/255 - 0.406)/0.225;
}
}

cv::Mat input = cv::imread("path_to_image_or_input_frame");
float normalize_img;
normalization(normalize_img, input);


```

Issues
- Above code seems perfectly fine. Except when these operations are performed on normal size image.
- When image needs to be resize then the blunders happen and output tensor for pre-processing kernel doesn't match
- As you may have notice the input tensor which we created in c++ is float and same is true for pytorch. But image read by us is in uint8 foramt. This causes problem while resize
- When we resize the uint8 image the resized image has whole numbers. Then even though we have perfectly proper pre-processing functions it won't generate same input tensor as that of pytorch
- This happens because round-ups are taken in consideration in opencv resize operation when input image is in unit8 format
- Many would say just divide the input value by 255 and it will get converted but it doesnt happen like that in such cases

Solution

```c++
image.convertTo(floatMat,  CV_32F);
// resizing and rest code
```

Following above step would produce same input as that of the expected one in pytorch. (There are exception to this too we need to take care of floating point standards by compiler and interpretur just not to have more unknowns).
One needs to take care particularly when pre-processing takes place in cuda. Then it really causes lot of difference. 
I will soon give an example of this.

