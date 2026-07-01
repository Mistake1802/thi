BAI GIAI 5 DE THI - CAU TRUC DU LIEU (CTDL)
=============================================

de01.c               -> De so 01: Cay BST sach (khoa=so trang) + Do thi tram kiem lam
de02.c               -> De so 02: Cay tong quat thu muc/tep tin + Do thi mang may tinh
de03.c               -> De so 03: Cay tong quat dong vat + Do thi ma tran ke tram sac xe dien
de04.c               -> De so 04: Cay BST hanh tinh (khoa=khoang cach) + Do thi ga tau
de_hk1_2023_2024.c   -> De thi cuoi ky HK1 2023-2024: BST so nguyen + Do thi co huong "biet nha"

Cach chay (co san gcc):
    gcc -o de01 de01.c && ./de01
(tuong tu cho cac file khac)

Cau 1 cua moi de co cho nhap 1 dong tu ban phim (ten sach/tep/loai dong vat can
kiem tra co trong cay khong) -> chi can nhap 1 dong ten roi Enter khi chay.

*** GHI CHU QUAN TRONG ***
File de_hk1_2023_2024.c, phan Cau 2 (do thi co huong "biet nha"): anh chup do
thi bi nghieng/mo nen minh khong doc chac chan duoc tat ca chieu mui ten va
so luong sinh vien chinh xac. Minh da phuc dung tam danh sach canh trong
main() (co ghi chu ro trong code). Cac ham xu ly cau b, c da viet dung logic
va se cho ket qua dung voi bat ky du lieu do thi nao ban nhap vao - ban chi
can doi chieu lai voi de goc va sua lai vai dong addDirectedEdge(...) cho
khop la dung ngay.

De 03, Cau 1: anh khong the hien 100% ro Monkey la con cua nhom nao (Mammals/
Birds/Reptiles), minh da chon Monkey la con cua Reptiles - kiem tra lai va
sua 1 dong neu can.
Nếu bị lỗi 
Dev-C++:

Tools → Compiler Options → Settings

hoặc

Project → Project Options → Parameters

đổi

-std=c++11

thành

-std=c99

hoặc

-std=c11

Sau đó Build lại.
