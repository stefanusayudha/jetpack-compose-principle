Disarankan untuk memparsing "Ready to display" data ke abstraksi level terendah dan menghindari proses reduksi lanjutan.
Kadang mungkin terasa lebih mudah untuk memparsing business data ke composable function dan membiarkan composable function tersebut berfikir bagaimana dia akan menampilkan data tersebut.
Itu bukan hal yang salah akan tetapi sangat disarankan untuk menjaga lowest level abstraction tetap murni dan tidak terikat pada konteks tertentu secara langsung.

# Why?
1. **Transparansi and Kontrak tak Kasat Mata**.
   Memparsing data mentah tanpa diformat mungkin akan melibatkan kesepakatan tidak terlihat. Misalnya saya:
   ```kotlin
   @Composable
   fun Date(entity: DomainModel) {
     Date(entity.date)
   }

   @Composable
   fun Date(date: String) {
     Text(date.formatDate("dd MMMM yyyy"))
   }
   ```

   Pada contoh di atas dapat dilihat bahwa: Pada abstraksi level terendah (Date Atom) terdapat reduksi lanjutan `date.formatDate("dd MMMM yyyy")`.
   Ini menimbulkan pertanyaan apakah ada format tertentu yang harus kita berikan pada argument `date: String`?
   Jawabannya: Ya. Ini adalah contoh kontrak tak kasat mata.

   Dalam contoh di atas, Date atom memiliki kesepakatan tidak terlihat dengan DomainModel dan kesepakatan ini mungkin tidak bekerja pada input yang lain.

   Oleh karena itu penting untuk tidak melakukan reduksi pada level abstraksi terendah. Disarankan untuk memindahkan proses reduksi ke dalam konteks wrapper. e.g:
   ```kotlin
   @Composable
   fun Date(entity: DomainModel) {
     Date(entity.date.formatDate("dd MMMM yyyy"))
   }

   @Composable
   fun Date(date: String) {
     Text(date, style = MaterialTheme.typography.caption)
   }
   ```
   Contoh di atas mengatakan bahwa fungsi atom `Date` pada abstraksi terendah tidak perduli pada bagaimana bentuk dari `date: String` yang dikirimkan. Dia hanya akan menampilkan data tersebut dengan styling yang sudah ditentukan.

2. **Membuat komponent menjadi tidak fleksible**.
   Pada contoh keliru sebelumnya; Kompenent `Date()` pada level abstraksi terendah nampak seperti reusable, akan tetapi pada kenyatannya tidak. Dia terikan kontrak tak kasat mata pada `DomainModel` object namun tidak mengatakan apapun tentang itu sama sekali.
   
