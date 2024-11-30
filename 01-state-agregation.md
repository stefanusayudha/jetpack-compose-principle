# State Agregation

## What?
State Agregation adalah metode mengelompokan dan membungkus state dalam 1 record class atau data class.

```kotlin
data class WidgetState(
  val accountName: String,
  val accountBalance: String
)

@Composable
fun Widget(
  state: WidgetState
) {
  Column {
    Text(state.accountName)
    Text(state.accountBalance)
  }
}
```

## Why?
1. **Menghindari Superfluous Arguments**.
   Jika kita memparsing data satu persatu ke masing-masing widget didalam screen,
   kita mungkin akan kehilangan jejak tentang kenapa argument itu diperlukan atau siapa yang memerlukan argument tersebut ketika component dalam composable function terlalu banyak
   atau komponent berada dalam scope yang sangat jauh didalam.

   Dengan melakukan agregasi state untuk setiap widget / component, akan lebih mudah mengetahui siapa yang memerlukan data tersebut. e.g:
   ```kotlin
   data class ScreenState(
     val widget1State: Widget1State,
     val widget2State: Widget2State,
     ...
   )
   ```
2. **Centralized Control**.
   Melakukan state agregation memungkinkan kita untuk memusatkan tanggung jawab kontrol state ke pada satu objek - dan atau ke satu tempat -
   serta membuat sumber kebenaran tunggal untuk seluruh kejadian pada composable function.

   Menempatkan kontrol state ke satu tempat akan memberikan kemudahan dalam pengujian, dan memusatkan kompleksitas logic di satu tempat.
   

## Concern
1. Anda mungkin memiliki concern bahwa itu mungkin akan mentriger rekomposisi ketika state pusat berubah maka semua akan di rekomposisi. Atau anda mungkin bertanya bagaimana menghindari rekomposisi?

   Jawab: Rekomposisi tidak sepenuhnya relevan dengan mekanisme Agregasi State, melainkan pada stability type.
   Jetpack compose tidak melihat bagaimana state akan di kirimkan kepadanya untuk melakukan rekomposisi, melainkan bagaimana karakteristik stabilitas dari argument tersebut.
   Jetpack compose cukup pintar untuk mengabaikan rekomposisi yakni dengan melakukan stability check.

   Stabilitas dalam jetpack compose mencakup : Never Equal ("!="), Structural Equal ("=="), dan Referential Equal ("===").
   Jika konsern anda adalah tentang rekomposisi maka anda perlu memperhatikan bagaimana ciri-ciri dari property yang anda kirimkan dan bagaimana anda memproduksinya.
   Misalnya; umumnya semua objek yang di import dari Java library akan diperlakukan sebagai unstable state.
2. Apakah harus menggunakan data class dan immutable state?

   Jawabannya adalah tergantung kebutuhan anda. Anda bisa menggunakan simple class sebagai Record atau data class jika anda memerlukan api dari data class seperi copy dan lain-lain.

## Catch
1. -
