# PDF - Embedded
Đề cho 1 file pdf và yêu cầu phải tìm flag được giấu bên trong. E đã dùng tool pdf-parser để trích xuất các object bên trong file ra và e phát hiện ở obj 77 thuộc loại embeddedfile
![image](https://hackmd.io/_uploads/BkktOu1W-x.png)
sau đó e đã trích obj 77 bằng dòng lệnh

>  python3 pdf-parser.py -o 77 -f -d flag.txt epreuve_BAC_2004.pdf

E nhận thấy dữ liệu được trích ra có dạng giống base 64 nên đã decode nó và thu được 1 file jpeg

![image](https://hackmd.io/_uploads/HJCj9_kZbe.png)

