# APS360_SRCNN_AI_Upscaler

This is an AI Image upscaler for a Deep Learning course at the University of Toronto which is trained to upscale bird images from low to high resolution using an SRCNN archiecture.
The Model was trained on approximately 5000 Bird Images which was benchmaked against an SRGAN model as well as qualitativley assesed against a CNN discriminator. 

The following Image is a depiction of the implemented architecture for the Baseline SRGAN model:
![Screenshot (109)](https://user-images.githubusercontent.com/79722816/174502837-d59bc1ce-dd79-4022-bbdb-4d0c5c872562.png)

In order to benchmark our SRCNN performance, we decided to implement a known SRGAN architecture as shown in this slide. This model takes in low resolution images, adds in noise and up samples a generated image while also feeding into a discriminator that compares the fake and high resolution images which is then used to improve the generator.

The Following is a visual depiction of our primary model's SRCNN architecture:
![Screenshot (111)](https://user-images.githubusercontent.com/79722816/174502969-e04ed2bf-eb7f-4e9f-a92d-1052244659de.png)

The SRCNN architecture, as displayed in the figure above, has three main components or layers. The first of which is the patch extraction layer, which uses a 9 by 9 kernel to derive patches and create a feature map of the original image. Next, the patches are mapped to higher dimensions in a second feature map by a 5 by 5 kernel, where each n-dimensional patch is enlarged to an m-dimensional patch. These new m-dimensional patches are then applied to the original n-dimensional feature map to create a increased resolution representation of the original image. Finally, in the reconstruction layer, sets of overlapping patches are taken and averaged to produce the final high resolution output.


The Following are comparison results of an inputted low resolution bird image and AI upscaled image. 

![Screenshot (107)](https://user-images.githubusercontent.com/79722816/174502653-5496eae8-6ede-4f53-8ef0-33a0407f6a4c.png)
![Screenshot (108)](https://user-images.githubusercontent.com/79722816/174502731-4702f3dd-d60f-4073-876f-c0f2b0cfcb81.png)

The following graph depicts the quantiatvie results of our model training results which maps the MSE loss of upscaled images against high resolution images using a CNN decriminator:
![Picture1](https://user-images.githubusercontent.com/79722816/174503124-9cd2f3f8-73f4-48af-a185-2134902edadc.png)

Based on the graph, 0.001 loss in training vs 0.0982 loss is an indication that the model is overfit so adding more samples would have helped, however, this was not computationally feasible given the team's GPU time constraints.

The loss function used in our SRCNN model is mean squared error, which calculates the difference of each pixel value in the original high res image, with the model generated image. Despite the fact that this loss is less than 1%, after simply looking at the images we generated it was clear that they were not sharper than the originals. This is because MSE averages the error, and will make the output blurry bc it will optimize for the average rather than each pixel. 

To accommodate for the fact that a simple MSE loss would not suffice as a measure of how well the model did, we created an additional classifier model, whose task was to differentiate between original HR and model generated images. Similar to a GAN, it is positive if this classifier performs poorly as it means that we are generating images very similar to the originals. 

The classifier was then tested on two datasets, one with SRCNN generated images and one with GAN generated images.
Due to the poorer test score from the SRCNN image data, it seems that the classifier has a harder time distinguishing between real HR images and SRCNN generated ones, than it does with GAN generated ones.

Visually, it seems the trade of between the benchmark and SRCNN is that the SRCNN makes smoother images, while the GAN increases contrast and makes details more vivid. 
![Screenshot (112)](https://user-images.githubusercontent.com/79722816/174503249-9027a0f6-e7f2-403d-abf1-2cf2d795c299.png)





