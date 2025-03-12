Drone Tutorial - Part 2: Attacking the Model
============

```{admonition} Reminder: Download Notebook
:class: important

Remember to download the associated Jupyter notebook to follow along and try the code yourself.  It is located [here](#)
``` 

## Introduction

::::{grid} 2

:::{grid-item}
:columns: 6
Welcome to Part 2 of the Drone Object Detection Tutorial for HEART.  In Part 1, you were introduced to Amelia and her use case for HEART, the requirements she needs to meet to use HEART, and how to install HEART and the relevant data set.



:::

:::{grid-item}
:columns: 6


In Part 2, you will following Amelia as she uses HEART to attack the drone object detection model with HEART.  This will inform Amelia on what vulnerabilities may exist in the model.  It will also set up Part 3 where Amelia will work to **defend** the model.

:::

::::

```{image} /_static/tutorial-drone/neural-network-wide.jpg
:alt: Amelia
```

## Determining Threat Vectors

As Amelia delves deeper into exploring HEART for their drone object detection project, they begin to identify potential threat vectors that could compromise the system's integrity. One such vector that catches their attention is the **Projected Gradient Descent (PGD) attack method.**

::::{grid} 2

:::{grid-item}
:columns: 6
```{admonition} Projected Gradient Descent (PGD) Attack Method
:class: seealso

Definition: A projected gradient descent (PGD) attack is a method used in adversarial machine learning to generate adversarial examples that can fool deep learning models by iteratively perturbing the input data, guided by the model's gradients, while keeping the perturbations within a defined constraint.  You can learn more about PGD attacks [here](#)
```
:::

:::{grid-item}
:columns: 6

```{image} /_static/tutorial-drone/pgd-attack.png
:alt: PGD Attack
```

:::

::::




Amelia starts by thoroughly reviewing HEART's documentation, focusing on the various attack techniques available. Intrigued by PGD's reputation as a powerful white-box attack method, they decide to investigate its potential implications for their use case.  Amelia considers several threat scenarios:


::::{grid} 3

:::{grid-item-card} Image Perturbations
An adversary could introduce nearly imperceptible perturbations into input images, attempting to mislead the model into misclassifying objects or overlooking threats.
:::

:::{grid-item-card} Evasion Attacks
Evasion attacks can take the form of adversarial patches or stickers, which, when applied to objects like vehicles or weapons, might make them disappear to the model. This could enable adversaries to hide threats or impersonate friendly forces.
:::

:::{grid-item-card} Adaptive Attacks
Attacks are generally independent from the model, although they often rely on the model’s specific gradients. However, in the case of defenses or very particular models, it may be necessary to tailor an attack to model specifics, for example a rejection threshold. Such attacks are called adaptive.
:::

::::


Amelia decides that using a PGD to attack her project's model with HEART is a great way to determine if the model is vulnerable to these types of common and sophisticated attacks. She decides that it would be good to use the PDG attack methods with HEART to test her model since it is a very common form of attack.  This should give her and her team a very good initial understanding of the model's adversarial robustness and vulnerabilities.




## Creating Adversarial Examples with HEART

First, Amelia will define the attack, projected gradient descent (PGD), which implements local changes. For PGD, she runs the attack in a MAITE evaluation that returns relevant statistics about the models performance, and then plot the images for visual inspection. To understand the differences, she only plots the patch attack images for review.

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
    model=detector, 
    dataset=data_with_detections,
    metric=metric,
    augmentation=attack,
)


print('PGD evaluation:')
pprint(results)
```
<!-- 
```
0%|          | 0/5 [00:00<?, ?it/s]

PGD evaluation:
{'classes': tensor([ 1,  2,  3,  4,  6,  7,  8,  9, 10, 14, 15, 27, 28, 56, 64], dtype=torch.int32),
 'map': tensor(0.08463),
 'map_50': tensor(0.08463),
 'map_75': tensor(-1.),
 'map_large': tensor(-1.),
 'map_medium': tensor(0.15476),
 'map_per_class': tensor(-1.),
 'map_small': tensor(0.14274),
 'mar_1': tensor(0.03261),
 'mar_10': tensor(0.13354),
 'mar_100': tensor(0.19876),
 'mar_100_per_class': tensor(-1.),
 'mar_large': tensor(-1.),
 'mar_medium': tensor(0.26923),
 'mar_small': tensor(0.31429)}
``` -->

```python
x_advPGD, y, metadata = attack(data=sample_data)
detections = detector(np.stack(x_advPGD))

for i in range(2): #for all, write range(len(x_adv)):
    preds_orig = extract_predictions(detections[i], 0.5)
    img = np.asarray(x_advPGD[i].transpose(1,2,0))
    plot_image_with_boxes(img=img.copy(), boxes=preds_orig[1], pred_cls=preds_orig[0], title="Detections")
```
## Produce Results and Comparison Metrics with HEART

### Image Outputs

:::: {grid} 2
::: {grid-item} 
```{image} /_static/tutorial-drone/dt-p2-1.png
:alt: Part 1 - Image 1
```
:::

::: {grid-item}
Description of what is in this image
:::
::::

:::: {grid} 2
::: {grid-item} 
```{image} /_static/tutorial-drone/dt-p2-2.png
:alt: Part 1 - Image 2
```
:::

::: {grid-item}
Description of what is in this image
:::
::::

<hr style="margin-bottom:60px;">

## You Completed Part 2!

Congratulations!  You completed Part 2 of the Drone Object Detection Tutorial.  So far, in Part 1 you have learned the background and value of HEART, the requirements and prerequisites to use it, how to load the necessary libraries, and how to load the necessary data and object detection model.

Now, in Part 2 you learned how to create both PGD and Patch Attacks for the model.  You also learned how to interpret the outputs

Next, you will use learn how to create defenses to defend the model against these types of attacks.  We will see you in Part 3 of the tutorial.

::::{grid} 2

:::{grid-item}
:columns: 4

:::

:::{grid-item}
:child-align: end
:columns: 4
```{button-ref} drone_tutorial-1
:color: primary
:expand:
:outline:
:ref-type: doc
Back to Part 1
:::

:::{grid-item}
:child-align: end
:columns: 4
```{button-ref} drone_tutorial-3
:color: primary
:expand:
:ref-type: doc
Go to Part 3
:::

::::