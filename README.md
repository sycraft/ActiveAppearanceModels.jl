# Active Appearance Models

Port of Luca Vezzaro's [ICAAM](http://www.mathworks.com/matlabcentral/fileexchange/32704-icaam-inverse-compositional-active-appearance-models).

## Introduction

Active appearance models provide a way to find a set of related points on an image. AAMs are based on 2 main concepts: shape and appearance. 

**Shape** consists of a fixed number of points (so-called landmarks) that describe configuration of some object on an image. For example, here's a shape describing some human's face:

![](...)

**Appearance** is made of all pixels on the image inside the shape. E.g. appearance, corresponding to the shape above looks like this: 

![](...)

Active appearance models are first trained on a bunch of `(image, shape)` pairs	and then, given a new image and initial guess for a shape, are fitted to this image to find exact location of landmarks. Let's take a concrete example. 

First, we need some data to train a model on. `FaceDatasets` package contains a simple dataset from original research by Tim Cootes: 

    using FaceDatasets
    imgs = load_images(:cootes)
    shapes = load_shapes(:cootes)
   
We will use simple leave-one-one cross-validation to see how training and testing works:

    tst = 6                                      # index of a test image
    all_but_tst = [1:tst-1, tst+1:length(imgs)]  # all other indexes

Training is simple: 
    
    using ActiveAppearanceModels
    aam = AAModel()
    train(aam, imgs[all_but_tst], shapes[all_but_tst])

Fitting model to a new image requires 2 more parameters: initial shape and number of iterations: 

    init_shape = shapes[3]
    n_iter = 20

Before fitting let's see `init_shape` position:

    viewshape(imgs[tst], init_shape)    

![]()


Fitting is straightforward too:

    fitted_shape, fitted_app = fit(aam, imgs[tst], init_shape, n_iter)

And here's the result: 

    using ImageView
    viewshape(imgs[tst], fitted_shape)
    view(fitted_app)

![]()