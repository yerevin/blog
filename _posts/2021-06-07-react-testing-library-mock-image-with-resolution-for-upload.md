---
title: 🚀React 🚀 - react-testing-library - mock image file with resolution for upload
---

[Github markdown version - can be easier to read](https://github.com/yerevin/blog/blob/gh-pages/_posts/2021-06-07-react-testing-library-mock-image-with-resolution-for-upload.md)

Let's say you have method to validate uploaded image file resolution and you call it to check if it pass and if doesn't pass you prevent file submit to API - for example: https://stackoverflow.com/a/47786555/6423846 (but prettier ;) )

And then in your tests you have case when you check if method to upload file is called

```typescript
  test("should call method to upload file on button click", async () => {
    // spy on some method eg. "uploadPhoto"

    const fileInput = (await screen.findByTitle(/upload image/i));

    // define canvas to set required image resolution to pass test
    const canvas = document.createElement("canvas");
    canvas.width = 600;
    canvas.height = 400;

    // generate file for userEvent.upload method
    canvas.toBlob(async (blob) => {
      const file = new File([blob], "instruction_image.png", {
        type: "image/png"
      });

      userEvent.upload(fileInput, file);

      // test should pass and you should get uploadPhoto method called
      expect(uploadPhoto).toHaveBeenCalledTimes(1);
    });
  });
```
