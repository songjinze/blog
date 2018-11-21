---
title: logo生成
date: 2018-11-22 01:37:33
tags: 闲言碎语
categories: 个人经验
---

# 新主题Logo生成

使用新主题的时候自带logo背景为透明，想换成自己的主题，但是图库里的图片背景为白色或者是黄色。  

没装PS，写个小程序自己改一下背景。需要的同学可以自取。

{% codeblock %}
#include <iostream>
#include <opencv2/opencv.hpp>

using namespace cv;
using namespace std;

void logoHandle(){
	Mat img = imread("logo.jpeg",-1);         //读取图像
	cv::cvtColor(img, img, CV_BGR2BGRA,4);    //变为四通道

	
	int r, b,g;
	for (int i = 0; i < img.rows; i++)
		for (int j = 0; j < img.cols; j++)
		{
			r = img.at<cv::Vec4b>(i, j)[2];
			g = img.at<cv::Vec4b>(i, j)[1];
			b = img.at<cv::Vec4b>(i, j)[0];

			if (r > 200 && b > 200 && g > 200)
				img.at<cv::Vec4b>(i, j)[3] = 0;     //将偏白色部分alpha值设0%，为透明
		}
	resize(img, img, Size(60, 60));  //变为60*60像素
	imwrite("new.png", img);          //写回
{% endcodeblock %}

## 示例：
原图：  

{% asset_img logo.jpeg logo.jpeg %}

变换之后：  

{% asset_img new.png new.png %}