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
	
