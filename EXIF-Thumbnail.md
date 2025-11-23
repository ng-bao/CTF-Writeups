# EXIF - Thumbnail
khi e sử dụng lệnh exiftool để xem metadata của tấm ảnh thì e thấy nó có 1 dòng là Thumbnail Image. Dựa vào đó e đã sử dụng lệnh
```bash
exiftool -b -ThumbnailImage ch10.jpg > res.jpg
```
sau đó e đã dùng exiftool để check ảnh mới thì phát hiện vẫn còn ảnh nhúng bên trong nên tiếp tục lặp lại lệnh trên với ảnh res và cho vào ảnh res1.jpg. kết quả thu được

<img width="361" height="367" alt="image" src="https://github.com/user-attachments/assets/de9b9583-d147-489f-a68a-81e5d9eb8c08" />
