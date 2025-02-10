# I2H - Image-to-HTML Generation

**Initial Approach: CLIP + T5**

The initial model design leveraged the CLIP (Contrastive Language-Image Pretraining) model for robust image feature extraction, paired with the T5 (Text-to-Text Transfer Transformer) model for HTML generation. The rationale behind this approach was:
- **CLIP:** Provided strong visual understanding with text-aligned features, which enhanced HTML structure generation.
- **T5:** A powerful generative model that performed well in converting extracted features into structured HTML code.

However, this approach required significant computational resources due to:
- CLIP’s high-dimensional image embeddings, lead to large memory consumption.
- T5’s heavy decoder layers, require substantial GPU power for training and inference.
- The overall pipeline is slow and hardware-intensive for real-time applications.

**Newer Approach: MobileNetV3 + DistilBART**

Due to hardware limitations, the model was redesigned to be lightweight while maintaining performance:
- **MobileNetV3 (Small):** A more efficient CNN-based image encoder with lower computational overhead.
- **DistilBART:** A smaller, distilled version of BART optimized for generation tasks with fewer parameters than T5.

Key modifications:
- MobileNetV3 replaced CLIP, significantly reducing feature extraction costs.
- A linear projection layer aligned MobileNetV3’s features with DistilBART’s hidden dimension.
- DistilBART, being a distilled transformer, allowed for faster and more efficient HTML generation.

Why Training is Slow?

- Dataset Streaming: The dataset is streamed, which might introduce delays in data loading.
- Large Models: CLIP and T5 are large models requiring significant computing power.
- Validation Overhead: Validation after every epoch adds computation time.

**Challenges and Considerations:**
- The newer model reduced memory and computation requirements but potentially sacrificed some quality in complex layouts.
- Experimenting with different lightweight vision encoders (e.g., EfficientNet, ViT-Tiny) could further optimize performance.
- Future iterations could explore quantization or pruning techniques to further reduce the computational footprint.

This shift enabled practical deployment in resource-constrained environments while still achieving competitive results in HTML generation from images.

