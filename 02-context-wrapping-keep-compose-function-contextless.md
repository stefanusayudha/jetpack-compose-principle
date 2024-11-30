# What?
Context Wrapping secara general adalah mekanisme untuk merubah perilaku dari suatu objek.
Sebuah object bisa memiliki sebuah perilaku identitas atau memiliki perilaku khusus pada konteks tertentu.

Mekanisme Context Wrapping sangat disarankan untuk menjaga suatu objek tetap murni pada level abstraksi paling dasar.

e.g:
```kotlin
@Composable
fun Person(
  emotion: Emotion = Emotion.Happy,
  sayHello: (() -> Unit)? = { println("Hello") }
)

// wrap with konteks at work
@Composable
fun PersonAtWork() {
  Person(
    emotion = Emotion.Serious,
    sayHello = null
  )
}
```

```kotlin
@Composable
fun Car(
  color: Color = Color.Green,
)

// Inactive Car
@Composable
fun InActiveCar() {
  Car(
    color = Color.Red
  )
}
```
