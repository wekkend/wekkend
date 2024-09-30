#include <iostream>
#include <string>
#include <regex>
#include <fstream>
#include <algorithm>
using namespace std;

struct SanPham {
    string maSanPham;
    string tenSanPham;
    string nhanHieu;
    string mauSanPham;
    int size;
    int soLuong;
};

void nhapSanPham(SanPham &sp) {
    regex maSanPhamPattern("^[A-Za-z]{3}[0-9]{3}$");
    regex tenSanPhamPattern("^.{1,50}$");
    regex nhanHieuPattern("^.{1,15}$");
    regex mauSanPhamPattern("^(Den|Trang|Xam|Xanh|Hong)$");
    regex sizePattern("^(39|40|41|42)$");

    
    while (true) {
        cout << "Nhap ma san pham (3 chu cai + 3 so): ";
        getline(cin, sp.maSanPham);
        if (regex_match(sp.maSanPham, maSanPhamPattern)) {
            break;
        }
        cout << "Ma san pham khong hop le, vui long nhap lai.\n";
    }

    // Nhập tên sản phẩm
    while (true) {
        cout << "Nhap ten san pham: ";
        getline(cin, sp.tenSanPham);
        if (regex_match(sp.tenSanPham, tenSanPhamPattern)) {
            break;
        }
        cout << "Ten san pham khong hop le, vui long nhap lai.\n";
    }

    // Nhập nhãn hiệu
    while (true) {
        cout << "Nhap nhan hieu: ";
        getline(cin, sp.nhanHieu);
        if (regex_match(sp.nhanHieu, nhanHieuPattern)) {
            break;
        }
        cout << "Nhan hieu khong hop le, vui long nhap lai.\n";
    }

    // Nhập màu sản phẩm
    while (true) {
        cout << "Nhap mau san pham (Den, Trang, Xam, Xanh, Hong): ";
        getline(cin, sp.mauSanPham);
        if (regex_match(sp.mauSanPham, mauSanPhamPattern)) {
            break;
        }
        cout << "Mau san pham khong hop le, vui long nhap lai.\n";
    }

    // Nhập size sản phẩm
    while (true) {
        cout << "Nhap size san pham (39-42): ";
        string sizeInput;
        getline(cin, sizeInput);
        if (regex_match(sizeInput, sizePattern)) {
            sp.size = stoi(sizeInput);
            break;
        }
        cout << "Size san pham khong hop le, vui long nhap lai.\n";
    }

    // Nhập số lượng sản phẩm
    while (true) {
        cout << "Nhap so luong san pham: ";
        cin >> sp.soLuong;
        if (sp.soLuong >= 0) {
            break;
        }
        cout << "So luong khong hop le, vui long nhap lai.\n";
    }
    cin.ignore();  // Bỏ qua ký tự xuống dòng sau khi nhập số lượng
}

void xuatSanPham(const SanPham &sp) {
    cout << "Ma san pham: " << sp.maSanPham << endl;
    cout << "Ten san pham: " << sp.tenSanPham << endl;
    cout << "Nhan hieu: " << sp.nhanHieu << endl;
    cout << "Mau san pham: " << sp.mauSanPham << endl;
    cout << "Size: " << sp.size << endl;
    cout << "So luong: " << sp.soLuong << endl;
}

void nhapDanhSachSanPham(SanPham ds[], int n) {
    for (int i = 0; i < n; i++) {
        cout << "Nhap thong tin san pham thu " << i + 1 << ":\n";
        nhapSanPham(ds[i]);
    }
}

void xuatDanhSachSanPham(const SanPham ds[], int n) {
    for (int i = 0; i < n; i++) {
        cout << "\nThong tin san pham thu " << i + 1 << ":\n";
        xuatSanPham(ds[i]);
    }
}

int timSanPhamTheoMa(const SanPham ds[], int n, const string &ma) {
    for (int i = 0; i < n; i++) {
        if (ds[i].maSanPham == ma) {
            return i;
        }
    }
    return -1;
}

void suaSanPham(SanPham ds[], int n) {
    cout << "Nhap ma san pham can sua: ";
    string ma;
    getline(cin, ma);

    int index = timSanPhamTheoMa(ds, n, ma);
    if (index == -1) {
        cout << "Khong tim thay san pham voi ma nay.\n";
    } else {
        cout << "Nhap thong tin moi cho san pham:\n";
        nhapSanPham(ds[index]);
        cout << "Sua thong tin san pham thanh cong.\n";
    }
}

void xoaSanPham(SanPham ds[], int &n) {
    cout << "Nhap ma san pham can xoa: ";
    string ma;
    getline(cin, ma);

    int index = timSanPhamTheoMa(ds, n, ma);
    if (index == -1) {
        cout << "Khong tim thay san pham voi ma nay.\n";
    } else {
        for (int i = index; i < n - 1; i++) {
            ds[i] = ds[i + 1];
        }
        n--;
        cout << "Xoa san pham thanh cong.\n";
    }
}

// Tìm giày có tên chứa tên nhập vào
void timGiayTheoTen(const SanPham ds[], int n) {
    cout << "Nhap ten san pham can tim: ";
    string ten;
    getline(cin, ten);
    
    cout << "Danh sach san pham co ten chua '" << ten << "':\n";
    bool found = false;
    for (int i = 0; i < n; i++) {
        if (ds[i].tenSanPham.find(ten) != string::npos) {
            xuatSanPham(ds[i]);
            found = true;
        }
    }
    if (!found) {
        cout << "Khong tim thay san pham nao.\n";
    }
}

// Tìm giày theo nhãn hiệu, size và màu sắc
void timGiayTheoNhaHieuMauSize(const SanPham ds[], int n) {
    string nhanHieu, mau;
    int size;
    
    cout << "Nhap nhan hieu: ";
    getline(cin, nhanHieu);
    
    cout << "Nhap mau: ";
    getline(cin, mau);
    
    cout << "Nhap size: ";
    cin >> size;
    cin.ignore(); // Bỏ qua ký tự xuống dòng

    cout << "Danh sach san pham tim duoc:\n";
    bool found = false;
    for (int i = 0; i < n; i++) {
        if (ds[i].nhanHieu == nhanHieu && ds[i].mauSanPham == mau && ds[i].size == size) {
            xuatSanPham(ds[i]);
            found = true;
        }
    }
    if (!found) {
        cout << "Khong tim thay san pham nao.\n";
    }
}

// Đếm số lượng giày theo size của một nhãn hiệu
void demSoLuongGiayTheoSize(const SanPham ds[], int n) {
    string nhanHieu;
    cout << "Nhap nhan hieu can dem: ";
    getline(cin, nhanHieu);

    int sizeCount[5] = {0}; // size 39-42

    for (int i = 0; i < n; i++) {
        if (ds[i].nhanHieu == nhanHieu) {
            sizeCount[ds[i].size - 39] += ds[i].soLuong;
        }
    }

    cout << "So luong giay theo size:\n";
    for (int i = 0; i < 4; i++) {
        cout << "Size " << (39 + i) << ": " << sizeCount[i] << endl;
    }
}

// Sắp xếp giày theo nhãn hiệu và số lượng
bool compare(const SanPham &a, const SanPham &b) {
    if (a.nhanHieu == b.nhanHieu) {
        return a.soLuong < b.soLuong; // Nếu nhãn hiệu giống nhau, sắp xếp theo số lượng
    }
    return a.nhanHieu < b.nhanHieu; // Sắp xếp theo nhãn hiệu
}

void sapXepGiay(SanPham ds[], int n) {
    sort(ds, ds + n, compare);
    cout << "Da sap xep danh sach san pham.\n";
}

// Tìm giày có số lượng nhỏ hơn một số và lưu vào file
void timVaLuuGiaySoLuongNhoHon(const SanPham ds[], int n) {
    int soLuong;
    cout << "Nhap so luong can tim: ";
    cin >> soLuong;

    ofstream outFile("giay_so_luong_nho.txt");
    if (!outFile) {
        cout << "Khong the mo file.\n";
        return;
    }

    cout << "Danh sach san pham co so luong nho hon " << soLuong << ":\n";
    bool found = false;
    for (int i = 0; i < n; i++) {
        if (ds[i].soLuong < soLuong) {
            xuatSanPham(ds[i]);
            outFile << ds[i].maSanPham << " " << ds[i].tenSanPham << " " 
                    << ds[i].nhanHieu << " " << ds[i].mauSanPham << " "
                    << ds[i].size << " " << ds[i].soLuong << endl;
            found = true;
        }
    }

    outFile.close();
    if (!found) {
        cout << "Khong tim thay san pham nao.\n";
    } else {
        cout << "Da luu danh sach san pham vao file giay_so_luong_nho.txt.\n";
    }
}

// Đọc file và xuất ra màn hình
void docFileVaXuat() {
    ifstream inFile("giay_so_luong_nho.txt");
    if (!inFile) {
        cout << "Khong the mo file.\n";
        return;
    }

    cout << "Danh sach san pham tu file:\n";
    SanPham sp;
    while (inFile >> sp.maSanPham >> sp.tenSanPham >> sp.nhanHieu 
                  >> sp.mauSanPham >> sp.size >> sp.soLuong) {
        xuatSanPham(sp);
    }

    inFile.close();
}

int main() {
    int n;
    cout << "Nhap so luong san pham: ";
    cin >> n;
    cin.ignore();

    SanPham ds[100];

    int choice;
    do {
        cout << "\nMENU\n";
        cout << "1. Nhap thong tin san pham\n";
        cout << "2. Xuat danh sach san pham\n";
        cout << "3. Sua thong tin san pham\n";
        cout << "4. Xoa san pham\n";
        cout << "5. Tim giay theo ten\n";
        cout << "6. Tim giay theo nhan hieu, mau, size\n";
        cout << "7. Dem so luong giay theo size\n";
        cout << "8. Sap xep giay\n";
        cout << "9. Tim giay so luong nho hon\n";
        cout << "10. Doc file va xuat\n";
        cout << "0. Thoat\n";
        cout << "Nhap lua chon: ";
        cin >> choice;
        cin.ignore();

        switch (choice) {
            case 1:
                nhapDanhSachSanPham(ds, n);
                break;
            case 2:
                xuatDanhSachSanPham(ds, n);
                break;
            case 3:
                suaSanPham(ds, n);
                break;
            case 4:
                xoaSanPham(ds, n);
                break;
            case 5:
                timGiayTheoTen(ds, n);
                break;
            case 6:
                timGiayTheoNhaHieuMauSize(ds, n);
                break;
            case 7:
                demSoLuongGiayTheoSize(ds, n);
                break;
            case 8:
                sapXepGiay(ds, n);
                break;
            case 9:
                timVaLuuGiaySoLuongNhoHon(ds, n);
                break;
            case 10:
                docFileVaXuat();
                break;
            case 0:
                cout << "Thoat chuong trinh.\n";
                break;
            default:
                cout << "Lua chon khong hop le.\n";
        }
    } while (choice != 0);

    return 0;
}
