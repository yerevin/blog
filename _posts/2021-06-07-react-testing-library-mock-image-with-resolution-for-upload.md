---
title: 🚀React 🚀 - react-testing-library - mock image file with resolution for upload
---

[Github markdown version - can be easier to read](https://github.com/yerevin/blog/blob/gh-pages/_posts/2021-06-07-react-testing-library-mock-image-with-resolution-for-upload.md)

Let's say you have method to validate uploaded image file resolution and you call it to check if it pass and if doesn't pass you prevent file submit to API - for example: https://stackoverflow.com/a/47786555/6423846 (but prettier ;) )

And then in your tests you have case when you check if method to upload file is called

```typescript
  test("should call method to upload file on button click", async () => {
    const uploadPhoto = jest.fn(); // Assuming this is your spy
    const fileInput = await screen.findByTitle(/upload image/i);
  
    const canvas = document.createElement("canvas");
    canvas.width = 600;
    canvas.height = 400;
  
    // 1. Wrap toBlob in a Promise to make it "awaitable"
    const blob = await new Promise((resolve) => canvas.toBlob(resolve, "image/png"));
    
    const file = new File([blob], "image.png", { type: "image/png" });
  
    // 2. await the upload event (crucial for user-event v14+)
    await userEvent.upload(fileInput, file);
  
    // 3. Now the assertion will run at the correct time
    expect(uploadPhoto).toHaveBeenCalledTimes(1);
  });
```
