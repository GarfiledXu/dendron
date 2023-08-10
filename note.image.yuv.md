---
id: s5xrz9gfpoi53mpctuyo9jz
title: Yuv
desc: ''
updated: 1691546568205
created: 1691544098073
---

### common yuv format
------
#### UYVY


#### NV21


#### NV12


#### I420


### image processing
------
#### nv21 four image splice into one
~~~c++
void concatenateNV21(const char *image1Path, const char *image2Path, const char *image3Path, const char *image4Path, const char *outputImagePath)
{
    FILE *image1File = fopen(image1Path, "rb");
    FILE *image2File = fopen(image2Path, "rb");
    FILE *image3File = fopen(image3Path, "rb");
    FILE *image4File = fopen(image4Path, "rb");
    FILE *outputFile = fopen(outputImagePath, "wb");

    if (!image1File || !image2File || !image3File || !image4File || !outputFile)
    {
        printf("Failed to open input/output files.\n");
        return;
    }

    // 计算文件大小
    fseek(image1File, 0, SEEK_END);
    long imageSize1 = ftell(image1File);
    fseek(image1File, 0, SEEK_SET);

    fseek(image2File, 0, SEEK_END);
    long imageSize2 = ftell(image2File);
    fseek(image2File, 0, SEEK_SET);

    fseek(image3File, 0, SEEK_END);
    long imageSize3 = ftell(image3File);
    fseek(image3File, 0, SEEK_SET);

    fseek(image4File, 0, SEEK_END);
    long imageSize4 = ftell(image4File);
    fseek(image4File, 0, SEEK_SET);

    // 读取图像数据
    char *image1Data = (char *)malloc(imageSize1);
    char *image2Data = (char *)malloc(imageSize2);
    char *image3Data = (char *)malloc(imageSize3);
    char *image4Data = (char *)malloc(imageSize4);

    fread(image1Data, 1, imageSize1, image1File);
    fread(image2Data, 1, imageSize2, image2File);
    fread(image3Data, 1, imageSize3, image3File);
    fread(image4Data, 1, imageSize4, image4File);

    // 关闭文件
    fclose(image1File);
    fclose(image2File);
    fclose(image3File);
    fclose(image4File);

    // 计算图像分辨率和大小
    int imageWidth = 1920; /* 根据实际情况设置 */

    int imageHeight = 1536; /* 根据实际情况设置 */

    // 计算拼接后图像的分辨率和大小
    int outputWidth = imageWidth * 2;
    int outputHeight = imageHeight * 2;

    // 创建新的缓冲区
    char *outputImageData = (char *)malloc(outputWidth * outputHeight * 3 / 2);

    // 拷贝第一个图像的Y分量
    for (int y = 0; y < imageHeight; y++)
    {
        for (int x = 0; x < imageWidth; x++)
        {
            outputImageData[y * outputWidth + x] = image1Data[y * imageWidth + x];
        }
    }

    // 拷贝第一个图像的UV分量
    for (int y = 0; y < imageHeight / 2; y++)
    {
        for (int x = 0; x < imageWidth / 2; x++)
        {
            outputImageData[outputWidth * outputHeight + y * outputWidth + x * 2] = image1Data[imageWidth * imageHeight + y * imageWidth + x * 2];
            outputImageData[outputWidth * outputHeight + y * outputWidth + x * 2 + 1] = image1Data[imageWidth * imageHeight + y * imageWidth + x * 2 + 1];
        }
    }

    // 拷贝第二个图像的Y分量
    for (int y = 0; y < imageHeight; y++)
    {
        for (int x = 0; x < imageWidth; x++)
        {
            outputImageData[y * outputWidth + x + imageWidth] = image2Data[y * imageWidth + x];
        }
    }

    // 拷贝第二个图像的UV分量
    for (int y = 0; y < imageHeight / 2; y++)
    {
        for (int x = 0; x < imageWidth / 2; x++)
        {
            outputImageData[outputWidth * outputHeight + y * outputWidth + x * 2 + imageWidth] = image2Data[imageWidth * imageHeight + y * imageWidth + x * 2];
            outputImageData[outputWidth * outputHeight + y * outputWidth + x * 2 + 1 + imageWidth] = image2Data[imageWidth * imageHeight + y * imageWidth + x * 2 + 1];
        }
    }

    // 拷贝第三个图像的Y分量
    for (int y = 0; y < imageHeight; y++)
    {
        for (int x = 0; x < imageWidth; x++)
        {
            outputImageData[(y + imageHeight) * outputWidth + x] = image3Data[y * imageWidth + x];
        }
    }

    // // 拷贝第三个图像的UV分量
    for (int y = 0; y < imageHeight / 2; y++)
    {
        for (int x = 0; x < imageWidth / 2; x++)
        {

            outputImageData[outputWidth * outputHeight + (y + imageHeight / 2) * outputWidth + x * 2] = image3Data[imageWidth * imageHeight + y * imageWidth + x * 2];
            outputImageData[outputWidth * outputHeight + (y + imageHeight / 2) * outputWidth + x * 2 + 1] = image3Data[imageWidth * imageHeight + y * imageWidth + x * 2 + 1];
        }
    }

    // 拷贝第四个图像的Y分量
    for (int y = 0; y < imageHeight; y++)
    {
        for (int x = 0; x < imageWidth; x++)
        {
            outputImageData[(y + imageHeight) * outputWidth + x + imageWidth] = image4Data[y * imageWidth + x];
        }
    }

    // 拷贝第四个图像的UV分量
    for (int y = 0; y < imageHeight / 2; y++)
    {
        for (int x = 0; x < imageWidth / 2; x++)
        {
            outputImageData[outputWidth * outputHeight + (y + imageHeight/2) * outputWidth + x * 2 + imageWidth] = image4Data[imageWidth * imageHeight + y * imageWidth  + x * 2];
            outputImageData[outputWidth * outputHeight + (y + imageHeight/2) * outputWidth + x * 2 + 1 + imageWidth] = image4Data[imageWidth * imageHeight + y * imageWidth  + x * 2 + 1];
        }
    }

    // 保存拼接后的图像数据到文件
    fwrite(outputImageData, 1, outputWidth * outputHeight * 3 / 2, outputFile);

    // 关闭输出文件
    fclose(outputFile);

    // 释放内存
    free(image1Data);
    free(image2Data);
    free(image3Data);
    free(image4Data);
    free(outputImageData);

    printf("Concatenated image saved: %s\n", outputImagePath);
}
~~~

