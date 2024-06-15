# Social-Network-Analysis

# Few-Shot Personalized Stable Diffusion

## Goal
Given a few images of an object, train Stable Diffusion for data synthesis and augmentation.
We want text-guided (conditioned) generation -> Multi-modal models / Language-Vision.


## Overview of Approaches

### Dreambooth : https://arxiv.org/abs/2208.12242
     
- Obtain a new set of parameters for the whole model.
  <br>  ++ Most accurate results <br>  -- Most expensive and time consuming, requires resources, returns large files
### Textual Inversion : https://arxiv.org/abs/2208.01618
- Obtain a text embedding that desctibes the new concept.
        <br>  ++ Very inexpensive, returns a simple text embedding, transferable
### LoRA: https://arxiv.org/abs/2106.09685
- Obtain weights for some intermediate layers we add, that are transferable from model to model.
      <br>  ++ Inexpensive, transferable

## Dreambooth

### Overview
Given ∼ 3−5 images of a subject we fine-tune a text-to-image diffusion model with the input images paired with a text prompt containing a unique identifier and the name of  the class the subject belongs to (e.g., “A [V] dog”), in parallel, we apply a class-specific prior preservation loss, which leverages the semantic prior that the model has on the class and encourages it to generate diverse instances belong to the subject’s class using the class name in a text prompt (e.g., “A dog”).

### Goal
Our goal is to “implant” a new (unique identifier, subject) pair
into the diffusion model’s “dictionary” . In order to bypass the overhead of writing detailed image descriptions for a given image set we opt for a simpler approach and label all input images of the subject “a [identifier [class noun]”, where [identifier] is a unique identifier linked to the subject and [class noun] is a coarse class descriptor of the subject (e.g. cat, dog, watch, etc.). ***Therefore we create an association between the new concept and the unique identifier.***

<img src="https://github.com/alexisfilippakopoulos/Social-Network-Analysis/assets/101940146/3aa1065d-737d-4e82-b032-8509f25a1d10" width="700" height="600">

### Methodology
We need a fixed prompt/sentence that uses the unique identifier token for the subject at hand and few images of said subject.
1. We apply n steps of noise to the training sample (image)

### Problems
- Dreambooth includes fine-tuning layers that are conditioned on the text embeddings, which gives rise to the problem of language drift, where a model that is pre-trained on a large text corpus and later fine-tuned for a specific task progressively loses syntactic and semantic knowledge of the language. As a result the model slowly forgets how to generate subjects of the same class as the target subject.
- Another problem is the possibility of reduced output diversity. Text-to-image diffusion models naturally posses high amounts of output diversity. When fine-tuning on a small set of images we would like to be able to generate the subject in novel viewpoints, poses and articulations. Yet, here is a risk of reducing the amount of variability in the output poses and views of the subject (e.g. snapping to the few-shot views). We observe that this is often the case, especially when the model is trained for too long.
### Solution
- To overcome these use prior preservation loss. In essence supervise the model with its own generated samples, in order for it to retain the prior once the few-shot fine-tuning begins. This allows it to generate diverse images of the class prior, as well as retain knowledge about the class prior that it can use in conjunction with knowledge about the subject instance.

### Applications
**Recontextualization** We can generate novel images for a specific subject in different contexts with descriptive prompts (“a [V] [class noun] [context description]”). Importantly, we are able to generate the subject in new poses and articulations, with previously unseen scene structure and realistic integration of the subject in the scene (e.g.contact, shadows, reflections).

<img src="https://github.com/alexisfilippakopoulos/Social-Network-Analysis/assets/101940146/5478d580-f351-4cb4-ad52-d30055ed2843" width="1200" height="500">

<img src="https://github.com/alexisfilippakopoulos/Social-Network-Analysis/assets/101940146/a35839a1-3cae-4122-a03b-53122164d019" width="1200" height="400">

<img src="https://github.com/alexisfilippakopoulos/Social-Network-Analysis/assets/101940146/7a06ffe8-6dc3-4d47-bb9b-e4d72d5f589c" width="1200" height="600">




## Textual Inversion

<img src="https://github.com/alexisfilippakopoulos/Social-Network-Analysis/assets/101940146/e3be02bd-709b-4cd6-a769-0a53b8238699" width="700" height="600">


## LoRA
