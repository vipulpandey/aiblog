---
title: Deploy Flask package of OpenCV Face Recognition on heroku!
date: "2019-07-08 09:38:43"
template: "post"
draft: false
slug: "/posts/opencv-image-flask-heroku-deployment/"
category: "AI Programming"
tags:
 - "Artificial Intelligence"
 - "Programming"
 - "Beginners"
 - "Python"
 - "OpenCV"
description: "This is an overview of how to deploy a flask application about face recognition using opencv on heroku. This is not coding example, Flask app is ready is the assumption."
---

Assumption is that one has already created a flask module or package using the haarcascades for facial recognition in the openCV. However, there is a quick follow up of the basic overview of the resources and tools required to create a flask application using opencv.

## About OpenCV

>(From official opencv website)

OpenCV (Open Source Computer Vision Library) is an open source computer vision and machine learning software library. OpenCV was built to provide a common infrastructure for computer vision applications and to accelerate the use of machine perception in the commercial products. Being a BSD-licensed product, OpenCV makes it easy for businesses to utilize and modify the code.
The library has more than 2500 optimized algorithms, which includes a comprehensive set of both classic and state-of-the-art computer vision and machine learning algorithms.

### Haar Cascade Classifiers

Face detection uses classifiers, which are algorithms that detects what is either a face(1) or not a face(0) in an image. Classifiers have been trained to detect faces using thousands to millions of images in order to get more accuracy. OpenCV uses two types of classifiers, LBP (Local Binary Pattern) and Haar Cascades.

[A very Helpful article for understanding Face Detection and HaarCascades](#https://becominghuman.ai/face-detection-using-opencv-with-haar-cascade-classifiers-941dbb25177)


## About Flask
[Flask](#http://flask.pocoo.org/)

Flask is a microframework that is used for web app development specifically for routing the request from browsers and executing the server side response, based on that request.

## Python with Heroku

> **AIM** : To create a flask web app that detects faces in the received the images( possibly in jpeg and png format) and deploy it to the heroku platform as a standalone app.

Skills and Tools Required

1). Python </br>
2). Flask  </br>
3). Gunicorn WSGI Server</br>
3). OpenCV </br>
4). Heroku

## One example "instance" of the app serving a request

+ Flask App receives image in .jpg or .png format
+ Image recognition logic in haarcascades files analyzes the image
+ Result image is served to the front end.
+ Both input and output images are saved.

Here's the test image that was run on local machine:

![flask folder tree](/media/pedestrian-phone.jpg)

Create and test core logic in your local machine using Flask, that receives input image and gives output image with number of faces detected.
A sample Structure of the files of a flask app that was created and deployed to heroku is:

![flask folder tree](/media/file_tree_flask.png)

To make it clear, this is a python package not the module, so it has a different command for Procfile.

## Deployment to heroku

At the level with the inside facecount directory( in this case ) or in the root directory of Python package. You need to create three files before deploying to heroku

1). Procfile ( no extension )  </br>
2). requirements.txt   </br>
3). Aptfile  ( no extension )

- Create this requirements.txt file using, it registers all the packages that is being used by the local virtual environment to run the app


   `pip freeze > requirements.txt`


- login to heroku cli </br>


   `heroku login`

- create app using cli </br>


   `heroku create yourherokuappname`

- Check whether your app is a module or its python package because the gunicorn command is different for both. Following code needs to be in the **Procfile**:

 If your webapp is python module (where app is a Flask object inside root python file)


   `web: gunicorn -w 4 "facecount:app" --log-level=DEBUG`
 If your webapp is  python package ( with create_app() function returns app object in __init__.py )


   `web: gunicorn -w 4 "facecount:create_app()" --log-level=DEBUG`


- **Aptfile** is for including any external python library or package that is required by heroku, In my case it contains:


   ```
   libsm6
   libxrender1
   libfontconfig1
   libice6

   ```

- Initialise and commit changes to git
- push to the heroku remote directory using


 `git push heroku master`

There you have it, if everything goes fine your app should be live at the heroku server. If there is some error check using `heroku logs`