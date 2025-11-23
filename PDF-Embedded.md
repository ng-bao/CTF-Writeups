# PDF - Embedded
Đề cho 1 file pdf và yêu cầu phải tìm flag được giấu bên trong. E đã dùng tool pdf-parser để trích xuất các object bên trong file ra và e phát hiện ở obj 77 thuộc loại embeddedfile

<img width="766" height="442" alt="image" src="https://github.com/user-attachments/assets/89934f71-2b38-4346-a1dd-d4f3e7cfc05d" />

sau đó e đã trích obj 77 bằng dòng lệnh

```bash
python3 pdf-parser.py -o 77 -f -d flag.txt epreuve_BAC_2004.pdf
```

E nhận thấy dữ liệu được trích ra có dạng giống base 64 nên đã decode nó và thu được 1 file jpeg

<img width="496" height="413" alt="image" src="https://github.com/user-attachments/assets/b523f6c2-2c81-4025-9a89-7d9c7909339a" />


