222022-10-27

Style: 

Domain:

Tags:

# Explainable AI for medical Images

Most of what goes by the name of Artificial Intelligence (AI) today is actually based on training and deploying Deep Learning (DL) models. Despite their impressive achievements in fields as diverse as image classification, language translation, complex games (such as Go and chess), speech recognition, and self-driving vehicles, DL models are inherently opaque and unable to explain their predictions, decisions, and actions.

The emerging field of XAI (eXplainable Artificial Intelligence) addresses this need and offers several methods to provide some level of explanation to deep learning AI solutions.

In this blog post, we focus primarily on the human factors of XAI**:** What needs to be explained and how can this be communicated to the (human) user of an AI solution?

![[Pasted image 20221027152633.png]]

## Human Factor

. In order to understand the _why_ and _how_ behind an AI model's decisions and get a better insight into its successes and failures, an explainable model must explain itself to a human user through some type of _explanation interface_. Ideally such interface should be rich, interactive, intuitive, and appropriate for the user and task

In image classification, a common explainability interface consists of overlaying the important information onto the image, to show witch areas fo the image were most important for the classification outcome. It can also assist in diagnosing potential blunders that the DL model might be making, producing results that are seemingly correct but reveal that the model was looking at the wrong places

![[Pasted image 20221027164218.png]]

We choose to see [explainability as a gradual process](https://pubs.rsna.org/doi/full/10.1148/ryai.2020190043) instead (Figure 2), where an AI system that predicts a medical condition from a patient's chest x-ray might use gradually increasing degrees of explainability: (1) **no explainability information**, just the outcome/prediction; (2) adding **output probabilities** for most likely predictions, giving a measure of confidence associated with them; (3) adding **visual saliency** information describing areas of the image driving the prediction; (4) combining predictions with results from a medical case retrieval (MCR) system and indicating **matched real cases** that could have influenced the prediction; and (5) adding computer-generated **semantic explanation**.

### Popular XAI Methods
There is no universally accepted taxonomy of XAI techniques. In this blog post we use the categorization of the field adopted by the authors of [a recent survey paper on the topic](https://arxiv.org/abs/2006.11371), which breaks down the field into _scope_, _methodology_ and _usage_, as follows:

-   **Scope**: Where is the XAI method focusing on?
    -   The scope could be **local** (where the focus is on individual data instances) or **global** (where the emphasis is on trying to understand the model as a whole).
-   **Methodology**: What is the algorithmic approach?
    -   The algorithm could focus on: (a) gradients back-propagated from the output prediction layer to the input layer; or (b) random or carefully chosen changes to features in the input data instance, also known as _perturbations_.
-   **Usage**: How is the XAI method developed?
    -   Explainability might be **intrinsic** to the neural network architecture itself (which is often referred to as _interpretability_) or **post-hoc**, where the algorithm is applied to already trained networks.

Some post-hoc XAI algorithms have become popular in recent years, among them [Grad-CAM](https://doi.org/10.1109/ICCV.2017.74) (Gradient-weighted Class Activation Mapping) and [LIME](https://arxiv.org/abs/1602.04938) (Local Interpretable Model-agnostic Explanation). Grad-CAM is gradient-based, has local scope, and can be used for computer vision tasks. LIME is perturbation-based, has both local and global scope, and can be used with images, text, or tabular data.


# Key takeaways

Deep Learning models are opaque and, consequently, have been deemed inadequate for several applications (since usually they can make predictions without explaining how they arrived at the result, or which factors had greater impact). Explainable AI (XAI) techniques are possible ways to inspire trust in an AI solution.

In this blog post we have used MATLAB to show how post-hoc explanation techniques can be used to show which parts of an image were deemed most important for medical image classification tasks. These techniques might be useful beyond the explanation of correct decisions, since they also help us identify blunders, i.e., cases where the model learned the wrong aspects of the images. 

XAI is an active research area, and new techniques and paradigms are likely to emerge in the near future. If youre interested in learning more about XAI and related issues, see the recommended resources below.



___
# References
[Explainable AI for Medical Images - DataScienceCentral.com](https://www.datasciencecentral.com/explainable-ai-for-medical-images/?utm_source=pocket_mylist)