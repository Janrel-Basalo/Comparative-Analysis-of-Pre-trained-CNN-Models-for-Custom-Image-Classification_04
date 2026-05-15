# 🌿 Comparative Analysis of Pre-trained CNN Models for Custom Image Classification

> **Laboratory Work 5** — Deep Learning | Custom Aquatic Plant Image Classifier  
> 📓 [Open in Google Colab](https://colab.research.google.com/drive/1vN6ASuTvLtf8spOPU7T1k3ZgZZPiRNM7#scrollTo=p13-guide)


## GUIDE QUESTIONS — Final Reflection

---
### A. Model Performance

**1. Which pre-trained model achieved the highest accuracy? Why?**

> **MobileNetV2** achieved the highest validation accuracy of **93.89%**. This is because MobileNetV2 uses depthwise separable convolutions and an inverted residual structure, which are highly efficient at extracting meaningful features while avoiding overfitting on mid-sized datasets. Its lightweight design also benefits from faster convergence during fine-tuning.

**2. Which model had the lowest performance? What could be the reason?**

> **VGG16** had the lowest validation accuracy of **86.34%**. VGG16 has approximately 138M parameters (14.7M in the base), making it prone to overfitting when dataset size is limited and training epochs are short. Its deep sequential architecture without skip connections also makes gradient flow less efficient compared to ResNet or MobileNetV2.

**3. How did loss values compare across models?**

> MobileNetV2 achieved the lowest validation loss (0.1987), followed by ResNet50 (0.2879), then VGG16 (0.4312). All models showed decreasing loss over 10 epochs, but VGG16 had a noticeably higher gap between training and validation loss, suggesting mild overfitting.

---
### B. Evaluation Metrics

**4. Why is accuracy not enough to evaluate a model?**

> Accuracy can be misleading on imbalanced datasets. For example, if one class dominates with 50% of images, a model predicting only that class achieves 50% accuracy while failing all other classes. Precision, Recall, and F1-Score provide per-class insight and capture false positives and false negatives independently.

**5. Which model had the best F1-score? What does it indicate?**

> **MobileNetV2** had the best macro F1-score of **0.9392**, indicating it maintains a strong balance between Precision (0.9392) and Recall (0.9395) across all 20 aquatic plant classes. A high F1-score means the model reliably identifies true positives without generating too many false alarms.

**6. How did Precision and Recall differ across models?**

> All three models showed closely matched Precision and Recall values, indicating balanced prediction behavior. MobileNetV2 (P: 0.9392, R: 0.9395) outperformed ResNet50 (P: 0.9102, R: 0.9100) and VGG16 (P: 0.8599, R: 0.8607). No model showed a significant Precision-Recall tradeoff, suggesting the dataset is reasonably balanced.

---
### C. Confusion Matrix Analysis

**7. Which classes were frequently misclassified?**

> Classes with visual similarities such as **DwarfHairgrass**, **DwarfChainSword**, and **MarsileaMinuta** showed the highest off-diagonal counts, particularly in VGG16. These classes share thin, blade-like leaf structures making visual discrimination challenging without fine-grained feature extraction.

**8. What patterns did you observe in the confusion matrix?**

> The confusion matrices showed strong diagonal dominance in all three models, confirming high overall accuracy. Misclassifications were concentrated among visually similar plant species (e.g., fine-leaved vs. broad-leaved plants). MobileNetV2 had the most clearly defined diagonal with fewest off-diagonal entries, while VGG16 showed the most scattered misclassifications.

---
### D. ROC and AUC

**9. Which model had the highest AUC score?**

> **MobileNetV2** had the highest macro-average AUC score of **0.9963**, followed by ResNet50 (0.9941) and VGG16 (0.9898). All three models performed well above random (0.5), indicating strong class discrimination ability.

**10. What does AUC tell us about model performance?**

> AUC (Area Under the ROC Curve) measures the probability that the model ranks a random positive example higher than a random negative one. An AUC of 1.0 is perfect; 0.5 is random. Our models' AUC scores above 0.98 confirm excellent multi-class discrimination even for visually similar plant categories.

---
### E. Explainability (Grad-CAM)

**11. What did Grad-CAM reveal about model decision-making?**

> Grad-CAM heatmaps revealed that all three models primarily activated on the **leaf structure and texture regions** of aquatic plants rather than background water or substrate. This confirms the models learned biologically meaningful features rather than background artifacts.

**12. Did the model focus on relevant image regions?**

> Yes. The Grad-CAM overlays showed strong activation on the distinctive leaf shapes and branching patterns of the plants. VGG16 showed slightly more diffuse activation, while MobileNetV2 produced the most focused and precise heatmaps concentrated on discriminative leaf regions.

**13. Which model produced the most meaningful heatmaps?**

> **MobileNetV2** produced the most localized and semantically meaningful Grad-CAM heatmaps, with clear concentration on key plant features. ResNet50 was comparable, while VGG16 showed broader activation patterns that occasionally included non-plant regions.

---
### F. Model Comparison & Improvement

**14. Which model would you recommend for deployment? Why?**

> **MobileNetV2** is recommended for deployment. It achieved the highest accuracy (93.89%), best F1-score (0.9392), and highest AUC (0.9963), while having the smallest model size (2.26M parameters vs VGG16's 14.7M and ResNet50's 23.6M). Its lightweight architecture also enables faster inference on mobile and embedded devices, which is ideal for a field-deployable aquatic plant identification app.

**15. How can you further improve your best-performing model?**

> Further improvements include: (1) **Fine-tuning** — unfreezing the top layers of MobileNetV2 and training with a very low learning rate (1e-5); (2) **Data augmentation** — adding brightness, saturation, and elastic distortions to simulate underwater lighting variation; (3) **Learning rate scheduling** — using cosine annealing; (4) **Class-weighted loss** for classes with fewer samples; (5) **Ensemble** combining MobileNetV2 and ResNet50 predictions.

---
### G. Real-World Application

**16. How can your model be applied in real-world scenarios?**

> The model can be applied in: (1) **Aquarium hobbyist apps** — allowing users to photograph and identify aquatic plants for care instructions and purchasing; (2) **Environmental monitoring** — automated classification of aquatic vegetation in lakes and rivers for ecological research; (3) **Invasive species detection** — identifying harmful aquatic plants like Hydrilla Verticillata early to prevent spread.

**17. What are the risks of deploying an inaccurate model?**

> Risks include: misidentification of invasive species leading to inaction on environmental threats; incorrect plant care recommendations causing aquarium plant death; loss of user trust from repeated incorrect predictions. In conservation contexts, false negatives for invasive species detection could have significant ecological consequences. Continuous model monitoring, user feedback loops, and human expert review for low-confidence predictions are essential safeguards.

**18. How can this system be integrated into a mobile/web app?**

> The trained MobileNetV2 model can be exported using **TensorFlow Lite** (.tflite) for Android/iOS integration, enabling on-device inference without an internet connection. For a web app, the model can be served via a **FastAPI** REST endpoint: users upload an image, the server resizes it to 224x224, runs inference, and returns the top-3 predicted classes with confidence scores. A React or Flutter frontend displays results with plant care information.
