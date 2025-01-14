# Segment Anything with Clip
[[HuggingFace Space](https://huggingface.co/spaces/curt-park/segment-anything-with-clip)] | [[COLAB](https://colab.research.google.com/github/Curt-Park/segment-anything-with-clip/blob/main/colab.ipynb)] | [[Demo Video](https://youtu.be/vM7MfAc3BdQ)]

Meta released [a new foundation model for segmentation tasks](https://ai.facebook.com/blog/segment-anything-foundation-model-image-segmentation/).
It aims to resolve downstream segmentation tasks with prompt engineering, such as foreground/background points, bounding box, mask, and free-formed text.
However, the text prompt is not released yet.

Alternatively, I took the following steps:
1. Get all object proposals generated by SAM (Segment Anything Model).
2. Crop the object regions by bounding boxes.
3. Get cropped images' features and a query feature from [CLIP](https://openai.com/research/clip).
4. Calculate the similarity between image features and the query feature.
```python
# How to get the similarity.
preprocessed_img = preprocess(crop).unsqueeze(0)
tokens = clip.tokenize(texts)
logits_per_image, _ = model(preprocessed_img, tokens)
similarity = logits_per_image.softmax(-1)
```

## How to run on local
[Anaconda](https://www.anaconda.com/) is required before start setup.
```bash
make env
conda activate segment-anything-with-clip
make setup
```

```bash
# this executes GRadio server.
make run
```
Open http://localhost:7860/
![](https://user-images.githubusercontent.com/14961526/232016821-dda192c1-1095-4086-adb8-e6a9f44b685f.png)

## References
- https://github.com/facebookresearch/segment-anything
- https://github.com/openai/CLIP
