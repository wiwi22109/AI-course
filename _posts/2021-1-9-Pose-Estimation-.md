---
layout: post
title: AI 自我學習
author: [曾韋]
category: [Lecture]
tags: [jekyll, ai]
---

AI 自我學習
---
### OpenAI

* **[OpenAI]
OpenAI 是美國一個人工智慧研究實驗室，由營利組織 OpenAI LP 與母公司非營利組織 OpenAI Inc 所組成，目的是促進和發展友好的人工智慧，使人類整體受益。組織目標是通過與其他機構和研究者的「自由合作」，向公眾開放專利和研究成果。<br>
![](https://tenbaggersnow.com/wp-content/uploads/2021/01/openai-1-320x133.png)

### GPT-3
![](https://tenbaggersnow.com/wp-content/uploads/2021/01/1_jfPejaM39BLFhR6FMD-pPQ-1-580x326.png)

此語言模型在推出前已經被大量的數據訓練。而模型已經從多個來源（包括維基百科和書籍）接受了大約 45 TB 文本數據的訓練，使用「In-context learning」的形式作訓練，是指訓練的過程中不為特定任務作微調或限制，而讓系統模型能夠從文本數據的上下文自行了解規律及訓練，並預測下一個文字及單詞。<br>

當預測錯誤時，系統模型會自我修複，經過過百萬次重複訓練，就能確保系統模型的準確性。<br>
![](https://github.com/wiwi22109/AI-course/blob/gh-pages/images/03-gpt3-training-step-back-prop.gif)



### 1.圖像生成<br>
### Usage<br>
### Generations<br>
The image generations endpoint allows you to create an original image given a text prompt. Generated images can have a size of 256x256, 512x512, or 1024x1024 pixels. Smaller sizes are faster to generate. You can request 1-10 images at a time using the n parameter.<br>
```
response = openai.Image.create(
  prompt="a white siamese cat",
  n=1,
  size="1024x1024"
)
image_url = response['data'][0]['url']
```
The more detailed the description, the more likely you are to get the result that you or your end user want. You can explore the examples in the DALL·E preview app for more prompting inspiration. Here's a quick example:<br>
<br>
1.a white siamese cat<br>
![](https://cdn.openai.com/API/images/guides/image_generation_simple.webp)<br>
<br>
2.a close up, studio photographic portrait of a white siamese cat that looks curious, backlit ears<br>
![](https://cdn.openai.com/API/images/guides/image_generation_detailed.webp)<br>

Each image can be returned as either a URL or Base64 data, using the response_format parameter. URLs will expire after an hour.<br>

### Edits<br>
The image edits endpoint allows you to edit and extend an image by uploading a mask. The transparent areas of the mask indicate where the image should be edited, and the prompt should describe the full new image, not just the erased area. This endpoint can enable experiences like the editor in our DALL·E preview app.<br>
```
response = openai.Image.create_edit(
  image=open("sunlit_lounge.png", "rb"),
  mask=open("mask.png", "rb"),
  prompt="A sunlit indoor lounge area with a pool containing a flamingo",
  n=1,
  size="1024x1024"
)
image_url = response['data'][0]['url']
```
The uploaded image and mask must both be square PNG images less than 4MB in size, and also must have the same dimensions as each other. The non-transparent areas of the mask are not used when generating the output, so they don’t necessarily need to match the original image like the example above.<br>

### Variations<br>
The image variations endpoint allows you to generate a variation of a given image.<br>
```
response = openai.Image.create_variation(
  image=open("corgi_and_cat_paw.png", "rb"),
  n=1,
  size="1024x1024"
)
image_url = response['data'][0]['url']
```
IMAGE<br>
![](https://cdn.openai.com/API/images/guides/image_variation_original.webp)<br>
OUTPUT<br>
![](https://cdn.openai.com/API/images/guides/image_variation_output.webp)<br>

### Content moderation<br>
Prompts and images are filtered based on our content policy, returning an error when a prompt or image is flagged. If you have any feedback on false positives or related issues, please contact us through our help center.<br>

### Language-specific tips<br>

### Using in-memory image data<br>
The Node.js examples in the guide above use the fs module to read image data from disk. In some cases, you may have your image data in memory instead. Here's an example API call that uses image data stored in a Node.js Buffer object:<br>
```
// This is the Buffer object that contains your image data
const buffer = [your image data];
// Set a `name` that ends with .png so that the API knows it's a PNG image
buffer.name = "image.png";
const response = await openai.createImageVariation(
  buffer,
  1,
  "1024x1024"
);
```
### Working with TypeScript<br>
If you're using TypeScript, you may encounter some quirks with image file arguments. Here's an example of working around the type mismatch by explicitly casting the argument:<br>

```
// Cast the ReadStream to `any` to appease the TypeScript compiler
const response = await openai.createImageVariation(
  fs.createReadStream("image.png") as any,
  1,
  "1024x1024"
);
```
And here's a similar example for in-memory image data:<br>
```
// This is the Buffer object that contains your image data
const buffer: Buffer = [your image data];
// Cast the buffer to `any` so that we can set the `name` property
const file: any = buffer;
// Set a `name` that ends with .png so that the API knows it's a PNG image
file.name = "image.png";
const response = await openai.createImageVariation(
  file,
  1,
  "1024x1024"
);
```
### Error handling<br>
API requests can potentially return errors due to invalid inputs, rate limits, or other issues. These errors can be handled with a try...catch statement, and the error details can be found in either error.response or error.message:<br>

```
try {
  const response = await openai.createImageVariation(
    fs.createReadStream("image.png"),
    1,
    "1024x1024"
  );
  console.log(response.data.data[0].url);
} catch (error) {
  if (error.response) {
    console.log(error.response.status);
    console.log(error.response.data);
  } else {
    console.log(error.message);
  }
}
```


### 2.自動生成網頁程式碼<br>
只需輸入你想要的網頁內容，系統即可自動產生網頁程式碼<br>
![](https://tenbaggersnow.com/wp-content/uploads/2021/01/https___bucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com_public_images_033c8f8c-8d90-4395-91de-5c988bec128c_600x364.gif)

### 3.自動生成文章<br>
完整生成一篇文章。<br>

參考資料:
https://tenbaggersnow.com/?p=6155
