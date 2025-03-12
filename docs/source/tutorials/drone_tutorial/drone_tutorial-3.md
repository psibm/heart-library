Drone Tutorial - Part 3: Defending the Model and Advanced Attacks
============

```{admonition} Reminder: Download Notebook
:class: important

Remember to download the associated Jupyter notebook to follow along and try the code yourself.  It is located [here](#)
``` 

## Introduction

::::{grid} 2

:::{grid-item}
:columns: 9
Welcome to Part 3 of the Drone Object Detection Tutorial for HEART.  In Part 1, you were introduced to Amelia and her use case for HEART, the requirements she needs to meet to use HEART, and how to install HEART and the relevant data set.  In Part 2, you used HEART to attack the drone object detection model and see how it was vulnerable to PGD attacks.

In Part 3, you will learn how to use various techniques to **defend** the model from attacks and apply advanced attack techniques.



:::

:::{grid-item}
:columns: 3

```{image} /_static/tutorial-drone/ai-defense.jpg
:alt: Amelia
```

:::

::::


## Defending the Model

The first defense method Amelia will use is **JPEG compression**.  JPEG compression can help to defend against adversarial attacks by "compressing away" visually imperceptible pixel manipulations that are often used in adversarial attacks.  This makes these types of attacks less effective when the image is fed into an object detector.

```python
preprocessing_defense = JpegCompression(clip_values=(0,1),
                                        channels_first=True,
                                        apply_predict=True,
                                        quality=25
                                        )

detector_defended = JaticPyTorchObjectDetector(model_type="detr_resnet50",
                                                device_type='cpu',
                                                input_shape=(3, 800, 800),
                                                clip_values=(0, 1), 
                                                attack_losses=( "loss_ce",
                                                    "loss_bbox",
                                                    "loss_giou",), 
                                                preprocessing=(MEAN, STD),
                                                preprocessing_defences=[preprocessing_defense])

'''
View detections on adversarial images
'''
adv_detections = detector_defended(x_advPGD)
for i in range(2): #to see all, use range(len(adv_detections)): 
    preds_orig = extract_predictions(adv_detections[i], 0.5)
    plot_image_with_boxes(img=x_advPGD[i].transpose(1,2,0).copy(),
                          boxes=preds_orig[1], pred_cls=preds_orig[0], title="Detections")
```




```{admonition} 
:class: note
Using cache found in /Users/kgr/.cache/torch/hub/facebookresearch_detr_main

```

:::: {grid} 2
::: {grid-item} 
```{image} /_static/tutorial-drone/dt-p3-1.png
:alt: Part 1 - Image 1
```
:::

::: {grid-item}
Description of what is in this image
:::
::::

:::: {grid} 2
::: {grid-item} 
```{image} /_static/tutorial-drone/dt-p3-2.png
:alt: Part 1 - Image 2
```
:::

::: {grid-item}
Description of what is in this image
:::
::::

:::: {grid} 2
::: {grid-item} 
```{image} /_static/tutorial-drone/dt-p3-3.png
:alt: Part 1 - Image 2
```
:::

::: {grid-item}
Description of what is in this image
:::
::::

Amelia notices that the performance of the model is indeed a little better. She will now further evaluate this by computing the mean average precision metric from above.  Ideally, she would here simply compute an attack on the full model, but this is currently note possible as the gradients do not propagate back through the compression.

```python
map_args = {"box_format": "xyxy",
            "iou_type": "bbox",
            "iou_thresholds": [0.5],
            "rec_thresholds": [0.0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0],
            "max_detection_thresholds": [1, 10, 100],
            "class_metrics": False,
            "extended_summary": False,
            "average": "macro"}

attack = JaticAttack(ProjectedGradientDescent(detector, max_iter=1, eps_step=0.01, eps=0.03, targeted=False, verbose=False), norm=2)

data_with_detections = ImageDataset(sample_data, deepcopy(detections), threshold=0.9)

assert isinstance(data_with_detections, od_dataset)

isinstance(attack, Augmentation)

metric = HeartMAPMetric(**map_args)

check_type('attack', attack, Augmentation)

results, _, _ = evaluate(
    model=detector_defended, 
    dataset=data_with_detections,
    metric=metric,
    augmentation=attack,
)


print('PGD evaluation:')
pprint(results)
```

### Defending the Model Wrap Up

This is where a wrap up section will go.

## Advanced Attacks

### Adaptive Patch Attack

The previous PGD attack is rather easy to defend with JPEG compression. The perturbations added are small pixel changes to the entire image, which are easily removed by the compression. 

Next, Amelia will use an **Adaptive Patch Attack** to work around this type of defense.  An adaptive attack exploits knowledge about the defense.  Amelia will use a patch attack. The patch is a strong, local perturbation, that is limited to a small area of the image. The JPEG compression is thus less likely to alter the perturbation strongly.

```{admonition} Patch Attack Method
:class: seealso

Definition: A patch attack, in the context of computer vision, is a type of adversarial attack where an attacker creates a specific, often small, patch (like a sticker) that, when placed on an image, can cause a machine learning model to misclassify the image.  You can learn more about Patch attacks [here](#).
```

```python
##### repeat with patch attack
detections = detector(sample_data)
targeted_data = TargetedImageDataset(sample_data, deepcopy(detections), NUM_SAMPLES)

assert isinstance(targeted_data, od_dataset)

targeted_data = torch.utils.data.Subset(targeted_data, list(range(1)))


rotation_max=0.0
scale_min=0.5
scale_max=1.0
distortion_scale_max=0.0
learning_rate=0.9
max_iter=50
batch_size=16
patch_shape=(3, 50, 50)
patch_location=(20,20)
patch_type="circle"
optimizer="Adam"

patchAttack = JaticAttack(
    AdversarialPatchPyTorch(detector, rotation_max=rotation_max, 
                      scale_min=scale_min, scale_max=scale_max, optimizer=optimizer, distortion_scale_max=distortion_scale_max,
                      learning_rate=learning_rate, max_iter=max_iter, batch_size=batch_size, patch_location=patch_location,
                      patch_shape=patch_shape, patch_type=patch_type, verbose=True, targeted=True)
    )

#Generate adversarial images
px_adv, y, metadata = patchAttack(data=targeted_data)

patch = metadata[0]["patch"]
patch_mask = metadata[0]["mask"]

#plot patch
plt.axis("off")
plt.imshow(((patch) * patch_mask).transpose(1,2,0))
_ = plt.title('Generated Adversarial Patch')
plt.show()
print('-------------')

for i in range(len(px_adv)):
    preds_orig = extract_predictions(detections[i], 0.5)
    img = np.asarray(px_adv[i].transpose(1,2,0))
    plot_image_with_boxes(img=img.copy(), boxes=preds_orig[1], pred_cls=preds_orig[0], title="Detections")
```

### Image Outputs

:::: {grid} 2
::: {grid-item} 
```{image} /_static/tutorial-drone/dt-p3-4.png
:alt: Part 1 - Image 1
```
:::

::: {grid-item}
Description of what is in this image
:::
::::

:::: {grid} 2
::: {grid-item} 
```{image} /_static/tutorial-drone/dt-p3-5.png
:alt: Part 1 - Image 2
```
:::

::: {grid-item}
Description of what is in this image
:::
::::



As expected, Amelia sees little difference between the original, undefended and the defended model, highliting the need for good T&E once more. 


<hr style="margin-bottom:60px;">

## You Completed Part 3!

Congratulations!  You completed Part 3 of the Drone Object Detection Tutorial.  So far, in Part 1 you have learned the background and value of HEART, the requirements and prerequisites to use it, how to load the necessary libraries, and how to load the necessary data and object detection model.  In Part 2, you learned how to create both PGD and Patch Attacks for the model.  You also learned how to interpret the outputs.

Now, in Part 3, you learned how to Defend the model and how to create an Adaptive Attack.

Next, in Part 4 we will conclude and summarize everything you learned from the tutorial.  We will see you in Part 4 of the tutorial.

::::{grid} 2

:::{grid-item}
:columns: 4

:::

:::{grid-item}
:child-align: end
:columns: 4
```{button-ref} drone_tutorial-2
:color: primary
:expand:
:outline:
:ref-type: doc
Back to Part 2
:::

:::{grid-item}
:child-align: end
:columns: 4
```{button-ref} drone_tutorial-4
:color: primary
:expand:
:ref-type: doc
Go to Part 4
:::

::::