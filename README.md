USE Day3
----------C1---------
SELECT khachhang.* FROM nhacungcap, khachhang
WHERE nhacungcap.tencongty = khachhang.tencongty

----------Cau2---------
SELECT dondathang.*, khachhang.makhachhang, khachhang.tencongty
FROM dondathang INNER JOIN khachhang
ON dondathang.makhachhang = khachhang.makhachhang
WHERE dondathang.noigiaohang = khachhang.diachi

----------Cau3---------
SELECT * FROM mathang
WHERE mathang.mahang NOT IN (SELECT mathang.mahang FROM 
					mathang INNER JOIN chitietdathang
					ON mathang.mahang = chitietdathang.mahang
					INNER JOIN dondathang
					ON dondathang.sohoadon = chitietdathang.sohoadon
					INNER JOIN khachhang
					ON dondathang.makhachhang = khachhang.makhachhang)

----------Cau8-------
SELECT mathang.mahang,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 1 THEN mathang.giahang 
	ELSE 0 END) AS Jan,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 2 THEN mathang.giahang 
	ELSE 0 END) AS Feb,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 3 THEN mathang.giahang 
	ELSE 0 END) AS Mar,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 4 THEN mathang.giahang 
	ELSE 0 END) AS Apr,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 5 THEN mathang.giahang 
	ELSE 0 END) AS May,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 6 THEN mathang.giahang 
	ELSE 0 END) AS Jun,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 7 THEN mathang.giahang 
	ELSE 0 END) AS Jul,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 8 THEN mathang.giahang 
	ELSE 0 END) AS Aug,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 9 THEN mathang.giahang 
	ELSE 0 END) AS Sep,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 10 THEN mathang.giahang 
	ELSE 0 END) AS Oct,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 11 THEN mathang.giahang 
	ELSE 0 END) AS Nov,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 12 THEN mathang.giahang 
	ELSE 0 END) AS 'Dec'
FROM mathang INNER JOIN chitietdathang
ON   mathang.mahang = chitietdathang.mahang
INNER JOIN dondathang
ON dondathang.sohoadon = chitietdathang.sohoadon
GROUP BY mathang.mahang

----------Cau9-------
SELECT 
	CASE 
		WHEN SUM(mathang.soluong) IS NULL THEN 0
		ELSE SUM(mathang.soluong)
	END 
	+
	CASE 
		WHEN SUM(chitietdathang.soluong) IS NULL THEN 0
		ELSE SUM(chitietdathang.soluong)
	END
FROM   mathang LEFT JOIN chitietdathang
ON     mathang.mahang = chitietdathang.mahang
GROUP BY mathang.mahang

--------Cau10--------
GO
SELECT TOP 1 nhanvien.manhanvien, nhanvien.ten, nhanvien.dienthoai,
         SUM(chitietdathang.soluong) AS soluong
FROM nhanvien
INNER JOIN dondathang
    ON nhanvien.manhanvien = dondathang.manhanvien
INNER JOIN chitietdathang
    ON chitietdathang.sohoadon = dondathang.sohoadon
GROUP BY nhanvien.manhanvien, nhanvien.ten, nhanvien.dienthoai
ORDER BY soluong DESC

---------Cau11--------
SELECT  DDH.sohoadon, mathang.mahang, mathang.tenhang,
		tongtien = (SELECT SUM(chitietdathang.giaban) 
			FROM   chitietdathang INNER JOIN dondathang
			ON	   chitietdathang.sohoadon = dondathang.sohoadon
			WHERE chitietdathang.sohoadon = DDH.sohoadon
			GROUP BY chitietdathang.sohoadon)
FROM	dondathang DDH INNER JOIN chitietdathang
ON 		DDH.sohoadon = chitietdathang.sohoadon
INNER JOIN mathang 
ON 		mathang.mahang = chitietdathang.mahang
GROUP BY DDH.sohoadon, mathang.mahang, mathang.tenhang

---------Cau12---------
SELECT loaihang.maloaihang, mathang.mahang, mathang.tenhang
FROM loaihang INNER JOIN mathang
       ON loaihang.maloaihang=mathang.maloaihang
GROUP BY loaihang.maloaihang, mathang.tenhang, mathang.mahang
ORDER BY loaihang.maloaihang

SELECT loaihang.maloaihang, mathang.mahang, SUM(mathang.soluong)
FROM loaihang INNER JOIN mathang
       ON loaihang.maloaihang=mathang.maloaihang
GROUP BY ROLLUP (loaihang.maloaihang, mathang.mahang)
ORDER BY loaihang.maloaihang

----------Cau13-------
SELECT mathang.mahang,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 1 THEN mathang.giahang 
	ELSE 0 END) AS Jan,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 2 THEN mathang.giahang 
	ELSE 0 END) AS Feb,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 3 THEN mathang.giahang 
	ELSE 0 END) AS Mar,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 4 THEN mathang.giahang 
	ELSE 0 END) AS Apr,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 5 THEN mathang.giahang 
	ELSE 0 END) AS May,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 6 THEN mathang.giahang 
	ELSE 0 END) AS Jun,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 7 THEN mathang.giahang 
	ELSE 0 END) AS Jul,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 8 THEN mathang.giahang 
	ELSE 0 END) AS Aug,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 9 THEN mathang.giahang 
	ELSE 0 END) AS Sep,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 10 THEN mathang.giahang 
	ELSE 0 END) AS Oct,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 11 THEN mathang.giahang 
	ELSE 0 END) AS Nov,
	SUM(CASE MONTH(dondathang.ngaydathang) WHEN 12 THEN mathang.giahang 
	ELSE 0 END) AS 'Dec',
	SUM(chitietdathang.soluong) AS soluong
FROM mathang INNER JOIN chitietdathang
ON   mathang.mahang = chitietdathang.mahang
INNER JOIN dondathang
ON dondathang.sohoadon = chitietdathang.sohoadon
GROUP BY mathang.mahang

----------Cau14------
SELECT * FROM dondathang

UPDATE  dondathang
SET    	ngaychuyenhang = ngaydathang
WHERE	ngaychuyenhang IS NULL

---------Cau15-------
UPDATE 	dondathang
SET		dondathang.noigiaohang = khachhang.diachi
FROM	dondathang INNER JOIN khachhang
ON		dondathang.makhachhang = khachhang.makhachhang
WHERE   dondathang.noigiaohang IS NULL

--------Cau16--------
UPDATE 	khachhang
SET		khachhang.diachi = nhacungcap.diachi,
		khachhang.dienthoai = nhacungcap.dienthoai,
		khachhang.email = nhacungcap.email,
		khachhang.fax = nhacungcap.fax
FROM	nhacungcap, khachhang
WHERE	nhacungcap.tencongty = khachhang.tencongty

--------Cau17--------

---------------------
SELECT * FROM 
nhacungcap INNER JOIN mathang
ON nhacungcap.macongty = mathang.macongty
INNER JOIN chitietdathang
ON mathang.mahang = chitietdathang.mahang
INNER JOIN dondathang
ON dondathang.sohoadon = chitietdathang.sohoadon
INNER JOIN khachhang
ON dondathang.makhachhang = khachhang.makhachhang
WHERE khachhang.makhachhang = nhacungcap.macongty
