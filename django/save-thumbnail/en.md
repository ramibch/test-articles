# Save a thumbnail in Django model


You can save space on server & speed up your speed by saving a thumbnail of an image. Sometimes we don't want to save images with size of 3 MB or 5 MB or even more, so can save just a thumbnail of that image for better performance of our app:

```python
from PIL import Image # pip install pillow

class Profile(models.Model):
  # other fields
  image = models.ImageField(upload_to="path/to/be/saved")

  def save(self, *args, **kwargs):
      super(Profile, self).save(*args, **kwargs)
      img = Image.open(self.image)
      if img.height > 200 or img.width > 200:
          new_size = (200, 200)
          # image proportion is manteined / we dont need to do extra work
          img.thumbnail(new_size)
          img.save(self.image.path)
```
