# updateStatusOutlet()
- tampilkan data semua pegawai dari berdasarkan idHarian nya, pake function getAllByIdHarian
	function getAllByIdHarian mendapat parameter id_absensi_standkeeper_harian dari request body 

- masukkan hasil data function getAllByIdharian ke variabel existingData 
- masukkan data idpegawai dari existingData ke variabel id_pegawai_lama 
- masukkan data idBulanan dari existingData ke variabel idBulananDataLama 

- tampilkan data semua pegawai yang terdeteksi di tabel bulanan pakai function getDistinctPegawaiWithAbsenStandBulanan
- masukkan hasil data function getDistinctPegawaiWithAbsenStandBulanan ke variabel allPegawaiArray 

- jalankan kondisi (jika request body outlet_buka_tutup === 1 / true / terbuka)
	- memperbarui data kehadiran per hari di tabel harian, pakai function updatePegawaiOutletBonById()
		function updatePegawaiOutletBonById mendapat parameter id_absensi_standkeeper_harian, id_pegawai, outlet_buka_tutup, bon dari request body 
	- memeriksa apakah id_pegawai yang baru saja diperbarui sudah memiliki id_absensi_standkeeper_bulanan di tabel bulanan, pakai function checkAndInsert
   		jika BELUM ADA, maka, 
		- membuat data baru di tabel bulanan dengan parameter input id_pegawai_baru, dan column data akan menyesuaikan	
		- kemudian, hitung dan tampilkan hitung total kehadiran bulanan dari masing masing pegawai pakai function getTotalKehadiranBulanan menggunakan parameter input 	allPegawaiArray
		- masukkan hasil total kehadiran ke variabel totalKehadiranBaruObj 
		- memperbarui data total kehadiran di tabel bulanan pakai function updateAttendanceAndBon menggunakan parameter input totalKehadiranBaruObj
		
  		jika SUDAH ADA, maka, 
		- lanjutkan 
	- memperbarui ketidakcocokan antara idpegawai yang ada perubahan dengan id_absensi_standkeeper_bulanan nya. Misal data awal id_pegawai = 41 dan id_absensi_standkeeper_bulanan = 2001, diubah menjadi id_pegawai = 42 oleh user. Artinya id_absensi_standkeeper_bulanan akan berubah menjadi 2002. 
	- menampilkan notifikasi jika ada perubahan pada id_pegawai atau tidak ada perubahan di console.log
	- jalankan lagi, tampilkan data semua pegawai yang terdeteksi di tabel bulanan pakai function getDistinctPegawaiWithAbsenStandBulanan
	- masukkan hasil data function getDistinctPegawaiWithAbsenStandBulanan ke variabel allPegawaiArray 
	- hitung dan tampilkan hitung total kehadiran bulanan dari masing masing pegawai pakai function getTotalKehadiranBulanan menggunakan parameter input allPegawaiArray
	- masukkan hasil total kehadiran ke variabel totalKehadiranBaruObj 
	- memperbarui data total kehadiran di tabel bulanan pakai function updateAttendanceAndBon menggunakan parameter input totalKehadiranBaruObj
	- menampilkan response status 200 dan message "Outlet status terbuka dan data berhasil di update ..."

- jalankan kondisi (jika request body outlet_buka_tutup === 0 / false / tertutup)
	- menampilkan data pegawai (id_pegawai) berdasarkan input data id_outlet, pakai function getPegawaiByOutlet
	> [!ERROR]
	> code: 'ER_BAD_FIELD_ERROR',
	errno: 1054,
  	sqlMessage: "Unknown column 'undefined' in 'where clause'",
  	sqlState: '42S22',
  	index: 0,
  	sql: 'SELECT pegawai.id_pegawai FROM pegawai \n' +
    '    JOIN outlet ON pegawai.id_outlet = outlet.id_outlet WHERE outlet.id_outlet = undefined'.

	> [!SOLUSI]
	> menambahkan req query id_outlet pada endpoint request PUT menjadi http://localhost:8080/api/absenstand/edit?id_outlet=11 dengan reqbody berikut : 
	{
  	"id_absensi_standkeeper_harian":2925,
  	"id_pegawai_baru": 42, 
  	"outlet_buka_tutup": 1,  
  	"bon":1000, 
  	"catatan":"nyatet doang"
	}

  	- untuk menghandle error di atas, menambahkan kondisi jika tidak ada id_outlet (!id_outlet), maka berikan status 400 & message 'id_outlet is undefined or invalid'
  
	- memperbarui data kehadiran per hari di tabel harian, pakai function updatePegawaiOutletBonById()
		karena outlet tertutup, maka data kehadiran akan reset seperti semula. Data kehadiran semula adalah outlet_buka_tutup = 0, bon = 0, catatan = null, id_pegawai = <id_pegawai semula ketika setelah digenerate>, id_absensi_standkeeper_harian = <input request body>. Kemudian, function updatePegawaiOutletBonById akan berparameter 					(id_absensi_standkeeper_harian, id_pegawai, outlet_buka_tutup, bon)

	- memeriksa apakah id_pegawai yang baru saja diperbarui sudah memiliki id_absensi_standkeeper_bulanan di tabel bulanan, pakai function checkAndInsert
		jika BELUM ADA, maka, 
			- membuat data baru di tabel bulanan dengan parameter input id_pegawai_baru, dan column data akan menyesuaikan	
			- kemudian, hitung dan tampilkan hitung total kehadiran bulanan dari masing masing pegawai pakai function getTotalKehadiranBulanan menggunakan parameter input 						allPegawaiArray
			- masukkan hasil total kehadiran ke variabel totalKehadiranBaruObj 
			- memperbarui data total kehadiran di tabel bulanan pakai function updateAttendanceAndBon menggunakan parameter input totalKehadiranBaruObj
		jika SUDAH ADA, maka, 
			- lanjutkan 
	- memperbarui ketidakcocokan antara idpegawai yang ada perubahan dengan id_absensi_standkeeper_bulanan nya. Misal data awal id_pegawai = 41 dan id_absensi_standkeeper_bulanan = 2001, 		diubah menjadi id_pegawai = 42 oleh user. Artinya id_absensi_standkeeper_bulanan akan berubah menjadi 2002. 
	- menampilkan notifikasi jika ada perubahan pada id_pegawai atau tidak ada perubahan di console.log
	- jalankan lagi, tampilkan data semua pegawai yang terdeteksi di tabel bulanan pakai function getDistinctPegawaiWithAbsenStandBulanan
	- masukkan hasil data function getDistinctPegawaiWithAbsenStandBulanan ke variabel allPegawaiArray 
	- hitung dan tampilkan hitung total kehadiran bulanan dari masing masing pegawai pakai function getTotalKehadiranBulanan menggunakan parameter input allPegawaiArray
	- masukkan hasil total kehadiran ke variabel totalKehadiranBaruObj 
	- memperbarui data total kehadiran di tabel bulanan pakai function updateAttendanceAndBon menggunakan parameter input totalKehadiranBaruObj
	- menampilkan response status 200 dan message "Outlet status tertutup dan data berhasil di update ke 0 ..."


# findAll()
- mendapatkan input request query dari user, request query berisi: id_outlet, bulan_absensi, tahun_absensi, id_pegawai(opsional/tidak wajib/NULL)
- menjalankan kondisi, jika id_outlet, bulan_absensi, tahun_absensi bernilai kosong, maka muncul notif 'Wajib mengisi id_outlet, bulan_absensi, dan tahun_absensi'.
- menjalankan function getOutletName() dengan parameter input id_outlet, dan menghasilkan nama outlet
- menjalankan function getAll() untuk tabel absensistandkeeperharian dengan parameter input id_outlet, bulan_absensi, tahun_absensi, id_pegawai, kemudian menghasilkan data kehadiran harian standkeeper
- menjalankan function getAll() untuk tabel absensistandkeeperbulanan dengan parameter input bulan_absensi, tahun_absensi, id_pegawai, kemudian menghasilkan data kehadiran bulanan standkeeper
	- menjalankan validasi, jika ada error, maka tampilkan error. Jika data terdeteksi atau kosong, maka muncul notif 'Data AbsenStandBulanan tidak ditemukan'
- membuatkan variable kosong bernama dataDetails bertipe array.
- menjalankan looping berasal dari input absenStandBulananData,
  	- menghitung total_kehadiran, pakai function getTotalKehadiran() dengan parameter input absenStandBulanan.id_pegawai, bulan_absensi, tahun_absensi 
  	- menghitung total_bon, pakai function getTotalBon() dengan parameter input absenStandBulanan.id_pegawai, bulan_absensi, tahun_absensi
  	- menghitung total_gaji_bulanan, pakai function getTotalGajiBulanan() dengan parameter input absenStandBulanan.id_pegawai, bulan_absensi, tahun_absensi
  	- menghitung total_gaji_bersih berasal dari total_gaji_bulanan kurangi total_bon
  	- mengambil data gaji_pokok
  	- mengambil data nama_pegawai, pakai function getNamaPegawai() dengan parameter input id_pegawai
	- mengirim data data yang diambil ditaruh ke absenStandBulanan.pegawai_details dan dataDetails yang akan ditampilkan di response body

# Alur Diagram untuk Fungsi `findAll()`
```plaintext
Start
|
|-- Validate Input
|   |-- Check if `id_outlet`, `bulan_absensi`, `tahun_absensi` are provided
|   |   |-- If not provided, return 400 error
|
|-- Set shared state
|   |-- Set `sharedState.id_outlet` to `id_outlet`
|
|-- Get Outlet Name
|   |-- Call `Pegawai.getOutletName(id_outlet, callback)`
|       |-- If error, return 500 error
|       |-- If outlet not found, return 404 error
|
|-- Get AbsenStandHarian Data
|   |-- Call `AbsenStandHarian.getAll({id_outlet, bulan_absensi, tahun_absensi, id_pegawai}, callback)`
|       |-- If error, return 500 error
|       |-- If no data found, return 404 error
|
|-- Get AbsenStandBulanan Data
|   |-- Call `AbsenStandBulanan.getAll({bulan_absensi, tahun_absensi, id_pegawai}, callback)`
|       |-- If error, return 500 error
|       |-- If no data found, return 404 error
|
|-- Process Data
|   |-- Initialize `dataDetails` array
|   |-- For each `absenStandBulanan` in `absenStandBulananData`:
|       |-- Get `total_kehadiran` for the employee
|       |-- Get `total_bon` for the employee
|       |-- Get `total_gaji_bulanan` for the employee
|       |-- Calculate `total_gaji_bersih` as `total_gaji_bulanan - total_bon`
|       |-- Get `nama_pegawai`
|       |-- Add `pegawai_details` to `absenStandBulanan`
|       |-- Push to `dataDetails` array
|
|-- Send Response
|   |-- Send response with `nama_outlet`, `bulan_absensi`, `tahun_absensi`, `absenStandHarianData`, `absenStandBulananData`, and `dataDetails`
|
End

