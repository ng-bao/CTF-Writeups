# Crypt-art
Đề cho 1 file ảnh có định dạng ppm, sau đó e đã check mã hex của nó thì phát hiện có một đoạn ghi là 

> The encrypted pass is: EPCQFBXKWURQCTXOIPMNV

sau đó e dựa vào gợi ý "a language where the programs are works of modern art" và biết được nó được viết bằng ngôn ngữ piet. Sau đó e bỏ lên trang npiet online để nó giải và thu được đoạn sau

> key is EYJFRGTT

Dựa trên dữ liệu được cho là encrypted và key thì chắc chắn đây là một loại mật mã.
Tiếp đó e thử từng loại mật mã thì biết được nó là loại Vigenère cipher. Cuối cùng chỉ cần bỏ 2 phần kiếm được ở trên vào và decode nó ra.

> password: ARTLOVERSWILLNEVERDIE
