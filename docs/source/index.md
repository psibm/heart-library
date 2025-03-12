% HEART-library documentation master file, created by
% sphinx-quickstart on Wed Mar 27 15:42:18 2024.
% You can adapt this file completely to your liking, but it should at least
% contain the root `toctree` directive.

<!-- HEART-library Documentation
============ -->
<!-- # Welcome to HEART-library's documentation! -->

::::{grid} 2

:::{grid-item}
:child-align: center
HEART-library Documentation
===========================
**Hardened Extension of the Adversarial Robustness Toolkit (HEART)** is an open-source python library that provides AI developers and researchers with testing and evaluation (T&E) tools to assess AI model performance under adversarial attacks and improve model resiliency.  
```{button-link} https://example.com
:color: primary
:shadow:
Get Started with HEART  {octicon}`arrow-right`
```
:::

:::{grid-item}

```{image} _static/theme/SVG/Example-8.svg
:alt: Description of the image
```
:::
::::

<hr style="margin-bottom:60px;">

::::::{grid} 2

:::::{grid-item-card} Quick Start
::::{grid} 2
:::{grid-item}
:columns: 3
```{image} _static/theme/SVG/quick-start.svg
:alt: Description of the image
:class: only-light
```
```{image} _static/theme/SVG/quick-start-dark.svg
:alt: Description of the image
:class: only-dark
```
:::
:::{grid-item}
:columns: 9
Use these resources to get an introduction to HEART and install the library.
:::
:::{grid-item}
:class: home-tile-link-list
:columns: 12
- [Introduction to HEART](quick-start/intro-heart.md)
- [Installation and Setup](quick-start/install-heart.md)

```{button-link} https://example.com
See all {octicon}`arrow-right`
```
:::
::::
:::::

:::::{grid-item-card} Tutorials
::::{grid} 2
:::{grid-item}
:columns: 3
```{image} _static/theme/SVG/tutorial.svg
:alt: Description of the image
:class: only-light
```
```{image} _static/theme/SVG/tutorial-dark.svg
:alt: Description of the image
:class: only-dark
```
:::
:::{grid-item}
:columns: 9
If you are new to Adversarial AI, the tutorials will introduce you to key concepts and workflows
:::
:::{grid-item}
:class: home-tile-link-list
:columns: 12
- [Tutorial: Drone Object Detection](tutorials/index.md)
- [Tutorial: Dog and Cat High Five](tutorials/notebooks.md)

```{button-link} https://example.com
See all {octicon}`arrow-right`
```
:::
::::
:::::

::::::

::::::{grid} 2

:::::{grid-item-card} How-to Guides
::::{grid} 2
:::{grid-item}
:columns: 3
```{image} _static/theme/SVG/How-to.svg
:alt: Description of the image
:class: only-light
```
```{image} _static/theme/SVG/How-to-dark.svg
:alt: Description of the image
:class: only-dark
```
:::
:::{grid-item}
:columns: 9
If you are familiar with Adversarial AI and know what you want to do, the how-to guides will show you step-by-step how to do it with HEART tools
:::
:::{grid-item}
:class: home-tile-link-list
:columns: 12
- [How to Simulate White Box Attacks](tutorials/index.md)
- [How to Simulate Black Box Attacks](tutorials/notebooks.md)
- [How to Simulate Auto Attacks](tutorials/notebooks.md)

```{button-link} https://example.com
See all {octicon}`arrow-right`
```
:::
::::
:::::

:::::{grid-item-card} Explanations
::::{grid} 2
:::{grid-item}
:columns: 3
```{image} _static/theme/SVG/explanation.svg
:alt: Description of the image
:class: only-light
```
```{image} _static/theme/SVG/explanation-dark.svg
:alt: Description of the image
:class: only-dark
```
:::
:::{grid-item}
:columns: 9
The explanations provide in-depth descriptions of relevant technical concepts
:::
:::{grid-item}
:class: home-tile-link-list
:columns: 12
- [Overview - Creating Adversarial Patches](tutorials/index.md)
- [Common Use Cases: Object Detection](tutorials/notebooks.md)
- [Common Use Cases: Image Classification](tutorials/notebooks.md)

```{button-link} https://example.com
See all {octicon}`arrow-right`
```
:::
::::
:::::

::::::

::::::{grid} 2

:::::{grid-item-card} References
::::{grid} 2
:::{grid-item}
:columns: 3
```{image} _static/theme/SVG/reference.svg
:alt: Description of the image
:class: only-light
```
```{image} _static/theme/SVG/reference-dark.svg
:alt: Description of the image
:class: only-dark
```
:::
:::{grid-item}
:columns: 9
References provide further resources for your understanding of both Adversarial AI concepts and HEART tools 
:::
:::{grid-item}
:class: home-tile-link-list
:columns: 12
- [Attack Cards](tutorials/index.md)
- [Evaluation Pathways](tutorials/notebooks.md)

```{button-link} https://example.com
See all {octicon}`arrow-right`
```
:::
::::
:::::

:::::{grid-item-card} About HEART
::::{grid} 2
:::{grid-item}
:columns: 3
```{image} _static/theme/SVG/about.svg
:alt: Description of the image
:class: only-light
```
```{image} _static/theme/SVG/about-dark.svg
:alt: Description of the image
:class: only-dark
```
:::
:::{grid-item}
:columns: 9
Learn more about HEART, its background, mission, and contributors 
:::
:::{grid-item}
:class: home-tile-link-list
:columns: 12
- [Background and Mission](about/background.md)
- [Latest Updates](about/latest-updates.md)
- [Contributors](about/contributors.md)

```{button-link} https://example.com
See all {octicon}`arrow-right`
```
:::
::::
:::::


::::::

<!-- HEART is a Python extension library for Machine Learning Security that builds on the popular Adversarial Robustness algorithms in [ART](https://github.com/Trusted-AI/adversarial-robustness-toolbox).

The extension library is operation-ready and tailored for real-world DoD use cases, offering essential adversarial robustness methods within the three evaluation tool dimensions: physical realizability, perturbation type, black/white box. HEART allows the user to leverage core ART algorithms, while providing additional benefits to the AI Test & Evaluation (T&E) engineer:

- Support for T&E of models for DoD use cases (developers, researchers and evaluators focused on adversarial machine learning capabilities)
- Alignment to MAITE protocols to access this subset of ART and other JATIC tools for seamless T&E workflows
- Essential subset of adversarial robustness methods for targeted AI security coverage
- Assessment quality assurance in the form of metadata
- In-depth support for users in the form of guides and examples
- Front-end application for low-code users: [HEART Gradio Application](https://huggingface.co/spaces/CDAO/HEART-Gradio)

Our extension is operation-ready for real-world DoD use cases, offering essential AR methods within the three evaluation tool dimensions: physical realizability, perturbation type, black/white box. -->

```{toctree}
:caption: 'Quick Start'
:maxdepth: 2
:hidden:

   Introduction to HEART <quick-start/intro-heart>
   Installation and Setup <quick-start/install-heart>
   <!-- Latest Updates <about/latest-updates> -->
```

```{toctree}
:caption: 'Documentation'
:maxdepth: 3
:hidden:

   Tutorials <tutorials/index>
   How-to Guides <how_to_guides/index>
   Explanations <explanations/index>
   Reference Materials <reference_materials/index>
```

```{toctree}
:caption: 'About HEART'
:maxdepth: 2
:hidden:

   Background and Mission <about/background>
   Latest Updates <about/news>
   Contributors <about/contributors>
```

<!-- ```{toctree}
:caption: 'Contents:'
:maxdepth: 4

setup_and_access
how_to_guides/index
tutorials/index
explanations/index
reference_materials/index
modules/index
glossary/index
legal
``` -->

<!-- ## Additional Resources

- {ref}`genindex`
- {ref}`modindex`
- {ref}`search` -->