# Deep Learning Project Repor

Date Created: April 22, 2021 9:54 PM
Status: Done 🙌

# Photographer editing style learning

## 1. Introduction

Photographers have their own shooting and editing styles. Also editing is big part of their jobs routinely. Here I proposed a method by using Deep Neutral Network to learning the editing style from a certain photographer. The model will work as the automated photo editing tools for photographers. Freelancer photographers usually use more than half of their working time on photo editing or so called "photo retouch". It's hard to image that taking photos is just a small portion of their daily work as a freelancer photographer. Editing also makes big difference with final products, see examples below:

![https://github.com/ChengpingYuan/PhotoEditingStyleLearning/blob/main/pic/DSC09738.jpg](https://github.com/ChengpingYuan/PhotoEditingStyleLearning/blob/main/pic/DSC09738.jpg)

Raw photo

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/DSC02819.jpg](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/DSC02819.jpg)

Raw photo

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/DSC09738%201.jpg](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/DSC09738%201.jpg)

Edited photo

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/DSC02819%201.jpg](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/DSC02819%201.jpg)

Edited photo

Every photo will be edited separately because photos are took in different light conditions and with different color schemes. Thus, editing requires skills to deal with different scenarios stated above. However, there is still a editing style with one photographer if you browse a lot of their works. So this project is aim to make the editing process automatically by learning the editing style from photographer. 

## 2. Method

### 2.1 Data Collection

Data collection is very straightforward. I collected 60 high resolution raw photos and edited photos from one photographer. Each photo with 7681x5123 resolution will be crop into 100x100 patches. So I got total 120,000 patches and divide them with 80% training and 20% testing for training and testing purpose. I hold all the copy right of the photos I used in this project. 

### 2.2 Loss Function

The method was inspired by this paper:

### [DSLR-Quality Photos on Mobile Devices with Deep Convolutional Networks](https://arxiv.org/pdf/1704.02470.pdf)

Although their model are used to enhance mobile photos to DSLR camera photos but we shared some common element of learning. We both use DL to train a model to learning a translation function, by which the photo will have same content but different color and texture (explained in details with the paper above). Thus, there are three parts in the Total loss function: 

- Color loss:  To measure the color difference between the raw and edited photos.
- Texture loss: GANs will be used here. It observes both raw and edited image, and "its goal is to predict whether the input image is real or not."
- Content loss:  A pre-trained VGG-19 model will be used. "Instead of measuring per-pixel difference between the images, this loss encourages them to have similar feature representation that comprises various aspects of their content and perceptual quality."

### 2.3 Data Preprocessing

The photos were captured using DSLR and saved in RAW format. Python standard library does not support RAW format processing so the first step was converting them to PNG format. After that, crop the 60 high-resolution photo into 120,000 patches. This process took about 45 minutes to finish. Then I divided them into 80% training and 20% testing.

## 3. Experiments and Results

The default hyper parameters in cited paper were:

- Texture loss weight: 1
- Color loss weight: 0.5
- Content loss weight: 10
- Batch size: 50
- Train iterations: 20, 000

Using this default setting, training one model of 20,000 iterations took me about 10 hours in a GTX 2080 video card. However, the result on this default setting are only acceptable not good enough. The color was over exposed, see example:

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/sony_DSC02852_iteration_29000_before_after.png](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/sony_DSC02852_iteration_29000_before_after.png)

Left: Raw photo. Right: Model processed photo

So I started to adjust loss weight between color and content loss. I also increased training iterations to 40,000. The best model was trained with parameter:

- Texture loss weight: 0.8
- Color loss weight: 0.6
- Content loss weight: 10
- Batch size: 50
- Train iterations: 40, 000

This model took me 15 hour to train. After this, I realize there is no way I can improve this during such short time with my limited computational resource. Below is the loss plots with the best model:

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled.png](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled.png)

Train Loss

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%201.png](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%201.png)

Content Loss

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%202.png](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%202.png)

Test Loss

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%203.png](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%203.png)

Color Loss

As you can see, the content loss and training loss seems can continue to decrease after 40,000 iterations but test loss and color loss had no improvement after 2000 iterations. Now let's review the product of our best model. First, I use

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/sony_DSC04138_iteration_29000_before_after.png](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/sony_DSC04138_iteration_29000_before_after.png)

Left: Raw photo. Right: Model processed photo

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/sony_DSC04169_iteration_29000_before_after.png](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/sony_DSC04169_iteration_29000_before_after.png)

Left: Raw photo. Right: Model processed photo

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/sony_DSC07510_iteration_38000_before_after.png](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/sony_DSC07510_iteration_38000_before_after.png)

Left: Raw photo. Right: Model processed photo

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/sony_DSC09703_iteration_38000_before_after.png](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/sony_DSC09703_iteration_38000_before_after.png)

Left: Raw photo. Right: Model processed photo

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/sony_DSC02867_iteration_29000_before_after.png](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/sony_DSC02867_iteration_29000_before_after.png)

Left: Raw photo. Right: Model processed photo

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/sony_DSC06677_iteration_29000_before_after.png](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/sony_DSC06677_iteration_29000_before_after.png)

Left: Raw photo. Right: Model processed photo

The evaluation of this result are total subjective, in my opinion, the model learned edit style of the photographer (who took and edit the datasets). However, the style was not perfect, especially when dealing with red channels, all the red color seems to be over exaggerated. Then comparison between human edited photo and model edited have shown this:

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%204.png](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%204.png)

Left: Model edited / Right: Human edited

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%205.png](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%205.png)

Left: Model edited / Right: Human edited

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%206.png](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%206.png)

Left: Model edited / Right: Human edited

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%207.png](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%207.png)

Left: Model edited / Right: Human edited

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%208.png](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%208.png)

Left: Model edited / Right: Human edited

![Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%209.png](Deep%20Learning%20Project%20Repor%200bd5a1c483b14c2c98586ab3f70ab56a/Untitled%209.png)

Left: Model edited / Right: Human edited

Overall, compared to raw images, my model can provide pretty decent editing but still sub-optimal compare to human edited photos.

## 4. Future improvement

If I have more to improve this project, I think I need to increase the variance of my training data first. Because there are only 60 different photos in my dataset, even though these high resolution photos can provide decent amount of patches for training, its still only covered very little scenarios, for example, different light condition, indoor or outdoor, portrait or landscape photos.

Second, training more iterations and tuning parameters will be worth the time consider the color loss can be further decreased over time.

At last, simplify the proposed model can not only help on reduce the training time but can be helpful give a better result since I notice the this model might be overfitted based on the loss values.
